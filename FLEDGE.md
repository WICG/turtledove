# First Experiment (FLEDGE)

This document describes an early prototype for ads serving in the TURTLEDOVE family, appropriate for experimentation before a fully-featured system is ready.  It would be the First "Locally-Executed Decision over Groups" Experiment.

This first experiment is currently a Chrome [Origin Trial](https://github.com/GoogleChrome/OriginTrials).  The goal is for us to gain implementer experience, and for the ads ecosystem to evaluate its usability, as soon as it is feasible to do so.  We need a robust API to take flight before the removal of third-party cookies shown on [Chrome's Privacy Sandbox timeline](https://privacysandbox.com/timeline).

We plan to hold regular meetings under the auspices of the WICG to go through the details of this proposal and quickly make any needed changes.  Please comment on the timing question in Issue [#88](https://github.com/WICG/turtledove/issues/88) if you want to attend these meetings to be involved in hammering out details.


- [Summary](#summary)
- [Background](#background)
- [Design Elements](#design-elements)
  - [1. Browsers Record Interest Groups](#1-browsers-record-interest-groups)
    - [1.1 Joining Interest Groups](#11-joining-interest-groups)
    - [1.2 Interest Group Attributes](#12-interest-group-attributes)
    - [1.3 Permission Delegation](#13-permission-delegation)
  - [2. Sellers Run On-Device Auctions](#2-sellers-run-on-device-auctions)
    - [2.1 Initiating an On-Device Auction](#21-initiating-an-on-device-auction)
    - [2.2 Auction Participants](#22-auction-participants)
    - [2.3 Scoring Bids](#23-scoring-bids)
    - [2.4 Scoring Bids in Component Auctions](#24-scoring-bids-in-component-auctions)
    - [2.5 Additional Trusted Signals (directFromSellerSignals)](#25-additional-trusted-signals-directfromsellersignals)
  - [3. Buyers Provide Ads and Bidding Functions (BYOS for now)](#3-buyers-provide-ads-and-bidding-functions-byos-for-now)
    - [3.1 Fetching Real-Time Data from a Trusted Server](#31-fetching-real-time-data-from-a-trusted-server)
    - [3.2 On-Device Bidding](#32-on-device-bidding)
    - [3.3 Metadata with the Ad Bid](#33-metadata-with-the-ad-bid)
    - [3.4 Ads Composed of Multiple Pieces](#34-ads-composed-of-multiple-pieces)
    - [3.5 Filtering and Prioritizing Interest Groups](#35-filtering-and-prioritizing-interest-groups)
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
  'priority': 0.0,
  'priorityVector': {
    'signal1': 2,
    'signal2': -3.5,
    ...
  }
  'prioritySignalsOverrides': {
    'signal1': 4.5,
    'signal2': 0,
    ...
  }
  'enableBiddingSignalsPrioritization' : true,
  'biddingLogicUrl': ...,
  'biddingWasmHelperUrl': ...,
  'dailyUpdateUrl': ...,
  'executionMode': ...,
  'trustedBiddingSignalsUrl': ...,
  'trustedBiddingSignalsKeys': ['key1', 'key2'],
  'userBiddingSignals': {...},
  'ads': [shoesAd1, shoesAd2, shoesAd3],
  'adComponents': [runningShoes1, runningShoes2, gymShoes, gymTrainers1, gymTrainers2],
};
const joinPromise = navigator.joinAdInterestGroup(myGroup, 30 * kSecsPerDay);
```


The browser will only allow the `joinAdInterestGroup()` operation with the permission of both the site being visited and the group's owner.  The site can allow or deny permission to any or all third parties via a `Permissions-Policy` (directive named "join-ad-interest-group"), where the default policy is to allow all in the top-level page and to deny all in cross-domain iframes.  The group's owner can indicate permission by `joinAdInterestGroup()` running in a page or iframe in the owner's domain, and can delegate that permission to any other domains via a list at a `.well-known` URL (see [1.3 Permission Delegation](#13-permission-delegation)).  These can be combined, to allow a DSP to add a person to one of its interest groups based on publisher context, as discussed in [TERN](https://github.com/WICG/turtledove/blob/master/TERN.md#c-contextual-interest-groups) — provided the publisher's `Permissions-Policy` permits interest group additions by its SSP, and the DSP gives this SSP this ability.

The returned `joinPromise` is resolved if the group is successfully joined, and rejected with an error if the join operation fails. The error message and the resolution time must _not_ depend on what interest groups a user is in, or any cross-origin browser state, apart from the results of the .well-known fetch, to avoid leaking any data across sites.

There is a complementary API `navigator.leaveAdInterestGroup(myGroup)` which looks only at `myGroup.name` and `myGroup.owner`. As with join calls, `leaveAdInterestGroup()` also returns a promise. As a special case to support in-ad UIs, invoking `navigator.leaveAdInterestGroup()` from inside an ad that is being targeted at a particular interest group will cause the browser to leave that group, irrespective of permission policies. Note that calling `navigator.leaveAdInterestGroup()` without arguments isn't supported inside a component ad frame.

The browser will remain in an interest group for only a limited amount of time.  The duration is specified in the call to `joinAdInterestGroup()`, and will be capped at 30 days.  This can be extended by calling `joinAdInterestGroup()` again later, with the same group name and owner.  Successive calls to `joinAdInterestGroup()` will overwrite the previously-stored values for any interest group properties, like the group's `userBiddingSignal` or list of ads.


#### 1.2 Interest Group Attributes

The `priority` is used to select which interest groups participate in an auction when the number of interest groups are limited by the `perBuyerGroupLimits` attribute of the auction config. If not specified, a `priority` of `0.0` is assigned. There is no special meaning to these values. These values are only used to select interest groups to participate in an auction such that if there is an interest group participating in the auction with priority `x`, all interest groups with the same owner having a priority `y` where `y > x` should also participate (i.e. `generateBid` will be called). In the case where some but not all interest groups with equal priority can participate in an auction due to `perBuyerGroupLimits`, the participating interest groups will be uniformly randomly chosen from the set of interest groups with that priority.

`priorityVector`, `prioritySignalsOverrides`, and `enableBiddingSignalsPrioritization` are optional values used to dynamically calculate a priority used in place of `priority`. `priorityVector` and `prioritySignalsOverrides` are mappings of strings to JavaScript numbers, while `enableBiddingSignalsPrioritization` is a bool that defaults to `false`. See [Filtering and Prioritizing Interest Groups](#35-filtering-and-prioritizing-interest-groups) for a description of how these fields are used.

The `userBiddingSignals` is for storage of additional metadata that the owner can use during on-device bidding, and the `trustedBiddingSignals` attributes provide another mechanism for making real-time data available for use at bidding time.

The `biddingWasmHelperUrl` field is optional, and lets the bidder provide computationally-expensive subroutines in WebAssembly, rather than JavaScript, to be driven from the JavaScript function provided by `biddingLogicUrl`. If provided, it must point to a WebAssembly binary, delivered with an `application/wasm` mimetype. The corresponding `WebAssembly.Module` will be made available by the browser to the `generateBid` function.

The `dailyUpdateUrl` provides a mechanism for the group's owner to periodically update the attributes of the interest group: any new values returned in this way overwrite the values previously stored (except that the `name` and `owner` cannot be changed, and `prioritySignalsOverrides` will be merged with the previous value, with `null` meaning a value should be removed from the interest group's old dictionary).  However, the browser will only allow daily updates when a sufficiently large number of people have the same `dailyUpdateUrl` , e.g. at least 100 browsers with the same update URL. This will not include any metadata, so data such as the interest group `name` should be included within the URL, so long as the URL exceeds the minimum count threshold.  (Without this sort of limit, a single-person interest group could be used to observe that person's coarse-grained IP-Geo location over time.)

The `executionMode` attribute is optional. The default value (`"compatibility"`) will run each invocation of `generateBid` in a totally fresh execution environment, which prevents one invocation from directly passing data to a subsequent invocation, but has non-trivial execution costs as each execution environment must be initialized from scratch.  The `"groupByOrigin"` mode will attempt to re-use the execution environment for interest groups with the same script that were joined on the same top-level origin, which saves a lot of these initialization costs. However, to avoid cross-site information leaking into `generateBid`, attempts to join or leave an interest group in `"groupByOrigin"` mode from more than one top-level origin will result in all `"groupByOrigin"` interest groups that were joined from the same top-level origin being removed. When the execution environment is re-used the script top-level will not be re-executed, with only the `generateBid` function being run the subsequent times. This mode is intended for interest groups that are extremely likely to be joined or left from a single top-level origin only, with the probability high enough that the penalty of removal if the requirement doesn't hold to be low enough for the performance gains to be a worthwhile trade-off.

The `ads` list contains the various ads that the interest group might show.  Each entry is an object that includes both a rendering URL and arbitrary metadata that can be used at bidding time.

The `adComponents` field contains the various ad components (or "products") that can be used to construct ["Ads Composed of Multiple Pieces"](https://github.com/WICG/turtledove/blob/main/FLEDGE.md#34-ads-composed-of-multiple-pieces)). Similarly to the `ads` field, each entry is an object that includes both a rendering URL and arbitrary metadata that can be used at bidding time. Thanks to `ads` and `adsComponents` being separate fields, the buyer is able to update the `ads` field via daily update without losing `adComponents` stored in the interest group.

All fields that accept arbitrary metadata objects (`userBiddingSignals` and `metadata` field of ads) must be JSON-serializable.
All fields that specify URLs for loading scripts or JSON (`biddingLogicUrl`, `biddingWasmHelperUrl`, and `trustedBiddingSignalsUrl`) must point to URLs whose responses include the HTTP response header `X-Allow-FLEDGE: true` to ensure they are allowed to be used for loading FLEDGE resources.

The browser will provide protection against microtargeting, by only rendering an ad if the same rendering URL is being shown to a sufficiently large number of people (e.g. at least 100 people would have seen the ad, if it were allowed to show).  While in the [Outcome-Based TURTLEDOVE](https://github.com/WICG/turtledove/blob/master/OUTCOME_BASED.md) proposal this threshold applied only to the rendered creative, FLEDGE has the additional requirement that the tuple of the interest group owner, bidding script URL, and rendered creative must be k-anonymous for an ad to be shown (this is necessary to ensure the current event-level reporting for interest group win reporting is sufficiently private). For interest groups that have component ads, all of the component ads must also separately meet this threshold for the ad to be shown. Since a single interest group can carry multiple possible ads that it might show, the group will have an opportunity to re-bid another one of its ads to act as a "fallback ad" any time its most-preferred choice is below threshold.  This means that a small, specialized interest group that is still below the daily-update threshold could still choose to participate in auctions, bidding with a more-generic ad until the group becomes large enough.


#### 1.3 Permission Delegation

When a frame navigated to one domain calls joinAdInterestGroup() or leaveAdInterestGroup() for an interest group with a different owner, the browser will fetch the URL https://owner.domain/.well-known/interest-group/permissions/?origin=frame.origin, where `owner.domain` is domain that owns the interest group and `frame.origin` is the origin of the frame. The fetch uses the `omit` [credentials mode](https://fetch.spec.whatwg.org/#concept-request-credentials-mode), using the [Network Partition Key](https://fetch.spec.whatwg.org/#network-partition-keys) of the frame that invoked the method. To avoid leaking cross-origin data through the returned Promise unexpectedly, the fetch uses the `cors` [mode](https://fetch.spec.whatwg.org/#concept-request-mode). The fetched response should have a JSON MIME type and be of the format:

```
{ "joinAdInterestGroup": true/false,
  "leaveAdInterestGroup": true/false
}
```

Indicating whether the origin in the path has permissions to join and/or leave interest groups owned by the domain the request is sent to. Missing permissions are assumed to be false.

Since joining or leaving a group may depend on a network requests, browsers may delay these requests, or run them out of order. Each frame must, however, run all pending joins and leaves for a single owner in the order in which they were made. Same-origin operations should be applied immediately. When a page or frame is navigated, the browser should make a best-effort attempt to complete pending join and leave operations that are blocked on a network fetch, but may choose to drop them if there are more than 20 for a single top-level frame. This is intended to allow joining or leaving a cross-origin interest group at the same time as starting a navigation in response to a user gesture, though previous join/leave calls may still cause such an operation to be dropped.

In order to prevent leaking data, join and leave calls must request the `.well-known` file, regardless of whether the user is in the group or not, as otherwise, whether or not a fetch is made can potentially leak data. Browsers may cache `.well-known` fetch results that share a network partition key.

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
  'auctionSignals': {...},
  'directFromSellerSignals: 'https://www.example-ssp.com/...',
  'sellerSignals': {...},
  'sellerTimeout': 100,
  'sellerExperimentGroupId': 12345,
  'perBuyerSignals': {'https://www.example-dsp.com': {...},
                      'https://www.another-buyer.com': {...},
                      ...},
  'perBuyerTimeouts': {'https://www.example-dsp.com': 50,
                       'https://www.another-buyer.com': 200,
                       '*': 150,
                       ...},
  'perBuyerCumulativeBiddingTimeouts': {'https://www.example-dsp.com': 500,
                                        'https://www.another-buyer.com': 600,
                                        '*': 450,
                                        ...},
  'perBuyerGroupLimits': {'https://www.example-dsp.com': 2,
                          'https://www.another-buyer.com': 1000,
                          '*': 15,
                          ...},
  'perBuyerPrioritySignals': {'https://www.example-dsp.com': {'signal1': 2.5,
                                                              'signal2': 2.5,
                                                              ...},
                              'https://www.another-buyer.com': {...},
                              '*': {...},
                              ...},
  'perBuyerExperimentGroupIds': {'https://www.example-dsp.com': 234,
                                 'https://www.another-buyer.com': 345,
                                 '*': 456,
                                 ...},
  'componentAuctions': [
    {'seller': 'https://www.some-other-ssp.com',
      'decisionLogicUrl': ...,
      ...},
    ...
  ],
  'signal': /* optionally, an AbortSignal */...,
};
const auctionResultPromise = navigator.runAdAuction(myAuctionConfig);
```


This will cause the browser to execute the appropriate bidding and auction logic inside a collection of dedicated worklets associated with the buyer and seller domains.  The `auctionSignals`, `sellerSignals`, and `perBuyerSignals` values will be passed as arguments to the appropriate functions that run inside those worklets — the `auctionSignals` are made available to everyone, while the other signals are given only to one party.

The optional `directFromSellerSignals` field can also be used to pass signals to the auction, similar to `sellerSignals`,  `perBuyerSignals`, and `auctionSignals`. The difference is that `directFromSellerSignals` are trusted to come from the seller because the content loads from a [subresource bundle](https://github.com/WICG/webpackage/blob/main/explainers/subresource-loading.md) loaded from a seller's origin, ensuring the authenticity and integrity of the signals. For more details, see [2.5 directFromSellerSignals](#25-additional-trusted-signals-directfromsellersignals).

In some cases, multiple SSPs may want to participate in an auction, with the winners of separate auctions being passed up to another auction, run by another SSP. To facilitate these "component auctions", `componentAuctions` can optionally contain additional auction configurations for each seller's "component auction". The winning bid of each of these "component auctions" will be passed to the "top-level" auction. How bids are scored in this case is further described in [2.4 Scoring Bids in Component Auctions](#24-scoring-bids-in-component-auctions). The `AuctionConfig` of component auctions may not have their own `componentAuctions`. When `componentAuctions` is non-empty, `interestGroupBuyers` must be empty.  That is, for any particular FLEDGE auction, either there is a single seller and no component auctions, or else all bids come from component auctions and the top-level auction can only choose among the component auctions' winners.

The returned `auctionResultPromise` object is a `FencedFrameConfig` object: it is not possible for any code on the publisher page to inspect the winning ad or otherwise learn about its contents from this config object, but it can be passed to a Fenced Frame for rendering.  (The [Fenced Frame Opaque-ads explainer](https://github.com/WICG/fenced-frame/blob/master/explainer/use_cases.md#opaque-ads) has initial thoughts about how this could be implemented.)  If the auction produces no winning ad, the return value can also be null, although this non-opaque return value leaks one bit of information to the surrounding page.  In this case, for example, the seller might choose to render a contextually-targeted ad.

Optionally, `sellerTimeout` can be specified to restrict the runtime (in milliseconds) of the seller's `scoreAd()` script, and `perBuyerTimeouts` can be specified to restrict the runtime (in milliseconds) of particular buyer's `generateBid()` scripts. If no value is specified for the seller or a particular buyer, a default timeout of 50 ms will be selected. Any timeout higher than 500 ms will be clamped to 500 ms. A key of `'*'` in `perBuyerTimeouts` is used to change the default of unspecified buyers.

Optionally, `perBuyerCumulativeBiddingTimeouts` is structured like `perBuyerTimeouts`, but the values cover the entirety of the time it takes to generate bids for all interest groups for each buyer, including downloading resources, starting processes, and all generate bid calls. The single limit applies collectively across all interest groups with the same owner. It's measured as wall clock time starting when dedicated tasks needed to generate a bid for a particular buyer start. Timers may be running for multiple bidders simultaneously.  It does not include time taken by the seller to score the buyer's bids. Once the timer expires, the affected buyer's interest groups may no longer generate any bids. Scripts may be unloaded, fetches cancelled, etc. All bids generated before the timeout will continue to participate in the auction. FLEDGE implementations should attempt, on a best-effort basis, to generate bids for each buyer in priority order, so lower priority interest groups are the ones more likely to be timed out. If promises are passed in to the auction config for fields that support them, the timer for a buyer only starts once all promises blocking that buyer's bidding scripts from running have been resolved.

Optionally, the `signal` field can be set to an [`AbortSignal`](https://dom.spec.whatwg.org/#interface-AbortSignal) object (generally from an [`AbortController`](https://dom.spec.whatwg.org/#interface-abortcontroller)'s [`signal`](https://dom.spec.whatwg.org/#dom-abortcontroller-signal) field) to permit aborting the execution of the auction.  When the [`abort()`](https://dom.spec.whatwg.org/#dom-abortcontroller-abort) method on the associated [`AbortController`](https://dom.spec.whatwg.org/#interface-abortcontroller) is called, an attempt to interrupt the auction will be made. Since the auction executes in parallel to the page, it's possible for this call to happen after the auction actually completed (perhaps unsuccessfully) but before this has been noticed by the caller of `runAdAuction`. In that case, the cancellation attempt is ignored. If the cancellation is successful, the promise is rejected, and no side effects of the whole auction (like reporting and bid statistics) occur, though priority adjustments still take place. Calling `abort()` after the promise from `runAdAuction` has resolved has no effect.

Optionally, `perBuyerGroupLimits` can be specified to limit the number of of interest groups from a particular buyer that participate in the auction. A key of `'*'` in `perBuyerGroupLimits` is used to set a limit for unspecified buyers. For each buyer, interest groups will be selected to participate in the auction in order of decreasing `priority` (larger priorities are selected first) up to the specfied limit. The selection of interest groups occurs independently for each buyer, so the priorities do not need to be comparable between buyers and could have a buyer-specific meaning. The value of the limits provided should be able to be represented by a 16 bit unsigned integer.

Optionally, `sellerExperimentGroupId` can be specified by the seller to support coordinated experiments with the seller's trusted server. If specified, this must be an integer between zero and 65535 (16 bits). Optionally,`perBuyerExperimentGroupIds` can be specified to support coordinated experiments with buyers' trusted servers. If specified, this must also be an integer between zero and 65535 (16 bits).

Optionally, `perBuyerPrioritySignals` is an object mapping string keys to Javascript numbers that can be used to dynamically compute interest group priorities before `perBuyerGroupLimits` are applied. See [Filtering and Prioritizing Interest Groups](#35-filtering-and-prioritizing-interest-groups) for more information.

Optionally, `resolveToConfig` is a boolean directing the promise to resolve to a `FencedFrameConfig` if true, for use in a `<fencedframe>`, or an opaque `urn:uuid` URL otherwise, for use in an `<iframe>`.

All fields that accept arbitrary metadata objects (`auctionSignals`, `sellerSignals`, and keys of `perBuyerSignals`) must be JSON-serializable.
All fields that specify URLs for loading scripts or JSON (`decisionLogicUrl` and `trustedScoringSignalsUrl`) must point to URLs whose responses include the HTTP response header `X-Allow-FLEDGE: true` to ensure they are allowed to be used for loading FLEDGE resources.

A `Permissions-Policy` directive named "run-ad-auction" controls access to the `navigator.runAdAuction()` API.

In the case of a component auction, all `AuctionConfig` parameters for that component auction are only scoped to buyer and seller scripts run as part of that auction component. Similarly, all values specified by the top-level auction are not applied to the component auctions. When the top-level auction has component auctions, fields that affect bidder scripts have no effect, since the top-level auction has no bidders in it (e.g, `perBuyerSignals`, `perBuyerTimeouts`, `perBuyerGroupLimits`, etc).

##### 2.1.1 Providing Signals Asynchronously

The values of some signals (those configured by fields `auctionSignals`, `sellerSignals`, `perBuyerSignals`, `perBuyerTimeouts`, and `directFromSellerSignals`) can optionally be provided not as concrete values, but as [Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise).  This permits some parts of the auction, such as loading of scripts and trusted signals, and launching of isolated worklet processes, to overlap the computation (or network retrieval) of those values.  The worklet scripts will only see the resolved values; if any such Promise rejects the auction will be aborted (unless it managed to fail already or get otherwise aborted anyway).


#### 2.2 Auction Participants

Each interest group the browser has joined and whose owner is in the list of `interestGroupBuyers` will have an opportunity to bid in the auction.  See the "Buyers Provide Ads and Bidding Functions" section, below, for how interest groups bid.

The seller may instead specify `'interestGroupBuyers': '*'` to permit all interest groups into the auction, and decide ad admissibility later in the process, based on criteria other than the interest group owner.  For example, a seller with an out-of-band creative review process might decide admissibility solely based on the creative, not the buyer.


#### 2.3 Scoring Bids

Once the bids are known, the seller runs code inside an _auction worklet_.  Within this worklet, the seller's auction script has an opportunity to consider each individual ad one at a time, with its associated bid and metadata, and then assign a numerical desirability score.  The seller's JavaScript is loaded from the auction configuration's `decisionLogicUrl`, which must expose a `scoreAd()` function:


```
scoreAd(adMetadata, bid, auctionConfig, trustedScoringSignals, browserSignals,
    directFromSellerSignals) {
  ...
  return {desirability: desirabilityScoreForThisAd,
          allowComponentAuction: componentAuctionsAllowed};
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
*   directFromSellerSignals is an object that may contain the following fields:
    *   sellerSignals: Like auctionConfig.sellerSignals, but passed via the [directFromSellerSignals](#25-additional-trusted-signals-directfromsellersignals) mechanism. These are the signals whose subresource URL ends in `?sellerSignals`.
    *   auctionSignals: Like auctionConfig.auctionSignals, but passed via the [directFromSellerSignals](#25-additional-trusted-signals-directfromsellersignals) mechanism. These are the signals whose subresource URL ends in `?auctionSignals`.

The output of `scoreAd()` is an object with the following fields:
* desirability: Number indicating how desirable this ad is.  Any value that is zero or negative indicates that the ad cannot win the auction.  (This could be used, for example, to eliminate any interest-group-targeted ad that would not beat a contextually-targeted candidate.) The winner of the auction is the ad object which was given the highest score.
* allowComponentAuction: (optional) If the bid being scored is from a component auction and this value is not true, the bid is ignored. If not present, this value is considered false. This field must be present and true both when the component seller scores a bid, and when that bid is being scored by the top-level auction.

If `scoreAd()` returns only a numeric value, it's equivalent to returning {'score': numericValue, `allowComponentAuction`: false}.

The logic in `scoreAd()` has access to the full auction configuration object, which means the seller can pass in arbitrary information from the publisher page.  In particular, the configuration object's `sellerSignals` field is exclusively for passing information into `scoreAd()`.  This field can include information based on looking up publisher settings, based on making a contextual ad request, and so on.  Examples of logic that could live in the `scoreAd()` function include:

*   Calculation of a publisher pay-out, based on the ad bid along with arbitrary seller data and arbitrary metadata included in the `adObject`.  This pay-out could influence both the ad's desirability and the post-auction reporting used for moving money around.
*   Honoring publisher deals that promote certain ads over others irrespective of a per-page auction price.
*   Disqualifying some ads, by returning a desirability score of 0 (which means the ad will not win).  This could, for example, be based on comparing ad metadata (e.g. advertiser or topic) to the publisher site's rules for what ads are allowed to appear on the page, or based on the bidder being slow (as revealed by `biddingDurationMsec`).
*   Checking whether the creative contents have been pre-approved by the seller.  This could be implemented by an out-of-band creative review process leading to the seller handing the buyer a cryptographically-signed token, which the buyer then includes in the ad's metadata.  See [TERN's Section 0](https://github.com/WICG/turtledove/blob/master/TERN.md#0-before-advertising-begins) for more discussion of this possibility.

Note that `scoreAd()` does not have any way to _store_ information for use later on a different page.  In particular, if the ad scoring logic on day 1 observes a bid from a particular interest group, and then on day 2 the browser interest group membership expires, there is no way for the ad scoring logic on day 3 to "remember" the pre-expiration membership information.


#### 2.4 Scoring Bids in Component Auctions

Seller scripts in component auctions behave a little differently.  They still expose a `scoreAd()` function to score each bid from the component auction's buyers, however all of its arguments come from the component auction, including `auctionConfig`. `browserSignals` has an additional `topLevelSeller` field, which contains the seller of the top-level auction. Instead of returning just a desirability score, the component seller's `scoreAd()` returns an object with the following fields:

* ad: (optional) Arbitrary metadata to pass to the top-level seller. Defaults to null, if not present.
* desirability: Numeric score of the bid. Must be positive or the ad will be rejected.
* allowComponentAuction: If this field is not true, the bid will be rejected.
* bid: (optional) Modified bid value to provide to the top-level seller script. If present, this will be passed to the top-level seller's `scoreAd()` and `reportResult()` methods instead of the original bid, if the ad wins the component auction and top-level auction, respectively.

Once all of a component auction's bids have been scored by the component auction's seller script, the bid with the highest score is passed to the top-level seller to score. For that bid, the top-level seller's `scoreAd()` method is passed the `ad` value from the component auction seller's `scoreAd()` method, and there is an additional `componentSeller` field in the `browserSignals`, which is the seller for the component auction. All other values are the same as if the bid had come from an interest group participating directly in the top-level auction. In the case of a tie, one of the highest scoring bids will be chosen randomly and only that bid will be passed to the top-level seller to score. The seller of a component auction may reject all bids by giving them scores <= 0. In that case, no bid from that component auction will be passed to the top-level auction.

The ultimate winner of the top-level auction is the single bid the top-level seller script gives the highest score, of the wining bids of the component auctions.


#### 2.5 Additional Trusted Signals (directFromSellerSignals)

While the browser ensures (using TLS) that information stored in a buyer's interest group and coming from a buyer's trusted bidding signals server comes from the buyer, information passed into `runAdAuction()` is not known to come from the seller unless the seller calls `runAdAuction()` from its own iframe.  In a multi-seller auction it becomes impossible to have all sellers create the frame calling `runAdAuction()`.  `directFromSellerSignals` allows the browser to ensure the authenticity and integrity of information passed into an auction from the seller.

The optional `directFromSellerSignals` field can be used to pass signals to the auction, similar to `sellerSignals` and `perBuyerSignals`. The difference is that `directFromSellerSignals` are trusted to come from a seller because the content loads from a [subresource bundle](https://github.com/WICG/webpackage/blob/main/explainers/subresource-loading.md) loaded from a seller's origin. If present, `directFromSellerSignals` should be an HTTPS URL prefix using the seller's origin -- when combined with a browser-provided suffix (see details below), the resultant URL should be a resource in a subresource bundle that has been loaded by the current document, whose contents are of type `application/json`, with the following response headers: `X-Allow-Fledge: true` and `X-FLEDGE-Auction-Only: true`.

The URL prefix should not have a query string (i.e. `?a=b&c=d`). Different calls to `navigator.runAdAuction()` on a page may use different prefixes -- for instance, to give different signals to different ad slots. The browser will append the following suffixes to the prefix:

*   `?perBuyerSignals=[origin]`, where [origin] is one of the origins in `interestGroupBuyers` (encoded as a URL component): this corresponds to the `perBuyerSignals` for the buyer `origin`
*   `?sellerSignals`: this corresponds to the `sellerSignals` only delivered to the seller
*   `?auctionSignals`: this corresponds to `auctionSignals` delivered to the seller, and all buyers

`runAdAuction()` will check all of the above URLs (`perBuyerSignals` for all buyers, `sellerSignals`, and `auctionSignals`) to see if they have been pre-registered as a subresource URL via `<script type="application/webbundle">` tags (note that signals may come from separate bundle files, but each bundle must be served from the seller's origin) -- it's possible to register only a subset of all possible signals.

The signals will be passed as the parameter `directFromSellerSignals` on worklet functions. See [generateBid()](#32-on-device-bidding), [scoreAd()](#23-scoring-bids), [reportWin()](#52-buyer-reporting-on-render-and-ad-events), and [reportResult()](#51-seller-reporting-on-render).

If the signals subresource download somehow fails for a given signal, null will be set on the corresponding field of the directFromSellerSignals object passed to worklet functions. Worklet scripts expecting signals can decide to score / bid / report anyways, or fail (i.e. throw an exception).

As the name implies, subresource bundles are bundles of HTTPS subresources.  These HTTPS subresources can contain various HTTPS response headers, including ones meant to enforce basic Web principles, e.g. CORS headers.  To prevent bypassing these basic Web principles by loading the bundles directly with [fetch()](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) or [XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) and ignoring the HTTPS headers within, the bundles should only be served when requested with the header `Sec-Fetch-Dest: webbundle`.  To protect against cross-origin side channel attacks, like what CORB does, the browser requires the subresources from the bundle are served with the header `X-FLEDGE-Auction-Only` which ensures they are only loaded via `directFromSellerSignals`.

The bundle may be fetched using credentials like cookies, as described in the [subresource bundles explainer](https://github.com/WICG/webpackage/blob/main/explainers/subresource-loading.md#requests-mode-and-credentials-mode).

`directFromSellerSignals` supports multiple sellers -- it may be set on the top-level auction, and on component auctions.

Worklet processes follow Chrome's standard site-isolation policies. On Android, due to resource constraints, it's possible that a worklet may run in the same process as a renderer.

##### CORS Required

The origin of the frame that called `runAdAuction()` is not required to match the origin of `directFromSellerSignals`, so CORS is used when fetching `directFromSellerSignals`.  This means subresources in the bundle should include the [Access-Control-Allow-Origin](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Origin) header to authorize the origin that calls `runAdAuction()` -- this should also be done on the bundle file itself.

For the JSON response, only the `https` scheme is supported -- the `uuid-in-package` scheme isn't supported as that scheme doesn't support CORS.


### 3. Buyers Provide Ads and Bidding Functions (BYOS for now)


Interest groups are used by their owners to bid in on-device auctions.  We refer to each party bidding in an auction as a _buyer_.  The buyer might be an advertiser who is also an interest group owner themselves, or a DSP that owns an interest group and acts on an advertiser's behalf.

Alternatively, the interest group owner might be a party whose responsibility is focused on building audiences and letting many different advertisers use them for targeting.  In this case, the owner of the interest group is still the buyer, for the purposes of the seller's on-device auction.  The interest group owner's choice of which advertiser's ad to display might be made via some server-side decision process, or might be made on a case-by-case basis on-device as part of the bidding process.

Buyers have three basic jobs in the on-device ad auction:



1. Buyers choose whether or not they want to participate in an auction.
2. Buyers pick a specific ad, and enter it in the auction along with a bid price and whatever metadata the seller expects.
3. Buyers perform reporting on the auction outcome.  All buyers have a mechanism for aggregate reporting.  _As a temporary mechanism_, the buyer who wins the auction has a way to do event-level reporting alongside their ad rendering; see the "Event-Level Reporting (for now)" section.


#### 3.1 Fetching Real-Time Data from a Trusted Server


Buyers may want to make on-device decisions that take into account real-time data (for example, the remaining budget of an ad campaign).  This need can be met using the interest group's `trustedBiddingSignalsUrl` and `trustedBiddingSignalsKeys` fields.  Once a seller initiates an on-device auction on a publisher page, the browser checks each participating interest group for these fields, and makes an uncredentialed (cookieless) HTTP fetch to a URL of the form:

    https://www.kv-server.example/getvalues?hostname=publisher.com&experimentGroupId=12345&keys=key1,key2&interestGroups=name1,name2

The base URL `https://www.kv-server.example/getvalues` comes from the interest group's `trustedBiddingSignalsUrl`, the hostname of the top-level webpage where the ad will appear `publisher.com` is provided by the browser, `experimentGroupId` comes from `perBuyerExperimentGroupIds` if provided, `keys` is a list of `trustedBiddingSignalsKeys` strings, and `interestGroupNames` is a list of the names of the interest groups that data is being fetched for.  The requests may be coalesced (for efficiency) across any number of interest groups that share a `trustedBiddingSignalsUrl` (which means they also share an owner).

The response from the server should be a JSON object of the form:

```
{ 'keys': {
      'key1': arbitrary_json,
      'key2': arbitrary_json,
      ...},
  'perInterestGroupData': {
      'name1': {
          'priorityVector': {
              'signal1': number,
              'signal2': number,
              ...}
      },
      ...
  }
}
```

and the server must include the HTTP response header `X-fledge-bidding-signals-format-version: 2`.  If the server does not include the header, the response will assumed to be an in older format, where the response is only the contents of the `keys` dictionary.

The value of each key that an interest group has in its `trustedBiddingSignalsKeys` list will be passed from the `keys` dictionary to the interest group's generateBid() function as the `trustedBiddingSignals` parameter. Values missing from the JSON object will be set to null. If the JSON download fails, or there are no `trustedBiddingSignalsKeys` or `trustedBiddingSignalsUrl` in the interest group, then the `trustedBiddingSignals` argument to generateBid() will be null.

The `perInterestGroupData` dictionary contains optional data for interest groups whose names were included in the request URL. The `priorityVector` will be used to calculate the final priority for an interest group, if that interest group has `enableBiddingSignalsPrioritization` set to true in its definition. Otherwise, it's only used to filter out interest groups, if the dot product with `prioritySignals` is negative. See [Filtering and Prioritizing Interest Groups](#35-filtering-and-prioritizing-interest-groups) for more information.

Similarly, sellers may want to fetch information about a specific creative, e.g. the results of some out-of-band ad scanning system.  This works in much the same way, with the base URL coming from the `trustedScoringSignalsUrl` property of the seller's auction configuration object. The parameter `experimentGroupId` comes from `sellerExperimentGroupId` in the auction configuration if provided. However, the URL has two sets of keys: "renderUrls=url1,url2,..." and "adComponentRenderUrls=url1,url2,..." for the main and adComponent renderUrls bids offered in the auction. It is up to the client how and whether to aggregate the fetches with the URLs of multiple bidders.  The response to this request should be in the form:

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

For detailed specification and explainers of the trusted key-value server, see also the following:

- [FLEDGE Key/Value Server APIs Explainer](https://github.com/WICG/turtledove/blob/master/FLEDGE_Key_Value_Server_API.md)
- [FLEDGE Key/Value Server Trust Model Explainer](https://github.com/privacysandbox/fledge-docs/blob/main/key_value_service_trust_model.md)

#### 3.2 On-Device Bidding

Once the trusted bidding signals are fetched, each interest group's bidding function will run, inside a bidding worklet associated with the interest group owner's domain.  The buyer's JavaScript is loaded from the interest group's `biddingLogicUrl`, which must expose a `generateBid()` function:


```
generateBid(interestGroup, auctionSignals, perBuyerSignals,
    trustedBiddingSignals, browserSignals, directFromSellerSignals) {
  ...
  return {'ad': adObject,
          'adCost': optionalAdCost,
          'bid': bidValue,
          'render': renderUrl,
          'adComponents': [adComponent1, adComponent2, ...],
          'allowComponentAuction': false};
}
```


The arguments to `generateBid()` are:



*   interestGroup: The interest group object, as saved during `joinAdInterestGroup()` and perhaps updated via the `dailyUpdateUrl`.
    * `priority` and `prioritySignalsOverrides` are not included. They can be modified by `generatedBid()` calls, so could theoretically be used to create a cross-site profile of a user accessible to `generateBid()` methods, otherwise.
*   auctionSignals: As provided by the seller in the call to `runAdAuction()`.  This is the opportunity for the seller to provide information about the page context (ad size, publisher ID, etc), the type of auction (first-price vs second-price), and so on.
*   perBuyerSignals: The value for _this specific buyer_ as taken from the auction config passed to `runAdAuction()`.  This can include contextual signals about the page that come from the buyer's server, if the seller is an SSP which performs a real-time bidding call to buyer servers and pipes the response back, or if the publisher page contacts the buyer's server directly.  If so, the buyer may wish to check a cryptographic signature of those signals inside `generateBid()` as protection against tampering.
*   trustedBiddingSignals: An object whose keys are the `trustedBiddingSignalsKeys` for the interest group, and whose values are those returned in the `trustedBiddingSignals` request.
*   browserSignals: An object constructed by the browser, containing information that the browser knows, and which the buyer's auction script might want to use or verify.  The `dataVersion` field will only be present if the `Data-Version` header was provided and had a consistent value for all of the trusted bidding signals server responses used to construct the trustedBiddingSignals. `topLevelSeller` is only present if `generateBid()` is running as part of a component auction. Additional fields can include information about both the context (e.g. the true hostname of the current page, which the seller could otherwise lie about) and about the interest group itself (e.g. times when it previously won the auction, to allow on-device frequency capping).
    ```
    { 'topWindowHostname': 'www.example-publisher.com',
      'seller': 'https://www.example-ssp.com',
      'topLevelSeller': 'https://www.another-ssp.com',
      'joinCount': 3,
      'bidCount': 17,
      'prevWins': [[time1,ad1],[time2,ad2],...],
      'wasmHelper': ... /* a WebAssembly.Module object based on interest group's biddingWasmHelperUrl */
      'dataVersion': 1, /* Data-Version value from the trusted bidding signals server's response(s) */
    }
    ```
*   directFromSellerSignals is an object that may contain the following fields:
    *   perBuyerSignals: Like auctionConfig.perBuyerSignals, but passed via the [directFromSellerSignals](#25-additional-trusted-signals-directfromsellersignals) mechanism. These are the signals whose subresource URL ends in `?perBuyerSignals=[origin]`.
    *   auctionSignals: Like auctionConfig.auctionSignals, but passed via the [directFromSellerSignals](#25-additional-trusted-signals-directfromsellersignals) mechanism. These are the signals whose subresource URL ends in `?auctionSignals`.

In the case of component auctions, an interest group's `generateBid()` function will be invoked in all component auctions for which it qualifies, though the `bidCount` value passed to future auctions will only be incremented by one for participation in that auction as a whole.

The output of `generateBid()` contains the following fields:

*   ad: (optional) Arbitrary metadata about the ad which this interest group wants to show. The seller uses this information in its auction and decision logic. If not present, it's treated as if the value were null.
*   adCost: (optional) A numerical value used to pass reporting advertiser click or conversion cost from generateBid to reportWin. The precision of this number is limited to an 8-bit mantissa and 8-bit exponent, with any rounding performed stochastically.
*   bid: A numerical bid that will enter the auction.  The seller must be in a position to compare bids from different buyers, therefore bids must be in some seller-chosen unit (e.g. "USD per thousand").  If the bid is zero or negative, then this interest group will not participate in the seller's auction at all.  With this mechanism, the buyer can implement any advertiser rules for where their ads may or may not appear.
*   render: A URL which will be rendered to display the creative if this bid wins the auction.
*   adComponents: (optional) A list of up to 20 adComponent strings from the InterestGroup's adComponents field. Each value must match an adComponent renderUrl exactly. This field must not be present if the InterestGroup has no adComponent field. It is valid for this field not to be present even when adComponents is present. (See ["Ads Composed of Multiple Pieces"](#34-ads-composed-of-multiple-pieces) below.)
*   allowComponentAuction: If this buyer is taking part of a component auction, this value must be present and true, or the bid is ignored. This value is ignored (and may be absent) if the buyer is part of a top-level auction.

`generateBid()` has access to the `setPrioritySignalsOverride(key, value)` method. This adds an entry to the current interest group's `prioritySignalsOverrides` dictionary with the specified `key` and `value`, overwriting the previous value, if there was already an entry with `key`. If `value` is null, the entry with the specified key is deleted, if it exists.


#### 3.3 Metadata with the Ad Bid

The metadata accompanying the returned ad is not specified in this document, because sellers and buyers are free to establish whatever protocols they want here.

Sellers can ask buyers to provide whatever information they feel is necessary for their ad scoring job.  Sellers have an opportunity to enforce requirements in their `scoreAd()` function, rejecting bids whose metadata they find lacking.

If `generateBid()` picks an ad whose rendering URL is not yet above the browser-enforced microtargeting prevention threshold, then the function will be called a second time, this time with a modified `interestGroup` argument that includes only the subset of the group's ads that are over threshold.  (The under-threshold ad will, however, be counted towards the microtargeting thresholding for future auctions for this and other users.)


#### 3.4 Ads Composed of Multiple Pieces

The [Product-level TURTLEDOVE](https://github.com/WICG/turtledove/blob/master/PRODUCT_LEVEL.md) proposal describes a use case in which the rendered ad is composed of multiple pieces — a top-level ad template "container" which includes some slots that can be filled in with specific "products".  This is useful because the browser's microtargeting threshold can be applied to each individual component of the ad without compromising on tracking protections.

The output of `generateBid()` can use the on-device ad composition flow through an optional adComponents field, listing additional URLs made available to the fenced frame the container URL is loaded in. The component URLs may be retrieved by calling `navigator.adAuctionComponents(numComponents)`, where numComponents is at most 20. To prevent bidder worklets from using this as a sidechannel to leak additional data to the fenced frame, exactly numComponents obfuscated URLs will be returned by this method, regardless of how many adComponent URLs were actually in the bid, even if the bid contained no adComponents, and the Interest Group itself had no adComponents either.


#### 3.5 Filtering and Prioritizing Interest Groups

When an interest group has a non-empty `priorityVector`, its priority is dynamically calculated before applying `perBuyerGroupLimits`. To do this, the browser computes the sparse dot product of the interest group's `priorityVector` and a `prioritySignals` vector. The __sparse dot product__ of two vectors `V` and `W` is the sum of the products `V[key] * W[key]` for each key in both `V` and `W`. For example, the sparse dot product of `{'x':3, 'y':7, 'z':12}` with `{'x':-2, 'y':1.7, 'teapot':418}` is `3*(-2) + 7*1.7 = 5.9`. The `prioritySignals` vector is the result of merging the following objects, which all have strings as keys and numbers as values, with entries in objects earlier in the list taking priority over entries later in the list:

* The interest group's `prioritySignalsOverrides` field.
* A browser-generated `prioritySignals` object, defined below.
* The `auctionConfig`'s `perBuyerPrioritySignals` entry for the interest group owner.
* The `auctionConfig`'s `perBuyerPrioritySignals` "\*" entry.

Additionally, keys starting with "browserSignals." are reserved, and may only appear in `prioritySignalsOverrides` and the browser-generated `prioritySignals` object.

The browser-generated `prioritySignals` object contains the following values:
* `browserSignals.one`: This is always 1. It's useful for adding a constant to the dot product.
* `browserSignals.basePriority`: The `priority` field in the interest group, which may have been modified by a `setPriority()` call.
* `browserSignals.firstDotProductPriority`: The sparse dot product of the interest group's `priorityVector` and `prioritySignals`. Only non-zero when using a `priorityVector` from a trusted bidding signals fetch, and the interest group also has a `prioritySignals` field. See below for more details.
* `browserSignals.ageInMinutes`: How long since the interest group was most recently joined, in minutes, as an integer. Guaranteed to be between 0 and 43200 (the number of minutes in 30 days, the maximum lifetime of an interest group), inclusive.
* `browserSignals.ageInMinutesMax60`: Same as `browserSignals.ageInMinutes`, but with a maximum value of 60. 60 is returned if the group is more than an hour old.
* `browserSignals.ageInHoursMax24`: The interest group's age in hours, as an integer, with a maximum value of 24. Always non-negative.
* `browserSignals.ageInDaysMax30`: The interest group's age in days, as an integer, with a maximum value of 30. Always non-negative.

If the resulting sparse dot product is negative, the interest group is immediately removed from the auction (note that if there's no `priorityVector`, interest groups with negative `priority` values currently are not filtered from auctions). After calculating new priorities as needed, and filtering out interest groups with negative calculated priorities, the `perBuyerGroupLimits` value is applied to all interest groups of a given owner, unless the interest group's `enableBiddingSignalsPrioritization` field is present and true.

If `enableBiddingSignalsPrioritization` is true, then rather than applying `perBuyerGroupLimits` immediately after calculating the sparse dot product as described above, group limit enforcement is delayed until after fetching the trusted bidding signals. In this case, if the trusted bidding signals specify a per-interest-group `priorityVector` for an interest group, the sparse dot product of that `priorityVector` and the `prioritySignals` vector is calculated. The `prioritySignals` vector is the same as in the first calculation, except that there's an additional `browserSignals.firstDotProductPriority` value, which is the result of multiplying the interest group's `priorityVector`, if present, with the `prioritySignals` of the auction. If this new dot product is negative, the interest group is removed from the auction. If there is no `priorityVector` for an interest group, the priority from earlier in the auction is used instead. Once all priorities have been calculated, then `perBuyerGroupLimits` is applied, and the auction continues as normal.

if `enableBiddingSignalsPrioritization` is false, then the `priorityVector` from the trusted bidding signals will still be multiplied by the `prioritySignals` as above, but it will only be used to skip the interest group if the result is less than 0. This parameter exists to improve performance - when it's false for all interest groups for a particular bidder in an auction, that bidder's interest groups can be filtered out before any bidding signals are fetched, reducing network usage and server load.

For example, with the following interest groups and auction config:

```
auctionConfig = {
  ...,
  'interestGroupBuyers': {'https://buyer1.com/'},
  'perBuyerPrioritySignals': {
    '\*': {'politics': 1},
  }
}

interestGroup1 = {
  'owner': 'https://buyer1.com/',
  'name': 'NoPolitics',
  'priorityVector': {'politics': -1},
  ...
}

interestGroup2 = {
  'owner': 'https://buyer1.com/',
  'name': 'BidFor240Minutes',
  'priorityVector': {'browserSignals.ageInMinutes': -1, 'browserSignals.one': 240},
  ...
}

interestGroup3 = {
  'owner': 'https://buyer1.com/',
  'name': 'FilterOnDataFromServer',
  'trustedBiddingSignalsUrl': 'https://buyer1.com/bidder_signals',
  ...
}
```

The `NoPolitics` interest group will not get a chance to bid, since the `AuctionConfig` has `politics` set to 1, and `NoPolitics` multiplies that by -1, giving it a priority of -1.

The `BidFor240Minutes` interest group will have a positive priority if it was joined during the first 240 minutes, starting with 240 right after being joined, and working its way down to 0 at the 240 minute mark, after which it will have a negative priority and so will not bid.

The `FilterOnDataFromServer` interest group will result in fetching `https://buyer1.com/bidder_signals?publisher=<...>&interest_groups=FilterOnDataFromServer,<...>`, and then if that result has a `perInterestGroupData.FilterOnDataFromServer.priorityVector` object, then that is used just like the `priorityVector` field from the other two examples, except that it's only used for filtering, not to set the priority (unless the group has a true `enableBiddingSignalsPrioritization` field).  A [user defined function](https://github.com/privacysandbox/fledge-docs/blob/main/key_value_service_trust_model.md#support-for-user-defined-functions-udfs) could be used on the FLEDGE Key-Value server to calculate that `priorityVector` value, and hence to decide if `FilterOnDataFromServer`'s `generateBid()` method is invoked or if it's filtered out.

### 4. Browsers Render the Winning Ad

The winning ad will be rendered in a [Fenced Frame](https://github.com/shivanigithub/fenced-frame): a mechanism under development for rendering a document in an embedded context which is unable to communicate with the surrounding page.  This communication blockage is necessary to meet the privacy goal that sites cannot learn about their visitors' ad interests.  (Note that the microtargeting prevention threshold alone is not enough to address this threat: the threshold prevents ads which could identify a single person, but it allows ads which identify a group of people that share a single interest.)

Fenced Frames are designed to be able to provide a second type of protection as well: they will not use the network to load any data from a server, instead only rendering content that was previously downloaded (e.g. as a Web Bundle).  This restriction is focused on preventing information leakage based on server-side joins via timing attacks.

_As a temporary mechanism, we will still allow network access,_ rendering the winning ad in a Fenced Frame that is able to load resources from servers.

The TURTLEDOVE privacy goals mean that this cannot be the long-term solution.  Rendering ads from previously-downloaded Web Bundles, as originally proposed, is one way to mitigate this leakage.  Another possibility is ad rendering in which all network-loaded resources come from a trusted CDN that does not keep logs of the resources it serves.  As with servers involved in providing the trusted bidding signals, the privacy model and browser trust mechanism for such a CDN would require further work.

Reports are only sent and most interest group state changes (e.g. updating `prevWins` and `bidCount`, updating k-anonymity information) are only applied if and when the winning `renderURL` is loaded in a fenced frame, in the case there is a winner, or when there is no winner. Priorities and `priorityOverrides` are updated immediately upon completion of the `generateBid()` call that invoked their respective update functions, since how the information from those are used is not expected to depend on whether the current auction was completed or not.


### 5. Event-Level Reporting (for now)

Once the winning ad has rendered in its Fenced Frame, the seller and the winning buyer each have an opportunity to perform logging and reporting on the auction outcome.  The browser will call one reporting function in the seller's auction worklet and one in the winning buyer's bidding worklet.

_As a temporary mechanism,_ these reporting functions will be able to send event-level reports to their servers.  These reports can include contextual information, and can include information about the winning interest group if it is over an anonymity threshold.  This reporting will happen synchronously, while the page with the ad is still open in the browser.

In the long term, we need a mechanism to ensure that the after-the-fact reporting cannot be used to learn the advertising interest group of individual visitors to the publisher's site — the same privacy goal that led to Fenced Frame rendering. The [Private Aggregation API](https://github.com/alexmturner/private-aggregation-api) proposal aims to satisfy this use case. Therefore event-level reporting is just a temporary model until an adequate reporting framework is settled and in place.


#### 5.1 Seller Reporting on Render

A seller's JavaScript (i.e. the same script, loaded from `decisionLogicUrl`, that provided the `scoreAd()` function) can also expose a `reportResult()` function. This is called with the bid that won the auction, if applicable. For component auction seller scripts, `reportResult()` is only invoked if the bid that won the component auction also went on to win the top-level auction.


```
reportResult(auctionConfig, browserSignals, directFromSellerSignals) {
  ...
  return signalsForWinner;
}
```


The arguments to this function are:

*   auctionConfig: The auction configuration object passed to `navigator.runAdAuction()`
*   browserSignals: An object constructed by the browser, containing information it knows about what happened in the auction.
    *   `topLevelSeller`, `topLevelSellerSignals`, and `modifiedBid` are only present for component auctions, while `componentSeller` is only present for top-level auctions when the winner came from a component auction.
    *   `modifiedBid` is the bid value a component auction's `scoreAd()` script passes to the top-level auction.
    *   `topLevelSellerSignals` is the output of the top-level seller's `reportResult()` method.
    *   `highestScoringOtherBid` is the value of a bid with the second highest score in the auction. It may be greater than `bid` since it's a bid instead of a score, and a higher bid value may get a lower score. Rejected bids are excluded when calculating this signal. If there was only one bid, it will be 0. In the case of a tie, it will be randomly chosen from all bids with the second highest score, excluding the winning bid if the winning bid had the same score. A component seller's `reportWin()` function will be passed a bid with the second highest score in the component auction, not the top-level auction. It is not reported to top-level sellers in a multi-SSP case because we expect a top-level auction in this case to be first-price auction only:

    ```
    { 'topWindowHostname': 'www.example-publisher.com',
      'topLevelSeller': 'https://www.example-ssp.com',
      'componentSeller': 'https://www.some-other-ssp.com',
      'interestGroupOwner': 'https://www.example-dsp.com/',
      'renderUrl': 'https://cdn.com/url-of-winning-creative.wbn',
      'bid:' bidValue,
      'desirability': desirabilityScoreForWinningAd,
      'topLevelSellerSignals': outputOfTopLevelSellersReportResult,
      'dataVersion': versionFromKeyValueResponse,
      'modifiedBid': modifiedBidValue,
      'highestScoringOtherBid': highestScoringOtherBidValue
    }
    ```
*   directFromSellerSignals is an object that may contain the following fields:
    *   sellerSignals: Like auctionConfig.sellerSignals, but passed via the [directFromSellerSignals](#25-additional-trusted-signals-directfromsellersignals) mechanism. These are the signals whose subresource URL ends in `?sellerSignals`.
    *   auctionSignals: Like auctionConfig.auctionSignals, but passed via the [directFromSellerSignals](#25-additional-trusted-signals-directfromsellersignals) mechanism. These are the signals whose subresource URL ends in `?auctionSignals`.

The `browserSignals` argument must be handled carefully to avoid tracking.  It certainly cannot include anything like the full list of interest groups, which would be too identifiable as a tracking signal.  The `renderUrl` can be included since it has already passed a k-anonymity check.  The browser may limit the precision of the bid and desirability values to avoid these numbers exfiltrating information from the interest group's `userBiddingSignals`.  On the upside, this set of signals can be expanded to include useful additional summary data about the wider range of bids that participated in the auction, e.g. the number of bids.  Additionally, the `dataVersion` will only be present if the `Data-Version` header was provided in the response headers from the Trusted Scoring server.

The `reportResult()` function's reporting happens by directly calling network APIs in the short-term, but will eventually go through the Private Aggregation API once it has been developed. The output of this function is not used for reporting, but rather as an input to the buyer's reporting function.


#### 5.2 Buyer Reporting on Render and Ad Events

The buyer's JavaScript (i.e. the same script, loaded from `biddingLogicUrl`, that provided the `generateBid()` function) can also expose a `reportWin()` function:


```
reportWin(auctionSignals, perBuyerSignals, sellerSignals, browserSignals,
    directFromSellerSignals) {
  ...
}
```


The arguments to this function are:

*   auctionSignals and perBuyerSignals: As in the call to `generateBid()` for the winning interest group.
*   sellerSignals: The output of `reportResult()` above, giving the seller an opportunity to pass information to the buyer. In the case where the winning buyer won a component auction and then went on to win the top-level auction, this is the output of component auction's seller's `reportResult()` method.
*   browserSignals: Similar to the argument to `reportResult()` above, though without the seller's desirability score, but with additional `adCost`, `seller`, `madeHighestScoringOtherBid` and potentially `interestGroupName` fields:
    *   The `adCost` field contains the value that was returned by `generateBid()`, stochastically rounded to fit into a floating point number with an 8 bit mantissa and 8 bit exponent. This field is only present if `adCost` was returned by `generateBid()`.
    *   The `interestGroupName` may be included if the tuple of interest group owner, name, bidding script URL and ad creative URL were jointly k-anonymous.
    *   The `madeHighestScoringOtherBid` field is true if the interest group owner was the only bidder that made bids with the second highest score.
    *   The `highestScoringOtherBid` and `madeHighestScoringOtherBid` fields are based on the auction the interest group was directly part of. If that was a component auction, they're from the component auction. If that was the top-level auction, then they're from the top-level auction. Component bidders do not get these signals from top-level auctions since it is the auction seller joining the top-level auction, instead of winning component bidders joining the top-level auction directly.
    *   The `dataVersion` field will contain the `Data-Version` from the trusted bidding signals response headers if they were provided by the trusted bidding signals server response and the version was consistent for all keys requested by this interest group, otherwise the field will be absent.
    *   If the winning bid was from a component auction, then `seller` will be the seller in the component auction, a `topLevelSeller` field will contain the seller of the top-level auction.
*   directFromSellerSignals is an object that may contain the following fields:
    *   perBuyerSignals: Like auctionConfig.perBuyerSignals, but passed via the [directFromSellerSignals](#25-additional-trusted-signals-directfromsellersignals) mechanism. These are the signals whose subresource URL ends in `?perBuyerSignals=[origin]`.
    *   auctionSignals: Like auctionConfig.auctionSignals, but passed via the [directFromSellerSignals](#25-additional-trusted-signals-directfromsellersignals) mechanism. These are the signals whose subresource URL ends in `?auctionSignals`.

The `reportWin()` function's reporting happens by directly calling network APIs in the short-term, but will eventually go through the Private Aggregation API once it has been developed. Once the Private Aggregation API has been integrated with FLEDGE the `interestGroup` object passed to `generateBid()` will be available to `reportWin()`.

Ads often need to report on events that happen once the ad is rendered.  One common example is reporting on whether an ad became viewable on-screen.  We will need a communications channel to allow the publisher page or the Fenced Frame to pass such information into the worklet responsible for reporting.  Some additional design work is needed here.


#### 5.3 Losing Bidder Reporting

We also need to provide a mechanism for the _losing_ bidders in the auction to learn aggregate outcomes.  Certainly they should be able to count the number of times they bid, and losing ads should also be able to learn (in aggregate) some seller-provided information about e.g. the auction clearing price.  Likewise, a reporting mechanism should be available to buyers who attempted to bid with a creative that had not yet reached the k-anonymity threshold.

This could be handled by a `reportLoss()` function running in the worklet.  Alternatively, the model of [SPURFOWL](https://github.com/AdRoll/privacy/blob/main/SPURFOWL.md) (an append-only datastore and later aggregate log processing) could be a good fit for this use case.  The details here are yet to be determined.


