---
coding: utf-8

title: Trusted Auction Services
docname: draft-ietf-trusted-auction-services
category: std

area: TODO
workgroup: TODO
keyword: Internet-Draft

ipr: trust200902

stand_alone: yes
pi: [toc, sortrefs, symrefs]

author:
 -
    name: Daniel Kocoj
    organization: Google
    email: dankocoj@google.com
 -
    name: Benjamin "Russ" Hamilton
    organization: Google
    email: behamilton@google.com

normative:
  CBOR: RFC8949
  CDDL: RFC8610
  OHTTP: RFC9458

informative:

--- abstract

Trusted Auction Services provide a way for advertising auctions to execute in a
remote trusted execution environment while preserving user privacy.

--- middle

Introduction        {#problems}
============

Today, real-time bidding and ad auctions are executed on servers that may not
provide technical guarantees of security. Some users have concerns about how
their data is handled to generate relevant ads and in how that data is shared.
Protected Audience API ([Android](https://developer.android.com/design-for-safety/privacy-sandbox/fledge),
[Chrome](https://developer.chrome.com/docs/privacy-sandbox/fledge/)) provides
ways to preserve privacy and limit third-party data sharing by serving
personalized ads based on previous mobile app or web engagement.

For all platforms, Protected Audience may require [real-time services](https://github.com/privacysandbox/fledge-docs/blob/main/trusted_services_overview.md).
In the initial [proposal by Chrome](https://github.com/WICG/turtledove/blob/main/FLEDGE.md),
bidding and auction for Protected Audience ad demand is executed locally. This
can demand computation requirements that may be impractical to execute on
devices with limited processing power, or may be too slow to render ads due to
network latency.

The Trusted Auction Services proposal outlines a way to allow Protected Audience
computation to take place on cloud servers in a trusted execution environment,
rather than running locally on a user's device. Moving computations to cloud in
a [Trusted Execution Environment (TEE)](https://github.com/privacysandbox/fledge-docs/blob/main/trusted_services_overview.md#trusted-execution-environment)
have the following benefits:

*   Scalable auctions.
  *   A scalable ad auction may include several buyers and sellers and that
      can demand more compute resources and network bandwidth.
*   System health of the user's device.
  *   Ensure better system health of the user's device by freeing up
      computational cycles and network bandwidth.
*   Better latency of ad auctions.
  *   Server to server communication on the cloud is faster than multiple device to server calls.
  *   Adtech code can execute faster on servers with higher computing power compared to a device.
*   Servers have better processing power.
  *   Adtechs can run more compute intensive workloads on a server compared to a device for better utility.
*   [trusted execution environment](https://github.com/privacysandbox/fledge-docs/blob/main/trusted_services_overview.md#trusted-execution-environment)
    can protect confidentiality of adtech code and signals.

Standardized protocols for interacting with Bidding and Auction Services are
essential to creating a diverse and healthy ecosystem for such services.

This document does not describe distribution of private keys to trusted auction
services.

Use Cases    {#use-cases}
---------

Terminology          {#Terminology}
-----------

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL
NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED",
"MAY", and "OPTIONAL" in this document are to be interpreted as
described in BCP 14 {{!RFC2119}} {{!RFC8174}} when, and only when, they
appear in all capitals, as shown here.

# Formats   {#format}

## Browser to Trusted Auction Server Request Message {#request}
The message is encrypted using HPKE with the encapsulation performed according
to {{OHTTP}}. Since we are repurposing the OHTTP encapsulation mechanism, we
define the media type "message/auction request" which is used for encryption.

The plaintext message has the following framing:

|Byte    | 0       | 0           | 1 to 4 | 5 to Size+4     | Size+5 to end |
|--------|---------|-------------|--------|-----------------|---------------|
|Bits    | 7-5     | 4-0         | *      | *               |      *        |
|--------|---------|-------------|--------|-----------------|---------------|
|Contents| Version | Compression | Size   | Request Payload | Padding       |

where the the first 3 bits of the frame header specify the payload version and
the following 5 bits specify the compression. The format described in this
document corresponds to version 0.

Messages SHOULD be zero padded up to one of the following bin sizes, where kB is
understood to mean 1024 bytes: 0kB, 5kB, 10kB, 20kB, 30kB, 40kB, 55kB. Messages
SHALL NOT be be larger than 55kB. An implementation may need to remove some
data from the payload to fit inside the largest bucket.

The compression method is defined as:

| Compression | Description   |
|:-----------:|:--------------|
|      0      | No Compression|
|      1      | Brotli        |
|      2      | GZIP          |
|    3-31     | Reserved      |

The request payload contains {{CBOR}} with the following format (described in {{CDDL}}):

~~~~~ cddl
request = {
  version: int,

  publisher: origin,

  interestGroups: {
    * origin => bstr
    ; CBOR encoded list of interest groups compressed
    ; using the method described in `compression`.
  },

  generationId: uuid,

  ? enableDebugReporting: bool
}
origin = tstr .regexp "https://([^/:](:[0-9]+)?/"
uuid = tstr .regexp "[a-fA-F0-9]{8}-[a-fA-F0-9]{4}-[a-fA-F0-9]{4}-[a-fA-F0-9]{4}-[a-fA-F0-9]{12}"
~~~~~

Where the values in the `interestGroups` field correspond to compressed {{CBOR}}
messages with the following format:

~~~~~ cddl
interestGroups = [ * interestGroup ]
interestGroup = {
  name: tstr,
  ? biddingSignalsKeys: [* tstr],
  ? userBiddingSignals: json,
  ? ads: [* adRenderId],
  ? components: [* adRenderId],
  ? browserSignals: {
    ? joinCount: int,
    ; Number of times the group was joined in the last 30
    ; days.

    ? bidCount: int,
    ; Number of times the group bid in an auction in the
    ; last 30 days.

    ? prevWins: [* [int, adRenderId]],
    ; Tuple of time-ad pairs for a
    ; previous win for this interest group that occurred
    ; in the last 30 days.
    ; The time is specified in seconds before the
    ; containing auctionBlob was requested.

    ? recencyMs: int
    ; The most recent join time for this group expressed
    ; in milli seconds before the containing auctionBlob
    ; was requested. This field will be used by newer client
    ; versions. For older devices, the precison will be in seconds.
    ; If recencyMs is present, this value will be used to offer
    ; higher precision. If not, recency will be used. Only
    ; one of the recency or recencyMs is expected to present in
    ; the request.
  }
}
adRenderId = tstr
json = tstr ; JSON encoded data
~~~~~

## Trusted Auction Server To Browser Response Message {#response}

The response message is encrypted using HPKE with the encapsulation performed
according to {{OHTTP}} as the response to the request message. Since we are
repurposing the OHTTP encapsulation mechanism, we define the media type
"message/auction response" which is used for encryption.

The message framing is as in {#request}, but the entire response payload is
compressed with Compression. The Server shall zero pad the response (TODO).

The response has the following format:

~~~~~cddl
AuctionResponse = {
  adRenderURL: tstr,
  ; The ad that will be rendered

  ? components: [* tstr],
  ; List of render URLs for
  ; component ads

  ? interestGroupName: tstr,
  ; Name of the InterestGroup

  ? interestGroupOwner: tstr,
  ; Origin of the Buyer who owns
  ; the interest group

  ? biddingGroups: {
    * origin => [* int]
    ; Indices of interest groups
    ; in the original request for
    ; this owner that submitted a bid
  },

  ? score: float,
  ; Score of the ad determined
  ; during the auction

  ? bid: float,
  ; Bid price corresponding to an ad

  ? bidCurrency: tstr,
  ; Optional currency of the bid

  ? buyerReportingId: tstr,
  ; Optional BuyerReportingId of
  ; the winning Ad

  ? buyerAndSellerReportingId: tstr,
  ; Optional
  ; BuyerAndSellerReportingId of
  ; the winning Ad

  ? isChaff: bool,
  ; Boolean to indicate that there
  ; is no remarketing winner

  ? winReportingUrls: winReportingUrlsDef,

  ? error: errorDef,

  ? adMetadata: json,
  ; Arbitrary metadata to pass
  ; to the top-level seller

  ? topLevelSeller: origin,
  ; Optional name/domain for
  ; top-level seller in case this
  ; is a component auction

  ? kAnonWinnerJoinCandidates: KAnonJoinCandidate,

  ? kAnonWinnerPositionalIndex: int,
  ; Positional index of the
  ; k-anon winner

  ? kAnonGhostWinners: [* KAnonGhostWinner]
}

origin = tstr .regexp "https://([^/:](:[0-9]+)?/"
json = tstr ; JSON encoded data
; Defines the structure of an error object
errorDef = {
  code: int,

  ;
  message: tstr

  ;
}

; Defines the structure for reporting URLs
reportingUrlsDef = {
  ? reportingUrl: tstr,

  ;
  ? interactionReportingUrls: { * tstr => tstr }

  ;
}

; Defines the structure for win reporting URLs
winReportingUrlsDef = {
  ? buyerReportingUrls: reportingUrlsDef,

  ;
  ? componentSellerReportingUrls: reportingUrlsDef,

  ;
  ? topLevelSellerReportingUrls: reportingUrlsDef

  ;
}

; Join candidates for K-Anonymity
KAnonJoinCandidate = {
  adRenderUrlHash: tstr,
  ; Protected Audience: SHA-256 hash
  ; of the tuple: render_url, interest group
  ; owner, reportWin() UDF endpoint.
  ; Protected App Signals: SHA-256 hash
  ; of render_url, reportWin UDF endpoint

  ? adComponentRenderUrlsHash: [* tstr],
  ; Protected Audience: SHA-256 hash
  ; of an ad_component_render_url
  ; Note: There is a maximum limit of 40 ad
  ; component render urls per render url.
  ; Note: Currently component ads are not in
  ; scope of Protected App Signals for Android

  reportingIdHash: tstr
  ; Protected Audience: SHA-256 hash
  ; should include IG owner, ad render url,
  ; reportWin() UDF url and one of the
  ; following:
  ; - If buyerAndSellerReportingId exists
  ; - Else if buyerReportingId exists
  ; - Else IG name should be included
  ; Note: An adtech can use either
  ; buyerReportingId or
  ; buyerAndSellerReportingId
  ; Note: Currently reporting Ids are not in
  ; scope of Protected Audience for Android
  ; and Protected App Signal for Android.
}

; In case of multiseller auction, the associated data for the
; ghost winner will be returned so that the ghost winning bid can
; be scored during top level auction
GhostWinnerForTopLevelAuction = {
  adRenderUrl: tstr,
  ; The ad render url corresponding
  ; to ghost winner

  ? adComponentRenderUrls: [* tstr],
  ; Render URLs for ads which are
  ; components of the main ghost winning ad

  modifiedBid: float,
  ; Modified bid price corresponding
  ; to ghost winning bid

  ? bidCurrency: tstr,
  ; Optional. Indicates the currency
  ; used for the bid price

  ? adMetadata: json,
  ; Arbitrary metadata associated
  ; with ghost winner to pass to the
  ; top-level seller

  buyerAndSellerReportingId: tstr
  ; BuyerAndSellerReportingId of
  ; the ghost winning Ad
}

; Private aggregation signals for the ghost winner
; Single seller auctions: This would correspond to a ghost
; winner if available
GhostWinnerPrivateAggregationSignals = {
  bucket: bytes,
  ; 128 bit integer in the form
  ; of bytestring

  value: int
  ; Note: Event type is "reserved.loss" and bid rejection
  ; reason is 8 when K-Anonymity threshold is not met
}

; Data for the ghost winner sent back to the client
; This should also include key-hashes corresponding to the
; ghost winning ad
; Refer https://wicg.github.io/turtledove/#k-anonymity
KAnonGhostWinner = {
  kAnonJoinCandidates: KAnonJoinCandidate,
  ; Join candidates for the
  ; K-Anon ghost winner

  ? interestGroupIndex: int,
  ; Interest group index
  ; corresponding to buyer / DSP
  ; that generated the ghost
  ; winning bid
  ; Note: This is only relevant in
  ; case of Protected Audience

  owner: tstr,
  ; Origin (Chrome) and domain
  ; (Android) of the buyer / DSP
  ; who owns the ghost winner

 ? ghostWinnerPrivateAggregationSignals:
 GhostWinnerPrivateAggregationSignals,

  ? ghostWinnerForTopLevelAuction: GhostWinnerForTopLevelAuction
}
~~~~~

# Security Considerations

TODO

# IANA Considerations

TODO

--- back

# Acknowledgments
{:numbered="false"}

TODO
