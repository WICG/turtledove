# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday February 5, 2025

January 29 meeting was cancelled due to empty agenda

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!



1. Michael Kleber (Google Privacy Sandbox)
2. Pooja Muhuri (Google Privacy Sandbox)
3. Brian May (unaffiliated)
4. David Dabbs (Epsilon)
5. Patrick McCann (Raptive)
6. Youssef Bourouphael (Google Privacy Sandbox)
7. Kevin Lee (Google Privacy Sandbox)
8. Matt Kendall (Index Exchange) 
9. Becky Hatley (Flashtalking)
10. Alexander Tretyakov (Google Privacy Sandbox)
11. Orr Bernstein (Google Privacy Sandbox)
12. Roni Gordon (Index Exchange)
13. Fabian Höring (Criteo)
14. Paul Jensen (Google Privacy Sandbox)
15. Anthony Yam (Mediaocean)
16. Hillary Slattery (IAB Tech Lab)
17. Laurentiu Badea (OpenX)
18. Isaac Foster (MSFT Ads)
19. Shafir Uddin(Raptive)
20. Risako Hamano (ly corporation)
21. Victor Pena (Google Privacy Sandbox)
22. Joshua Prismon (Index Exchange)
23. Bharat Rathi (Google Privacy Sandbox)
24. Eyal Segal (Taboola)
25. Sven May (Google Privacy Sandbox)
26. Sid Sahoo (Google Privacy Sandbox)
27. Fabrice Gaignier (Criteo)
28. Lydon, Jason (FT)
29. Owen Ridolfi (Flashtalking)
30. Jeroune Rhodes (Privacy Sandbox)
31. Kenneth Kharma (OpenX)
32. Trenton Starkey (Google Privacy Sandbox)
33. Alex Peckham (Flashtalking)
34. Jeremy Bao (Google Privacy Sandbox)
35. Kristina Baron (Dun & Bradstreet)
36. Nour Nabil (Google Privacy Sandbox)
37. Suresh Chahal (MSFT)
38. Aditya Agarwal (media.net)
39. Abishai Gray (Google Privacy Sandbox)
40. Andrew Pascoe (NextRoll)
41. Alonso Velasquez (Google Privacy Sandbox)
42. Koji Ota (CyberAgent)


## Note taker: Orr Bernstein & Paul Jensen


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here (meetings with no items may be canceled)



*   Fabian Höring
    *   Discuss feedback given here https://github.com/privacysandbox/protected-auction-key-value-service/issues/72#issuecomment-2485843775
    *   Discuss  https://github.com/WICG/turtledove/issues/1367#issuecomment-2545927798
*   Patrick McCann - https://github.com/prebid/Prebid.js/issues/10740#issuecomment-2607774957
    *   How do we get the nonce to the component sellers if the TLS hasn't generated it yet?  References: https://github.com/WICG/turtledove/blob/main/FLEDGE.md#61-auction-and-bid-nonces and https://github.com/user-attachments/files/18024060/External.PA.API.a.k.a.FLEDGE.Additional.Bids.with.Negative.Interest.Group.Targeting.pdf

For next week:



*   Fabrice Gaignier/Fabian Höring
    *   Discuss  https://github.com/WICG/turtledove/issues/1367#issuecomment-2545927798


# Announcements

Privacy Sandbox folks are holding every-second-Wednesday meetings in the hour after this meeting to discuss the trusted server elements of Protected Audience work. For more information see https://github.com/WICG/protected-auction-services-discussion/issues/27.

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!


# Notes 


## Discuss feedback given here https://github.com/privacysandbox/protected-auction-key-value-service/issues/72#issuecomment-2485843775



*   Michael Kleber
    *   Context: There's another meeting about trusted server side things, this came up there, and there were things that were more browser-focused
*   Fabian Höring
    *   Adding more signals to the trusted server call
    *   Issues https://github.com/WICG/turtledove/issues/1105 , https://github.com/WICG/turtledove/issues/1319 
    *   On the B&A services side
    *   Something we have been asking for since the very beginning. A way to reduce on-device auction cost. Much easier to delegate things to the K/V server than to B&A services.
    *   That's why I think most of the value of the Trusted Bidding Signals call is if it happens for on-device auctions.
    *   Now that we've done the CMA test, also started noticing that this call is very expensive. If we start injecting new signals, need to be careful to make them opt-in.
    *   Also, across channels. One call for all SSPs; if it's made per SSP, very expensive.
*   Michael Kleber
    *   From a philosophical point of view, this is the call we're talking about browser side stuff, not the capabilities of the trusted servers. What is it possible to do with the signals you get in the trusted servers is an excellent topic for the other call. Let's not get into that today. But the question of what signals can be sent to the trusted signals server is a great discussion to have today.
    *   K/V servers are in a BYOS state, but in the future, we'd like those to be in a trusted server state. All discussions of adding additional signals are relegated to the trusted server state. Discussion of what additional signals can be sent. Not making those signals too large, expensive, numerous; and not having a latency impact by having to wait longer to send those requests.
*   David Dabbs
    *   will defer to Paul first
*   Paul Jensen
    *   We've noticed that there are a number of issues around adding signals to the TKV. Just in the process of launching the TKV support in Chrome. Because it's better privacy, can add more signals.
    *   Considering what signals, how it affects parallelism.
    *   In an earlier design, I called these "early buyer signals" - those signals available to trusted bidding signals requests. These network round-trips are very expensive, want to issue them as early as possible. Try to initiate as much as possible right as the auction is called. The reason we call them 'early buyer signals' is that they're needed as early as possible. Should you send stuff early/later into the auction - early includes URL to the publisher (or part of the URL that's visible to the seller). With work to parallelize the auction, can eliminate one network round trip, which is huge. By sending that early, we get huge latency win, but can only send signals only available at the very beginning of the auction.
    *   I would lean towards the earlier side - send less information but don't incur the latency hit, and don't incur the cost of sending all of the contextual signals from client to server.
    *   The design that has one Promise for each buyer, certainly feasible, but complicates things and encourages this higher-latency thing might mean that some buyers wait until after the contextual thing.
    *   We could try it without that; sellers could experiment with/without the parallelization thing, and we can then see if we want to work on the per-buyer Promise work.
*   Fabian
    *   Interesting insights. I don't think it should be on per-buyer signals.
    *   You always said contextual signals, but I think other signals like clicks would also be useful.
    *   Splitting the promise for per-buyer signals - not related to this discussion - but it means that one buyer could impact other buyers. So, this is important to do, but not for this reason. We should discuss this, but not now.
*   Paul
    *   Would another option be for buyers to convey that they don't want to wait for per-buyer signals.
    *   Fabian is suggesting giving each buyer choice as to whether or not they want to wait for the contextual bid.
*   Michael
    *   So, the idea would be - an IG could say, I would like to bid based on the browser signals and trusted signals , and not on the signals that come from the contextual path.
*   Paul
    *   K/V could also signal that.
*   Michael
    *   Interesting. Mind-expanding topic. I don't remember any previous discussions of that.
*   Isaac
    *   So, the buyer could say, just run my generateBid as quickly as possible.
*   Michael
    *   If everything you want to do, you could do with the response from your K/V server, then this would allow you to do so.
    *   Possibly a super latency improvement.
    *   But just to be clear about the kinds of things that are not possible with this model.
        *    Suppose that you want to inspect the URL. If you get this on your standard contextual path, you could scan this URL if you had never seen before. But your TKV is in its own isolated environment, so if you haven't seen a contextual URL before, you don't get to scan that URL.
    *   Another thing you just said, Fabian, is that you addressed the value of the previousWins information already sitting on the browser. I'd like to know more about the previous information about this IG. If we're talking about including prevWins, it might be helpful to know the minimum amount of data needed instead of sending everything.
*   Fabian
    *   For previous wins, good enough to have aggregated data - number for a campaign. Or otherwise, you could never reuse your K/V call.
    *   It's a stable signal - user data, not a contextual signal.
*   Issac
    *   The K/V folks (on Nov 5 or 6) had talked about making the contextual signals available in the trusted environment.
    *   But Fabian you said that there's something about the flow, and not just the call - what did you mean?
*   Fabian
    *   We can talk about this in the B&A meeting. I don't see how sending more signals to the K/V call is helpful for B&A. It's really helpful for on-device auctions.
*   Issac
    *   I don't agree. Why would it not be useful for B&A?
*   Fabian
    *   I use the k/v layer for storage, and the b&a layer for bidding.
*   Michael
    *   It sounds like you still need the entire publisher page URL.
*   Fabian
    *   The URL, yes, but the other signals - no. The other range of signals, including fine-grained signals, we don't need for B&A.
*   Issac
    *   Other information contingent on the page - deal that's available or size that's available.
    *   Stepping outside of my ASAPI role - if it's just me representing Xander - I'm not sure what you're saying is correct, that you don't want the ability to look up keys that are in part derived from your textual information.
*   Hillary Slattery
    *   +1 to what Issac said
*   Michael
    *   Good reminder that we benefit from a wide range of perspectives.
*   David Dabbs
    *   Fabian - It is possible to bring it down to a concrete ask for what you're wanting? If I understand, the ask is to change the on-device FLEDGE?
*   Michael
    *   To reframe: What additional data would you like in the trusted K/V server, taking into account the latency of early vs late?
*   David Dabbs
    *   Yes, that's what I'm looking for.
*   Fabian
    *   Every possible signal, but should be cross channel.
*   David Dabbs
    *   Sounds like a fundamental change to the signaling as opposed to the content that's sent over the signal.
*   Fabian
    *   Single promise is a problem.
*   Michael
    *   So the only things you don't want are e.g. the 16-bit experiment ID that could be different from the two sellers. Not having seller-sent information avoids splitting the request, cache miss.
*   David Dabbs
    *   Programmatic ads, openRTB - almost guaranteed to have some different information because of e.g. different deals.
*   Michael
    *   I think this is why Fabian is prioritizing the user signals on the browser over seller-provided contextual signals.
*   Fabian
    *   If we could deactivate it on our side, that would help. We don't control the flag, so if there are five sellers, we get five requests, which is a lot more expensive.
*   Michael
    *   So, there's a flag that allows the removal of different seller signals.
    *   And the additional of browser signals and on-device signals that could help you do the processing inside the TKV server.
    *   early buyer signals and some sort of summary statistic of appropriate pre-processed version of prevwins.
*   David
    *   You mentioned the TKV, but device is BYOS.
*   Michael
    *   Our attention is only on the trusted KV of the future, not on the BYO server.
    *   And you can kind of dip your toe in the water by using a TKV.
    *   (DDabbs after the meeting): see this Chrome team post: https://github.com/WICG/turtledove/issues/1395 
*   Brian
    *   The other discussion we had earlier about sending signals early/late - is there an issue for that?
*   Paul
    *   Proposal we're seeking feedback on - there's early signals and late signals. But it wasn't yet a discussion of what to send to TKV.


## Patrick McCann - https://github.com/prebid/Prebid.js/issues/10740#issuecomment-2607774957



*   Patrick How do we get the nonce to the component sellers if the TLS hasn't generated it yet?
    *   References: https://github.com/WICG/turtledove/blob/main/FLEDGE.md#61-auction-and-bid-nonces and https://github.com/user-attachments/files/18024060/External.PA.API.a.k.a.FLEDGE.Additional.Bids.with.Negative.Interest.Group.Targeting.pdf
*   Patrick: Request for additional bids support in PreBid.  Want to test with various component sellers.  Need to create nonce and pass to component seller.  Need to call seller’s endpoint with nonce.  Nonce can be passed via new proposal.  Problem is component seller doesn’t have access to the nonce.
*   David: How old is doc? Does it mention new nonce mechanism.
*   Patrick: Nonce generation not changed by new mechanism.
*   Orr: Each component seller creates their own nonce.  Each of these nonces is put into component auction config.  This can be the same nonce sent in their contextual request, and forward onto buyers.  The new mechanism provides a more secure way where nonce is combined with a seller-created nonce.
*   Patrick: Does component seller need to call this function to get a nonce.
*   Orr: Yes.
*   Patrick: Can call repeatedly and give to each component seller?
*   Michael: Yes, call once on behalf of each component seller.  Each nonce then sent as part of each sellers network call and then be put into that seller’s component auction config.  E.g. Five different sellers each have one nonce (five nonces) then pass along in their component config.
*   Patrick: Can I get 1000 nonces?
*   Orr: Cost is low to creating nonces.  We optimized, so cost is low.  Unused nonces are not a problem.  Only requirement is that the nonce in additional bids needs to match (or derived from using new mechanism) nonce in auction config.
*   Patrick: So I can include nonce in component auction config that’s given to top-level-seller?
*   Michael: Yes, no reason to wait for round trip to SSP to insert nonce in auction config.
*   Roni: Documentation says top-level-seller and component seller must have nonces?
*   Orr: Names aren’t great.  Auction-nonce is related to each seller. In a multi-level auction, the top-level seller doesn't get an auction nonce. The top-level seller has no buyers, so can't use additional bids, and so shouldn't have an auction nonce. In a multi-level auction, only the component sellers get auction nonces.
*   Roni: > The seller must combine the auction and seller nonces to construct a bid nonce
    *   https://github.com/WICG/turtledove/blob/main/FLEDGE.md#61-auction-and-bid-nonces
*   Third paragraph.  How to combine nonces?
*   Orr: Auction nonce is from browser.  Seller nonce is the one that seller combines with auction nonce, not from browser.
*   Michael: Auction nonce is generated by browser by calling getAuctionNonce(), called once for each component seller.  Component seller server-side creates seller nonce.  Bid nonce is the combination and in the additional bid.
*   Roni: Auction nonce is part of component auction config.  Now clearer that this is server-side combining.
*   Orr: Why is it not called component auction nonce? Because single-seller auction wouldn’t have component auctions.
*   Matt Kendall: Top-level-seller required to be included in additional bid?
*   Orr: We’ve heard this feedback before that it may not be known.  Previous suggestion was to allow a wildcard top-level-seller in additional bids.  Buyers could ignore the browserSignals.topLevelSeller in generateBid().
*   Michael: If buyers don’t feel like they need to know top-level seller, then we don’t need to require it.
*   Patrick: need an issue for what Matt said?
*   Matt opened an issue: https://github.com/WICG/turtledove/issues/1394
