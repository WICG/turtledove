> FLEDGE has been renamed to Protected Audience API. To learn more about the name change, see the [blog post](https://privacysandbox.com/intl/en_us/news/protected-audience-api-our-new-name-for-fledge)

# FLEDGE Browser Bidding & Auction API

## Background

This document seeks to propose an API for web pages to perform FLEDGE auctions using Bidding and Auction (B&A) servers running in Trusted Execution Environments (TEE), as was announced [here](https://developer.chrome.com/blog/bidding-and-auction-services-availability/). This document seeks to document the web-exposed JavaScript API. The browser is responsible for formatting the data sent to the B&A servers using the B&A server API documented in [this explainer](https://github.com/privacysandbox/fledge-docs/blob/main/bidding_auction_services_api.md).

## Steps to perform a FLEDGE auction using B&A

#### Step 1: Get auction blob from browser

To execute an on-server FLEDGE auction, sellers begin by calling `navigator.startServerAdAuction()`:
```  
const auctionBlob = navigator.startServerAdAuction({
  // ‘seller’ works the same as for runAdAuction.
  'seller': 'https://www.example-ssp.com',
});
```
 
The returned `auctionBlob` is HPKE encrypted with an encryption header like that used in OHTTP. The encryption is done using public keys the browser fetches that correspond with private keys only shared with B&A servers running in TEEs. `auctionBlob` is a blob of compressed data derived from the interest groups stored in the browser and formatted like [the B&A API](https://github.com/privacysandbox/fledge-docs/blob/main/bidding_auction_services_api.md) dictates. This derived information will include the contents of the interest groups, the k-anonymity status of the interest groups’ ads, and information like `joinCount`, `bidCount`, and `prevWins`. The blob contains padding (e.g. exponentially bucketed total sizes) to reduce the cross-site identity leaked by its size.

#### Step 2: Send auction blob to servers

A seller’s JavaScript then sends auctionBlob to their server, perhaps by initiating a [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/fetch) using a PUT or POST method with auctionBlob attached as the request body:

<pre>
fetch('https://www.example-ssp.com/auction', {
  method: "PUT",
  <b>body: auctionBlob</b>,
  …
})
</pre>

Their server then passes the blob off to B&A servers which conduct on-server auctions. Several on-server auctions may take place, e.g. one for each ad slot on the page. Each on-server auction returns an encrypted response blob.

#### Step 3: Get response blobs to browser

These response blobs are sent back to the browser. A seller’s JavaScript can [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/fetch) them back to the browser, perhaps as the response body to the Fetch initiated in Step 2. This Fetch is initiated with a flag to prepare the browser to look for `X-fledge-auction-results` HTTP response headers:
<pre>
fetch('https://www.example-ssp.com/auction', { <b>adAuctionHeaders: true</b>, … })
</pre>
Note that `adAuctionHeaders` only works with HTTPS requests.
For each response blob sent back to the browser, the seller’s server attaches a response header containing the SHA-256 hash of the response blob:
  
```
X-fledge-auction-result: ba7816bf8f01cfea414140de5dae2223b00361a396177a9cb410ff61f20015ad
```

It should be noted that the `fetch()` request using `adAuctionHeaders` can also be used to send `auctionBlob` (e.g. in the request body) and receive the response blob (e.g. in the response body).

#### Step 4: Complete auction in browser

Now the auction can be completed by passing the response blob to runAdAuction() as part of a specially configured auction configuration:

```
const auctionResultPromise = navigator.runAdAuction({
  'seller': 'https://www.example-ssp.com',
  'serverResponse': response_blob,
});
```

The browser verifies it witnessed a Fetch request to the `seller`’s origin with `“adAuctionHeaders: true”` that included a `X-fledge-auction-results` header with hash of `response_blob`. Since the Fetch request required HTTPS which authenticates the seller’s origin, this verification authenticates that the seller produced the response blob. `runAdAuction()` then proceeds as if the auction happened on device. This specially configured auction configuration can be used for single seller auctions or as a component auction configuration for multi-seller auctions (in this case the `startServerAuction()` call must include a `top_level_seller` field that must later match the top-level seller passed to `runAdAuction()`). To facilitate parallelizing on-device and on-server auctions, the `serverResponse` could be a Promise that resolves to the blob later.

## Privacy Considerations

The blobs sent to and received from the B&A servers can contain data that could be used to re-identify the user across different web sites. To prevent this data from being used to join the user’s cross-site identities, the data is encrypted with public keys whose corresponding private keys are only shared with B&A server instances running in TEEs and running public open-source binaries known to prevent cross-site identity joins (e.g. by preventing logging or other activities which might permit such joins).

There are however side-channels that could leak a much smaller amount of cross-site identity, the first and most obvious one being the size of the encrypted blob. To minimize this leakage further we suggest above that the encrypted blob be padded. For example, we can use exponential bucketing for request blobs. Keeping the number of buckets small, for instance, 7 will lead to <3 bits of leaked entropy per call to `startServerAdAuction()`. Some [mitigation techniques for the “1-bit leak”](https://github.com/WICG/turtledove/issues/211#issuecomment-889269834) may be applicable here.  

To prevent repeated calls to `startServerAdAuction()` leaking additional cross-site identity, we’ve decided, at least in the near-term, to make repeated calls to `startServerAdAuction()` return a blob of the same size. The tradeoff being that the blob cannot be dependent on inputs to `startServerAdAuction()`. For example whereas `runAdAuction()` normally takes a `interestGroupBuyers` list dictating which buyers to include in the auction, `startServerAdAuction()`’s returned blob cannot depend on such a list and so must include all buyers that have stored interest groups on the device. This may cause larger blobs, and in turn slower network requests sending and receiving those blobs. This could in turn make the API more susceptible to abuse, e.g. fake calls to `joinAdInterestGroup()` bloating the blob size with spam interest groups.

Another way to prevent the encrypted blob’s size from being a leak is to have the browser send the blob directly to the B&A servers instead of exposing it to JavaScript at all. Requiring this today has significant downsides:

1.  This would hugely complicate the B&A server’s interactions and API, making adoption likely infeasible. The B&A API would no longer be a RESTful API as it would have to coordinate communication from both the browser and other servers (e.g. contextual auction server).
    
1.  This would also require the on-device JavaScript to determine whether to send the FLEDGE request to the B&A server, perhaps at a time before it has the results of the contextual auction which might influence the decision. Without this information the device would have to send the encrypted blob for every ad request, even in cases where the contextual call indicated it was wasteful to do so.

Exposing size of the blob is a temporary leak that we hope to mitigate in the future:

1.  As TEEs become more prevalent and adtechs gain experience operating and deploying them, we can reconsider whether the server that the browser sends the blob to could in fact be a trusted one operating in a TEE.

1.  The size of the encrypted blob may shrink considerably, where padding may become a more effective privacy prevention (e.g. imagine fixed size blobs) and may introduce little overhead. [We recently announced](https://github.com/WICG/turtledove/issues/361#issuecomment-1430069343) changes to the interest group update requirements that should facilitate significantly fewer interest groups. Having many fewer interest groups can greatly reduce the size of the encrypted blob. These same changes allow for more information to be stored in the real-time trusted bidding signals server and simply indexed with a small identifier kept in the interest group, again shrinking the interest groups.
