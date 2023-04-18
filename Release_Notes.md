> FLEDGE has been renamed to Protected Audience API. To learn more about the name change, see the [blog post](https://privacysandbox.com/intl/en_us/news/protected-audience-api-our-new-name-for-fledge)

# FLEDGE Release Notes


## Chrome M109



*   Since version 109.0.5414.16, the [`sendReports` parameter to `navigator.deprecatedURNToURL()`](https://github.com/WICG/turtledove/blob/main/Proposed_First_FLEDGE_OT_Details.md#advertisement-rendering) is respected.


## Chrome M108



*   Support reporting bid reject reason to buyers through calling `forDebuggingOnly.reportAdAuctionLoss()` API in `generateBid()` and including `${rejectReason}` in the report URL's query string.  See [FOT#1 reporting details](https://github.com/WICG/turtledove/blob/main/Proposed_First_FLEDGE_OT_Details.md#reporting) for more information.


## Chrome M107



*   Bug fixes:
    *   [The `forDebuggingOnly.reportAdAuctionLoss()` and `forDebuggingOnly.reportAdAuctionWin()` APIs were fixed to perform post-auction signal parameter replacement on URL query parameters instead of the URL path](http://crbug.com/1338233).


## Chrome M104



*   Add `.well-known` join/leave permission cache.
*   Clear pending report request queue after a certain time to prevent reports continuing for unbounded amounts of time.
*   Implement improved process model for Android (for local FLEDGE testing by developers while the FLEDGE origin trial is not supported on Android).


## Chrome M103



*   Add `chrome://tracing` support for FLEDGE auctions (including worklets).
*   [Add basic `.well-known` support](https://crbug.com/1315805) for determining if a site is allowed to join or leave interest groups for an owner.  `.well-known` ownership delegation is [documented in the explainer](https://github.com/WICG/turtledove/blob/main/FLEDGE.md#13-permission-delegation).
*   `joinAdInterestGroup()` and `leaveAdInterestGroup()` changed to return promises (fulfilled or rejected based on whether the join or leave is allowed based on the `.well-known` lookup).
*   Rate-limit report network requests, to prevent over-consuming networking resources and allow requests to be sent in parallel.
*   Add `navigator.deprecatedReplaceInURN() `function.  For more information see [Issue #286](https://github.com/WICG/turtledove/issues/286).
*   Bug fixes:
    *   [Fix error when component auction does not bid](https://bugs.chromium.org/p/chromium/issues/detail?id=1321941).


## Chrome M102



*   [Fenced Frame reporting](https://github.com/WICG/turtledove/blob/main/Fenced_Frames_Ads_Reporting.md) added. Worklets now support a `registerAdBeacon()`  function in `reportWin()` and `reportResult()` which can be used to register reporting URLs.
*   As described in the FLEDGE explainer, ads selected by a FLEDGE `runAdAuction()` call and rendered in a Fenced Frame can now [call `navigator.leaveAdInterestGroup()` without arguments](https://github.com/WICG/turtledove/blob/main/FLEDGE.md#13-permission-delegation:~:text=invoking%20navigator.leaveAdInterestGroup()-,from%20inside%20an%20ad,-that%20is%20being) to leave the interest group that triggered the ad.
*   Limits on the number of interest groups allowed to participate in auctions cause auctions to choose interest groups uniformly and randomly. They have equal priority instead of in order of decreasing expiration time. See `perBuyerGroupLimits` [in the FLEDGE explainer](https://github.com/WICG/turtledove/blob/main/FLEDGE.md#12-interest-group-attributes) for more information about interest group limits in auctions.
*   Post-auction signals:
    *   Post-auction signals `highestScoringOtherBid` and `madeHighestScoringOtherBid` added to `browser_signals` provided to reporting functions `reportWin()` and `reportResult()` as documented by [pull request #316](https://github.com/WICG/turtledove/pull/316).
    *   Post-auction signals can be included in the URLs fetched through the [`reportAdAuctionLoss()` and `reportAdAuctionWin()` APIs](https://developer.chrome.com/blog/fledge-api/#temporary-reporting): text placeholders like “${winningBid}” in the URL will be replaced with the corresponding value from the auction. For example a URL provided to `reportAdAuctionLoss()` as <code>'https://example.com/report/group=myGroup/bid=${winningBid}/otherBid=${highestScoringOtherBid}'</code> may be fetched as <code>'https://example.com/report/group=myGroup/bid=1/otherBid=2'</code> Note that due to a [bug](http://crbug.com/1338233) replacement is currently supported only in the URL path, but will be changed to only be supported in the URL query parameters when that bug is fixed. Further details are present in the [FLEDGE OT explainer](https://github.com/WICG/turtledove/blob/main/Proposed_First_FLEDGE_OT_Details.md#reporting).
*   As discussed in https://crbug.com/1311478, <code>forDebuggingOnly.reportAdAuctionLoss()</code> now sends a loss report event if an error or timeout occurs after the report URL was registered.  This can be useful for measuring how often worklets do not complete due to timeouts or throwing exceptions.
*   <code>componentAuctions</code> and <code>trustedScoringSignalsUrl</code> from auction config passed to seller worklets.
*   Add <code>biddingLogicUrl</code>, <code>biddingWasmHelperUrl</code>, <code>trustedBiddingSignalsUrl</code>, <code>trustedBiddingSignalsKeys</code>, and <code>dailyUpdateUrl</code> fields to <code>interestGroup</code> parameter passed to `generateBid()`.
*   Worklet features
    *   <code>ad</code> output field of <code>generateBid()</code> now optional
    *   [`setBid()` function](https://github.com/WICG/turtledove/issues/241) added to <code>generateBid()</code>. This function sets a default return value to be used if the worklet times out. <code>setBid()</code> can be called multiple times, with newer values overwriting previous ones. This function may be useful for bidding scripts that progressively increase.
    *   <code>reportAdAuctionWin()</code> and <code>reportAdAuctionLoss()</code> can be called multiple times within a worklet, with newer values overwriting previous ones.


## Chrome M101



*   `joinAdInterestGroup()` can specify interest group priority using the `priority` value.  Limits on the number of interest groups per bidder can be specified using the `perBuyerGroupLimits` value in the auction configuration passed to `runAdAuction()`. The default interest group priority is 0. Per-bidder limits on interest groups restrict the number of interest groups that can participate in an ad auction, choosing higher priority interest groups preferentially.
*   Update interest groups that were involved in an auction, as described in the [proposal](https://github.com/WICG/turtledove/blob/main/Proposed_First_FLEDGE_OT_Details.md#interest-group-updating)  for the first FLEDGE origin trial. Chrome collects the origins of owners for interest groups that participated in the auction, including inside component auctions, then randomizes the order and updates each one. Updates occur for both successful and failed auctions.
*   Component auctions are now supported.
*   Bug fixes:
    *   Interest group updates can change `biddingWASMHelperURL` and `adComponents` fields within the stored interest groups. 
    *   `forDebuggingOnly.reportAdAuctionLoss()` now sends a report when no bid wins and the auction runs to completion. If an auction is canceled (for example, due to a new navigation), no reports will be generated.


## Chrome M100



*   Support passing `Data-Version` from the trusted key/value server to the worklet via the `dataVersion` value as specified in the explainer.
*   Devtools Interest Group storage under the Applications panel shows interest groups as they are used. Described in the FLEDGE API [Developer Guide](https://developer.chrome.com/blog/fledge-api/#debug-fledge-worklets). 
*   Per-Buyer worklet timeout: In `AuctionConfig`, `perBuyerTimeouts` can be specified to restrict the runtime (in milliseconds) of particular buyer's `generateBid()` scripts. If no value is specified for a particular buyer, a default timeout of 50 ms will be selected. Any timeout higher than 500 ms will be clamped to 500 ms. A key of '\*' is used to change the default of unspecified buyers.  Addresses [FLEDGE issue #90](https://github.com/WICG/turtledove/issues/90).
*   Seller worklet timeout: In `AuctionConfig`, `sellerTimeouts` can be specified to restrict the runtime (in milliseconds) of the seller's `scoreAd()` scripts. If no value is specified, a default timeout of 50 ms will be selected. Any timeout higher than 500 ms will be clamped to 500 ms.
*   Full [console](https://developer.mozilla.org/en-US/docs/Web/API/console) API should now be available for worklets (rather than a small subset).
