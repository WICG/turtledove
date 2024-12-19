# Protected Audience WICG Calls: Agenda  & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Dec 11, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!



1. Michael Kleber (Google Privacy Sandbox)
2. Roni Gordon (Index Exchange)
3. Pooja Muhuri (Google Privacy Sandbox)
4. Paul Jensen (Google Privacy Sandbox)
5. Shivani Sharma (Google Privacy Sandbox)
6. Paul Spadaccini (Flashtalking)
7. Becky Hatley (Flashtalking)
8. Sid Sahoo (Google Privacy Sandbox)
9. Charlie Harrison (Google Privacy Sandbox)
10. Patrick McCann (Raptive)
11. Anthony Yam (Flashtalking)
12. Owen Ridolfi (Flashtalking)
13. David Dabbs (Epsilon)
14. Matt Kendall (Index Exchange)
15. Aditya Agarwal (Media.Net)
16. Luckey Harpley (Remerge.io)
17. Arthur Coleman (ThinkMedium)
18. Peiwen Hu (Google Privacy Sandbox)
19. Eyal Srgal (Taboola)
20. Lydon, Jason FLASHTALKING
21. Matt Davies (Bidswitch | Criteo)
22. Ivan Staritskii (BidSwitch | Criteo) 
23. Trenton Starkey (Google Privacy Sandbox)
24. Tejasvi S. Tomar (Google Privacy Sandbox)
25. Nour Nabil (Google Privacy Sandbox)
26. Elmostapha BEL JEBBAR (Lucead)
27. David Tam (paapi.ai)
28. Diana Torres (MLB)
29. Daniel Rojas (Google Privacy Sandbox)
30. McLeod Sims (M.net)
31. Alex Peckham (Flashtalking)
32. Garrett McGrath (Magnite)
33. Martin Pál (Google Privacy Sandbox)
34. Sathish Manickam (Google Privacy Sandbox)
35. Jeremy Bao (Google Privacy Sandbox)
36. Abishai Gray (Google Privacy Sandbox)
37. Donato Borrello (Google Ad Manager Video)


## Note taker: Peiwen Hu


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here (meetings with no items may be canceled)



*   [Aditya Agarwal] Implementation queries around Pa-Api Video Ads
*   [Matt Davies] Billable impression event for Privacy Sandbox with calls to third party on reporting
    *   https://github.com/WICG/turtledove/issues/1220
*   [Charlie Harrison] Small change to proposed Private Model Training (PMT) API surface (https://github.com/WICG/turtledove/pull/1359)
*   [Patrick McCann] https://github.com/WICG/turtledove/issues/1338 to support B+a


# Announcements

Privacy Sandbox folks are holding every-second-Wednesday meetings in the hour after this meeting to discuss the trusted server elements of Protected Audience work. For more information see https://github.com/WICG/protected-auction-services-discussion/issues/27.

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!


# Notes 


## [Charlie Harrison] Small change to proposed Private Model Training (PMT) API surface (https://github.com/WICG/turtledove/pull/1359)

Charlie: have this Pull Request https://github.com/WICG/turtledove/pull/1359. Overall minor change we have to this PMT surface which is to try to move work out of the critical bidding path. If you’re trying to generate a modeling signal payload, egressing from the device in an encrypted fashion. There’s some concerns that generating the payload can increase auction latency. We reworked so the payload is generated outside the critical path without impacting the privacy and utility aspects of the design. Can go through and hope it’s somewhat not controversial.

What we had before was that we created the raw bidding signal inside generateBid, thus blocking the auction.

We’re changing it to that basically we make it a new param, aggregateWinSignals, maybe a JSON serializable object. So it doesn’t require you to serialize it into a uint8 array. Can put the signal in the out param. There’s a new function reportAggregateWin that takes it as input. Within it we can process the signals. Maybe the process is slow so here it won’t block the auction. 

API is flexible enough and can potentially use private aggregation API.

reportWin today doesn’t get sensitive information but aggregateWinSignals does get sensitive information, so you can use private aggregation etc but only if you want. aggregateWinsignals is a new place for doing the processing.

The queueReportAgregateWin.modelingsignalsConfig is used to configure this. 



*   David Dabbs: So this is just a new egress. There’s a discussion on how much we can count on the reporting happening. This is a separate effort making sure reporting is guaranteed?
*   Charlie: wouldn’t make a guarantee about the reliability of the report. More to improve the latency. There may be a slight negative impact on reliability. Now we need it to be sequenced after reportWin. We expect this will maybe only improve things. Before we need to create the signals once per bidder for all PA bidders now we only need to do it once. Hopefully it will improve things.
*   Michael: Making it slightly delayed makes the reliability a bit worse but the better latency should better make up for it so overall it should be a win.
*   David: addition of the extra stuff might be factored in for adopting separating the reportWin code from generateBid.
*   Shivani: a question on implementation detail. reportAggregatewin can be called but the report gets sent later when the ad frame starts navigation, like reportWin’s behavior? 
*   Charlie: haven’t gone through details but something like that seems fine. There's a potential privacy issue since the reportAggregateWin gets sensitive information. We might have to deal with potential issues by having a timeout. Haven’t drilled down into the details.
*   Charlie: one more thing: one thing discussed as an open item: what the format of aggregateWinSignal should be. If you have an opinion, happy to hear it on the PR. right now the thought is some JSON serializable format. Maybe it could be an arbitrary JS object if you already have access to it and you don’t want to do the JSON serialization. If you’ve been thinking about how you want to use PMT and have an opinion, I would appreciate feedback.


## [Patrick McCann] https://github.com/WICG/turtledove/issues/1338 to support B+A

Pat: In another document, the sandbox team proposed 2 workflows in prebid depending on how the API works. Didn’t see the plan on how to change the API. a little befuddled by the doc. 

Paul: change going from 1 seller to multiple sellers?

Pat: yes.

Paul: This is something we plan on supporting. Idk if there’s another github issue for this. The timeline does indicate it’s something we plan on implementing.

Pat: The question is what we should implement in Prebid.  Should we hold on to the current B&A integration, wait for this new API to be ready or not?

Paul: It’s just some optimization to accept multiple sellers.

Pat: call all sellers and give a map?

Roni: there’s time on when these things can start. I’m hearing from Paul if the difference is whether to call it in a loop and just a syntax sugar of the API. if every adapter should do it, it depends on whether or not we need to pass a list of seller origin. Idk how a priori we can know the seller origins.
Pat: We already need to do that right? 

David: You need to know if anyone needs to do B&A stuff.

Pat: the difference here is that if the map is returned we need to call it much earlier. We need to let the chrome team make the change. Seems reasonable to suspend B&A dev and wait for this.

Paul: I wouldn't recommend Prebid waiting for the new API design to roll out. Would better to continue the work until this is available and then start looking at this.

Roni: The tricky part is where the seller origin is defined. In the non-parallel flow the seller origin isn’t known until after contextual response.

Michael: The takeaway is Chrome plans to modify the API so you can call once and get blobs for multiple sellers. No plan on timeline when that’ll happen. Now you can use a for loop to simulate calling once and getting all things back. Maybe it’s best to design your code so it’s easy to move to the new API later on. Don’t know exactly when that will be available though. Don’t think it’ll be O(n) - not like this would be sped up by multiple of sellers. There’s already internal chrome optimization that makes it not take N times long. Don’t be misled to think this will be linear improv.

Pat: Is the typical case one per auction or once per page?

Michael: Once per auction is expected. Once per page is nicer.

Roni: depends on the caching semantics. 

Michael: inside chrome there's a lot of caching layers. There’s also encryption so that can be once instead of multiple times.

Pat: Do we suspend work or not? 

Paul: That's up to you.


## [Aditya Agarwal] Implementation queries around Pa-Api Video Ads

Aditya Agarwal: want to confirm that currently PA supports outstream video only?

Sid Sahoo: there’s some demo that API is able to render the video on screen. The demo doesn’t replicate the real world. Not the reality of how things work.

Michael: Is there some forum for people to talk about that further? 

Pat linked a Google Ads demo: https://github.com/google/ads-privacy/tree/master/proposals/protected-audience-video

Sid: can link the demo that the chrome team published. Welcome feedback. What Pat linked is GAds’s proposal and how they do it. If there’s specific question for how GAds does it then can comment there. 

Aditya: Is there a proposal for instream too?

Alonso: cued Pooja.

Pooja: Aditya, can you share your clients and more context? Are you interested in outstream only or also instream.

A: only outstream. Wanted to know what to change to support outstream. Thinking about implementing it. Need to scope out the work.

Pooja: for outstream, PA API can render outstream ads if they are self contained, no additional wrapping, currently.

Sid: if you’re only interested in outstream, can ignore what i said about the demo as it’s all about instream. Outstream isn’t as complicated.

Pooja: another question: can you share more about media.net. We’re not all familiar with the company.

(Some offline chat…)

Elmostapha BEL JEBBAR: there’s announcement from GAM about instream video API in Oct 2024. Have these tests been made? If yes, is there anything shared?

Alonso: Any one from GAM in this call? 

Michael: no one from GAds to talk about their work on video. Chrome people here are  not in the best position to answer it. 

Patrick McCann: GAM is running, you can observe it happening in the wild right now. If you look at the IMA SDK there’s a list of origins they are calling. Haven’t started multiseller AFAIK.

Michael: every adtech can speak for themselves. We inside chrome are happy to talk to anybody if they have problems with test running. We are not going to pass along information about adtechs running what tests. That’s in the hands of any individual adtech whether that’s google internal or external.

_Donato Borrello, Google Ad Manager Video: (joined late, adding reply to notes) GAM is currently rendering instream PA ads in a small scale experiment, using iframe rendering frame and `deprecatedReplaceInURN`. Implementation follows almost exactly the public post in [github here](proposals/protected-audience-video/README.md). Pat is correct that GAM has not implemented multi-seller yet, while we verify the simpler use-cases first. Not much to share in terms of results, other than that impressions are able to flow, video can play successfully and latency improvements will need to be prioritized as a next step._


## [Matt Davies] Billable impression event for Privacy Sandbox with calls to third party on reporting, https://github.com/WICG/turtledove/issues/1220

Matt: We spoke in multiple calls in the past. What’s the latest info about 3p reporting if you’ve got everything you need now. Workload diagram or other things? Any ETA on when that might be developed.

Michael: Could you remind us what it is?

Matt: effectively when the auction is run and won, current report ad win and so on sends off DSP and SSP. Trying to create an impression event that can be sent to multiple partners. Dsp can still get their report via reporting interface. Ssp can say if this is impression, we need to go to abcd. Ssp can call multiple impression events to whoever in the chain for the original auction.

Paul: another issue is analytic providers for someone like bidswitch involved in the auction. Not necessarily the buyer or seller.

Shivani: clarify: I looked at the diagram you posted. It seems it suggests there could be multiple parties who can receive a reserved event and a new event. Does that require any data from the ad itself? It could be report without any data from the ad? It seems it doesn’t. It’s more like info like the ad has started. We just want to do more than just for the ssp?

Matt: at the minimum. We want to know if the impression is served and the price. For billing claim. Ortb - bidswitch - DSP.. When winning, we want to know what’s spent. Nature of what we suggest, impression occurred, served, impression beacon would go to DSP and also bidswitch but doesn’t have to be bidswitch. In evaluation, we can tell there’s 2/3/4 nodes in the chain . and maybe someone else in the chain also wants to know. Info can be sent from ssp to whoever in the chain, could be 3rd party. 

Shivani: seems like an extension of the report result. If we want to make it more flexible for other events during the ad lifetime, then automatic beacons are helpful. What should this be an extension of? Helps to understand just extending the report result is sufficient. Is there data from inside the ad also required?

Michael: to add a bit and contextualize a bit, the discussion in https://github.com/WICG/turtledove/issues/1220 focused on the reserved event: automatic event triggered when the ad begins to render. We don’t have a event call for impression now. Report win is the signal of begin impression render. Distinction Shivani is making here: is it enough to get a report that impression is starting to render . that’s equivalent to reportWin made available to additional parties. If you need events other than ads start to render. Maybe ad has been on screen for a sec, viewability, instead of report win, that’s different. Our question is whether we can just use report win to handle this? Or do we need more complicated later event in the life cycle functionality.

Ivan Staritskii: Let me clarify. Currently we have restrictions .. sendReportTo is an event level report. We found out the beacon event may be a more fair impression than the impression in the send report to. For us the Main thing is to be able to trigger some event more than once. action of this event could be billable. For us the only thing is to be able to trigger the event more than once. If there’s other feedback that this can be better than sendReportTo, but like beacon, maybe it can be beacon.

Matt: Most people run on render. In our instance, some people do it on render, some people do it on viewability. It’s tricky. Can there be 2 separate beacons, one for render, one for viewability check, or is there just one?

Michael: exactly the question shivani is asking. Is it just 1 beacon sent to multiple parties then there’s one way to do it. If people want more ways to do it then there’s another way to do it. Shivani, are there more steps in the chain that need to happen for higher reliability for example for report win.

Shivani: if we want to add beacon - same reliability as reportResult/reportWin. 

Michael: from browser POV, we should think of them as the same type of beacon handled in the same loop and no difference from browser POV.

Ivan: something that can be triggered more than once, can be a beacon or something else. For ssp there’s nothing in the context of impression, nothing interesting inside the ad. Beacon may not be useful for the ssp. We just want the impression event.

Paul: Boil down to sendReportTo multiple times? With multiple different urls?

Ivan: sendReportTo works. 

Michael: Whether we allow multiple calls, multiple domains or multiple beacons works for people asking. It’s implementation details. Either addresses the ask.

Shivani: if the event is the same the ssp can only use one url. There are changes needed to allow sending more even in registerAdBeacon. 

Ivan: for beacon there's a problem because for ssp it’s required to call separate events with separate names. 

Paul: In the first comment in the issue, you have a few auction config. Multiple per buyer 3p signals. 

Ivan: That was the original proposal but we simplified now to just call multiple times. For now the last comment is more the latest thinking.

Arthur: What is the privacy budget impact of calling sendreport() twice?

Michael: no impact. multiple reports to multiple parties but they contain the same information. Privacy model: everyone in the world might as well share information. So it doesn’t impact how we think of the system.

David Tam: adjacent question. There’s different types of beacons. Do you get different signals for different beacons. Am seeing now not all beacons get the same signals. I have impression clicks , reserve, start, serve. When I look at the impression / reserve, I see some signals come across in some of them but no all of them. For impression the data is in the data payload rather than the url header/params.

Michael: Sounds like a question for shivani. Only 1 min left.

Shivani: there’s multiple variants of how the report event works. 1: data can be sent in the url that’s possible . for data sent via the ad that needs to send some APIs like SetEventDataForAutomaticBeacon. Not sure which one you’re talking about. Can talk offline in a github issue.

David: don’t see documentation. If we can point that out that’d be great.

Shivani: https://github.com/WICG/turtledove/blob/main/Fenced_Frames_Ads_Reporting.md
