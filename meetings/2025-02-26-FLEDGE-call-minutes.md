# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday February 26, 2025

(February 19 was cancelled because no agenda items)

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!



1. Michael Kleber (Google Privacy Sandbox)
2. Jason Lydon (Innovid)
3. Brian May (unaffiliated)
4. Matt Kendall (Index Exchange)
5. Phil Acker (Amazon)
6. Fabian Höring (Criteo)
7. Garrett McGrath (Magnite)
8. Paul Jensen (Google Privacy Sandbox)
9. Orr Bernstein (Google Privacy Sandbox)
10. Sathish Manickam (Google Privacy Sandbox)
11. Laurentiu Badea (OpenX)
12. Jeremy Bao (Google Privacy Sandbox)
13. Bharat Rathi ( Google Privacy Sandbox)
14. Kevin Lee (Google Privacy Sandbox)
15. Alonso Velasquez (Google Privacy Sandbox)
16. Suresh Chahal (MSFT Ads)
17. Trenton Starkey (Google Privacy Sandbox)
18. Kenneth Kharma (OpenX)
19. Alexander Tretyakov (Google Privacy Sandbox)
20. David Dabbs (Epsilon)
21. Sid Sahoo (Google Privacy Sandbox)
22. Tal Bar Zvi (Taboola)


## Note taker: Sathish Manickam


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which  can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here (meetings with no items may be canceled):



*   [Jeremy Bao] Deals: Share the proposal on allowing sellers to query deal ID metadata through K/V server during scoreAd(): https://github.com/WICG/turtledove/issues/873#issuecomment-2652255187 
*   [Phil Acker] What is the latest status of the dial-up plan for Real Time Metrics and Extended Private Aggregation reporting metrics rollouts? 
    *   It looks like RTM should be fully dialed up after launching with Chrome M132 in January (https://github.com/WICG/turtledove/issues/430#issuecomment-2590916564). 
        *   [Paul Jensen] RTM was rolled out with M129 to all but Mode A & B: https://github.com/WICG/turtledove/issues/430#issuecomment-2397357734
        *   [Paul Jensen] RTM was rolled out with M132 to all traffic: https://github.com/WICG/turtledove/issues/430#issuecomment-2590916564
    *   Has the Extended PA metrics also fully rolled out? (https://github.com/WICG/turtledove/blob/main/FLEDGE_extended_PA_reporting.md)
        *   [Paul Jensen] Extended PA metrics were rolled out a while ago (2023?).  We have added some features more recently: https://groups.google.com/a/chromium.org/g/blink-dev/c/F0GAisJlomc/m/aoD_b_CX[AAAJ](https://groups.google.com/a/chromium.org/g/blink-dev/c/F0GAisJlomc/m/aoD_b_CXAAAJ) which are rolling out; the roll out status can be followed on the related GitHub entries, e.g. https://github.com/WICG/turtledove/issues/1151#issuecomment-2631433357 
*   [Phil Acker] Are there any updates about Chrome’s Elevated CX for controlling cookies?
*   Fabian Höring Discuss https://github.com/WICG/turtledove/issues/1361#issuecomment-2666429313


# Announcements

Privacy Sandbox folks are holding every-second-Wednesday meetings in the hour after this meeting to discuss the trusted server elements of Protected Audience work. For more information see https://github.com/WICG/protected-auction-services-discussion/issues/27.

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!


# Notes 


## Jeremy Bao: Update on Deals https://github.com/WICG/turtledove/issues/873#issuecomment-2652255187 



*   Proposal: Allowing selectable buyerAndSellerReportingIDs as a key in trusted Scoring signals. 
*   We have heard from sellers that in PA they may have to preload the metadata, which is not ideal. We were requested so add to the signals. Requesting feedback
*   Matt Kendall: Agree with the proposal being useful.. Can the ordering of the IDs be matched to the renderURL?  
*   Orr Bernstein: Yes, it can be matched. For example the first ID will be matched to the first renderURL etc. It is also the same thing we are doing in Creative Scanning. It can’t be matched, it will be an empty string. If someone wants to select an empty string, as opposed to an empty string is what is available - that distinction is not currently supported, but we can think about it if needed. 
*   Matt: Not sure the use case where someone needs to select an empty string. We prefer to have it as one-to-one match as well. 
*   Laurentiu Badea: Some Deals may not have an URL.. so, would it match with comma, comma.. Comma? 
*   Orr: Yes.. it would be a match with that. 
*   Kleber: Thanks for the discussion. Github post is available as well for additional comments. 


## Phil Acker: Status of Real Time metrics https://github.com/WICG/turtledove/issues/430#issuecomment-2590916564



*   Should we assume that it is currently fully rolled out and most users have fully adopted. 
*   Paul Jensen: A month after a stable roll out it is ok to assume most people have adopted. There will be some people who haven’t yet restarted, so, use feature detection if needed. 
*   Phil: Do you have any updates on the roll out of the UX? 
*   Kleber: We don’t cover time or make announcements in this meeting. It will be via the usual public channels. 
*   Kleber: To follow up on Paul’s comments - Chrom has the concept of Long Term Stable, which is used for large groups to keep it on that version for a longer period of time (not the usual monthly updates). This may stay on for a while, e.g. enterprise sysadmins who want to control browser update cadence for their whole organization. So, you should assume that there will be a set of clients that will not have an update. Best to check the feature detection as needed. And folks are welcome to check the stats if needed. 


## Fabian Horing: https://github.com/WICG/turtledove/issues/1361#issuecomment-2666429313	



*   Could we get some responses to the above post? 
*   Kleber: Sure.. we can do that. As we see, you have reported on the AB tests, and this is very helpful. Feel free to add more info.. 
*   Fabian: Seller script takes about a second.. Some of this is not clear to us why.. 
*   Kleber: This may need that we need to increase the default timeout.. GAM has already done that. But we can consider this more. 
*   Fabian: It is an easy fix, but it may have an impact on auction
*   Paul Jensen: thanks for putting out these results, quite interesting. First thing to notice is that the reporting timeouts have a greater impact on reports that go out. We increased the default timeout to address this earlier. 
*   Paul: The second thing is the difference between buyer and seller reports. If you look at the chrome trace, the bidding or generate bid happens first followed by scoread. We run these in separate process for security reasons. It is resource intensive, but the separation is needed. That is the auction side. Right after, we run the report win and report result, it will reuse the processes from the buyers. There is a gap between generate bid, and if it is successful we run the reportwin. Scoread runs separately. This causes some discrepancy in the numbers because the processes for an origin will go away. When we recreate from cache, the processes, it may be slower. This will happen more often for buyers than for sellers. We have noticed it, and we are thinking about.. This difference is likely what accounts for the things you saw in your B - population
*   Paul: In your C-population, the reporting timeout does not include the fetch for reporting script. That is why there isn’t a significant difference between b and c population
*   Kleber: Thanks for the summary. To recap, there is a relatively simple fix for timeout. And we are also thinking through the process side changes on chrome internals. Thanks for bringing this up, greatly helpful to have these inputs. 
*   Fabian: It is not just runtime only..  We are interested in getting this to work as best as possible. What about group by origin? Does it apply here? 
*   David Dabbs: It is for bidding right? 
*   Fabian: It is in the same javascript.. 
*   Paul: we can’t use this since grouping across origins are not allowed… Combining reporting.. I also recommend going through the protected audience implementation document.. and it will go through a bunch of these details See link: 
*   https://github.com/WICG/turtledove/blob/main/PA_implementation_overview.md
*   Brian May: Is there a sense of an upper bound on the resource pool? 
*   Kleber: It is at about 10 right now, but if there is a large benefit on how / when to get rid of one, we can think more on this. But at this moment we don’t need that level of optimization
*   Brian: Not exactly about optimization, but the number of people participating in the auction.. 
*   Kleber: It is not about the number of people.. Any number can participate.. It is not limiting that.. 
*   David: Thanks for pointing out the document.. But if anything has changed since it was originally created, please update them. 
*   Kleber: Yes.. some of the optimizations if we incorporate, we will include them. It's a good call out for us to update and others to remind that this may be needed.


## Garrett McGrath: One of the main tenets is that  cross-site tracking is not allowed. With User choice cookies, has that changed? 



*   Kleber: With user choice, if one user wants to have 3p cookies on, and another chooses to have 3p cookies off, the browser is expected to behave differently. When a user chooses to have 3p cookies off, the original tenets will continue to remain. I don’t think any of us are under the impression that disabling cookies will automatically result in the complete end of cross-site tracking. Prevention of cross-site tracking on web platform is an ongoing work. This is only a step along the road, but not the end of it. 
*   David: The fingerprinting announcement in the public.. Is from the Google Ads, and refer to their functionality. But not a statement from Chrome or Google right? 
*   Kleber: I am not the expert in Google Ads, so not the best person to talk about. From the Privacy Sandbox side, prevention of cross-site tracking, and promoting privacy is the philosophical goal. This hasn’t changed. 
*   Brian: Will this also include the IP protection beyond incognito mode? 
*   Kleber: we have only said about incognito mode. No plans on non-incognito mode.
