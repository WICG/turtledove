# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday January 22, 2025

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!



1. Michael Kleber (Google Privacy Sandbox)
2. Brian May (unaffiliated)
3. Roni Gordon (Index Exchange)
4. Garrett McGrath (Magnite)
5. Felipe Gutierrez (Microsoft)
6. Orr Bernstein (Google Privacy Sandbox)
7. Sven May (Google Privacy Sandbox)
8. Paul Jensen (Google Privacy Sandbox)
9. Pooja Muhuri (Google Privacy Sandbox)
10. Ivan Straritskii (BidSwitch | Criteo)
11. Matt Kendall (Index Exchange)
12. Shivani Sharma (Google Privacy Sandbox)
13. Nour Nabil (Google Privacy Sandbox)
14. Kristina Baron (Dun & Bradstreet)
15. Kevin Lee (Google Privacy Sandbox)
16. Matt Menke (Google Chrome)
17. Laurentiu Badea (OpenX)
18. Brian Schmidt (OpenX)
19. Lydon, Jason (Flashtalking)
20. Youssef Bourouphael (Google Privacy Sandbox)
21. Andrew Pascoe (NextRoll)
22. Kenneth Kharma (OpenX)
23. Harshad Mane (PubMatic)
24. Alexander Tretyakov (Google Privacy Sandbox)
25. David Tam (Paapi)
26. David Dabbs (Epsilon)
27. Jeremy Bao (Google Privacy Sandbox)
28. Shafir Uddin (Raptive)
29. Bharat Rathi (Google Privacy Sandbox)
30. Koji Ota (CyberAgent)
31. Sid Sahoo (Google Privacy Sandbox)


## Note taker: Orr Bernstein


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here (meetings with no items may be canceled)



*   Third Party reporting (Ivan Staritskii, BidSwitch),   https://github.com/WICG/turtledove/issues/1220
 
*   Creative Scanning proposal,    https://github.com/WICG/turtledove/issues/792 



# Announcements

Privacy Sandbox folks are holding every-second-Wednesday meetings in the hour after this meeting to discuss the trusted server elements of Protected Audience work. For more information see https://github.com/WICG/protected-auction-services-discussion/issues/27.

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!


# Notes 


## Third Party reporting (Ivan Staritskii, BidSwitch)

https://github.com/WICG/turtledove/issues/1220



*   Ivan
    *   Tried to reach Shivani regarding this
    *   Since we've clarified everything, want to understand some other questions, things to discuss here to proceed.
*   Alonso
    *   We wanted to discuss the possibilities. Unique use case. Want to make sure we understand what you need, soliciting more feedback. Other folks on the call - if you have a similar use case, please speak up. Will let the technical design discussion continue.
*   Michael Kleber
    *   Two important things. As we design solutions, we have better solutions if we have heard from at least two different parties that can provide feedback on the solution. If there's a second party interested in this idea, we encourage them to speak up.
    *   Second, more intended users makes it easier to prioritize and provide the solution sooner.
*   Shivani Sharma
    *   Echo both what Alonso and Michael said.
    *   Maybe extending reportResult is easier than an extension to FFAR.
*   Paul Jensen
    *   One small thing - if we allow sendReportTo to be called multiple times with different arguments, can break existing scripts that rely on this being called exactly once.
    *   Does anyone know of reportResult that would e.g. throw an error between one call and the next?
*   Laurentiu
    *   To clarify, are we talking about calling sendReportTo twice, or call it with an array?
*   Michael Kleber
    *   We do have other APIs that calling multiple times overwrites previous calls, e.g. forDebugOnly and setBid.
*   Paul Jensen
    *   Also, how many times do we allow it to be called? For setBeacon, we talked about that to be called at most ten times.
*   Michael Kleber
    *   Can also consider setReportTo to be called at most once per domain.
*   Ivan
    *   Yes, we would expect it to go to different domains. But there are a lot of SSPs that may act as an intermediary. Lots of hops on the bid stream. Might be called by their first SSP, then some SSP intermediary, and then BidSwitch.
*   Michael Kleber
    *   That gives us two different answers. Let's try not to do something that calling it for the same domain twice should keep being an error, just generalize to allow it to be called multiple times.
    *   Number two - you said that many SSPs are going to play this role. That suggests that there are parties other than BidSwitch that may rely on this feature that we can ask for feedback on this feature.
*   Ivan
    *   Yes.
*   David Dabbs
    *   I haven't looked at this for a while. Aside from calling the current API multiple times, is the configuring of an intermediary going to be in the hands of a seller? I don't recall how that was shaping up.
*   Ivan
    *   The design - we (BidSwitch) pass the billing URL on the bid response to the SSP and the SSP should - through their auction config - transfer this bidding URL to the seller's logic. Add required information - bid price - and yes, report the impression URL with the bid price.
*   David
    *   To decode that - returning with the bid response would be what PA calls the “contextual” auction or openRTB. Whatever that is, the seller will plumb that down so that they can activate the right APIs so you get the right report.
*   Ivan
    *   Yes, they just have to make the URL whole.
*   David
    *   Other metadata?
*   Ivan
    *   All that's required is the price.
*   David
    *   Never say that all you need is the price, there's always other things. 🙂 But if you can bake that into the URL, you can do that.
*   Ivan
    *   From the auction, the only thing you need to grab is the price.
*   Laurentiu
    *   Thought of another case that would turn an SSP into a sort of entity that would need to be able to call `sendReportTo` twice. Integration with OB - Open Bidding - this is where GAM sends protobuf requests to an SSP and the SSP would pass it onto the buyer. In that case, an SSP can only return IG bids and not full component auction config. Not sure if that's a valid use case, but could be close.
*   Michael Kleber
    *   I don't think we have anyone from GAM here today, so whether they should need to use this for their reporting system is unclear.
    *   The question remains - if there are parties aside from BidSwitch who are interested in using this, it would be good to hear directly from additional candidate users. When one person starts talking about the way another person would use something, we end up having to backtrack when the actual user ends up in the room representing themselves instead of being proxied by another user.
*   Laurentiu
    *   This means that us as SSPs we currently don't do any PAAPI when called from OB or TAM
*   _Roni Gordon over chat: https://developers.google.com/authorized-buyers/rtb/protected-audience-api says "Exchange and network bidders participating in Open Bidding are not supported yet."_
*   David
    *   I was thinking of intermediaries like TAM et al too
*   Michael
    *   OK, that sounds very promising.
*   Shivani
    *   There was a reference to the max number of times it can be called. Looking at FFAR we don't have a limit on registerAdBeacon. This would be similar - destination URL is given by the worklet (vs registerAdMacro where the destination url is given by the ad frame and is limited to max 10 destination urls).
*   Michael
    *   That sounds right to me. The additional calls are entirely in the hands of the SSP and named in the auction config.
*   Shivani
    *   And still the enrollment requirement. Any origin that the API is sending info to must be enrolled and attested.
*   Brian May
    *   Just want to encourage everyone who knows of others that can use this to chime in.
*   David Dabbs
    *   List of allowed domains in JavaScript could be possible too.
*   Michael Kleber
    *   Yes, no reason to have them list the same domain in two places (JS and auction config). Could be available in seller signals too.
*   David Dabbs
    *   In the current setup, if an intermediary needs to have access to some of the aggregated auction reporting or real-time reporting, is that anything they're able to participate in?
*   Ivan
    *   The main thing for us is to register the impression that went from our pipes.
    *   Just for me to understand, we register to bring a couple of SSP or tech partners that share this need?
*   Alonso
    *   It's not just about the partners who are receiving the request, it's also about others that are sending the same request. Also, let's talk about scale.
*   Ivan
    *   Yes, we'll try to find other interested parties. May not be a lot in the beginning. In scale, it'll be the case.


## Creative Scanning proposal (not added by anyone on the call)

https://github.com/WICG/turtledove/issues/792 



*   David Dabbs
    *   Asked for confirmation from Orr on recent comment on that issue
*   Orr
    *   We've been working on a response, coming soon, nothing contentious
*   Roni
    *   If it's WIP then I'm happy to wait
*   Orr
    *   The questions you've asked are more details; anything you want to discuss?
*   David
    *   If we provide a Seat in BASRI, then can we avoid repeating it in creative scanning data?
*   Orr
    *   There is a difference between BASRI and creative scanning data, it gets wound up in the way reporting IDs work, BASRI is only provided to scoreAd when a selectable BASRI is also provided.
*   David
    *   You could also send an empty string in as the BASRI, to get around that
*   Orr
    *   That does still have impact, though: having _any_ selectable BASRI has an impact on k-anonymity, which now covers _all_ the reporting IDs.  So this affects what IDs are available and/or what ads can win
*   David
    *   It's very desirable for buyers and sellers to have a single place to put Seat, and not need to duplicate it in different places or different times.
*   Roni
    *   It's just about how we're going to get it back.  If it goes out to the KV and it's safe to pass through to scoreAd, just a question of where to put it so I can learn the seat in scoreAd outside the KV.  Just need to know how to get it.
*   Orr
    *   If the goal is to always put Seat in BASRI, and you always want it available in scoreAd and reporting functions and KV, then would you expect to _always_ have a selected BASRI (even if just an empty string), accepting the k-anon impact?
*   Roni
    *   Potentially, yes.  Conceptually no difference, except for the k-anon ramp-up — in the steady state, Seat applies regardless, so yes, even if the selected one is blank to make the Seat flow through, that's what we would expect.
*   David
    *   BASRI seems to be the place where Seat is going to be signaled, sellers need Seat everywhere, so perhaps we just have to live with it through BASRI and we accept the consequences.  We prefer that it be the same channel all the time.  (Other people are +1'ing in emoji reactions)
    *   Regarding the k-anon impact, if I'm bidding on a deal, the deal ID is going to go in as selectable, the Seat is going in as BASRI also.  If I bid with a blank selectable, I don't see the k-anon benefit: I need to give the seller a seat and I need my own reporting ID, so I'm going to need to pay the k-anon price
*   Orr
    *   So you would return a selectable BASRI regardless, notwithstanding needing to communicate a deal, because the other IDs are communication you need, and a bid without those other IDs is not useful?
*   David
    *   I need to provide my seller with a seat, and I need my own reporting ID.  If I'm not buying on a deal, which happens quite a bit as well, I still need to provide the seller with seat where they receive it in reporting, that's essential.  So I'm assuming that I’ll need to use BASRI no matter what.  That's what leads to empty-string-selectable shenanigans.
    *   Other than that, happy to wait for your response for other issues.  The Seat is really the important thing.


## David Dabbs new topic: There is a special header for B&A letting the seller pass along a stronger nonce to the buyer.



*   Paul
    *   The auction nonce?
*   Orr
    *   There are two different ones, one for B&A and one for additional bids
*   David
    *   Does Chrome listen for that response header on any request happening on that page?  Even a response to a seemingly unrelated request?
*   Orr
    *   This is detailed really well in the spec.  It's effectively anything that is a fetch() request that has an explicit ad auction header, or an iframe that has an ad auction attribute in the iframe object.  Those are the responses that are inspected for the response header.
    *   Probably the fetch() is the most relevant one — causes the request to have a special request header, and causes the response to be watched for the response header.
*   Roni
    *   We've been digging into the challenges of additional bids with the prebid folks.  There are challenges with order of operations, some plumbing that needs to exist to request the nonce early enough.  The extra header on the fetch call is interesting — it's the same header that B&A uses, so the header increases the payload size, because it throws a big encrypted blob in to the header.  The additional bid also needs to include the top-level seller origin, but that is not known at the time the auction is kicked off.  So if you're wondering why you don't see a lot of additional bid activity, that's why: it's impossible to make it work today with the open packages.
*   Orr
    *   Which part is the blocker?
*   Roni
    *   Top-level seller is tricky, because you could always just generate a nonce and pass it through, but knowledge of who the top-level seller is going to be is something that isn't known that early, haven't even figured out parallel auctions.  Being discussed in the prebid repo.
*   Paul
    *   Can you explain your first concern, about the two different headers?  I thought the B&A blob was something deliberately attached
*   Roni
    *   Works in a single-seller environment, but in a multi-seller environment we haven't figured out who makes the decision.  It's an adoption thing.
*   Michael
    *   The thing you pointed out about the top-level seller domain not being known early enough, if that's a blocker, then please come to us about that more, and we can try to make the timing work better for what's possible. It's possible that we didn't design this well because we didn't know about how the implementation would work in prebid. Don't feel that you on the prebid side need to do more because we didn't know at the time.
*   Roni
    *   I won't speak for the prebid JS developer, but when they get there, it's good to know that this is not fixed in time.
*   Michael
    *   It's always possible there's something hard here that we need to figure out, but it's possible that there's an easy change available as well.
