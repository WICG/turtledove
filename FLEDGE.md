# First Experiment (FLEDGE)

This document describes an early prototype for ads serving in the TURTLEDOVE family, appropriate for experimentation before a fully-featured system is ready.  It would be the First "Locally-Executed Decision over Groups" Experiment.

Chrome expects to build and ship this first experiment during 2021.  The goal is for us to gain implementer experience, and for the ads ecosystem to evaluate its usability, as soon as it is feasible to do so.  We need a robust API to take flight before Chrome's expected removal of third-party cookies in 2023.

We plan to hold regular meetings under the auspices of the WICG to go through the details of this proposal and quickly make any needed changes.  Please comment on the timing question in Issue [#88](https://github.com/WICG/turtledove/issues/88) if you want to attend these meetings to be involved in hammering out details.


- [Summary](#summary)
- [Background](#background)
- [Design Elements](#design-elements)
  - [1. Browsers Record Interest Groups](#1-browsers-record-interest-groups)
    - [1.1 Joining Interest Groups](#11-joining-interest-groups)
    - [1.2 Interest Group Attributes](#12-interest-group-attributes)
  - [2. Sellers Run On-Device Auctions](#2-sellers-run-on-device-auctions)
    - [2.1 Initiating an On-Device Auction](#21-initiating-an-on-device-auction)
    - [2.2 Auction Participants](#22-auction-participants)
    - [2.3 Scoring Bids](#23-scoring-bids)
  - [3. Buyers Provide Ads and Bidding Functions (BYOS for now)](#3-buyers-provide-ads-and-bidding-functions-byos-for-now)
    - [3.1 Fetching Real-Time Data from a Trusted Server](#31-fetching-real-time-data-from-a-trusted-server)
    - [3.2 On-Device Bidding](#32-on-device-bidding)
    - [3.3 Metadata with the Ad Bid](#33-metadata-with-the-ad-bid)
    - [3.4 Ads Composed of Multiple Pieces](#34-ads-composed-of-multiple-pieces)
  - [4. Browsers Render the Winning Ad](#4-browsers-render-the-winning-ad)
  - [5. Event-Level Reporting (for now)](#5-event-level-reporting-for-now)
    - [5.1 Seller Reporting on Render](#51-seller-reporting-on-render)
    - [5.2 Buyer Reporting on Render and Ad Events](#52-buyer-reporting-on-render-and-ad-events)
    - [5.3 Losing Bidder Reporting](#53-losing-bidder-reporting)


## Summary

During 2021, Chrome plans to run an Origin Trial for a first experiment which includes:



*   Interest Groups, stored by the browser, and associated with arbitrary metadata that can affect how these groups bid and render ads.
*   A mechanism for periodic background updating of these interest groups, available as long as the number of people in the interest group exceeds a k-anonymity threshold.
*   On-device bidding by buyers (DSPs or advertisers), based on interest-group metadata and on data loaded from a trusted server at the time of the on-device auction — with a _temporary and untrusted_ "Bring Your Own Server" model, until a trusted-server framework is settled and in place.
*   On-device ad selection by the seller (an SSP or publisher), based on bids and metadata entered into the auction by the buyers.
*   Microtargeting protection based on the browser ensuring that the same ad or ad component is being shown to at least some minimum number of people.
*   Ad rendering in a temporarily relaxed version of Fenced Frames that prevents interaction with the surrounding page — but that does allow _normal network access for rendering the ad, and for logging and reporting some event-level outcomes_, as a temporary model until both a trusted-server reporting framework and ad delivery via Web Bundles are settled and in place.

Most of these ideas are drawn from the past year's ongoing discussion of variants on the [original TURTLEDOVE](https://github.com/WICG/turtledove/blob/master/Original-TURTLEDOVE.md) idea.  Interest group metadata, and applying k-anonymity thresholds only to network updates and rendered ads, come from [Outcome-based TURTLEDOVE](https://github.com/WICG/turtledove/blob/master/OUTCOME_BASED.md) and [Product-level TURTLEDOVE](https://github.com/WICG/turtledove/blob/master/PRODUCT_LEVEL.md).  The separation and clarification of the DSP and SSP roles are along the lines described in [TERN](https://github.com/WICG/turtledove/blob/master/TERN.md) and [PARRROT](https://github.com/prebid/identity-gatekeeper/blob/master/proposals/PARRROT.md).  The trusted servers to support bidding, rendering, and reporting come from the [SPARROW](https://github.com/WICG/sparrow) Gatekeeper and [Dovekey](https://github.com/google/ads-privacy/tree/master/proposals/dovekey) Key-Value server.

This still lacks some features that are important for web advertising, and lacks some privacy protections that are important for preventing cross-site tracking.  For example, the first experiment does not (yet) have a mechanism for multi-level on-device auctions, an ad ecosystem feature that needs more design work before we can offer on-device support.  And the temporary italicized carve-outs in the above bulleted list all need privacy-safe replacements before our work is done.

But while third-party cookies are still available, this simplified opt-in preview of a post-3p-cookies technique offers a way that we can all experiment with the on-device ad selection approach together.


## Background

In January 2020, Chrome published an explainer for TURTLEDOVE, proposing a new way that advertisers and ad tech companies could target an ad to an audience they had built, without the browser revealing anyone's browsing habits or ad interests.

Over the course of extensive engagement and discussion with many participants in the ad tech ecosystem, we've learned many ways in which the original proposal could better meet their needs.  Discussion took place over the past year, primarily in (1) GitHub issues on the TURTLEDOVE repo, (2) the many counter-proposals that built on the original explainer (most things in [this list](https://github.com/w3c/web-advertising#ideas-and-proposals-links-outside-this-repo) with bird names), and (3) the weekly meetings of the W3C's Improving Web Advertising Business Group.

Of the improvements proposed, some can be accommodated through straightforward changes to APIs, some will require deeper gathering of requirements and careful design work, and some will require new server-side infrastructure with novel trust relations with multiple parties.  This update focuses primarily on the first of these types.


## Design Elements

Ads can be targeted at **_interest groups_**.  As in the original TURTLEDOVE proposal, the browser is responsible for knowing which interest groups it has joined.

Every interest group has an **_owner_** who will act as a buyer in an on-device ad auction.  The owner is ultimately responsible for the group's membership and usage, but can delegate those tasks to third parties if they so desire.  Many sorts of entities might want to be owners of interest groups.  Some examples include:

*   An advertiser (or a third party working on the advertiser's behalf) might create and own an interest group of people whom they believe are interested in that advertiser's product.  Classical remarketing/retargeting use cases fall under this example.
*   A publisher (or a third party working on the publisher's behalf) might create and own an interest group of people who have read a certain type of content on their site.  Publishers can already use first-party data to let advertisers target their readers on the publisher site.  A publisher-owned interest group could let publishers do the same even when those people are browsing other sites.  Publishers would presumably charge for the ability to target this list.
*   A third-party ad tech company might create and own an interest group of people whom they believe are in the market for some category of item.  They could use that group to serve ads for advertisers who work with that ad tech company and sell things in that category.

All the logic of the on-device auctions will execute inside a collection of dedicated **_worklets_**.  Each worklet is associated with a single domain, and runs code written by either a buyer or a seller.  The code in the worklets cannot access or communicate with the publisher page or the network.  The browser is responsible for creating those worklets, loading the relevant buyer or seller logic from the provided URLs, fetching real-time data from a trusted server, calling the appropriate functions with specified input, and passing on the output.  We will publish a separate explainer on dedicated worklets.

The on-device bidding flow includes a way that the worklets can use some data loaded from a **_trusted server_**.  The browser is willing to ask this server questions which might reveal sensitive information, like the set of all interest groups it has joined.  This requires a server that performs no event-level logging and has no other side effects based on these requests.  One can imagine a wide range of ways that a server might earn the trust of a browser, including both policy approaches (trusted third party, audited code, etc) and technical guarantees (secure multi-party computation, secure enclaves, etc).  We began robust discussions in early 2021 on what sorts of server-trust models seem feasible to browsers and buyers, with the expectation that initially productionization speed is essential, but trust requirements may increase over time.


### 1. Browsers Record Interest Groups


#### 1.1 Joining Interest Groups

Browsers keep track of the set of interest groups that they have joined.  For each interest group, the browser stores information about who owns the group, what ads the group might choose to show, various JavaScript functions and metadata used in bidding and rendering, and what servers to contact to update or supplement that information.


```
const myGroup = {
  'owner': 'https://www.example-dsp.com',
  'name': 'womens-running-shoes',
  'biddingLogicUrl': ...,
  'biddingWasmHelperUrl': ...,
  'dailyUpdateUrl': ...,
  'trustedBiddingSignalsUrl': ...,
  'trustedBiddingSignalsKeys': ['key1', 'key2'],
  'userBiddingSignals': {...},
  'ads': [shoesAd1, shoesAd2, shoesAd3],
  'adComponents': [runningShoes1, runningShoes2, gymShoes, gymTrainers1, gymTrainers2],
};
navigator.joinAdInterestGroup(myGroup, 30 * kSecsPerDay);
```


The browser will only allow the `joinAdInterestGroup()` operation with the permission of both the site being visited and the group's owner.  The site can allow or deny permission to any or all third parties via a `Permissions-Policy` (directive named "join-ad-interest-group"), where the default policy is to allow all in the top-level page and to deny all in cross-domain iframes.  The group's owner can indicate permission by `joinAdInterestGroup()` running in a page or iframe in the owner's domain, and can delegate that permission to any other domains via a list at a `.well-known` URL.  These can be combined, to allow a DSP to add a person to one of its interest groups based on publisher context, as discussed in [TERN](https://github.com/WICG/turtledove/blob/master/TERN.md#c-contextual-interest-groups) — provided the publisher's `Permissions-Policy` permits interest group additions by its SSP, and the DSP gives this SSP this ability.  If some permission is missing, `joinAdInterestGroup()` will raise an `Error` describing the reason for failure.

There is a complementary API `navigator.leaveAdInterestGroup(myGroup)` which looks only at `myGroup.name` and `myGroup.owner`.  As a special case to support in-ad UIs, invoking `navigator.leaveAdInterestGroup({})` from inside an ad that is being targeted at a particular interest group will cause the browser to leave that group, irrespective of permission policies.

The browser will remain in an interest group for only a limited amount of time.  The duration is specified in the call to `joinAdInterestGroup()`, and will be capped at 30 days.  This can be extended by calling `joinAdInterestGroup()` again later, with the same group name and owner.  Successive calls to `joinAdInterestGroup()` will overwrite the previously-stored values for any interest group properties, like the group's `userBiddingSignal` or list of ads.


#### 1.2 Interest Group Attributes

The `userBiddingSignals` is for storage of additional metadata that the owner can use during on-device bidding, and the `trustedBiddingSignals*` attributes provide another mechanism for making real-time data available for use at bidding time.

The `biddingWasmHelperUrl` field is optional, and lets the bidder provide computationally-expensive subroutines in WebAssembly, rather than JavaScript, to be driven from the JavaScript function provided by `biddingLogicUrl`. If provided, it must point to a WebAssembly binary, delivered with a `application/wasm` mimetype. The corresponding `WebAssembly.Module` will be made available by the browser to the `generateBid` function.

The `dailyUpdateUrl` provides a mechanism for the group's owner to periodically update the attributes of the interest group: any new values returned in this way overwrite the values previously stored (except that the `name` and `owner` cannot be changed).  However, the browser will only allow daily updates when a sufficiently large number of people have the same `dailyUpdateUrl` , e.g. at least 100 browsers with the same update URL. This will not include any metadata, so data such as the interest group `name` should be included within the URL, so long as the URL exceeds the minimum count threshold.  (Without this sort of limit, a single-person interest group could be used to observe that person's coarse-grained IP-Geo location over time.)

The `ads` list contains the various ads that the interest group might show.  Each entry is an object that includes both a rendering URL and arbitrary metadata that can be used at bidding time.

The `adComponents` field contains the various ad components (or "products") that can be used to construct ["Ads Composed of Multiple Pieces"](https://github.com/WICG/turtledove/blob/main/FLEDGE.md#34-ads-composed-of-multiple-pieces)). Similarly to the `ads` field, each entry is an object that includes both a rendering URL and arbitrary metadata that can be used at bidding time. Thanks to `ads` and `adsComponents` being separate fields, the buyer is able to update the `ads` field via daily update without losing `adComponents` stored in the interest group.

The browser will provide protection against microtargeting, by only rendering an ad if the same rendering URL is being shown to a sufficiently large number of people (e.g. at least 100 people would have seen the ad, if it were allowed to show).  As discussed in the [Outcome-Based TURTLEDOVE](https://github.com/WICG/turtledove/blob/master/OUTCOME_BASED.md) proposal, this threshold applies only to the rendered creative; all of the metadata associated with ad bidding is not under any such restriction.  Since a single interest group can carry multiple possible ads that it might show, the group will have an opportunity to re-bid another one of its ads to act as a "fallback ad" any time its most-preferred choice is below threshold.  This means that a small, specialized interest group that is still below the daily-update threshold could still choose to participate in auctions, bidding with a more-generic ad until the group becomes large enough.


### 2. Sellers Run On-Device Auctions

Interest groups are used to bid in on-device auctions on sites selling ad space.  We refer to the party running the auction as the _seller_.  Many parties might act as sellers: a site might run its own ad auction, or might include a third-party script to run the auction for it, or might use an SSP that combines running an on-device auction with other server-side ad auction activities.

Sellers have three basic jobs in the on-device ad auction:



1. Sellers decide (a) which buyers may participate, and (b) which of the bids from those buyers' interest groups are eligible to enter the auction.  This lets the seller enforce the site's rules for what ads are allowed to appear on the page.
2. Sellers are responsible for the business logic of the auction: JavaScript code which considers each bid's price and metadata, and calculates a "desirability" score.  The bid with the highest desirability score wins the auction.
3. Sellers perform reporting on the auction outcome, including information about clearing price and any other pay-outs.  (The winning and losing buyers also get to do their own reporting; see below for buyer jobs.)


#### 2.1 Initiating an On-Device Auction

A seller initiates an auction by invoking a JavaScript API inside the publisher's page:


```
const myAuctionConfig = {
  'seller': 'https://www.example-ssp.com',
  'decisionLogicUrl': ...,
  'trustedScoringSignalsUrl': ...,
  'interestGroupBuyers': ['https://www.example-dsp.com', 'https://buyer2.com', ...],
  'additionalBids': [otherSourceAd1, otherSourceAd2, ...],
  'auctionSignals': {...},
  'sellerSignals': {...},
  'sellerTimeout': 100,
  'perBuyerSignals': {'https://www.example-dsp.com': {...},
                        'https://www.another-buyer.com': {...},
                        ...},
  'perBuyerTimeouts': {'https://www.example-dsp.com': 50,
                        'https://www.another-buyer.com': 200,
                        '*': 150,
                        ...},
};
const auctionResultPromise = navigator.runAdAuction(myAuctionConfig);
```


This will cause the browser to execute the appropriate bidding and auction logic inside a collection of dedicated worklets associated with the buyer and seller domains.  The `auctionSignals`, `sellerSignals`, and `perBuyerSignals` values will be passed as arguments to the appropriate functions that run inside those worklets — the `auctionSignals` are made available to everyone, while the other signals are given only to one party.

The returned `auctionResultPromise` object is _opaque_: it is not possible for any code on the publisher page to inspect the winning ad or otherwise learn about its contents, but it can be passed to a Fenced Frame for rendering.  (The [Fenced Frame Opaque Source explainer](https://github.com/shivanigithub/fenced-frame/blob/master/OpaqueSrc.md) has initial thoughts about how this could be implemented.)  If the auction produces no winning ad, the return value can also be null, although this non-opaque return value leaks one bit of information to the surrounding page.  In this case, for example, the seller might choose to render a contextually-targeted ad.

Optionally, `sellerTimeout` can be specified to restrict the runtime (in milliseconds) of the seller's `scoreAd()` script, and `perBuyerTimeouts` can be specified to restrict the runtime (in milliseconds) of particular buyer's `generateBid()` scripts. If no value is specified for the seller or a particular buyer, a default timeout of 50 ms will be selected. Any timeout higher than 500 ms will be clamped to 500 ms. A key of `'*'` in `perBuyerTimeouts` is used to change the default of unspecified buyers.

A `Permissions-Policy` directive named "run-ad-auction" controls access to the `navigator.runAdAuction()` API.


#### 2.2 Auction Participants

Each interest group the browser has joined and whose owner is in the list of `interestGroupBuyers` will have an opportunity to bid in the auction.  See the "Buyers Provide Ads and Bidding Functions" section, below, for how interest groups bid.

The seller may instead specify `'interestGroupBuyers': '*'` to permit all interest groups into the auction, and decide ad admissibility later in the process, based on criteria other than the interest group owner.  For example, a seller with an out-of-band creative review process might decide admissibility solely based on the creative, not the buyer.

The auction configuration's list of `additionalBids` is meant to allow a second way for ads to participate in this on-device auction.  More design work is needed to figure out how to support buyers' and sellers' needs in the multi-level decision-making use cases like those discussed in Issues [#59](https://github.com/WICG/turtledove/issues/59) or [#73](https://github.com/WICG/turtledove/issues/73) — a simple approach like putting one auction's `auctionResultPromise` into another auction's `additionalBids` is probably not sufficient.  This field may not be usable yet in the First Experiment stage, depending on how much new complexity it adds to the design.


#### 2.3 Scoring Bids

Once the bids are known, the seller runs code inside an _auction worklet_.  Within this worklet, the seller's auction script has an opportunity to consider each individual ad one at a time, with its associated bid and metadata, and then assign a numerical desirability score.  The seller's JavaScript is loaded from the auction configuration's `decisionLogicUrl`, which must expose a `scoreAd()` function:


```
scoreAd(adMetadata, bid, auctionConfig, trustedScoringSignals, browserSignals) {
  ...
  return desirabilityScoreForThisAd;
}
```


The function gets called once for each candidate ad in the auction.  The arguments to `scoreAd()` are:

*   adMetadata: Arbitrary metadata provided by the buyer.
*   bid: A numerical bid value.
*   auctionConfig: The auction configuration object passed to `navigator.runAdAuction()`.
*   trustedScoringSignals: A value retrieved from a real-time trusted server chosen by the seller and reflecting the seller's opinion of this particular creative, as further described in [3.1 Fetching Real-Time Data from a Trusted Server](#31-fetching-real-time-data-from-a-trusted-server) below.  (In the case of [ads composed of multiple pieces](https://github.com/WICG/turtledove/blob/main/FLEDGE.md#34-ads-composed-of-multiple-pieces) this should instead be some collection of values, structure TBD.)
*   browserSignals: An object constructed by the browser, containing information that the browser knows and which the seller's auction script might want to verify:
    ```
    { 'topWindowHostname': 'www.example-publisher.com',
      'interestGroupOwner': 'https://www.example-dsp.com',
      'renderUrl': 'https://cdn.com/render_url_of_bid',
      'adComponents': ['https://cdn.com/ad_component_of_bid',
                       'https://cdn.com/next_ad_component_of_bid',
                       ...],
      'biddingDurationMsec': 12,
      'dataVersion': 1, /* Data-Version value from the trusted scoring signals server's response */
    }
    ```

The output of `scoreAd()` is a number indicating how desirable this ad is.  Any value that is zero or negative indicates that the ad cannot win the auction.  (This could be used, for example, to eliminate any interest-group-targeted ad that would not beat a contextually-targeted candidate.)   The winner of the auction is the ad object which was given the highest score.

The logic in `scoreAd()` has access to the full auction configuration object, which means the seller can pass in arbitrary information from the publisher page.  In particular, the configuration object's `sellerSignals` field is exclusively for passing information into `scoreAd()`.  This field can include information based on looking up publisher settings, based on making a contextual ad request, and so on.  Examples of logic that could live in the `scoreAd()` function include:

*   Calculation of a publisher pay-out, based on the ad bid along with arbitrary seller data and arbitrary metadata included in the `adObject`.  This pay-out could influence both the ad's desirability and the post-auction reporting used for moving money around.
*   Honoring publisher deals that promote certain ads over others irrespective of a per-page auction price.
*   Disqualifying some ads, by returning a desirability score of 0 (which means the ad will not win).  This could, for example, be based on comparing ad metadata (e.g. advertiser or topic) to the publisher site's rules for what ads are allowed to appear on the page, or based on the bidder being slow (as revealed by `biddingDurationMsec`).
*   Checking whether the creative contents have been pre-approved by the seller.  This could be implemented by an out-of-band creative review process leading to the seller handing the buyer a cryptographically-signed token, which the buyer then includes in the ad's metadata.  See [TERN's Section 0](https://github.com/WICG/turtledove/blob/master/TERN.md#0-before-advertising-begins) for more discussion of this possibility.

Note that `scoreAd()` does not have any way to _store_ information for use later on a different page.  In particular, if the ad scoring logic on day 1 observes a bid from a particular interest group, and then on day 2 the browser interest group membership expires, there is no way for the ad scoring logic on day 3 to "remember" the pre-expiration membership information.


### 3. Buyers Provide Ads and Bidding Functions (BYOS for now)


Interest groups are used by their owners to bid in on-device auctions.  We refer to each party bidding in an auction as a _buyer_.  The buyer might be an advertiser who is also an interest group owner themselves, or a DSP that owns an interest group and acts on an advertiser's behalf.

Alternatively, the interest group owner might be a party whose responsibility is focused on building audiences and letting many different advertisers use them for targeting.  In this case, the owner of the interest group is still the buyer, for the purposes of the seller's on-device auction.  The interest group owner's choice of which advertiser's ad to display might be made via some server-side decision process, or might be made on a case-by-case basis on-device as part of the bidding process.

Buyers have three basic jobs in the on-device ad auction:



1. Buyers choose whether or not they want to participate in an auction.
2. Buyers pick a specific ad, and enter it in the auction along with a bid price and whatever metadata the seller expects.
3. Buyers perform reporting on the auction outcome.  All buyers have a mechanism for aggregate reporting.  _As a temporary mechanism_, the buyer who wins the auction has a way to do event-level reporting alongside their ad rendering; see the "Event-Level Reporting (for now)" section.


#### 3.1 Fetching Real-Time Data from a Trusted Server


Buyers may want to make on-device decisions that take into account real-time data (for example, the remaining budget of an ad campaign).  This need can be met using the interest group's `trustedBiddingSignalsUrl` and `trustedBiddingSignalsKeys` fields.  Once a seller initiates an on-device auction on a publisher page, the browser checks each participating interest group for these fields, and makes an uncredentialed (cookieless) HTTP fetch to a URL of the form:

    https://www.kv-server.example/getvalues?hostname=publisher.com&keys=key1,key2

The base URL `https://www.kv-server.example/getvalues` comes from the interest group's `trustedBiddingSignalsUrl`, the hostname of the top-level webpage where the ad will appear `publisher.com` is provided by the browser, and `keys` is a list of `trustedBiddingSignalsKeys` strings, perhaps coalesced (for efficiency) across any number of interest groups that share a `trustedBiddingSignalsUrl`.  The response from the server should be a JSON object whose keys are key1, key2, etc., and whose values will be made available to the buyer's bidding functions (un-coalesced).

Similarly, sellers may want to fetch information about a specific creative, e.g. the results of some out-of-band ad scanning system.  This works in much the same way, with the base URL coming from the `trustedScoringSignalsUrl` property of the seller's auction configuration object. However, it has two sets of keys: "renderUrls=url1,url2,..." and "adComponentRenderUrls=url1,url2,..." for the main and adComponent renderUrls bids offered in the auction. It is up to the client how and whether to aggregate the fetches with the URLs of multiple bidders.  The response to this request should be in the form:

    ```
    { 'renderUrls': {
          'https://cdn.com/render_url_of_some_bid': arbitrary_json,
          'https://cdn.com/render_url_of_some_other_bid': arbitrary_json,
          ...},
      'adComponentRenderUrls': {
          'https://cdn.com/ad_component_of_a_bid': arbitrary_json,
          'https://cdn.com/another_ad_component_of_a_bid': arbitrary_json,
          ...}
    }
    ```

The value of `trustedScoringSignals` passed to the seller's `scoreAd()` function is an object of the form:

    ```
    { 'renderUrl': {'https://cdn.com/render_url_of_bidder': arbitrary_value_from_signals},
      'adComponentRenderUrls': {
          'https://cdn.com/ad_component_of_a_bid': arbitrary_value_from_signals,
          'https://cdn.com/another_ad_component_of_a_bid': arbitrary_value_from_signals,
          ...}
    }
    ```

_As a temporary mechanism_ during the First Experiment timeframe, the buyer and seller can fetch these bidding signals from any server, including one they operate  themselves (a "Bring Your Own Server" model).  However, in the final version after the removal of third-party cookies, the request will only be sent to a trusted key-value-type server.  Because the server is trusted, there is no k-anonymity constraint on this request.  The browser needs to trust that the server's return value for each key will be based only on that key and the hostname, and that the server does no event-level logging and has no other side effects based on these requests. 

Either trusted server may optionally include a numeric `Data-Version` header on the response to indicate the state of the data that generated this response, which will then be available in bid generation/scoring and reporting.  This version number should not depend on any properties of the request, only the state of the server.  Ideally, the number would only increment and at any time would be identical across all servers in a fleet.  In practice a small amount of skew is permitted for operational reasons, including propagation delays, staged rollouts, and emergency rollbacks. The version number should be formatted with only the digits `[0-9]` with no leading `0`s and fit in a 32-bit unsigned integer.

#### 3.2 On-Device Bidding

Once the trusted bidding signals are fetched, each interest group's bidding function will run, inside a bidding worklet associated with the interest group owner's domain.  The buyer's JavaScript is loaded from the interest group's `biddingLogicUrl`, which must expose a `generateBid()` function:


```
generateBid(interestGroup, auctionSignals, perBuyerSignals, trustedBiddingSignals, browserSignals) {
  ...
  return {'ad': adObject, 'bid': bidValue, 'render': renderUrl, 'adComponents': [adComponent1, adComponent2, ...]};
}
```


The arguments to `generateBid()` are:



*   interestGroup: The interest group object, as saved during `joinAdInterestGroup()` and perhaps updated via the `dailyUpdateUrl`.
*   auctionSignals: As provided by the seller in the call to `runAdAuction()`.  This is the opportunity for the seller to provide information about the page context (ad size, publisher ID, etc), the type of auction (first-price vs second-price), and so on.
*   perBuyerSignals: The value for _this specific buyer_ as taken from the auction config passed to `runAdAuction()`.  This can include contextual signals about the page that come from the buyer's server, if the seller is an SSP which performs a real-time bidding call to buyer servers and pipes the response back, or if the publisher page contacts the buyer's server directly.  If so, the buyer may wish to check a cryptographic signature of those signals inside `generateBid()` as protection against tampering.
*   trustedBiddingSignals: An object whose keys are the `trustedBiddingSignalsKeys` for the interest group, and whose values are those returned in the `trustedBiddingSignals` request.
*   browserSignals: An object constructed by the browser, containing information that the browser knows, and which the buyer's auction script might want to use or verify.  The `dataVersion` field will only be present if the `Data-Version` header was provided and had a consistent value for all of the trusted bidding signals server responses used to construct the trustedBiddingSignals. Additional fields can include information about both the context (e.g. the true hostname of the current page, which the seller could otherwise lie about) and about the interest group itself (e.g. times when it previously won the auction, to allow on-device frequency capping).
    ```
    { 'topWindowHostname': 'www.example-publisher.com',
      'seller': 'https://www.example-ssp.com',
      'joinCount': 3,
      'bidCount': 17,
      'prevWins': [[time1,ad1],[time2,ad2],...],
      'wasmHelper': ... /* a WebAssembly.Module object based on interest group's biddingWasmHelperUrl */
      'dataVersion': 1, /* Data-Version value from the trusted bidding signals server's response(s) */
    }
    ```

The output of `generateBid()` contains four fields:



*   ad: Arbitrary metadata about the ad which this interest group wants to show.  The seller uses this information in its auction and decision logic.
*   bid: A numerical bid that will enter the auction.  The seller must be in a position to compare bids from different buyers, therefore bids must be in some seller-chosen unit (e.g. "USD per thousand").  If the bid is zero or negative, then this interest group will not participate in the seller's auction at all.  With this mechanism, the buyer can implement any advertiser rules for where their ads may or may not appear.
*   render: A URL, or a list of URLs, which will be rendered to display the creative if this bid wins the auction.  (See "Ads Composed of Multiple Pieces" below.)
*   adComponents: An optional list of up to 20 adComponent strings from the InterestGroup's adComponents field. Each value must match an adComponent renderUrl exactly. This field must not be present if the InterestGroup has no adComponent field. It is valid for this field not to be present even when adComponents is present.


#### 3.3 Metadata with the Ad Bid

The metadata accompanying the returned ad is not specified in this document, because sellers and buyers are free to establish whatever protocols they want here.

Sellers can ask buyers to provide whatever information they feel is necessary for their ad scoring job.  Sellers have an opportunity to enforce requirements in their `scoreAd()` function, rejecting bids whose metadata they find lacking.

If `generateBid()` picks an ad whose rendering URL is not yet above the browser-enforced microtargeting prevention threshold, then the function will be called a second time, this time with a modified `interestGroup` argument that includes only the subset of the group's ads that are over threshold.  (The under-threshold ad will, however, be counted towards the microtargeting thresholding for future auctions for this and other users.)


#### 3.4 Ads Composed of Multiple Pieces

The [Product-level TURTLEDOVE](https://github.com/WICG/turtledove/blob/master/PRODUCT_LEVEL.md) proposal describes a use case in which the rendered ad is composed of multiple pieces — a top-level ad template "container" which includes some slots that can be filled in with specific "products".  This is useful because the browser's microtargeting threshold can be applied to each individual component of the ad without compromising on tracking protections.

The output of `generateBid()` can use the on-device ad composition flow through an optional adComponents field, listing additional URLs made available to the fenced frame the container URL is loaded in. The component URLs may be retrieved by calling `navigator.adAuctionComponents(numComponents)`, where numComponents is at most 20. To prevent bidder worklets from using this as a sidechannel to leak additional data to the fenced frame, exactly numComponents obfuscated URLs will be returned by this method, regardless of how many adComponent URLs were actually in the bid, even if the bid contained no adComponents, and the Interest Group itself had no adComponents either.


### 4. Browsers Render the Winning Ad

The winning ad will be rendered in a [Fenced Frame](https://github.com/shivanigithub/fenced-frame): a mechanism under development for rendering a document in an embedded context which is unable to communicate with the surrounding page.  This communication blockage is necessary to meet the privacy goal that sites cannot learn about their visitors' ad interests.  (Note that the microtargeting prevention threshold alone is not enough to address this threat: the threshold prevents ads which could identify a single person, but it allows ads which identify a group of people that share a single interest.)

Fenced Frames are designed to be able to provide a second type of protection as well: they will not use the network to load any data from a server, instead only rendering content that was previously downloaded (e.g. as a Web Bundle).  This restriction is focused on preventing information leakage based on server-side joins via timing attacks.

_As a temporary mechanism, we will still allow network access,_ rendering the winning ad in a Fenced Frame that is able to load resources from servers.

The TURTLEDOVE privacy goals mean that this cannot be the long-term solution.  Rendering ads from previously-downloaded Web Bundles, as originally proposed, is one way to mitigate this leakage.  Another possibility is ad rendering in which all network-loaded resources come from a trusted CDN that does not keep logs of the resources it serves.  As with servers involved in providing the trusted bidding signals, the privacy model and browser trust mechanism for such a CDN would require further work.


### 5. Event-Level Reporting (for now)

Once the winning ad has rendered in its Fenced Frame, the seller and the winning buyer each have an opportunity to perform logging and reporting on the auction outcome.  The browser will call one reporting function in the seller's auction worklet and one in the winning buyer's bidding worklet.

_As a temporary mechanism,_ these reporting functions will be able to send event-level reports to their servers.  These reports can include contextual information, and can include information about the winning interest group if it is over an anonymity threshold.  This reporting will happen synchronously, while the page with the ad is still open in the browser.

In the long term, we need a mechanism to ensure that the after-the-fact reporting cannot be used to learn the advertising interest group of individual visitors to the publisher's site — the same privacy goal that led to Fenced Frame rendering.  Therefore event-level reporting is just a temporary model until an adequate trusted-server reporting framework is settled and in place.  (Once we have a trusted reporting mechanism, we can consider allowing the reports to be influenced by the interest group's `userBiddingSignals`.)


#### 5.1 Seller Reporting on Render

The seller's JavaScript (i.e. the same script, loaded from `decisionLogicUrl`, that provided the `scoreAd()` function) can also expose a `reportResult()` function:


```
reportResult(auctionConfig, browserSignals) {
  ...
  return signalsForWinner;
}
```


The arguments to this function are:

*   auctionConfig: The auction configuration object passed to `navigator.runAdAuction()`
*   browserSignals: An object constructed by the browser, containing information it knows about what happened in the auction:

    ```
    { 'topWindowHostname': 'www.example-publisher.com',
      'interestGroupOwner': 'https://www.example-dsp.com/',
      'renderUrl': 'https://cdn.com/url-of-winning-creative.wbn',
      'bid:' bidValue,
      'desirability': desirabilityScoreForWinningAd,
      'dataVersion': versionFromKeyValueResponse,
    }
    ```

The `browserSignals` argument must be handled carefully to avoid tracking.  It certainly cannot include anything like the full list of interest groups, which would be too identifiable as a tracking signal.  The `renderUrl` can always be included since it has already passed a k-anonymity check, for example, but the winning `interestGroupName` will only be present if it has exceeded the threshold which gates daily updates.  Similarly, the browser may limit the precision of the bid and desirability values to avoid these numbers exfiltrating information from the interest group's `userBiddingSignals`.  On the upside, this set of signals can be expanded to include useful additional summary data about the wider range of bids that participated in the auction, e.g. the second-highest bid or the number of bids.  Additionally, the `dataVersion` will only be present if the `Data-Version` header was provided in the response headers from the Trusted Scoring server.

The `reportResult()` function's reporting happens by calling browser-provided aggregate reporting APIs or, temporarily, directly calling network APIs.  The output of this function is not used for reporting, but rather as an input to the buyer's reporting function.


#### 5.2 Buyer Reporting on Render and Ad Events

The buyer's JavaScript (i.e. the same script, loaded from `biddingLogicUrl`, that provided the `generateBid()` function) can also expose a `reportWin()` function:


```
reportWin(auctionSignals, perBuyerSignals, sellerSignals, browserSignals) {
  ...
}
```


The arguments to this function are:

*   auctionSignals and perBuyerSignals: As in the call to `generateBid()` for the winning interest group.
*   sellerSignals: The output of `reportResult()` above, giving the seller an opportunity to pass information to the buyer.
*   browserSignals: Similar to the argument to `reportResult()` above, though without the seller's desirability score, but with additional `interestGroupName` and `seller` fields.  The `dataVersion` field will contain the `Data-Version` from the trusted bidding signals response headers if they were provided by the trusted bidding signals server response and the version was consistent for all keys requested by this interest group, otherwise the field will be absent.  Additional fields could also include some buyer-specific signal like the second-highest bid from that particular buyer.

The `reportWin()` function's reporting happens by calling browser-provided aggregate reporting APIs or, temporarily, directly calling network APIs.

Ads often need to report on events that happen once the ad is rendered.  One common example is reporting on whether an ad became viewable on-screen.  We will need a communications channel to allow the publisher page or the Fenced Frame to pass such information into the worklet responsible for reporting.  Some additional design work is needed here.


#### 5.3 Losing Bidder Reporting

We also need to provide a mechanism for the _losing_ bidders in the auction to learn aggregate outcomes.  Certainly they should be able to count the number of times they bid, and losing ads should also be able to learn (in aggregate) some seller-provided information about e.g. the auction clearing price.  Likewise, a reporting mechanism should be available to buyers who attempted to bid with a creative that had not yet reached the k-anonymity threshold.

This could be handled by a `reportLoss()` function running in the worklet.  Alternatively, the model of [SPURFOWL](https://github.com/AdRoll/privacy/blob/main/SPURFOWL.md) (an append-only datastore and later aggregate log processing) could be a good fit for this use case.  The details here are yet to be determined.


