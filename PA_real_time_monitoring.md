# Real-time reporting


## Background

Users of the [Protected Audience API](FLEDGE.md) may need several different categories of reporting:

| Type | Cardinality/fidelity | Latency | Availability |
| --- | --- | --- | --- |
| [Private Aggregation](https://developers.google.com/privacy-sandbox/relevance/private-aggregation) | Very High, $2^{128}$ buckets | hours | All auction participants, every auction |
| [Event-level win reporting](FLEDGE.md#5-event-level-reporting-for-now) | Very High | seconds | Winning buyer and sellers, every auction |
| [forDebuggingOnly (downsampled)](FLEDGE.md#71-fordebuggingonly-fdo-apis) | Very High | seconds | All auction participants, rare usage |
| Real-time monitoring (this explainer) | Lower, ~1000 buckets | seconds | All auction participants, every auction |

To ensure the browser protects user privacy, no one mechanism can satisfy all three requirements: high cardinality/fidelity, low latency, and available for all participants in every auction.

The different strengths and weaknesses of each type of reporting make them useful for different needs:

| Type | Example Usage |
| --- | --- |
| [Private Aggregation](https://developers.google.com/privacy-sandbox/relevance/private-aggregation) | Analysis and monitoring with delay (e.g. rolling out new versions of scripts or data) |
| [Event-level win reporting](FLEDGE.md#5-event-level-reporting-for-now) | Billing |
| [forDebuggingOnly](FLEDGE.md#71-fordebuggingonly-fdo-apis) | Root cause analysis of aberrant bidding and scoring situations (e.g. JavaScript exception) |
| Real-time monitoring | Real-time failure detection |


## Motivation

The goal of real-time reporting is to get auction monitoring data to the buyer and seller as quickly as possible (e.g. < 5 mins). The primary use-case we are trying to capture with this reporting surface is rapid error detection i.e. detecting quickly whether there are major problems with unexpected behavior in `generateBid()`, `scoreAd()`, or loading of bidding or scoring scripts or trusted signals.

The high level Real Time Reporting API here is similar to the [Private Aggregation API](https://developers.google.com/privacy-sandbox/relevance/private-aggregation) in that it allows you 
to contribute to a histogram, but the contribution is subject to substantial local noise and is sent soon after the auction is completed to allow for a fast detection SLA. Once an error has been detected, [downsampled](https://github.com/WICG/turtledove/blob/main/FLEDGE.md#712-downsampling) `forDebuggingOnly` can be utilized for root cause analysis.

This API will provide histogram contribution values of only 0s and 1s, which can be used to correspond to whether or not the event being monitored behaved expectedly or aberrantly. There are a number of events that ad techs may wish to monitor in real time - ad techs assign meanings to buckets for events in `generateBid()` and `scoreAd()` (e.g. an ad tech might define bucket #44 as signal parsing error in generateBid). For certain event-types that cannot be reported from the aforementioned worklets, the browser defines a set of platform-defined buckets (e.g. the browser might define bucket #1024 as failure to fetch bidding or scoring script). Read more about [Platform Contributions here](#platform-contributions-reporting-errors-not-detectable-in-worklet-JS).

Note that the type of buckets that ad techs can define in `generateBid()` and `scoreAd()` can already be created in the Private Aggregation API. Where latency is acceptable, Private Aggregation API (with Aggregation Service) will provide more accuracy and should be the preferred solution for reporting beyond event-level `reportWin()` and `reportResult()`.


## Opting into reporting

In order to begin receiving reports, sellers must opt-in via the auction config. Buyers could either do a one-time, offline coordination with each seller, and let them know if they would like to opt-in to receiving reports from the API, or could indicate their report opt-in on an auction-by-auction basis. Sellers would then indicate which buyers and sellers have opted-in to receive contributions in their auctionConfig:

```js
const myAuctionConfig = {
  ...
  perBuyerRealTimeReportingConfig: {
    'https://buyer.example': {
    	type: 'default-local-reporting'
    },
  }
  sellerRealTimeReportingConfig: {
    type: 'default-local-reporting'
  }
  ...
};
```

Note that randomly sampling when to opt-in can be used to randomly sample auctions for reporting, if desired.


## Configuring reports during an auction

The primary API surface is a new method to emit a single histogram contribution from Protected Audience bidding or scoring script, i.e. within either `generateBid()` for a buyer or `scoreAd()` for a seller.

```js
// Error condition when bid is far too high.
const BID_TOO_HIGH_BUCKET = 123;
const BID_TOO_HIGH_WEIGHT = 10;
// Error condition when bidding logic throws an exception.
const BIDDING_THREW_BUCKET = 124;
const BIDDING_THREW_WEIGHT = 10;
// Indicate when bidding is slow.  We expect these reports at some background rate
// due to slow devices or networks.  When the rate increases it indicates a problem.
const BIDDING_SLOW_BUCKET = 125;
const BIDDING_SLOW_WEIGHT = 0.1;

function generateBid(...) { // or scoreAd
  // Contribute to the histogram if the worklet execution takes longer than a
  // specified # of ms.
  realTimeReporting.contributeToHistogram(
    { bucket: BIDDING_SLOW_BUCKET,
      priorityWeight: BIDDING_SLOW_WEIGHT,
      latencyThreshold: 500}); // In milliseconds

  let bidOut;

  try {
     // Bid generation logic...
     bidOut = ...
     ...
  } catch (error) {
    realTimeReporting.contributeToHistogram(
      { bucket: BIDDING_THREW_BUCKET,
        priorityWeight: BIDDING_THREW_WEIGHT });
    return;
  }
  
  if (bidOut.bid > MAX_REASONABLE_BID) {
    realTimeReporting.contributeToHistogram(
      BID_TOO_HIGH_BUCKET,
      { bucket: BID_TOO_HIGH_BUCKET,
        priorityWeight: BID_TOO_HIGH_WEIGHT });
    return;
  }

  return bidOut;
}
```


## Platform contributions: reporting errors not detectable in worklet JS

There may be some errors that occur that are not visible in either `scoreAd()` or `generateBid()`. These include things like failures to fetch the bidding script, trusted real-time signals, or creative URL. In these cases, we allocate a certain portion of the resulting histogram for platform errors. While this can be done as a completely separate set of buckets that are immutable from JavaScript, we would still require the number of total contributions across all buckets to be capped at 1, so these platform-assisted contributions will come with a default weighting and participate in the prioritization algorithm just like regular contributions.

The priority weight of platform contributions is hardcoded as 1, so you can reason about the behavior across platform and developer contributions.

Platform contributions are reported in the report’s `platformHistogram` field, separately from regular contributions. See [below](#sending-reports-after-an-auction) for more details.

| Bucket | Error |
| --- | --- |
| 0 | Bidding script fetch error |
| 1 | Scoring script fetch error |
| 2 | Trusted bidding signals fetch error |
| 3 | Trusted scoring signals fetch error |

Fetch errors include non-2xx response code, response header does not have “Ad-Auction-Allowed: true”, response body cannot be parsed as valid json for trusted signals, etc,.

## Sending reports after an auction

After the auction completes, all opted-in participants (sellers, and buyers with interest groups present on device) will always emit one report per participant per auction, even if no ad wins or no `contributeToRealTimeHistogram()` calls are made. These reports will be sent to `domain.example/.well-known/interest-group/real-time-report` where `domain.example` would be replaced with the seller’s origin or the buyer’s interest group owner origin. These reports are not tied to specific interest groups, they are aggregated over all interest groups. See [below](#histogram-contributions-and-the-rappor-noise-algorithm) for the high level description of the encoding. Reporting domains may be configurable in the future. 

Participants who did not call `contributeToRealTimeHistogram()` will contribute an array of zeros by default, which will still require the input going through the noising mechanism to satisfy the privacy requirements. After the noise mechanism, it is very unlikely that output will be all zeros. Even still, if we encounter a contribution of all zeros post-noising, we will still report on it. This is important for debiasing the noisy results, as explained below.

Real time reports sent through a post request to ad techs are encoded as [CBOR](https://www.rfc-editor.org/rfc/rfc8949.html) with the following schema (specified using [JSON Schema](https://datatracker.ietf.org/doc/html/draft-bhutton-json-schema-01)):

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "version": {
      "type": "number",
      "description": "Data version." },
    "histogram": {
      "type": "object",
      "properties": {
        "buckets": {
          "type": "byte string",
          "description": "a byte string bit packed from regular contribution buckets, padded with zeros to the nearest byte."
        },
        "length": "integer",
        "description": "the number of regular contribution buckets before bit packing."
      },
    },
    "platformHistogram": {
      "type": "object",
      "properties": {
        "buckets": {
          "type": "byte string",
          "description": "a byte string bit packed from platform contribution buckets, padded with zeros to the nearest byte."
        },
        "length": "integer",
        "description": "the number of platform contribution buckets before bit packing."
      },
    },
  "required": ["version", "histogram", "platformHistogram"]
}
```

Each histogram bucket value is encoded as a bit in a CBOR byte string ("buckets" byte strings in histogram and platformHistogram objects). Histogram bucket 0's value will be indicated by the most-significant bit in the first byte of the CBOR byte array. For example, [1,0,0,0,0,0,1,1, 1] will be packed to [131, 128]. See the "Data Notations" section of https://www.rfc-editor.org/rfc/rfc1700 for more information.

Example:
```json
{
  "version": 1,
  "histogram": {"buckets": [8, 9, …, 0, 1, 132], "length": 1024},
  "platformHistogram": {"buckets": [144], "length": 4}, 
}
```

In this example above, `histogram` field's buckets vector length is 128, and the number of buckets before bit packing is 1024. `platformHistogram`'s buckets vector is [1,0,0,1] before bit packing.

## Histogram contributions and the RAPPOR noise algorithm

Each participant in the auction will maintain a histogram with a fixed number of buckets. Initially, the API fixes the number of buckets to 1028 (1024 regular contribution buckets and 4 platform contribution buckets), which was chosen both for system health reasons (bandwidth, etc), but also to discourage the measurement of fine-grained events, which are not easily compatible with strong local privacy protections. Callers that need fewer than 1028 buckets should just ignore the unused fraction of the histogram.

Each auction participant can contribute a single time to this histogram from an auction. If an auction participant calls `contributeToRealTimeHistogram()` multiple times, one of those contributions is selected based on the `priorityWeight` param in the above API, and at the end of the auction a contribution is selected via weighted sampling. The `priorityWeight` param, required to be a [Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number) greater than zero and less than infinity, dictates the relative likelihood of which bucket will get the contribution, so the priority for critical error types should be set significantly higher than other error types.  For example, if an auction participant calls `contributeToRealTimeHistogram()` twice, once contributing to bucket 123 with a `priorityWeight` of 0.1 and once contributing to bucket 456 with a `priorityWeight` of 0.2, then their actual contribution from the auction has a $^1/_3$ chance of being in bucket 123 and a $^2/_3$ chance of being in bucket 456.

The direct histogram encoding output from the auction could look something like this:

```
buyer1.example: [0, 0, 0, 0, 1, ..., 0, 0, 0]
buyer2.example: [0, 0, 0, 0, 0, ..., 0, 1, 0]
buyer3.example: [0, 0, 0, 0, 0, ..., 0, 0, 0]
buyer4.example: [1, 0, 0, 0, 0, ..., 0, 0, 0]
seller.example: [0, 0, 0, 0, 0, ..., 0, 0, 1]
```

i.e. each auction participant holds a bit vector of length 1028, with at most one "1". This encoding is important to describe the noise mechanism: RAPPOR ([Erlingsson et al 2014](https://research.google/pubs/rappor-randomized-aggregatable-privacy-preserving-ordinal-response/)). Basic RAPPOR noises each coordinate of the bit vector independently, and it is parameterized by `epsilon`, a measure of privacy loss. Here is an reference python implementation of the algorithm:

```python
import math
import random
def rappor(epsilon: float, value: int, histogram_length: int):
  """Run the RAPPOR algorithm

  Arguments:
  epsilon: the privacy loss (higher is less private)
  value: the contribution to the histogram (encoded as an index into it)
  histogram_length: the number of buckets in the histogram
  """
  out = [0] * histogram_length
  out[value] = 1
  f = 2 / (1 + math.exp(epsilon / 2))
  for i in range(histogram_length):
    if random.random() < f/2:
      out[i] ^= 1 # flip
  return out
```

For this API, we set an epsilon value of 1 yielding a flipping probability of ~0.378 (value of f of ~.755).

The very large flipping probability is the essential privacy protection of this proposal. In the report sent to the auction participant, probably around 370 to 400 of the 1028 buckets will be 1s.  Moreover the actual bucket to which the auction contributed a 1 also has a 37.8% chance of its value being flipped, to a 0.

This means that data returned from this mechanism should not be interpreted as-is, because it will be heavily biased from all the random flipping. Rather than interpreting directly, individual reports should be aggregated and then subsequently debiased with the following reference code:

```python
# Implements an unbiased estimator for aggregates of RAPPOR output.
# The variance of this estimator on a per-bucket basis is:
# N * exp(epsilon / 2) / (exp(epsilon/2) - 1)**2. 
# = ~3.9 N for epsilon = 1
from typing import Sequence
def debias_rappor(epsilon: float, N: int, histogram: Sequence[int]):
  """Debias the output of the RAPPOR algorithm

  Arguments:
  epsilon: the privacy loss of the rappor function
  N: the number of values input into the rappor function. In this proposal this is
     the number of real-time reports that are sent.
  histogram: coordinate-wise aggregates from rappor function
  """
  f = 2 / (1 + math.exp(epsilon / 2))
  return [(h - N * f / 2) / (1 - f) for h in histogram]
```

The standard deviation ($\sigma$) from true count can be calculated using the formula, $\sigma \approx 2\sqrt{N}$, where N is the number of contributions. Measuring deviation at $2\sigma$ from true count will give the 95% confidence interval for errors.

This API’s primary goal is to help ad techs detect that errors are occurring and may need debugging. Due to the noisy contributions and probabilistic nature of the `priorityWeight` param, the count and frequency of these errors are not exact. For this reason, we recommend interpreting the output as a means to identify deviation from trends rather than representative counts for the frequency of contributions. Let’s consider an example:

Suppose an ad tech receives 1M reports in 5 minutes, and Bucket 4’s count is 390,000. We can estimate the number of times an auction actually contributed to Bucket 4 using the formula (390000 - 1000000 * .755/2) / (1-.755), which gives us an estimate of approximately 51,000 contributions to Bucket 4 in this example. The standard deviation is approximately $2\sqrt{1M} = 2000$, so the 95% confidence interval is around 4000 on either side of the estimate.  The ad tech can be highly confident that of the 1M auctions that sent a report, the number that contributed their 1 to Bucket 4 is somewhere between 47,000 and 55,000, i.e. between 4.7% and 5.5% of the auctions. 

As the API will send reports soon after the auction is completed, the only delay in measuring aggregates comes from aggregating reports after the fact. The cadence and grouping of aggregation is entirely up to the API user, and due to the local noise any report can be queried or analyzed an unlimited number of times. For example, running the de-noising calculation on 1 minute's worth of reports, rather than waiting to collect for 5 minutes, will give estimates 4 minutes sooner, but at the cost of relatively more noise: the size of the 95% confidence interval will be approximately $\sqrt{5} \approx 2.2$ times as wide.

We have published a [colab notebook](http://bit.ly/rappor-colab) exploring the RAPPOR algorithm in more detail.


### Interpreting the output with priorityWeight

When using the `priorityWeight` parameter, it becomes more difficult to debias the results as the true count of the bucket changes based on the independence of the error with respect to other errors due to the weighted sampling. In this example, let's say the buyer wants to measure two signals which have an underlying correlation:

*   How often the worklet latency exceeds 500ms, bucket 0 with priorityWeight: 1
*   How often the perBuyerTimeout is hit, bucket 1 with priorityWeight: 3

Let's assume that worklet latency exceeds 500ms in 25% of auctions, the perBuyerTimeout is hit in 10% of auctions and worklet latency always exceeds 500ms when perBuyerTimeout is hit. Assuming there are 1M auctions, we expect the true number of contributions to be:

*   Bucket 0: (0.25-0.1) * N (the 15% of auctions which don't hit the timeout) + (0.1 * 1 / 4) * N (the 10% of auctions which hit the timeout, where both errors are recorded) = 175,000
*   Bucket 1: (0.1 * 3 / 4) * N = 75,000

After noise and debiasing, at first glance, it appears that 17.5% of auctions exceed 500ms and 7.5% result in the perBuyerTimeout being hit. To understand the real distribution requires prior knowledge of the correlation between the errors for bucket 0 and bucket 1. As more contributions with less obvious correlations are involved in the weighted sampling:

*   the true distribution can be harder to determine
*   lower frequency signals may lose statistical significance

For this reason, it's important to be careful when interpreting estimated counts while using `priorityWeight`.


## Opt-in mechanism consideration

One consideration with this proposal is that the seller can unilaterally control whether or not the real-time reporting mechanism is turned on for buyers. We chose this design to avoid wasting bandwidth on buyers not interested in the reporting, and buyers do not have a secure hook-point to configure auction behavior before an auction starts.

Given that sellers have significant existing control over the auction, we deemed this an acceptable trade-off to avoid wasted user (and buyer) bandwidth.


## Privacy considerations

At the `epsilon` we are proposing ($\epsilon$ = 1), the entropy leaked is limited to approximately 0.18 bits per auction. This makes it very difficult for a bad actor to gain any meaningful user identifying information from an auction using this API. 

While the tight privacy parameters provide strong protections, there are two privacy considerations of note:

*   It reveals a small amount of information from scoreAd and generateBid to sellers and bidders, respectively. These contents are protected by the locally differentially private RAPPOR algorithm. The scope of this risk can be measured with the privacy loss epsilon parameter. This risk could be magnified by a bad actor running many auctions solely for the purpose of collecting more information from a publisher page. We mitigate this risk by bounding the number of contributions that this API will send to an ad tech from a page in a given period of time for each browser. It’s set to 10 reports per reporting origin per page per 20 seconds (per browser).
*   It reveals to the ad tech the fact that it had an interest group present on the device. This is mitigated by the fact that the reports do not contain any information about which auction triggered them, and the report is heavily noised. We also considered sending reports to all *eligible* auction participants for a given auction (i.e. all those present in `interestGroupBuyers`, even if they do not have interest groups), but this will result in an overwhelming number of reports sent.

We plan to address both these considerations in [future work](#limitations-and-future-work).


## Alternatives

### Why not use the Aggregation Service?

The primary reason the [Aggregation Service](https://developers.google.com/privacy-sandbox/relevance/aggregation-service) is not fit for this use-case as-is is that it is designed for higher latency batch reporting with latency on the order of hours. This proposal, on the other hand, can support processing reports with no delay.

Like we mention [below](#limitations-and-future-work), we are interested in improvements to server-mediated real-time reporting. This could include improvements to the Aggregation Service, or some other real-time reporting service.

### Why not use sampled `forDebuggingOnly` signals?

[Downsampled](https://github.com/WICG/turtledove/blob/main/FLEDGE.md#712-downsampling) `forDebuggingOnly` signals can be used for real-time monitoring, but due to the heavy downsampling and lengthy cooldown/lockout periods, reports are only sent from a small fraction of auctions and it can only be used very sparingly. The lockout will introduce bias as future errors for locked-out clients will go unreported, and additionally incentivize reporting only for major issues, due to the heavy cost of incurring a locked out client.

The downsampled reporting is better suited for root-cause analysis.

### Why not use the Private Aggregation API (PAA)?

We could consider extending the [Private Aggregation API](https://developers.google.com/privacy-sandbox/relevance/private-aggregation) rather than introducing a new API surface for real-time bid reporting. There were a few reasons we chose a new API:

1.   Due to the random client-side delay, ad tech time to accumulate reports and batch processing time, PAA reporting SLA may not be brought down to the real time nature of reporting for monitoring required by some ad techs. Reducing the delay is possible but would likely require sending more null reports to satisfy the privacy requirements.
1.   PAA is designed for a very large domain of keys in its underlying histogram, whereas the RAPPOR mechanism cannot readily support such large domains due to bandwidth / computation constraints.
1.   Utilizing PAA could interfere with existing PAA privacy budgets, and it isn’t clear exactly how to share budgets across central and local mechanisms.


## Limitations and future work

The API is limited in scope due to the strong privacy guarantee and use of local noise. In the future, we plan on exploring alternative architectures such as shufflers or trusted servers to improve utility without compromising on privacy or the real-time requirement. In particular, we are exploring two solutions with the following goals:

*   Decrease the amount of total noise
*   Allow for measuring larger and more fine-grained histograms
*   Allow participants to emit multiple histograms contributions per auction
*   Address the privacy concern that this proposal leaks the auction participation

The two solutions we are considering are 1) add a local shuffle and IP address proxy mechanism to the Real Time Monitoring API, or 2) replace the Real Time Monitoring API with a low-latency Private Aggregation solution.
