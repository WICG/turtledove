> FLEDGE has been renamed to Protected Audience API. To learn more about the name change, see the [blog post](https://privacysandbox.com/intl/en_us/news/protected-audience-api-our-new-name-for-fledge)

# Extended Private Aggregation Reporting in FLEDGE

## Introduction
The [Private Aggregation API](https://github.com/patcg-individual-drafts/private-aggregation-api)
proposes a mechanism for participants of FLEDGE auctions to create 
[summary reports](https://github.com/WICG/attribution-reporting-api/blob/main/AGGREGATION_SERVICE_TEE.md#key-terms)
which can measure generic cross-site data.

This proposal seeks to extend this API to support further measurement use-cases
for FLEDGE auctions using the same mechanism to aggregate reports from the browser.
The new APIs added allow for reporting bidding information and new signals (post-auction signals, latency)
at different points in auction (e.g. win, loss, bid, click), while retaining the same privacy and
aggregation aspects of the Private Aggregation API proposal.

## Goals
* Allow all auction participants (both winner and losers) to measure aggregate data about their bids, competing bids, and other auction signals
  * “How far off the auction clearing price was my bid?”
  * "Why was my bid rejected?"
  * "How far off the second highest bid value was my bid?"
* Allow auction participants to measure aggregate data using signals only available during bid generation
  * "How many times did my campaign bid?" 
  * "What is my campaign's win rate?"
* Allow auction winners to measure aggregate data about the auction relative to events which happen with the rendered ad (such as a click on the ad)
  * "What is the bid cost per click?"
  * "What is the bid cost per render?" 
* Allow auction sellers to measure aggregate data on auction latency and other statistics
  * "Which participants are contributing the most to auction latency?"
* Allow auction buyers to measure aggregate data on their bidding latency
  * “How much latency are my bidding signals fetches and bid generation code contributing?

## Background
The [Private Aggregation API](https://github.com/patcg-individual-drafts/private-aggregation-api)
currently allows auction winners to generate histogram contributions within their
reportResult()/reportWin() methods using a new API available in the worklet:

```
function reportResult(auctionConfig, browserSignals) {
  …
  privateAggregation.contributeToHistogram({
      bucket: convertBuyerToBucketId(browserSignals.interestGroupOwner),
      value: convertBidToReportingValue(browserSignals.bid)
    });
}
```
While this provides a way to gather important information about a winning creative, for instance,
the average bid submitted for a creative, it still lacks some information that can be useful for
auction participants as well as sellers. Information which is also not available through event
level reporting. Just to mention a few:
* The ability for a seller to track the average number of auction participants
* The ability for a buyer to know the win rate of a campaign
* The ability for a buyer/seller to know the highest other bid (HOB) for training bid shading models
* The ability for a buyer/seller to measure actions contributing to auction latency
* The ability for a buyer to link useful bidding signals (for instance time a user has spent in 
an interest group) with a click event — this information is essential for model training. 

Below, we propose an extension of the Private Aggregation API that tries to address these use cases.

## Declaring Contributions

A natural way to address the above use cases is for auction participants and sellers to be able to
trigger arbitrary reports at different events. For instance, a buyer may want to count the number of
times users that were in an interest group for less than an hour clicked on the winning ad. Or they
may want to track auction signals associated with a non-winning bid, etc.

Our proposed API extension allows a bidder to register a set of histogram contributions associated
with an arbitrary `event_key` within `generateBid`, `scoreAd`, `reportWin`, and `reportResult`.

### Example 1: Correlating bidding signals with click information.

We consider the scenario where a buyer wants to learn the click-through rate of ads when a user has
been in an interest group for a given time.

To generate the bucket that represent interest group age, the buyer may implement `getImpressionReportBucket()` and `getClickReportBucket()` functions which return buckets that map an interest group and the time the user has spent in that interest group to a 128-bit integer as `BigInt`. The `browserSignals.recency` value inside `generateBid()` specifies the duration in minutes since the user joined the interest group.

Once the buckets have been derived, the buyer can call Private Aggregation inside `generateBid()`:  

```
function generateBid(interestGroup, auctionSignals, perBuyerSignals, trustedBiddingSignals, browserSignals) {
  // …
  privateAggregation.contributeToHistogramOnEvent(“reserved.win”, {
      bucket: getImpressionReportBucket(), // 128-bit integer as BigInt
      value: 1
  });
  privateAggregation.contributeToHistogramOnEvent("click", {
      bucket: getClickReportBuckets(), // 128-bit integer as BigInt
      value: 1
  });
}
```

The impression report will be sent if the [`reserved.win`](#reporting-bidding-data-for-wins) event is triggered, which is a reserved event for when the bid wins the auction. The click report will be sent if the `click` event is triggered by the [`window.fence.reportEvent("click")`](#reporting-bidding-data-associated-with-an-event-in-a-frame) call originating from the fenced frame of the ad.

The buyer can then generate the summary report of the impression count and click count to infer the click-through rate of the users in different interest group age buckets. 

### Example 2: Getting the average bid gap for an ad. 

A buyer may be interested in understanding how much higher they should have bid in order to win an
auction. To do this, not only do we need to trigger a report at loss time (see [Reporting for bids
which do not win](#reporting-for-bids-which-do-not-win)), but also have the value of the report depend on the outcome of the auction. To
do this, we introduce a field to the “contributions” object called `signalValue`. This field allows
the report to depend on post auction information. A `signalValue` object is composed of the following
values:
* `baseValue`: The name of the auction result value we want to report. For instance, winning-bid. 
* `scale`: Optional scale factor by which we want to multiply the auction result value. This is
useful for controlling the amount of noise added by the aggregation service. Scale is applied
before `offset` is added.
* `offset`: Optional offset to add to the auction result value.


After the auction happens, the final value of the generated report is (`baseValue` * `scale`) + `offset`.
The following example shows how to return the gap between an ad bid and the winning bid:

```
function generateBid(...) {
  bid = 100;
  privateAggregation.contributeToHistogramOnEvent(
    "reserved.loss",
    {
      bucket: 1596n, // represents a bucket for interest group x winning bid price
      value: {
        baseValue: "winning-bid",
        scale: 2, // Number which will be multiplied by browser value
        offset: -bid * 2 // Numbers which will be added to browser value, after scaling
       }
    });

  return bid;
}
```

In the event this bid does not win the auction, and the winning bid was 200, the histogram
contribution would be generated as:

```
bucket: 1596n
value: 200 // = (winning-bid * scale) + offset = (200 * 2) + (-100 * 2)
```
This would correspond to the gap by which the advertiser lost the auction, scaled by 2.
Scaling is important as it allows bidders to better control the amount of noise which will
be added to various aggregation keys.

### Example 3: Understand the reason an ad did not win

Similar to the above example, sometimes, the key that we want to aggregate over may depend
on the outcome of an auction. To solve this use case we provide an object called `signalBucket`.
The final bucket id of the bucket will depend on the outcome of the auction. The following
example allows the buyer to keep track of how many times their bid was rejected for particular reasons.


```
function generateBid(...) {
  privateAggregation.contributeToHistogramOnEvent(
    "reserved.loss",
    {
      bucket: {
        baseValue: "bid-reject-reason",
        offset: 500n // Offset buckets
       },
      value: 1
    });

  return bid;
}
```
If the bid is rejected for being below auction floor, this would result in a contribution being generated:

```
bucket: 502n // 500n + 2n (2n is the value associated with bids below auction floor, see bid-reject-reason below)
value: 1
```

## Reporting API informal specification

```
privateAggregation.contributeToHistogramOnEvent(eventType, contribution)
```

The parameters consist of:
* an `eventType` which is a string identifying the event type that triggers this report to
be sent (see [Triggering reports](#triggering-reports) below), and
* a `contribution` object which contains:
  * a `bucket` which is a 128bit ID or a `signalBucket`  which tells the browser how to calculate the bucket (represented as BigInt) and
  * a `value` which is a non-negative integer or a `signalValue` which tells the browser how to calculate the value.


Where `signalBucket` and `signalValue` is a dictionary which consists of:
* a `baseValue` field indicating which value the browser should use to calculate the resulting bucket or value.  A `signalValue.baseValue` or `signalBucket.baseValue` may be any of the following:
  * `winning-bid`: the value used is the winning bid value.
  * `highest-scoring-other-bid`: the value used is the bid value that was scored as second highest.
  * `script-run-time`: milliseconds of CPU time that the calling function required, when called.
  * `signals-fetch-time`: milliseconds required to fetch the trusted bidding or scoring signals, when called from `generateBid()` or `scoreAd()` respectively.
  * `bid-reject-reason`: one of the following values:
    * 0: indicates seller rejected bid without providing a reason, i.e., bid reject reason not available
    * 1: indicates seller rejected bid because “Invalid Bid”
    * 2: indicates seller rejected bid because “Bid was Below Auction Floor”
    * 3: indicates seller rejected bid because “Creative Filtered - Pending Approval by Exchange”
    * 4: indicates seller rejected bid because “Creative Filtered - Disapproved by Exchange”
    * 5: indicates seller rejected bid because “Creative Filtered - Blocked by Publisher”
    * 6: indicates seller rejected bid because “Creative Filtered - Language Exclusions”
    * 7: indicates seller rejected bid because “Creative Filtered - Category Exclusions”
    * 8: indicates seller rejected bid because "Creative Filtered - Did Not Meet The K-anonymity Threshold"
    * 9: indicates bid produced by `generateBid()` was rejected because it failed a currency check (e.g. the bid returned by `generateBid()` doesn't match
    what's specified by `perBuyerCurrency`)
    * 10: indicates bid passed through or altered by `scoreAd()` was rejected
    because it failed a currency check (e.g. the bid returned or passed through
    by `scoreAd()` in a component auction doesn't match the `sellerCurrency` of
    its auction or the `perBuyerCurrency` required by the top-level auction)
    * Perhaps other values indicating:
      * generateBid() hitting timeout
      * The auction was aborted (i.e. calling endAdAuction())
      * an auction that never rendered the ad
* optional `offset` and `scale` that allow the reporter to manipulate the browser signal to customize the buckets and values they will receive in reports:
  * `scale` will be multiplied by the browser provided value. Scale is applied before `offset` is added. Default value is 1.0.
  * `offset` will be added to the browser provided value (allows you to shift buckets, etc). Default value is 0.

## Triggering reports

### Reporting bidding data associated with an event in a frame

A fenced frame can trigger the sending of contributions associated with an arbitrary event
by calling into a new API:

```
window.fence.reportEvent("click");
```

This will cause any contributions associated with a call to `contributeToHistogramOnEvent()`
with an event-type of `click` to be reported/sent. 

In this example, `"click"` is an event-name chosen by the auction bidder. There are a number
of event names that are reserved and invoked directly by the browser. All reserved values will
have the `"reserved."` prefix, and all non-reserved values cannot have the `"reserved."` prefix.

### Reporting bidding data for wins

The `reserved.win` event-type is a special value that indicates a report should be sent when a
bid wins the auction.

The browser will trigger sending if contributions were registered for a winning bid.

### Reporting for bids which do not win

The `reserved.loss` event-type is a special value that indicates a report should be sent when a
bid does not win the auction. If the bid does not win, the browser will trigger reporting for
the `loss` contributions.

### Reporting bidding data no matter whether wins or not

The `reserved.always` event-type is a special value that indicates a report should be sent no
matter whether a bid wins the auction or not. The browser will always trigger reporting for the
`always` contributions.

## Reporting Per-Buyer Latency and Statistics to the Seller

The seller may want to collect aggregate statistics on latency and bids for their auctions.
Measuring this per buyer will allow the seller to evaluate performance information at a more
granular level and give helpful guidance and feedback to their bidders.

In order for the seller to collect this additional information from buyers, we need to give
buyers a way to express which sellers they’re comfortable sharing this information with, so we
add a new mechanism which allows each buyer to declare a set of approved sellers: The interestGroup
provided to `navigator.joinAdInterestGroup()` will contain a new field named `sellerCapabilities`, a
dict keyed by seller origin (or "*", to set defaults for non-specified seller origins) with lists of
permission strings as values (as described below). Sellers might want to only score bids from
interest groups that will share aggregate statistics, so a field, `requiredSellerCapabilities` will
also be added to the auction config. Any interest group that doesn't permit (for the auction's seller)
all the `sellerCapabilities` listed in `requiredSellerCapabilities` will not participate in the
auction.

For the seller to declare reporting, the `auctionConfig` passed to `runAdAuction` is amended to
contain a configuration for the seller latency report.

```
const auctionConfig = {
  'seller': 'https://www.example-ssp.com',
  …
  ‘interestGroupBuyers’: ['https://buyer1.com', 'https://buyer2.com', …],
  // An aggregation key for each buyer. This represents the starting contribution bucket
  // associated with the corresponding buyer.
  ‘auctionReportBuyerKeys’: [100n, // key for buyer1.com
                             105n, // key for buyer2.com
                             …],
  // Configures what values will be reported for each buyer. The key declares what signal will be
  // measured, and the bucket declares the bucket this will be placed relative to the
  // buyer key and the scale determines how the value is scaled.
  `auctionReportBuyers`: {
    `interestGroupCount`:       { `bucket`: 0n, `scale`: 1,  },
    ‘bidCount’:                 { `bucket`: 1n, `scale`: 1,  },
    ‘totalGenerateBidLatency’:  { `bucket`: 2n, `scale`: 1,  },
    ‘totalSignalsFetchLatency’: { `bucket`: 3n, `scale`: .1, },
  }
}

```

The seller is able to measure the following for each buyer, assuming permission is granted via the indicated `sellerCapabilities` for that seller:
* `interestGroupCount`: The number of the interest groups which could participate in the auction
(i.e. the number of interest groups on the machine for this buyer -- note the count *isn't* limited by the auction config's `perBuyerGroupLimits`). This requires the `interest-group-counts` `sellerCapabilities` permission.
* `bidCount`: The number of valid bids generated by this buyer. This requires the `interest-group-counts` `sellerCapabilities` permission.
* `totalGenerateBidLatency`: The sum of execution time for all generateBids() in milliseconds. This requires the `latency-stats` `sellerCapabilities` permission.
* `totalSignalsFetchLatency`: The total time spent fetching trusted buyer signals in milliseconds. If the interest group didn't fetch any trusted signals, then 0 milliseconds is reported. This requires the `latency-stats` `sellerCapabilities` permission.

Given the `auctionConfig` above, if buyer1.com had two interest groups participate in the auction,
their trusted buyer signals fetch taking 10ms, their `generateBid()` scripts running for 2ms and
3ms, and one of the interest groups returning a bid, the following histogram reports would be
generated by the browser:

```
{ bucket: 100n,// = 100n + 0n = buyer1.com’s key + interestGroupCount’s bucket
  value: 2 }, // = 2 interest groups were given the chance to bid.
{ bucket: 101n,// = 100n + 1n = buyer1.com’s key + bidCount’s bucket
  value: 1 }, // = 1 interest group returning a bid.
{ bucket: 102n,// = 100n + 2n = buyer1.com’s key + totalGenerateBidLatency’s bucket
  value: 5 }, // = 3ms + 2ms generateBid() runtimes.
{ bucket: 103n,// = 100n + 3n = buyer1.com’s key + totalSignalsFetchLatency’s bucket
  value: 1 }, // = 10ms total trusted bidding signals fetch time multiplied by .1 scale.
```

Here's an example of what the `sellerCapabilities` interest group field could look like: 

```
'sellerCapabilities': {
  'https://seller.com': [ 'interest-group-counts', 'latency-stats' ],
  'https://seller2.com': [ 'latency-stats' ],
  '*': [ 'interest-group-counts' ]
}
```

This would grant both `interest-group-counts` and `latency-stats` permission to https://seller.com, `latency-stats` to https://seller2.com, and `interest-group-counts` to all other sellers.

NOTE: the permission names `interestGroupCounts` and `latencyStats` are *deprecated* and will be removed in a future Chrome release, as they do not follow the documented WebIDL [naming conventions](https://webidl.spec.whatwg.org/#idl-enums).

## Data Volume

To control the data volume of aggregatable reports, auction participants may want to subsample
reports on the client to avoid high costs or latency associated with aggregation. Auction
participants can do this by only utilizing the Private Aggregation API some fraction of the time.

Subsampling on the client side as opposed to the server is ideal to avoid oversampling infrequent
events / campaigns.

## Privacy Considerations

See [Private Aggregation Privacy Consideration](https://github.com/alexmturner/private-aggregation-api#privacy-and-security)

Reports generated with the mechanisms in this proposal will be subject to the same contribution
bounding and bucketing for Private Aggregation Reports.

While new data is being used to generate histogram contributions (bidding signals, information from
within the fenced frame), the general privacy properties offered by the Aggregation Service still hold.

## Ideas For Future Iteration

### Including remaining user contribution budget as an input to reports

In the current proposal, the browser limits the overall sensitivity of the contributions made by a
single user per reporter per day. It may be desirable to allow reporters/interest group owners to select
their histogram contributions knowing the amount of budget which remains. Currently, reports created after
the budget has been exhausted are ignored.

### Reporting bidding data for renders

We may want to add a `reserved.render` event-type or similar which allows a bidder to measure contributions
when the browser shows the ad, or potentially when the browser finishes fetching the document.
