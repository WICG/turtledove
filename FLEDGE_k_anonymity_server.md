# Privacy Sandbox k-Anonymity Server

## The k-anonymity server

The [FLEDGE](FLEDGE.md) proposal calls for k-anonymity thresholds on network
updates to interest groups.  A browser should not request interest group
updates unless there are at least $k$ other browsers that reported being in
the same interest group within a TTL period.  Joining an interest group is
a local action stored by the user's browser, so implementing k-anonymity
thresholds requires a central server to count how many different browsers
have joined a given interest group and reveal to all browsers when that count
becomes at least k.  In this explainer we discuss this counting server and
how we're taking a private approach to its design.

Interest groups are one use case for k-anonymity thresholds.  FLEDGE also
calls for thresholds on the `renderUrl` that won an auction, and there
are other Privacy Sandbox APIs, such as Shared Storage, that may impose
k-anonymity thresholds.  Other browser features, like [`start_url` parameters
for progressive web apps](https://github.com/w3c/manifest/issues/399),
might benefit from k-anonymity thresholds as well.

Given the variety of use cases, we intend to implement a k-anonymity server
that's quite general.  Let's define an object as the browser-stored state
(e.g. an interest group) that we wish to have a k-anonymity threshold for.
We'll design the server to operate on integers `s = Hash(object)`; that
is, every **object** will be hashed consistently across browsers.  On the
server each hash will map to a single **set**, where that set contains all the
browsers that have told the server they have the object as local browser-state.
To support different use cases, with possibly different server-side behavior,
we'll define a **type** for each set.

The diagram below shows the write path, which we call **`Join`**, from a
browser to the server.  The browser has an identifier or token, `b`, that is
used by the server for counting purposes.  The browser hashes the object,
computing a set hash `s`.  It sends these parameters, along with the type
of the set (e.g. "interest group"), `t`, to the server: `Join(b, t, s)`.
The server will store this membership and apply a type-defined TTL that's
configured on the server.  When the TTL expires, the server will stop
considering the browser `b` as part of `(t,s)` unless the browser sends
another `Join` request that resets the TTL for its membership.

![join request](assets/kanon_join_request.svg)

The server also needs a read endpoint to expose back to the browser whether
a particular set is above the k-anonymity threshold.  We call this endpoint
**`Query`**, and it takes a type `t` and the set `s` to check: `Query(t, s)`.
It returns a boolean that is true if the set has met the k-anonymity threshold.

The list of sets above the threshold, each represented as `(t, s)` and used to
serve `Query` requests, is updated periodically with recent `Join` requests.
A given typed set $S$ is included in the update if the cardinality of the
set is at least $|S|\geq k\pm\epsilon$.  If the cardinality falls below this
threshold the set may be removed on the next update.

![query request](assets/kanon_query_request.svg)

The browser will periodically call `Query` for its local objects and update
one bit per-object, `is_kanon`, with the result. This bit can be used by the
browser to enforce thresholds.  For example, the browser could only request
network updates for interest groups where `is_kanon == true` or only render
an ad if the stored `renderUrl` has the bit set.

This simple design implements the business functionality of the server,
but there are many other things we want to do to ensure the server protects
the privacy of Chrome users.

## How we're thinking about privacy

We recognize the sensitivity of the information being sent to this server:
the set hashes might represent browsing behavior, whether an interest group
or otherwise.  These are highly sensitive, and we don't want the server,
or someone interacting with the server, to be able to link set hashes back
to individual users.

In designing this server we're taking an iterative, privacy-focused approach.
Our initial design contains robust privacy protections that are outlined in
more detail below.  Over time we plan to further strengthen privacy protections
as research areas advance and new technologies and tools become available.

### What we're doing now

#### Willful IP blindness

First, this server will adhere to the [willful IP blindness
principles](https://github.com/bslassey/ip-blindness/blob/master/proposed_willful_ip_blindness_principles.md).
The server doesn't need to know the IP address of a client, and
we won't store it or use it for any purpose except the conforming
use cases described in the principles.  When [Chrome's Near-path
NAT](https://github.com/bslassey/ip-blindness/blob/master/near_path_nat.md)
launches we expect that calls to this server will be routed through that
proxy for users that turn it on, further hiding those users' IP addresses
from this server.

#### Low-entropy identifiers

Next, we're taking a conservative approach to the browser identifier, `b`,
that is sent with `Join` requests.  Even the operator of the k-anonymity server
shouldn't be able to identify unique users calling `Join`.  If the operator
cheats, and examines the database stored by `Join`, we're protecting users
by only sending to `Join` a value `b` with entropy limited to $j$ bits.
We expect $8\leq j\leq 16$, meaning there are no more than 65,536 different
possible identifiers; which is far lower than the number of 1-day active
users of desktop Chrome.

With our IP blindness principles, we have no way of distinguishing users that
share a `b` when they call `Join(b, t, s)`.  When we count the cardinality
of a set on the server, each distinct `b` will be counted only once, even if
multiple users join the same set with identical values of `b`.  This means
that our cardinality calculation may undercount and that we can only count
up to a limit of $2^j$.  We expect all the k-anonymity thresholds we need to
enforce will have $k\leq 2^j$.  Chrome code that calls `Join` will be part of
the Chromium open source codebase, and can enforce on the user's device that
`b` is not longer than $j$ bits.

#### Abuse and invalid traffic

Abusive or malicious writes to `Join` can undermine the k-anonymity thresholds,
misleading browsers into thinking they are members of a k-anonymous interest
group when, in fact, the other members of the group are not real.  It is
important that we protect `Join` against malicious write traffic, and,
to maintain privacy, that we do this in an anonymous way.

To protect this endpoint we will use [Trust
Tokens](https://github.com/WICG/trust-token-api).  Every write to Join will
require a one-time-use Trust Token be attached to the request, and tokens
will be bound to a specific low-entropy identifier, `b`.  Each browser will
be issued tokens with its assigned `b`, and it can spend those tokens as it
wishes to make `Join` calls to the server.

We will operate a Trust Token issuer specific to this server and these tokens;
we'll call this issuer **`Sign`**.  In our current proposal, `Sign` will
require, at least initially for desktop Chrome, that the user be signed-in
to Chrome with a Google Account.  Requiring sign-in lets us rate limit the
number of tokens issued to a given user, assign each user a stable, but
resettable, value for `b`, and prevent naive abuse of `Join` by anonymous
users.  Even though the user is signed-in, and Google Account credentials
are used to issue Trust Tokens, the Trust Tokens received by `Join` [cannot be
linked](https://github.com/WICG/trust-token-api#cryptographic-property-unlinkability)
back to the Google Account they were issued to.  The Trust Token issuer can
learn which users join a large number of interest groups.  To guard against
this, we're exploring options that include having the client request tokens
at a constant rate and discard unused tokens.

`Query` is a read-only API, so it doesn't have the same abuse concerns as
`Join`.  We won't require Trust Tokens, or a Google Account, for a browser
to call `Query`.

#### Differential privacy of public data

We're working to ensure that the data this server exposes through `Query`, that
is the set of k-anonymous hashes, meets a quantifiable level of [differential
privacy](https://medium.com/georgian-impact-blog/a-brief-introduction-to-differential-privacy-eacf8722283b)
and does not reveal information about what sets a non-malicious user may have
joined.  In addition, we want to bound false negatives and false positives
within the set of k-anonymous hashes because false positives pose a privacy
risk and false negatives limit the utility of FLEDGE.

### Privacy enhancements we are exploring

To build on the privacy protections we are implementing today, there are a
few different areas we're researching that could offer even better privacy
to Chrome users.  None of these approaches are ready for production today,
but we commit to continue investing in research, prototyping, and testing
in these areas.

#### Private information retrieval

The `Query` endpoint to this server receives sensitive information in
the form of set hashes that the browser wants to check the k-anonymous
property of.  If `Query` requests are batched, with multiple set
hashes in a single request, then that request contains cross-site data
known to be from a single browser.  [Private information retrieval
(PIR)](https://en.wikipedia.org/wiki/Private_information_retrieval) is a
technique that could allow the server to process `Query` requests, either
batched or unbatched, without the server knowing which set hashes are being
queried.  We're exploring both single-party PIR, which currently has a lot
of network and computational overhead, and multi-party PIR, which has less
overhead but the additional complexity of operating two non-colluding servers
with consistent copies of the dataset.

#### Anonymous tokens to replace low entropy identifiers

We're working on researching and testing a privacy improvement to low-entropy
browser identifiers.  The $j$-bit identifier, `b`, is constant for a given
browser, which allows some inferences to be made by the `Join` server, in
spite of collisions between users.  To improve the privacy of this scheme
and increase the accuracy of our cardinality calculations, while maintaining
our ability to prevent abusive traffic, we're researching additions to Trust
Token APIs.

We are working on extending anonymous tokens in a way that enables users to
obtain signatures on the set being joined without revealing the set to `Sign`.
We also aim to enable additional functionality that prevents users from
obtaining multiple tokens for the same set, letting us verify on the server
that browsers aren't joining the same set more frequently than they should.

#### Trusted execution environments

We are exploring approaches to transition components
of this server to run in [trusted execution environments
(TEEs)](https://en.wikipedia.org/wiki/Trusted_execution_environment) with open
source code.  TEEs implemented by chip manufacturers and cloud providers could
allow the browser to verify the server code executing matches the open source
project and offer encryption of the server's RAM while in-use, protecting
some of the server's data from insider access.  Combined with thoughtful
key management, TEEs could offer an opportunity to increase the privacy of
other server functions like counting cardinalities and persisting state.

#### Device attestations

Our initial reliance on Google Accounts to issue Trust Tokens
that authenticate writes is necessary partly because we
don't have other methods of authenticating a Chrome browser
to a server.  Some platforms, like Android with [SafetyNet
attestations](https://developer.android.com/training/safetynet/attestation),
can assure the server that a request originates from a legitimate device
and client application.  Desktop Chrome, however, runs on many different
platforms with varying degrees of platform-level security.  In the future we
hope to develop methods of attesting to our server that requests are from
a legitimate instance of desktop Chrome without necessarily requiring the
user to be signed-in to their Google Account.

## The impact of our privacy decisions on advertisers

The choices we are making here to protect user privacy impact the behavior
of the server, and the behavior of FLEDGE within Chrome.

By using low-entropy identifiers that intentionally collide among browsers
we by design undercount the cardinality of a given set hash.  This has the
potential to require more than k browsers to join a set before it is marked
k-anonymous.  Even if $n>k$ browsers join a given set, if all those browsers by
chance have the same `b`, then the set won't be marked k-anonymous.  We will
mitigate this by choosing a uniform distribution of `b` identifiers across
browsers.  Over time we hope to migrate from low-entropy identifiers to the
anonymous token scheme, which does not undercount cardinality in the same way.

To ensure differential privacy of the output data from this
server, i.e. the set of k-anonymous set hashes, we must
limit how frequently we update the data.  We must also [add
noise](https://en.wikipedia.org/wiki/Additive_noise_mechanisms) to the
membership of a given set hash in the output.  These restrictions mean that an
interest group will not be marked k-anonymous immediately after the $k^{\rm th}$
user joins; there may be some delay due to added noise.  This noise is expected
to be larger than the undercounting error from low-entropy identifiers.
Noise is required to provide a differentially-private output data set,
so we don't anticipate changing this behavior.

To prevent abuse of the `Join` API, we are only allowing writes from users
that are signed-in to Chrome.  Developers can still use FLEDGE for users
that aren't signed-in to Google within the Chrome browser, including making
calls to `Query` to check k-anonymity thresholds. However, we recognize that
the addition of those users to interest groups won't contribute to counts that
make a set hash k-anonymous. We recognize that this may bias the system against
interest groups that might be more popular with signed-out users. Over time
we expect to reduce, or otherwise eliminate, this potential bias by adding
support for device attestation or other approaches to device-level trust.

The Trust Token issuer, `Sign`, will enforce limits on token issuance to
each Google Account.  Tokens are one-time-use, so these limits will restrict
the number of `Join` calls a browser can make in a given period of time.
This doesn't necessarily limit the number of interest groups the browser can
join locally, only the number it will be considered a part of when computing
k-anonymity on the server.  This limit will be per-user, and the browser can
make decisions about which interest groups to spend its tokens on and make
`Join` calls for.
