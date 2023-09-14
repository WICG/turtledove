> FLEDGE has been renamed to Protected Audience API. To learn more about the name change, see the [blog post](https://privacysandbox.com/intl/en_us/news/protected-audience-api-our-new-name-for-fledge)

# FLEDGE Browser Bidding & Auction API

## Background

This document seeks to propose an API for web pages to perform FLEDGE auctions using Bidding and Auction (B&A) servers running in Trusted Execution Environments (TEE), as was announced [here](https://developer.chrome.com/blog/bidding-and-auction-services-availability/). This document seeks to document the web-exposed JavaScript API. The browser is responsible for formatting the data sent to the B&A servers using the B&A server API documented in [this explainer](https://github.com/privacysandbox/fledge-docs/blob/main/bidding_auction_services_api.md).

## Steps to perform a FLEDGE auction using B&A

### Step 1: Get auction blob from browser

To execute an on-server FLEDGE auction, sellers begin by calling `navigator.getInterestGroupAdAuctionData()` with returns a `Promise<AdAuctionData>`:

```javascript
const auctionBlob = navigator.getInterestGroupAdAuctionData({
  // ‘seller’ works the same as for runAdAuction.
  'seller': 'https://www.example-ssp.com',
});
```
The returned `auctionBlob` is a Promise that will resolve to an `AdAuctionData` object. This object contains `requestId` and `request` fields.
The `requestId` contains a UUID that needs to be presented to `runAdAuction()` along with the response.
The `request` field is a `Uint8Array` containing the information needed for the [ProtectedAudienceInput](https://github.com/privacysandbox/fledge-docs/blob/main/bidding_auction_services_api.md#protectedaudienceinput) in a `SelectAd` B&A call,
encrypted using HPKE with an encryption header like that used in [OHTTP](https://www.ietf.org/archive/id/draft-thomson-http-oblivious-01.html).
The encryption is done using public keys the browser fetches from a trusted endpoint that correspond with
private keys only shared with B&A servers running in TEEs. The `request` is derived from the interest groups
stored in the browser to be provided to [the B&A API](https://github.com/privacysandbox/fledge-docs/blob/main/bidding_auction_services_api.md).
The request includes the contents of the interest groups, and information that the B&A server can pass
unmodified in `browserSignals` for bidding like `joinCount`, `bidCount`, and `prevWins`.
More details on the request format are given in [the appendix](#request-blob-format).

### Step 2: Send auction blob to servers

A seller’s JavaScript then sends auctionBlob to their server, perhaps by initiating a [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/fetch) using a PUT or POST method with auctionBlob attached as the request body:

<pre>
fetch('https://www.example-ssp.com/auction', {
  method: "PUT",
  <b>body: auctionBlob</b>,
  …
})
</pre>

Their server then passes the blob off to B&A servers which conduct on-server auctions. Several on-server auctions may take place, e.g. one for each ad slot on the page. Each on-server auction returns an encrypted response blob in the format specified in the [appendix](#response-blob-format).

### Step 3: Get response blobs to browser

These response blobs are sent back to the browser. A seller’s JavaScript can [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/fetch) them back to the browser, perhaps as the response body to the Fetch initiated in Step 2. This Fetch is initiated with a flag to prepare the browser to look for `Ad-Auction-Result` HTTP response headers:
<pre>
fetch('https://www.example-ssp.com/auction', { <b>adAuctionHeaders: true</b>, … })
</pre>
Note that `adAuctionHeaders` only works with HTTPS requests.

For each response blob sent back to the browser, the seller’s server attaches a response header containing the base64url encoded (RFC 4648 section 5) SHA-256 hash of the response blob:

```
Ad-Auction-Result: ungWv48Bz-pBQUDeXa4iI7ADYaOWF3qctBD_YfIAFa0=
```

Multiple hashes can be included in a response by either repeating the
header or by specifying multiple hashes separated by a `,` character. So
```
Ad-Auction-Result: ungWv48Bz-pBQUDeXa4iI7ADYaOWF3qctBD_YfIAFa0=,9UTB-u-WshX66Xqz5DNCpEK9z-x5oCS5SXvgyeoRB1k=
```
is equivalent to
```
Ad-Auction-Result: ungWv48Bz-pBQUDeXa4iI7ADYaOWF3qctBD_YfIAFa0=
Ad-Auction-Result: 9UTB-u-WshX66Xqz5DNCpEK9z-x5oCS5SXvgyeoRB1k=
```
and both versions should be accepted.

It should be noted that the `fetch()` request using `adAuctionHeaders` can also be used to send `auctionBlob` (e.g. in the request body) and receive the response blob (e.g. in the response body).

### Step 4: Complete auction in browser

Now the auction can be completed by passing the response blob to `runAdAuction()` as part of a specially configured auction configuration:

```javascript
const auctionResultPromise = navigator.runAdAuction({
  'seller': 'https://www.example-ssp.com',
  'interestGroupBuyers': ['https://www.example-dsp.com'],
  'requestId': requestId,
  'serverResponse': response_blob,
});
```

The browser verifies it witnessed a Fetch request to the `seller`’s origin with `"adAuctionHeaders: true"` that included an `Ad-Auction-Result` header with hash of `response_blob`. It also checks that the `response_blob` corresponds to the `navigator.getInterestGroupAdAuctionData()` request with the provided `requestId` that was sent from the same frame. Since the Fetch request required HTTPS which authenticates the seller’s origin, this verification authenticates that the seller produced the response blob. `runAdAuction()` then proceeds as if the auction happened on device. This specially configured auction configuration can be used for single seller auctions or as a component auction configuration for multi-seller auctions (in this case the `getInterestGroupAdAuctionData()` call must include a `top_level_seller` field that must later match the top-level seller passed to `runAdAuction()`). To facilitate parallelizing on-device and on-server auctions, the `serverResponse` could be a Promise that resolves to a `Uint8Array` containing the blob later.

## Privacy Considerations

The blobs sent to and received from the B&A servers can contain data that could be used to re-identify the user across different web sites. To prevent this data from being used to join the user’s cross-site identities, the data is encrypted with public keys whose corresponding private keys are only shared with B&A server instances running in TEEs and running public open-source binaries known to prevent cross-site identity joins (e.g. by preventing logging or other activities which might permit such joins).

There are however side-channels that could leak a much smaller amount of cross-site identity, the first and most obvious one being the size of the encrypted blob.
To minimize this leakage further we suggest above that the encrypted blob be padded. For example, we can use exponential bucketing for request blobs.
Keeping the number of buckets small, for instance, 7 will lead to <3 bits of leaked entropy per call to `getInterestGroupAdAuctionData()`.
Some [mitigation techniques for the “1-bit leak”](https://github.com/WICG/turtledove/issues/211#issuecomment-889269834) may be applicable here.

To prevent repeated calls to `getInterestGroupAdAuctionData()` leaking additional cross-site identity, we’ve decided, at least in the near-term, to make repeated calls to `getInterestGroupAdAuctionData()` return a blob of the same size. The tradeoff being that the blob cannot be dependent on inputs to `getInterestGroupAdAuctionData()`. For example whereas `runAdAuction()` normally takes a `interestGroupBuyers` list dictating which buyers to include in the auction, `getInterestGroupAdAuctionData()`’s returned blob cannot depend on such a list and so must include all buyers that have stored interest groups on the device. This may cause larger blobs, and in turn slower network requests sending and receiving those blobs. This could in turn make the API more susceptible to abuse, e.g. fake calls to `joinAdInterestGroup()` bloating the blob size with spam interest groups.

Another way to prevent the encrypted blob’s size from being a leak is to have the browser send the blob directly to the B&A servers instead of exposing it to JavaScript at all. Requiring this today has significant downsides:

1.  This would hugely complicate the B&A server’s interactions and API, making adoption likely infeasible. The B&A API would no longer be a RESTful API as it would have to coordinate communication from both the browser and other servers (e.g. contextual auction server).

1.  This would also require the on-device JavaScript to determine whether to send the FLEDGE request to the B&A server, perhaps at a time before it has the results of the contextual auction which might influence the decision. Without this information the device would have to send the encrypted blob for every ad request, even in cases where the contextual call indicated it was wasteful to do so.

Exposing size of the blob is a temporary leak that we hope to mitigate in the future:

1.  As TEEs become more prevalent and adtechs gain experience operating and deploying them, we can reconsider whether the server that the browser sends the blob to could in fact be a trusted one operating in a TEE.

1.  The size of the encrypted blob may shrink considerably, where padding may become a more effective privacy prevention (e.g. imagine fixed size blobs) and may introduce little overhead. [We recently announced](https://github.com/WICG/turtledove/issues/361#issuecomment-1430069343) changes to the interest group update requirements that should facilitate significantly fewer interest groups. Having many fewer interest groups can greatly reduce the size of the encrypted blob. These same changes allow for more information to be stored in the real-time trusted bidding signals server and simply indexed with a small identifier kept in the interest group, again shrinking the interest groups.

# Appendix

## Request Blob Format

The schema below defines the format of the blob mentioned [earlier](#step-1-get-auction-blob-from-browser).
The blob is padded to either one of the 7 allowed sizes to reduce the cross-site identity leaked by its size. In the
event the `request` cannot be created the resulting array will have zero length.

Prior to encryption the `request` is encoded as [CBOR](https://www.rfc-editor.org/rfc/rfc8949.html) with the following schema (specified using [JSON Schema](https://datatracker.ietf.org/doc/html/draft-bhutton-json-schema-01)):

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "/BuyerInput/AuctionBlob",
  "type": "object",
  "properties": {
    "version": {
      "type": "number",
      "description": "Schema version used by this CBOR message. The schema version represented here is version 0." },
    "compression": {
      "enum": ["gzip", "brotli", "none"],
      "default": "gzip",
      "description": "Compression format to use when compressing the elements of the interestGroups field"
    },
    "publisher": { "type": "string" },
    "generationId": {
      "type": "string",
      "format": "uuid",
      "description": "Random identifier produced by the browser that the server may use for rate limiting"
    },
    "enableDebugReporting": {
       "type": "boolean",
       "default": "false",
       "description": "Controls whether the server should allow the forDebuggingOnly.reportAdAuctionLoss() and forDebuggingOnly.reportAdAuctionWin() APIs"
    },
    "interestGroups": {
      "patternProperties": {
        "^https://": {
            "type": "string",
            "description": "CBOR encoded list of interest groups compressed using the method described in `compression`."
        }
      },
      "additionalProperties": false
    }
  },
  "required": [
    "version", "interestGroups", "publisher", "generationId"
  ]
}
```

where each compressed interest group item in `interestGroups` is an object where the key is the owner and value is a CBOR encoded list of the interest groups for that owner, compressed with the algorithm specified in `compression`.

The schema for the CBOR encoding of the interest group (specified using [JSON Schema](https://datatracker.ietf.org/doc/html/draft-bhutton-json-schema-01)) is:

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "/BuyerInput/AuctionBlob/InterestGroup",
  "$defs": {
    "adRenderId": {
      "type": "string",
      "description": "A short identifier (up to 12 characters long) that can be used to reconstruct the ad object based on server-side information. Sent instead of the ad object."
    }
  },
  "type": "object",
  "required": [ "name" ],
  "properties": {
    "name": {"type": "string"},
    "biddingSignalsKeys": {
      "type": "array",
      "items": {"type": "string", "uniqueItems": true}
    },
    "userBiddingSignals": {"type": "string"},
    "ads": {
      "type": "array",
      "items": {
       "$ref": "#/$defs/adRenderId"
      }
    },
    "components": {
      "type": "array",
      "items": {
        "$ref": "#/$defs/adRenderId"
      }
    },
    "browserSignals": {
      "type": "object",
      "properties": {
        "joinCount": {
          "type": "number",
          "description": "Number of times the group was joined in the last 30 days."
        },
        "bidCount": {
          "type": "number",
          "description": "Number of times the group bid in an auction in the last 30 days."
        },
        "recency": {
          "type": "number",
          "description": "The most recent join time for this group expressed in seconds before the containing auctionBlob was requested."
        },
        "prevWins": {
          "type": "array",
          "items": {
            "type": "array",
            "description":
              "Tuple of time-ad pairs for a previous win for this interest group that occurred in the last 30 days. The time is specified in seconds before the containing auctionBlob was requested.",
            "items": false,
            "prefixItems": [
              {
                "type": "number"
              },
              { "$ref": "#/$defs/adRenderId"}
            ]
          }
        }
      }
    }
  }
}
```

This roughly matches the specification of the interest group in [the B&A API](https://github.com/privacysandbox/bidding-auction-servers/blob/4a7accd09a7dabf891b5953e5cdbb35d038c83c6/api/bidding_auction_servers.proto#L96C26-L96C26).

The request is framed with a 5 byte header, where the first byte is the value 0x02 and the following 4 bytes are the length of the request message in network byte order.
Then the request is zero padded to a set of pre-configured lengths (TBD).

### Example

The JSON equivalent of an example `auctionBlob` would look like this:

```json
{
  "version": 0,
  "publisher": "https://foo.com",
  "generationId": "11111111-2222-3333-4444-555555555555",
  "interestGroups": {
    "https://owner1.com": "<bytes>",
    "https://owner2.com": "<bytes>"
  }
}
```

Where `<bytes>` are filled in with gzip compressed CBOR encoded list of interest groups for each owner.

The JSON equivalent of the interest group would look like the following example:
```json
{
  "name": "cars",
  "biddingSignalsKeys": ["key1", "key2"],
  "userBiddingSignals": "signals",
  "ads": [
    "adRenderId",
    "adRenderId2"
  ],
  "components": [],
  "browserSignals": {
    "joinCount": 2,
    "bidCount": 0,
    "recency": 500,
    "prevWins": [
        [2, "adRenderId"],
        [3, "adRenderId"]
      ]
  }
}
```

## Response Blob Format

The response blob from a B&A auction contains an HPKE encrypted message containing the information from [AuctionResult](https://github.com/privacysandbox/bidding-auction-servers/blob/main/api/bidding_auction_servers.proto#L193).
This response has an encryption header like that
used in OHTTP and serves as the response for the encryption context started by the `auctionBlob` from `navigator.getInterestGroupAdAuctionData`.
The response contains a framing header like the request and contains a blob of compressed data, using the same schema version and same compression algorithm as specified in the `auctionBlob`.
The response needs to be padded to a set of sizes to limit the amount of information leaking from the auction.

Prior to compression and encryption, the AuctionResult is encoded as CBOR with the following schema (specified using [JSON Schema](https://datatracker.ietf.org/doc/html/draft-bhutton-json-schema-01)):

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "/SellerFrontEnd/AuctionResponse",
  "type": "object",
  "$defs": {
    "errorDef": {
      "type": "object",
      "properties": {
        "code": {
          "description": "HTTP status code for the request.",
          "type": "number"
        },
        "message": {
          "description": "Human readable error message.",
          "type": "string"
        }
      },
      "additionalProperties": false
    },
    "reportingUrlsDef": {
      "type": "object",
      "properties": {
        "reportingUrl": {
          "type": "string",
          "format": "uri",
          "description": "Reporting URL passed from sendReportTo in the corresponding reporting worklet."
        },
        "interactionReportingUrls": {
          "description": "Map from events to beacon URLs for Fenced Frame Ads Reporting created by the corresponding reporting worklet. See https://github.com/WICG/turtledove/blob/main/Fenced_Frames_Ads_Reporting.md",
          "additionalProperties": {
            "type": "string",
            "format": "uri"
          }
        }
      }
    },
    "winReportingUrlsDef": {
      "type": "object",
      "buyerReportingUrls": { "$ref": "#/$defs/reportingUrlsDef" },
      "componentSellerReportingUrls": { "$ref": "#/$defs/reportingUrlsDef" },
      "topLevelSellerReportingUrls": { "$ref": "#/$defs/reportingUrlsDef" }
    }
  },
  "properties": {
    "adRenderURL": {
      "type": "string",
      "format": "uri",
      "description": "The ad that will be rendered on the end user's device."
    },
    "components": {
      "type": "array",
      "description": "List of render URLs for component ads to be displayed as part of this ad",
      "items": {
        "format": "uri",
        "type": "string"
      }
    },
    "interestGroupName": {
      "type": "string",
      "description": "Name of the InterestGroup (Custom Audience), the remarketing ad belongs to."
    },
    "interestGroupOwner": {
      "type": "string",
      "format": "uri",
      "description": "Origin of the Buyer who owns the interest group that includes the ad."
    },
    "biddingGroups": {
      "type": "object",
      "description": "Map from interest group owner to a list of interest groups that bid in this auction",
      "patternProperties": {
        "^https://": {
          "type": "array",
          "items": {
            "type": "number",
            "description": "Indices of interest groups in the original request for this owner that submitted a bid for this auction. This is used to update the bidCount for these interest groups. Note that we send the index instead of the name because that allows us to avoid compressing separately."
          }
        }
      },
      "additionalProperties": false
    },
    "score": {
      "type": "number",
      "description": "Score of the ad determined during the auction. Any value that is zero or negative indicates that the ad cannot win the auction. The winner of the auction would be the ad that was given the highest score. The output from ScoreAd() script is desirability that implies score for an ad."
    },
    "bid": {
      "type": "number",
      "description": "Bid price corresponding to an ad."
    },
    "isChaff": {
      "type": "boolean",
      "description": "Boolean to indicate that there is no remarketing winner from the auction. AuctionResult may be ignored by the client (after decryption) if this is set to true."
    },
    "winReportingUrls": { "$ref": "#/$defs/winReportingUrlsDef" },
    "error": { "$ref": "#/$defs/errorDef" }
  }
}
```
