# Feature detecting Protected Audience

As Chrome ships and experiments with various Protected Audience APIs, it can be useful for web developers to detect the presence of individual features
in users’ browsers.  This document seeks to list feature detection mechanisms for each shipped or experimented feature.

## Protected Audience
[Intent to Ship](https://groups.google.com/a/chromium.org/g/blink-dev/c/igFixT5n7Bs/m/ZNrDcQ2dDQAJ)
```
let pa = typeof navigator.joinAdInterestGroup !== 'undefined';
```
## directFromSellerSignals via headers
[Intent to Ship](https://groups.google.com/a/chromium.org/g/blink-dev/c/JpWOdoPi5Wo/m/YyTaUzkxAgAJ)
```
let dfss = false;
navigator.runAdAuction({get directFromSellerSignalsHeaderAdSlot(){dfss = true;}}).catch((e)=>{});
```
## Negative targeting
[Intent to Ship](https://groups.google.com/a/chromium.org/g/blink-dev/c/xzrWfs-BwFk/m/a90JCji_AAAJ)
```
let nt = typeof navigator.createAuctionNonce !== 'undefined';
```
## clearOriginJoinedAdInterestGroups()
[Intent to Ship](https://groups.google.com/a/chromium.org/g/blink-dev/c/IfmYsMCUoHc/m/yCddTUfgBgAJ)
```
let nt = typeof navigator.clearOriginJoinedAdInterestGroups !== 'undefined';
```
## Interest group limit changes
[Intent to Ship](https://groups.google.com/a/chromium.org/g/blink-dev/c/IfmYsMCUoHc/m/yCddTUfgBgAJ)

These limits are intended to be guardrails against exceptional behavior, and not meant to be reached under normal conditions, so there should not be a 
need to find out what the guardrails are.  In [the best practices we previously published](https://developer.chrome.com/docs/privacy-sandbox/protected-audience-api/latency/#fewer-interest-groups-bidding),
we encourage buyers to use fewer interest groups.
## kAnonStatus
[Intent to Ship](https://groups.google.com/a/chromium.org/g/blink-dev/c/IfmYsMCUoHc/m/yCddTUfgBgAJ)
```
reportWin(auctionSignals, perBuyerSignals, sellerSignals, browserSignals, directFromSellerSignals) {
  ...
  let ka = typeof browserSignals.kAnonStatus !== 'undefined';
}
```
## Bidding & Auction Services
[Intent to Experiment](https://groups.google.com/a/chromium.org/g/blink-dev/c/2bwMHd3Yz7I/m/BwMKwPP6GQAJ) (note that this is an
[Origin Trial](https://developer.chrome.com/en/docs/web-platform/origin-trials/) and as such
requires [Origin Trial tokens](https://developer.chrome.com/en/docs/web-platform/origin-trials/#iframe))
```
let ba = typeof navigator.getInterestGroupAdAuctionData !== 'undefined';
```
## Recency in generateBid()
[Intent to Ship](https://groups.google.com/a/chromium.org/g/blink-dev/c/-bQKNLit6nw/m/vPe0uSXtAAAJ)
```
generateBid(interestGroup, auctionSignals, perBuyerSignals,
    trustedBiddingSignals, browserSignals, directFromSellerSignals) {
  ...
  let rg = typeof browserSignals.recency !== 'undefined';
}
```
## Rounding bids and scores
[Intent to Ship](https://groups.google.com/a/chromium.org/g/blink-dev/c/-bQKNLit6nw/m/vPe0uSXtAAAJ)

This is a privacy protection that does not require different behavior from Protected Audience API callers and hence has no feature detection mechanism.
## Credentialed automatic beacons
[Intent to Ship](https://groups.google.com/a/chromium.org/g/blink-dev/c/rMyTWCo-f_I)

Whether automatic beacons will attach credentials cannot be detected from inside Protected Audience ads. From the perspective of the server, the request
has either credentials mode “omit” (feature disabled) or “include” (feature enabled), which affects how headers are processed in the same way as the rest
of the web platform.
## reserved.top_navigation_start/commit
This cannot be detected from the web platform. Instead, an ad auction can register both a `reserved.top_navigation_commit` and a `reserved.top_navigation`
beacon, and then the ad frame can set the same automatic beacon data for both event types. The beacon destination server can then compare
`top_navigation_commit` and `top_navigation` beacons, and filter out duplicate beacons that have the same exact data.

## Increase in limit to number of component ads
[Intent to Ship](https://groups.google.com/a/chromium.org/g/blink-dev/c/3RUQk0GCC9Q/m/wmbXOOB8AAAJ)

Inside `generateBid` one can determine the currently active limit on number of components ads as follows:
```
const maxAdComponents = browserSignals.adComponentsLimit ?
                        browserSignals.adComponentsLimit : 20;
```

From the context of a web page, the limit can also be queried as follows:
```
const maxAdComponents = navigator.protectedAudience ?
    navigator.protectedAudience.queryFeatureSupport("adComponentsLimit") : 20;
```

## Reporting timeout
[Intent to Ship](https://groups.google.com/a/chromium.org/g/blink-dev/c/ZdZXN1D-MtI/)

Inside `reportWin` one can determine its reporting timeout as follows:
```
const reportingTimeout = browserSignals.reportingTimeout ?
                        browserSignals.reportingTimeout : 50;
```

Inside `reportResult` one can determine its reporting timeout as follows:
```
const reportingTimeout = auctionConfig.reportingTimeout ?
                        auctionConfig.reportingTimeout : 50;
```

From the context of a web page, whether custom reporting timeout is enabled can be queried as follows:
```
const reportingTimeoutEnabled = navigator.protectedAudience &&
    navigator.protectedAudience.queryFeatureSupport("reportingTimeout");
```

## Returning multiple bids from generateBid()
[Intent to Ship](https://groups.google.com/a/chromium.org/g/blink-dev/c/ZdZXN1D-MtI/)

Inside `generateBid()`, if `browserSignals.multiBidLimit` exists then returning
an array of bids is supported. The value of `browserSignals.multiBidLimit`
returns the maximum numbers of bids that can be returned, which may be as low as
1.

## Component ad subsetting with targetNumAdComponents
[Intent to Ship](https://groups.google.com/a/chromium.org/g/blink-dev/c/ZdZXN1D-MtI/)

Inside `generateBid()`, if `browserSignals.multiBidLimit` exist then
the `targetNumAdComponents` and `numMandatoryAdComponents` bid fields will be
considered.

## Cross-origin trusted signals
[Intent to Ship](https://groups.google.com/a/chromium.org/g/blink-dev/c/5nvBAjmoO2g)

From context of a web page:
```
navigator.protectedAudience && navigator.protectedAudience.queryFeatureSupport(
    "permitCrossOriginTrustedSignals")
```

## Real time reporting
[Intent to Ship](https://groups.google.com/a/chromium.org/g/blink-dev/c/9_dR-BdyeWE)

From context of a web page:
```
navigator.protectedAudience && navigator.protectedAudience.queryFeatureSupport(
    "realTimeReporting")
```

## Selectable Reporting IDs

From context of a web page:
```
navigator.protectedAudience && navigator.protectedAudience.queryFeatureSupport(
    "selectableReportingIds")
```

## Getting browser-side detectable features as an object
Sometimes it's desirable to get status of all features detectable via `queryFeatureSupport` in a
forward-compatible way. Sufficiently recent versions provide this functionality via
`queryFeatureSupport('*')`, which returns a dictionary describing state of various features. Since
that functionality isn't available in older versions, backwards-compatibility polyfilling is
suggested:

```
let qfs = navigator.protectedAudience ?
    navigator.protectedAudience.queryFeatureSupport.bind(navigator.protectedAudience) : null;

let allFeatureStatus = qfs ?
  (qfs("*") || {
    adComponentsLimit: qfs("adComponentsLimit"),
    deprecatedRenderURLReplacements: qfs("deprecatedRenderURLReplacements"),
    reportingTimeout: qfs("reportingTimeout")
   }) : {}
```

An example return value would be:
```
{
  "adComponentsLimit":40,
  "deprecatedRenderURLReplacements":false,
  "permitCrossOriginTrustedSignals":true,
  "realTimeReporting":true,
  "reportingTimeout":true
}
```
