# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Dec 4, 2024 

(The past several meeting times were cancelled due to no agenda and US Thanksgiving)

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!



1. Michael Kleber (Google Privacy Sandbox)
2. Luckey Harpley (Remerge)
3. Brian May (Unaffiliated)
4. David Dabbs (Epsilon)
5. Youssef Bourouphael (Google Privacy Sandbox)
6. Orr Bernstein (Google Privacy Sandbox)
7. Sven May (Google Privacy Sandbox)
8. Jacob Goldman (Google Ad Manager)
9. Peiwen Hu (Google Privacy Sandbox)
10. Sid Sahoo (Google Privacy Sandbox)
11. Garrett McGrath (Magnite)
12. Paul Jensen (Google Privacy Sandbox)
13. Matt Kendall (Index Exchange)
14. Brandon Maslen (Microsoft Edge)
15. Russ Hamilton (Google Privacy Sandbox)
16. Don Marti (Raptive)
17. Kevin Lee (Google Privacy Sandbox)
18. Arthur Coleman (ThinkMedium)
19. Tim Taylor (Flashtalking)
20. Pooja Muhuri (Google Privacy Sandbox)
21. Matt Davies (Bidswitch | Criteo) 
22. Tamara Yaeger (Bidswitch | Criteo) 
23. Laurentiu Badea (OpenX)
24. Kenneth Kharma (OpenX)
25. Victor Pena (Google Privacy Sandbox)
26. Shivani Sharma (Google Privacy Sandbox)
27. Aditya Agarwal (Media.Net)
28. Yanush Piskevich(Microsoft Ads)
29. Nour Nabil (Google Privacy Sandbox)
30. Suresh Chahal (MSFT)
31. Risako Hamano (LY Corp)
32. David Tam (paapi.ai)
33. Ivan Staritskii (BidSwitch | Criteo)
34. Abishai Gray (Google Privacy Sandbox)
35. Harshad Mane (PubMatic)
36. Koji Ota (CyberAgent)


## Note taker: Shivani Sharma


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here (meetings with no items may be canceled)



*   [Brandon Maslen] Different server behaviors design for B&A (https://github.com/WICG/turtledove/issues/1346) if Brandon is on the call
*   [Aditya Agarwal] Implementation queries around Pa-Api Video Ads
*   [Matt Davies] Billable impression event for Privacy Sandbox with calls to third party on reporting
    *   https://github.com/WICG/turtledove/issues/1220
*   [Charlie Harrison] Small change to proposed PMT API surface (https://github.com/WICG/turtledove/pull/1359)


# Announcements

Privacy Sandbox folks are holding every-second-Wednesday meetings in the hour after this meeting to discuss the trusted server elements of Protected Audience work. For more information see https://github.com/WICG/protected-auction-services-discussion/issues/27.

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!


# Notes 


## [Brandon Maslen] Different server behaviors design for B&A (https://github.com/WICG/turtledove/issues/1346) 



*   Brandon: MS for ad selection API added an extra platform header when there is a server B&A call. It says MS edge/ Chrome/ Firefox + the minimum version number of the API that the browser blob is compatible with.
*   Michael: Like to be clear on what the intention of the information is. Could be communicated as a header or any other way can be discussed separately
*   Brandon: API or browser name. From the server side there are 2 fundamentally different server platforms, Chrome vs Edge. Seller needs to look at the request and know to which server it should send it. Implicit API contract b/w browser and SFE , right now there’s a protobuf. As long as they are both up to date. If the browser does a breaking change, it could send a request that will only work with a new enough server.  Front end can check server version should be at least a given min version so server can decide to process it or not, rather than fail.
*   Michael: Seems to me that there is already a mechanism for this. What server can or cannot handle the encrypted blob returned by the JS API. Caller of the API tells the API on device the encryption key which should be used. The choice of the key reflects what server the browser is willing to talk to. If there is a change in the server binary then there will need to be a new hash and the public/private key pair will change accordingly. So this is not a choice that needs to be made by the browser. Only choice browser makes is: when the API caller picks a particular public key, the browser can agree or say no I won't talk to that server. Negotiation already happens at the time of the JS call.
*   Russ: fetch call is incapable of knowing that what blob is attached.
*   Michael: That's a second issue, a problem with the HTTP header suggestion: if there are 2 calls, one for server X and Y, browser doesn’t know how to correlate that to a given fetch request.
*   Brandon: little nuance. Different server versions here are PA (Chrome) vs Ad selection (Edge) and not GCP vs AWS etc. encrypted blob can say GCP, Azure key and can support version 1 to 5 even though the browser only has for version, say 4
*   Russ: is the concern there will be a version of the server that doesn’t support a version of chrome. There is a version number inside the payload but haven’t used it since we’ve been making backward/forward compatible changes
*   Brandon: when we remove fields from schema, we need at least this version of B&A
*   Russ: we know that the coordinator enforces a build timeline which 
*   Brandon: preference to explicitly codify that for sellers rather than relying on the coordinator
*   Russ: so far not removed fields/ changed the fields. Gets complicated. Better to do feature detection because a single version number might not do
*   Brandon: simpler for the platform. What is the min version of B&A that supports that schema. 
*   Russ: version number in the payload is for that purpose. Server actually parses the CBOR to protobuf
*   Paul: should it be at request generation time. For TKV it is in the IG. For B&A, ad selection , getIGAdAuctionData seems like the right place. We could attach information to it in the clear. Will increase size of the data and may be redundant. Like doing it at the request time so it is independent of how it is received by the server. Http headers have a higher bar for adding harder to get rid of, size implications, not as flexible as JS
*   Brian May: clarifying the information about client and server capabilities. Additional information that could be used for other purposes. Server can provide something to the browser
*   Russ: like an additional arg to the JS API to specify the version.
*   Michael: version negotiation is the issue. Brandon’s point: server can give it to multiple backends to hand it to. Browser is not in that position. Cryptographic keys is a version of what Brian suggested. While Brandon’s ask is for a version number so server can choose diff backends. Letting a choice happen at the server is a right answer here. Seems like the question if there is already present in the encrypted blob, is there any harm in also sending it in clear text. I don’t see a problem with that, seems like it can be derived from the UA string/release number so doesn’t look like extra info than UA-CH. Don’t see a downside to adding it to the JS object that the API returns.
*   Russ: probably should be part of the blob instead of a header
*   Paul: huge proponent of versioning but also averse to using them much. In the web there is a huge problem of version based checks and we moved to feature detection. Options can be passed to GetAdIGData. Tend to say add arguments to the API, user of the API are the ones running the server and they have the option to mention that.
*   Russ: already have options like k-anonymity enforcement, &lt;some more>etc. Chosen by the browser right now for backward compatibility.
*   MK: a string that represents the version number in some way is maximally adaptable. The browser gets to decide what the string entails and the various on/off features can be added to that string, in cases where it’s important to reveal that info outside the encrypted data. 
*   Russ: prob with string , should have a well defined structure instead
*   Paul: as our feature detection increases, list is monotonically increasing. Problem with implied features. It can grow pretty big so don’t want to attach it to the blob. What’s in the blob are the arguments from browser + GetAdIGData
*   Brandon: discussion about detection - one of the reasons we lean towards min version vs a feature list so we can say bare min version. Whether a single flag is on or off a version of B&A will be able to support it. Minimizes the complexity.even if the coordinator didn’t revoke it, it’s telling servers you have to have atleast a given version
*   Russ: what all is required; reporting, private agg reporting, debugging. Which of these require an increment this number, in the spec, we do have a first byte which is always 0. If we make a breaking change it will change but not ergonomic for the seller since it’s in encrypted blob
*   MK: that’s up to the caller of the API. browser will produce the blob and processed by the server. Can we provide enough info so that it lets you know what B&A servers can process it. Worried about min version’s concept. Situation in which chrome and edge are both supporting the apis. There is no ordering, can we not have min and just have version number.
*   Brandon: open to other ideas. b/w edge and chrome it is the same. Header easier for devs browser will directly add it. It will be a pretty static value
*   Paul: add to the api browser can then reply whether it supports or not. Can’t do header support, some servers
*   MK: Untrusted sfe receiving the encrypted blob looks at the version num and a) 2 diff fleets of backends that it might send it to b) realizes that it doesn’t have any backend for it so unable to process.
*   Brandon : both options . for b)server needs to update or a) small fleet of servers that serve canary/dev audiences
*   MK: b) is adequately handled by Paul’s suggestion where the API caller says and browser accepts/rejects it, a) will likely require the metadata
*   Brian: not clear where the client gets this info. Sounds like there is a persistent relationship with backends
*   Brandon: imagine chrome canary makes a change in the way blob is handled
*   MK: +1 to Brandon’s statement. E.g. a not backward compatible change in the browser, and canary requires a newer B&A version while stable is ok with older versions.  Seems like a reasonable flow.
*   Russ: will change the version in the encrypted as well as clear. Key id is in the second byte of the blob. Servers will be sharding by key id
*   Paul: we can’t have browsers break older stuff though. Only if used by 0.00x page loads
*   MK: if edge requires it but chrome doesn’t need it, do we have a problem with including it in the metadata? Chrome’s position is it’s not needed. 
*   Paul: I object. Anything that passes to the getadig will need to be added to the clear. If we start introducing the version, we will need fallback mechanism. Config for the blob dictates the format, contents. Don’t want to keep adding clear text metadata to the blob. It could be xml, fetch. We can’t make assumptions how it will be handled
*   Russ: but not k-anonymity enforcement
*   Paul: but in terms of server configuration
*   MK: Version number separate of the blob doesn’t seem harmful. It’s just a return value of the api 
*   Paul: we recommend attaching the whole return value
*   MK/ Russ: no, will need to update it in the explainer.
*   Russ: do they need key id as well?
*   MK: proposal: change the auction blob to include a new field version number, available to the JS, browser can drop it, 2) change https://github.com/WICG/turtledove/blob/main/FLEDGE_browser_bidding_and_auction_API.md to only send the encrypted blob to the server 
*   Brandon: would they still need to send the id, will check with the team. Good progress there.
*   Russ: don’t need to send it but only to the API. runAdAuction has seen the header with the hash of the response. Never need to send the id
*   MK: excellent use of in-person time. Other agenda topics next time.
