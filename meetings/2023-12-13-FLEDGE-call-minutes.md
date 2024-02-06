# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 4pm UTC (during winter)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Dec 13, 2023

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Shankar Venkataraman (Jivox)
3. Brian May (dstillery)
4. Amit Gupta (Jivox)
5. Taranjit Singh (Jivox)
6. Antoine Niek (Optable)
7. Xavier Capaldi (Optable)
8. Roni Gordon (Index Exchange)
9. David Dabbs (Epsilon)
10. Harshad Mane (PubMatic)
11. Phil Lee (Google Privacy Sandbox)
12. Orr Bernstein (Google Privacy Sandbox)
13. Garrett McGrath (Magnite)
14. Caleb Raitto (Google Chrome)
15. Paul Jensen (Google Chrome)
16. Shankar Venkataraman (Jivox)
17. Isaac Foster (MSFT Ads)
18. Alex Cone (Google Privacy Sandbox)
19. Tim Hsieh (Google Ad Manager)
20. Stan Belov (Google Ad Manager)
21. David Tam (Relay42)
22. Fabian Höring (Criteo)
23. Youssef Bourouphael (Google Privacy Sandbox)
24. Sid Sahoo (Google Chrome)
25. Matt Menke (Google Chrome)
26. Alonso Velasquez (Google Chrome)
27. Jonasz Pamuła (RTB House)
28. Zach Edwards (Victory Medium)
29. Miguel Morales (Tech Lab)
30. Jacob Goldman (Google Ad Manager)
31. Matt Davies (Bidswitch (Criteo))
32. Andrew Pascoe (NextRoll)
33. Abishai Gray (Google Chrome)
34. Shivani Sharma (Google Chrome)
35. Daniel Rojas (Google Chrome)


## Note taker: Orr Bernstein


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


## Suggest agenda items here:



*   [General Announcement] Jeroune
    *   The Google Privacy Sandbox team will be hosting our next series of webinars on the Protected Audience API. This set of webinars will focus on reporting and how you can measure data related to a Protected Audience auction. The first **Americas friendly session** is happening on** Jan. 16th 3-4 pm ET**. A second **EMEA friendly session** is happening **Jan. 18th 12-1 pm GMT**. A third **Japanese language session** will be held on **Jan 30th 9-11 am JST**. To join, please register below: 
        *   AMER-friendly: [Register Here](https://rsvp.withgoogle.com/events/protected-audience-webinar-3-reporting-amer)
        *   EMEA-friendly: [Register Here](https://rsvp.withgoogle.com/events/protected-audience-webinar-3-reporting-emea)
        *   Japanese language: [Register Here](https://rsvp.withgoogle.com/events/protected-audience-office-hour-3)  
*   Isaac:
    *   Buyer/Seller Reporting Questions: https://github.com/WICG/turtledove/issues/682#issuecomment-1710965068
    *   <span style="text-decoration:underline;">Persistent Opt Outs, Maybe CHOPS - https://github.com/WICG/turtledove/issues/915</span>
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Harshad Mane
    *   https://github.com/WICG/turtledove/issues/937 Need of a debug tool in Chrome for Protected Audience like Professor Prebid Chrome Extension developed by Prebid community 
*   David Dabbs
    *   Non-official chrome [testing labels](https://developers.google.com/privacy-sandbox/setup/web/chrome-facilitated-testing) seen in the wild (e.g. “preperiod”). Are these somehow related to this Chrome code (thanks to Przemysław Iwańczak for this src reference)**:** \
https://github.com/chromium/chromium/blob/c0fcffe3bbcf87e4946dae228bd62d740568a429/components/safe\_browsing/core/browser/user\_population.cc#L91 \
`bool is_preperiod = group.find("Preperiod") != std::string::npos;`
*   Shankar Venkataraman
    *   Interest group ownership construct missing for Third Party Ad servers whitelisted by Advertiser ([#924](https://github.com/WICG/turtledove/issues/924)). We will discuss and explain the model that we would like supported in the context of Protected Audiences. 
*   Xavier Capaldi 
    *   Status on support for multi-level frequency capping (using prevWinsMs beyond a single interest group) https://github.com/WICG/turtledove/issues/138
*   Tim Hsieh
    *   Follow up discussion on Native support for FLEDGE https://github.com/WICG/turtledove/issues/265#issuecomment-1823582905. 
    *   We would like to propose an alternative option using web bundle that could potentially address both signal mixing and k-anonymity issue
*   Itay
    *   Announcement: first Protected Auction Services meeting in an hour
    *   
    *   https://github.com/WICG/protected-auction-services-discussion
*   Jonasz (RTB House)
    *   3pc deprecation timeline: https://github.com/WICG/turtledove/issues/717#issuecomment-1847118918
*   Roni Gordon
    *   Sensitive signals - https://github.com/WICG/turtledove/issues/824
*   Matt Davies (Bidswitch)
    *   Origin / Traffic shaping -<span style="text-decoration:underline;"> https://github.com/WICG/turtledove/issues/951</span>


# Notes

## https://github.com/WICG/turtledove/issues/937 Need of a debug tool in Chrome for Protected Audience like Professor Prebid Chrome Extension developed by Prebid community 



*   Harshad Mane
    *   This issue is about having debugging in Chrome like what’s available in Professor Prebid
    *   Professor Prebid is a Chrome extension developed by the community that helps to show which bidders are configured for which ad slots
    *   Already, all of the configuration that goes into the auction can be seen here.
    *   Would like to have something similar for Protected Audience as well.
    *   Someone who’s debugging would need to know what’s going on.
    *   For Professor Prebid, it uses information accessible to anyone. However, Protected Audience is a closed system.
*   Michael Kleber
    *   Is your goal to have a separate extension, or integrating into the existing Professor Prebid extension?
*   Harshad
    *   Would like to see a new extension. There’s a lot of complexity already in the Professor Prebid extension.
*   Kleber
    *   Some tradeoffs. Professor Prebid already has a lot of the detail of how auctions work.
*   David Dabbs
    *   There’s a middle ground available. 
*   Paul Jensen
    *   There’s a page that has information on Protected audience debugging on the web using DevTools: https://developers.google.com/privacy-sandbox/relevance/protected-audience-api/troubleshoot
*   Harshad
    *   The problem I’ve seen is that it’s not obvious from these tools which ad slot the auction is happening.
*   Paul
    *   That’s true. We could extend the information in DevTools to show more information about which slot and auction everything is associated with. The information in DevTools is likely accessible to an extension.
*   Kleber
    *   Is your goal to have DevTools extended to satisfy more of these needs, or to support a community-driven solution?
*   Harshad
    *   Would prefer a community-driven solution.
*   Paul
    *   One way we can support this is to improve the information we show via DevTools, for example, we could show more information about the call to runAdAuction.
*   Kleber
    *   One thing we can do is to Identify what we should add as information exposed in DevTools to support a community-driven solution.
*   Harshad
    *   Is there any concern from Chrome on showing more information in DevTools?
*   Kleber
    *   The Chrome philosophy is that as much as possible should be seen in DevTools. You should always be able to see what’s going on in your own machine.
*   Harshad
    *   Is there someone on Chrome team that can help us to proceed?
*   Paul
    *   We’ll find someone who knows the intersection of DevTools and Chrome Extensions.
*   Brian May
    *   Was going to suggest that, if Chrome could put together an extension that showed what’s happening in the auction, then someone in the Prebid community could use it as a reference implementation
*   David
    *   I heard some targeted set of things Chrome can do - low hanging fruit - e.g. key things in the timeline
*   Paul
    *   Yes, adding runAdAuction calls to the timeline, attributing other events to the different auctions.

## Non-official chrome [testing labels](https://developers.google.com/privacy-sandbox/setup/web/chrome-facilitated-testing) seen in the wild (e.g. “preperiod”). Are these somehow related to this Chrome code (thanks to Przemysław Iwańczak for this src reference): 
https://github.com/chromium/chromium/blob/c0fcffe3bbcf87e4946dae228bd62d740568a429/components/safe\_browsing/core/browser/user\_population.cc#L91
<code>bool is_preperiod = group.find("Preperiod") != std::string::npos;</code></strong>



*   David Dabbs
    *   Is this something that it’s possible to be produced by Chrome? We’re receiving these on our DSP platform from some upstream partner. Trying to determine the provenance.
*   Sid Sahoo
    *   This is produced by Chrome. It’s for the experiment that’s coming up for Q1/Q2, selection of the population needs to be random. When we say it’s 5% of traffic, it’s not an exact science as well. It’s adjusted, you then see if you’re overshooting/undershooting, and adjusted to get closer to 5%.
*   David
    *   So, anything that’s labeled with preperiod, are those going to be in the actual groups? Or is just checking that your dice rolling is correct.
*   Sid
    *   Not necessarily. Some of these may be in different groups.
*   David
    *   So, it’s helpful to add warnings to the documentation warning people not to rely on these.
*   Kleber
    *   Yes, we should add this to the experimentation guide. You should treat these undocumented labels the same as you would any other unlabelled traffic.
*   Brian
    *   Will Chrome publish something about how it’s picking these populations?
*   Kleber
    *   The populations that are being used in the Chrome-facilitated testing are using the standard Chrome randomly assigned testing infrastructure. Every browser has a random seed between 1 and 4000 on it. This 4000 buckets is the way Finch trials work in general. Trying to find good buckets to use to make sure that we’re not using ones that are imbalanced in terms of size. It’s the same infra we use for all of our A/B tests.
*   Shankar
    *   Is the trial, when you do this kind of sampling, do you need the advertiser to participate in these trials?
*   Kleber
    *   When Chrome creates the random sample, it’s before any of these other things happen. When Chrome creates the label, it’s just 1.5% of all the browsers in the world. The API is available for everybody; 1.5% will have this label attached to them. People are welcome to test on whatever traffic they want. Advertisers are welcome to use the PAAPI to create audiences on any traffic they want. These labels indicate a population that’s a good sample to test on. Having a small slice of traffic labeled might make your testing useless e.g. if your audience is particularly small.
*   Shankar
*   Kleber
    *   There are a few slices of traffic that GAM has said that they will always run Protected Audience auctions - the mode B slices and one of the five mode A slices.
*   Sid Sahoo (in chat)
    *   https://support.google.com/admanager/answer/13178817
*   Kleber
    *   This shows which slices of traffic GAM is going to use PAAPI always. In other traffic, GAM does what everyone else is doing, potentially using the PAAPI, but not necessarily.
*   Roni (in chat)
    *   ah, I remember my question -- there is a "fake" prefix that is also sent from "forcing" labels via experiment flags -- that, too, isn't documented
*   Roni
    *   There are other labels there too from the experimental flags, which add a "fake" prefix. Can the documentation be exhaustive?
*   David Dabbs (in chat)
    *   Sid Sahoo I will ask this question in the chat: are the label experiments listed as "client variations?"
*   Kleber
    *   Any labels that you see that are not listed, don’t rely on them. You can go digging through source code. But like Sid said, we should definitely update the documentation to make some progress in that direction.

## Status on support for multi-level frequency capping (using prevWinsMs beyond a single interest group) https://github.com/WICG/turtledove/issues/138



*   Xavier Capaldi 
    *   Reference a discussion in the thread from 2021, we need a frequency capping that’s beyond a single interest group. If we have IGs scoped to campaigns, we can’t frequency cap. Has there been more research on this?
*   Kleber
    *   Are you talking about a person joined into multiple IGs, which were joined from the same site? Or joined from two different sites and want to frequency cap across those?
*   Xavier
    *   Two different sites.
*   Kleber
    *   You’re right that’s not possible right now. Making that possible is not in line with how we’re envisioning privacy within the PAAPI. Every IG should have information coming only from information coming from a single site. If its bidding behavior is affected by information about a different site, then a clever person could use prevWins from other IGs to build a cross-site profile, and learn about all the websites you’ve been on. It’s true that we don’t completely lock down information sharing, e.g. the presence of prevWins at all does some accumulation of information, so it’s not rock solid right now. But this would create more cross-site information at IG auction time. 
    *   However, there is a concept of negative targeting, this is a feature we just added to the Protected Audience API recently, after some discussion in this forum. It’s a way for user behavior on one site to filter out contextually-targeted ads on another site. There’s an open issue (filed by Critero, I believe?) that’s asking about the possibility of using negative targeting for bids that came from IGs joined from a different site.
*   Alonso (in chat)
    *   https://github.com/WICG/turtledove/issues/896
*   Kleber
    *   Might gesture in the direction of what you’re alluding to. If this would be helpful to you, please chime in on the issue.
*   Alonso
    *   https://github.com/WICG/turtledove/issues/319 (negative targeting against contextual ads) is already implemented. https://github.com/WICG/turtledove/issues/896 is the the idea to use negative targeting against positive IGs.
*   Xavier
    *   I’ll take a look at them, thank you.

## Announcement: first Protected Auction Services meeting in an hour

**Protected Auction Services discussion - notes**

**https://github.com/WICG/protected-auction-services-discussion**

*   Itay Sharfi
    *   In 15 minutes, we are starting, for the first time, the meeting about trusted servers. If you’re interested, add your topic to the agenda and you’re welcome to join. Phil and I are both organizing the meeting for now.
*   Kleber
    *   I hope people will join that meeting. I have a conflict, but Paul will be joining.

## Follow up discussion on Native support for FLEDGE https://github.com/WICG/turtledove/issues/265#issuecomment-1823582905.

**We would like to propose an alternative option using web bundle that could potentially address both signal mixing and k-anonymity issue**

*   Tim Hsieh
    *   The idea is that the seller will provide a generateHTML function. As part of the HTTP response header, they can provide the WebBundle that contains any asset as referenced from the HTML.
    *   The buyer will provide a native asset as JSON in the header. They can provide the WebBundle that contains any asset as referenced from the HTML.
    *   No network access from the rendering frame. Hopefully, fewer requests that contain both contextual information from seller and rendering information from the buyer.
    *   Extension of the second proposal. Going to flesh it out. An overview of that idea.
*   Kleber
    *   Let me set the stage by comparing this to what we talked about last week.
        *   Your goal is that it should be possible for buyer and seller to combine inside the creative.
        *   The reason we were nervous about this last week is that it’s an opportunity to exfiltrate combined information to the network. Ads rendered in the way you're proposing would involve information from IG and information from surrounding publisher page to combine.
        *   The thing that you're proposing which makes it OK to combine these is that the FenceFrame has relinquished its access to the network. The problem we got stuck on last week was the resources needed for rendering — for example, what if there’s a font that’s needed, and it’s not yet available?
        *   Your new answer, which we didn't think of last week, is that all of those resources could be made available in a WebBundle.  Since that web bundle is provided along with the ad/IG, we can load it before information from the publisher page and the IG combine, before we need to give up network access.
*   Shivani (the primary engineer who’s been thinking about rendering in FencedFrames)
    *   What happens before FencedFrame renders - considering the input part, the buyer needs to provide some information back to the publisher. Anything that’s returned back from runAdAuction needs to be opaque. Need to make sure that we’re still satisfying that guarantee.
*   Tim
    *   That’s correct. The native asset will be provided as an argument for generateHTML. It’ll be running inside Chrome, in a worklet that has no network access.
*   Shivani
    *   In terms of a FencedFrame that doesn’t have network access, what happens when navigation needs to happen from that ad, and reporting?
*   Tim
    *   Good point. For reporting, FencedFrameReporting API. For top-frame navigation, we need some way for buyer or seller to declare, here’s the set of URLs to which we can be navigating to. Haven’t thought through that yet.
*   Shivani
    *   Might need to consider pre-declared navigation URL. With reporting, it would be possible to exfiltrate the combined information of seller and buyer. Would need aggregate reporting.
*   Shankar
    *   This is the protocol currently in place for video ads. The seller renders the creatives. In the context of Fenced Frames, it will be an allowlisted set of URLs that you’ll be able to tie back to the origin. The buyer wins the auction, sends the dummy asset, and the seller renders it.
*   Kleber
    *   Just to recap from last week, the fundamental issue is that if the seller-provider information is universal - not based on the publisher page or the URL - easily accommodated today. Do the combination in advance. Lots of ways that combination can happen. But if the information provided by the SSP is provided at the moment of the auction, if the information about the user is flowing into the rendering frame, it’s on the other side of the privacy guarantee we’re trying to provide. Either that information about the user does not combine, or that if it does combine, that it can’t be exfiltrated.
*   Shivani
    *   Have a capability coming up that’s in the Fenced Frame repositories, either that the FencedFrame is created with no network access, or that it starts with network access, and then it renounces network access and in exchange can access information from SharedStorage.
*   Kleber
    *   What Tim is suggesting is that the FencedFrame relinquishes its network access, and in exchange can access a WebBundle
*   Roni
    *   Also needs to work in an iframe, and we have the same problem today. Video XML in iframes before 2026.
*   Kleber
    *   What we’re talking about is easy to do today if you have code on the publisher page and can postMessage into the iframe, but if you don’t have code on the publisher page (like you are a component-seller SSP), then this is still an open issue.
