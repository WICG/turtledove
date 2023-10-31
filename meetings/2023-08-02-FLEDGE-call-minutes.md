# Protected Audience (formerly FLEDGE) WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Aug 2, 2023


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Nick Llerandi (Triplelift)
3. Andrew Aikens (TripleLift)
4. Nick Llerandi (Triplelift)
5. Roni Gordon (index Exchange)x
6. Andrew Aikens (TripleLift)
7. Roni Gordon (index Exchange)
8. David Dabbs (Epsilon)
9. Sven May (Google Privacy Sandbox)
10. Tamara Yaeger (BidSwitch)
11. Stan Belov (Google Ads)
12. Russ Hamilton (Google Chrome)
13. Youssef Bourouphael (Google Chrome)
14. Paul Jensen (Google Chrome)
15. Marco Lugo (NextRoll)
16. Steve Luo (PubMatic)
17. Matt Menke (Google Chrome)
18. Orr Bernstein (Google Chrome)
19. Nitin Nimbalkar(PubMatic)
20. Fabian Höring (Criteo)
21. Priyanka Chatterjee (Google Privacy Sandbox)
22. Manny Isu (Google Chrome)
23. Isaac Foster (MSFT Ads)
24. Harshad Mane (PubMatic)
25. Viacheslav Levshukov (MSFT Ads)
26. Risako Hamano (Yahoo Japan) 
27. Tianyang Xu(Google Chrome)
28. Sid Sahoo (Google Chrome)
29. Andrew Pascoe (NextRoll)
30. Jeff Wieland (Magnite)
31. Rotem Dar (eyeo)
32. Yanush Piskevich (MSFT Ads)


## Note taker: mannyisu 


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here — no agenda no meeting!]



*   Isaac (MSFT) Issues (don’t all have to be mine, so grouping so we can easily filter)
    *   B&A Multi Tag Auctions  https://github.com/WICG/turtledove/issues/724
    *   <span style="text-decoration:underline;">https://github.com/privacysandbox/fledge-docs/issues/52</span>
    *   <span style="text-decoration:underline;">Production Support: https://github.com/WICG/turtledove/issues/620</span>
    *   <span style="text-decoration:underline;">More Spinoff Meetings for uncovered topics?</span>
*   Yanush (MSFT):
    *   Clarify BA Final Decision: https://github.com/WICG/turtledove/issues/739


# Notes


## B&A Multi Tag Auctions  https://github.com/WICG/turtledove/issues/724



*   [Isaac Foster] This is important to the MSFT side of things for unified ads request
    *   There is going to be generation ID for the Ads request for the seller front end that will be used for debugging and preventing attacks. The concern is trying to run multiple auctions in a single request - run one and cannot run the other one due to fraud. (Isaac Edit: UUID generated for request and then used to prevent replay attacks, assuming this basically does a used-more-than-once validation, that would prevent running multiple ad auctions with a single request).
*   [David Dabbs] To clarify, are you referring to the recent spec for Ad Auction Nonce? What Identifier are you referring to?
*   [Isaac] I do not believe this is what I am referring to - This is what I am referring to: https://github.com/privacysandbox/fledge-docs/blob/main/bidding\_auction\_services\_system\_design.md#throttling-in-sellerfrontend. Also there is a field called requestID: https://github.com/privacysandbox/bidding-auction-servers/blob/main/api/bidding\_auction\_servers.proto#L47
*   [Priyanka] The concern is spot on - valid that B&A comes with a bunch of threats. We want to incorporate chaffing as a mitigation strategy but it also comes with some threats so we want to integrate anti-abuse. The issue is that generationID is generated from the browser. For a single slot auction, it should be okay. But for multi slot, it is an issue - As of now, we have not placed a replay attack mitigation in place and not incorporated chaffing. We want to discuss with SSPs on a viable solution keeping privacy in mind and not have a broken feature once a mitigation is in place.
*   [Isaac] It seems like this is one of the features that will be turned on afterwards. Can you clarify the timeline?
*   [Priyanka] While 3PCD is there, it is fine to take on this but we need to do this ASAP. We believe that this is going to come last. We need to put in the mitigations by mid 2024.
*   [Issac] I am referring to the case where there are multiple ad slots on the same page - Basically a single request from a web page to the SSP, and SSP does a bunch of inline things… I am not referring to single ads that might have components
*   [Priyanka] Yes, we are talking about the same thing.
*   [Isaac] On timing, it is good to know that when we are doing B&A testing, we will be able to do the multi ads per page testing. But if that were to be deprecated inline with the deployments, that will be problematic and a massive hit to our SLAs. I will push a bit for some clarity on this one (Isaac edit: both on timing and on us all developing the solution).
*   [Priyanka] We will get back to you on exact timelines after further discussions with Product. We will make sure features continue to work. Also, how does the single request going out for all slots work for all SSPs here?
*   [Harshad] Most SSPs are on-page using Prebid, and that coalesces all the slots on the page to a single request
*   [Roni] Everything coming from one page ad slots is sent as a single HTTPS request to the exchange via PBJS
*   [David] The security model has padding payloads - They need to be encrypted and transit through the SSPs relay and relay that packet into the TEE. Since they need to be encrypted, is today’s model compatible with that?
*   [Priyanka] We do send back encrypted and padded responses through the seller to the client. If there are multiple requests, we will treat that as different requests at this point
*   [MK] Priyanka, can you take a sec to talk through what the replay attack that you’re mitigating is?
*   [Priyanka] The SSP needs to send the chaff request to a few buyers. This way, the sellers don't figure out which buyers are on the browser. We want to randomize the number of buyers that will be sent. The seller ultimately determines which buyers will take part in the auction. If the browser has an IG for those buyers, only those buyers will get the request. The abuse here is that we need to keep the set of chaffs random and folks getting the chaffs random. This ensures that there is no kind of abuse happening.
*   [MK] It seems like the risk could be addressed in other ways. If this is the only leakage we are worried about, I would like to have a conversation on the design to avoid this need for preventing replays.  As long as this is just about random chaffand the response that comes from a TEE to the untrusted server isn't leaking information, some kind of deterministic random number generator based on stuff coming from the browser could let us get around this problem entirely.
*   **ACTION:** Priyanka Chatterjee & Michael Kleber will discuss this design offline.


## [Isaac] As we are getting closer to deployment date, I would like to propose a structure for meetings so we can make progress on some of the additional items on the agenda.

[MK] If the questions are around server setup or mechanics like that, those should probably be a different meeting.

[Isaac] There are things that are worthy of public follow up discussions - some might make sense as WICG. Alternatively, if it's hyper specific, perhaps it can be a one off meeting and doesn’t have to be a WICG session.


## https://github.com/privacysandbox/fledge-docs/issues/52



*   [Isaac] On server side, it seems like there is a lot of caching that happens on startup. If IG comes with a new thing that it hasn’t seen before…
*   [Priyanka] Here is some documentation in the explainer: https://github.com/privacysandbox/fledge-docs/blob/main/bidding\_auction\_services\_system\_design.md#code-blob-fetch-and-code-version
    *   We reviewed and wanted to fetch the code modules. Within the bidding and auction server, the code will be prefetched at server startup and periodically. We are also supporting a way to fetch from cloud storage where the adtech can push code modules as they roll out, push to the buckets and notify the bidding servers. We will support multiple versions of the code modules. However, the adtech can pass the version of the code modules for requests. Both buyers and sellers can pass any version of the code modules they want to use.
*   [Isaac] Regardless of the version being passed, that will still be factored into the k anon, correct?
    *   [MK] The bidding script is also the script used for reporting. 
    *   [Priyanka] If we fetch from cloud storage, we will not be able to micro target the user of the url.
    *   [MK] The actual url specified inside the IG will need to go through a k-anon check.  It's OK if the file that gets loaded is different in different servers.  The point is that it must not be different for different _people_, in a way that is consistent as they browse across sites, otherwise it's a tracking risk.
    *   [Isaac] Let’s button this up on the ticket.


## Yanush (MSFT) - Clarify BA Final Decision: https://github.com/WICG/turtledove/issues/739

*   [Yanush] How do we choose on client side between the contextual and private bids returned from the B&A flow?
*   [MK] see https://github.com/WICG/turtledove/blob/main/FLEDGE\_browser\_bidding\_and\_auction\_API.md#step-4-complete-auction-in-browser
    *   When you execute an auction on B&A service, you end up with a server response blob. This contains the results of the B&A auction that took place server side. It doesn’t contain contextual ads.  Feed the server response blob into the on-device auction.
    *   Use the contextual response if the on-device auction has no winner.  This is the same as for a purely on-device auction with no B&A at all.
*   [Yanush] So I can use contextual ads if something goes wrong?
    *   [MK] Basically the contextual auction is a fallback if the PA auction decides that there is no winner.
*   [Yanush] And I can render contextual ads without a fenced frame, correct?
    *   [MK] Yes
 
## [Rotem Dar] Can an SSP and DSP work together without an endpoint integration?
*   [MK] For a purely ondevice PA auction, it is an optional engagement between SSP and DSP.  Of course the SSP and DSP need some business relationship with each other, since they are exchanging money!  But they can do it without any real-time server-to-server communication, if they want to.
*   [Rotem] This is something that we should highlight a little bit more - it is not trivial

##  [David] So whoever is running the top most auction is the orchestrator - If I am a seller, is it a binary thing that if I am going to do server only or on-device only or can I mix and match? I may have DSP partners that aren’t as sophisticated? How is this possible? How is it going to work?
*   [MK] An SSP that wants to work with some DSPs who want to do server side B/A stuff and others who want to do on-device stuff can run two different component auctions. The SSP should pretend there are two separate SSPs.
