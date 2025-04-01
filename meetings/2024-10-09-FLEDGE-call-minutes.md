# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Oct 9, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!	 



1. Michael Kleber (Google Privacy Sandbox)
2. Matt Menke (Google Chrome)
3. Roni Gordon (Index Exchange)
4. David Dabbs (Epsilon)
5. Paul Jensen (Google Privacy Sandbox)
6. Luckey Harpley (Remerge)
7. Alex Peckham (Flashtalking)
8. Anthony Yam (Flashtalking)
9. Becky Hatley (Flashtalking)
10. Chris Nachmias (Flashtalking)
11. Laura Morinigo (Samsung)
12. Patrick McCann (raptive)
13. Alonso Velasquez (Google Privacy Sandbox)
14. Laurentiu Badea (OpenX)
15. Courtney Johnson (Google Privacy Sandbox)
16. Patrick McCann (raptive)
17. Garrett McGrath (Magnite)
18. Matt Kendall (Index Exchange)
19. Caitlin Stahle (Google Privacy Sandbox)
20. Diana Torres (MLB)
21. Jeremy Bao (Google Privacy Sandbox)
22. Shivani Sharma (Google Privacy Sandbox)
23. Sathish Manickam (Google Privacy Sandbox)
24. Lydon, jAIson (FT)
25. Shafir Uddin (Raptive)
26. Pooja Muhuri (Google Privacy Sandbox)
27. Harshad Mane (PubMatic)
28. Hillary Slattery (IAB Tech Lab)
29. Aymeric Le Corre (Lucead)
30. Koji Ota(CyberAgent)
31. Priyanka Chatterjee (Google Privacy Sandbox)
32. Abishai Gray (Google Privacy Sandbox)
33. Sid Sahoo (Google Privacy Sandbox)
34. Yanush Piskevich(Microsoft Ads)
35. Matt Davies (Bidswitch | Criteo)
36. Jacob Goldman (Google Ad Manager)
37. Owen Ridolfi (Flashtalking)
38. Hari Krishna Bikmal (Google)
39. Rohan Bedmutha (Google Privacy Sandbox)
40. Victor Pena (Google Privacy Sandbox)


## Note taker: Sathish Manickam 


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here (meetings with no items may be canceled)



*   Anthony Yam (Flashtalking)
    *   Discussion of / update on this issue: https://github.com/WICG/turtledove/issues/1028  

*   Pat McCann: https://github.com/prebid/Prebid.js/issues/11730 and solution at https://github.com/prebid/Prebid.js/pull/12205 [also resolves https://github.com/WICG/turtledove/issues/1093 and https://github.com/WICG/turtledove/issues/851]


# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!


# Notes 


## Prelim announcements - 



*   Speakers make sure your points / comments are captured accurately in the notes. 
*   Join the WICG to contribute.


## Kleber:  Microsoft blog post on Ad Selection API - The API is in limited preview on Edge!



*   https://blogs.windows.com/msedgedev/2024/10/08/ad-selection-api-limited-preview/
*   Yanush (MSFT):  blog post has details on how to join / test. Edge folks will be holding their every-other-week call next week (10/17).


## Anthony Yam: Discussion of / update on this issue: https://github.com/WICG/turtledove/issues/1028 



*   Anthony:
    *   Post started in Feb this year. About an Ad Server IG. Today, a DSP or a media buyer, can have their IGs, bidding, and their tags on the browser. Ad Servers also provide a tag and it gets written in at that time. Having these two things independently  help w/ scaling. We build different audiences than the DSPs. We are making progress - keep independent Buyer and Creative Ad selection IGs, so the logic can run on their own. Expecting same privacy constraints on both the type of IGs - worklets and two-site data. 
    *   Ad URL has to meet the k-anon criteria. Does this make sense? 
*   Kleber: It does.. Alonso, thoughts? 
*   Alonso: We have updated a high level spec (in Sept), please take a look. There are some open questions on the issue, we can talk through them. 
*   Anthony: I can summarize some of the key questions. 
    *   Not clear on the sequencing of things: Bid selection, auction, win the auction and then this happens? Or something else? 
    *   Who will inform Chrome which ad selection logic to be used? 
*   Roni Gordon: Same comment as: https://github.com/WICG/turtledove/issues/1028#issuecomment-2397152038
    *   Render URL is no longer the URl at the time of rendering, it is swapped invisibly, which may be a problem
    *   Anthony: Even today’s logic, DSP tells which renderURL, which calls the adserver, and gets rendered. 
    *   Roni: But that URL that a seller receives still represents that ad the was shown on page – with this proposal, it no longer does
    *   David Dabbs: Yes, the URL given by the DSP is no longer used. 
    *   Kleber; Summarizing the discussion The order of operations:
        *   Bidding logic bids as usual
        *   Seller decides based on all bids and renderURLs
        *   Out of those only the winner goes through another additional process. If the bid says it wants to override the winner bid, it gets a chance to call the ad selection
*   Kleber:
    *   Anthony’s comment is that this is already how it happens in the open RTB. 
    *   Anthony: Difference is that there is a default URL.. which will get updated / swapped out to a render URL by the adserver.
    *   Kleber: We do this at the end for the winning URL, and renderURL comes before the auction may need to pass the k-anon. Browser needs to handle the fact that any URL submitted needs to pass through k-anon before it can be rendered. So, if multiple urls are submitted, it is possible that nothing passes, and we need to handle it. 
    *   Anthony: It is what happens in Shared storage as well... 
    *   Kleber: Yes, it is correct.
    *   David Dabbs: Once a renderURL wins, that win results goes into our reports, and we need to monitor, count, but win results and number of renders. If there is disparity it would be a problem. We will lose visibility. I get that Ad Servers get some mechanism, but we lose visibility - we know winning results, but number of renders are lost. 
    *   Kleber: In todays’ world, when a URL is rendered, both DSP and CAS consider that it is their decision that is rendered. Alonso’s proposal provides where the pixels are coming from and both DSP and CAS get reports. 
    *   Anthony: I don’t think the DSPs are naive in assuming that their choice of the pixels on the screen is what is shown to the user.
    *   David: Yes, we are aware of that the pixels shown are coming from CAS
    *   Alonso: Which particular field are you concerned about? 
    *   David: We don’t have visibility on if the attribution, or the actual creatives are rendered and appropriately counted. If there is some catastrophic bug, we won’t even know about it. 
    *   Kleber: The design suggestion was about which parties are expected to get reports,  to execute the reporting logic in the reporting worklet during rendering. That is handled by FF reporting mechanism This can happen irrespective of who is putting the pixels in the FF. 
    *   David: It is possible that we can do this in future. However today, we know how to retrieve ad tag, measurement tag, or other 3p tags to manage our work. Is it possible to do it in the future? Possibly I am misunderstanding what will be visible to the DSP. Today, we can properly retrieve the information from the embedder. 
    *   Alonso: Can we document this gap we are discussing? 
    *   Rohan: We are trying to understand the delta, between open RTB today, and in PA proposed changes that would be helpful. 
    *   Shivani: To confirm David’s concerns
        *   The actual URL doesn't go back to the buyer. 
        *   May be the URL can go back as part of reportEvent and combine
        *   Would that answer? 
    *   David: 
        *   We are on the hook for winning and rendering it. 
        *   We need to get both, to avoid any disparity. 
    *   Shivani: We can have some kind of automatic beacon based on the JS. but looks like the concern is bigger - that the DSP doesn’t get a chance to run their proprietary reports in the ad frame
    *   Laurentiu: As an SSP we use the render URL to extract the adomain, seat, creative ID, and brand for ad quality. 
    *   Kleber: This was the concern Roni brought up earlier - DSPs won’t know which URL is being rendered. Q to anthony: what sort of invariants , what stays the same in the way CAS works vs the changes a CAS can make for rendering today? What you are saying, seems like, once the pixels are shown, there are decisions and reporting needs to happen. The current proposal seems to allow arbitrary power to CAS. 
    *   Anthony: Appreciate the concerns Roni, David brought up. Today, there may be a scheduled ad (for example before a football game), which may not be seen before. 
    *   Kleber: What you are saying is: that there may be a campaign that is run by an advertiser on a large retailer’s website and at the time of rendering the CAS will render this. Is this accurate? Today 3p cookies help with that. What happens if there is failure? 
    *   Anthony: One thing to note that DSPs don’t render, they rely on CAS. There is a fall back. At the time of ads loading onto the container, we need the browser to use the render logic.. Our question is, does this break privacy? 
    *   Kleber: That approach is less like, we want a separate IG, and more like shared storage, right? 
    *   Anthony: No we need the IGs too, to run the ad selection logic
    *   Roni: In the current iteration of the proposal, for example, if a coca cola ad needs to be shown, we don’t have a way of knowing if a coca cola ad is being shown (same ad, may be the different creative in the rotation).. 
    *   Kleber: In the example Anthony discussed, the question is based on what the thing is seen: for example if an ad is seen in Walmart, and what is shown can it be any product in Walmart? 
    *   Roni: more than that.. Today, we can be sure it is a product  from Walmart but in the proposal, we won’t even know if it is from Walmart. 
    *   Kleber: What SSP sees is the same as the renderURL that is the winning url? 
    *   Roni: I need to relate the render URL (which no one could touch currently), with the actual URL.. 
    *   Kleber; Ran out of time.. We will chat more on this next time. Great discussion
 
