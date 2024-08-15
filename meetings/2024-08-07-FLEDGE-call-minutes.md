# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Aug 7, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!	 



1. Michael Kleber (Google Privacy Sandbox)
2. Roni Gordon (Index Exchange)
3. Pooja Muhuri (Google Privacy Sandbox)
4. Paul Jensen (Google Privacy Sandbox)
5. Orr Bernstein (Google Privacy Sandbox)
6. David Dabbs (Epsilon)
7. Matt Kendall (Index Exchange)
8. Harshad Mane (PubMatic)
9. Laura Morinigo (Samsung)
10. Isaac Foster (MSFT Ads)
11. Anthony Yam (Flashtalking)
12. David Tam (paapi.ai formerly Relay42)
13. Fabian Höring (Criteo)
14. Kevin Lee (Google Privacy Sandbox)
15. Alex Peckham (Flashtalking)
16. Rickey Davis (Flashtalking)
17. Sid Sahoo (Google Chrome)
18. Lydon, Jason (fine… Flashtalking)
19. Tamara Yaeger (BidSwitch)
20. Sarah Harris (Flashtalking) 
21. Becky Hatley (Flashtalking)
22. Owen Ridolfi (Flashtalking)
23. Bharat Rathi [Google Privacy sandbox]
24. Laurentiu Badea (OpenX)
25. Jeremy Bao (Google Privacy Sandbox)
26. Sathish Manickam (Google Privacy Sandbox)
27. Laszlo Szoboszlai (Audigent)
28. Koji Ota(CyberAgent)
29. Paul Spadaccini (Flashtalking)
30. Josh Singh (MSFT)
31. Matthew Atkinson (Samsung)
32. Shafir Uddin (Raptive)
33. McLeod Sims (Media.net)


## Note taker: Orr Bernstein	


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here:



*   Isaac Foster:
    *   Brief revisit the “coarse information sharing” thing, we had talked about setting up time but never did, all got too busy…can even answer here
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736 

*   David Dabbs
    *   Clarification on reporting values in Deals explainer: 
https://github.com/WICG/turtledove/pull/1237#discussion_r1700378453 

    *   Request for nascent header join/leave capability to support <code>[fetchLater API](https://developer.chrome.com/blog/fetch-later-api-origin-trial)</code> (entered as [comment](https://github.com/WICG/turtledove/issues/896#issuecomment-2233667864) on #896) 
 
        Chrome is [migrating](https://issues.chromium.org/issues/40236167) keep-alive request handling from the “renderer” (front-end) to the “browser” process (back-end). [Explainer](https://docs.google.com/document/d/1ZzxMMBvpqn8VZBZKnb7Go8TWjnrGcXuLS_USwVVRUvY/edit#). Attribution Reporting API (ARA) supports event and trigger header registrations on background requests, and this will move to the browser. ARA team has [extended that hook](https://issues.chromium.org/issues/40242339#comment48) so that <code>fetchLater</code> API requests will also be able to set ARA headers. Requesting that Protected Audience header join/leave capability also supports <code>fetchLater</code>.  

    *   K-Anonymity 
        According to the k-anon [doc](https://developers.google.com/privacy-sandbox/relevance/protected-audience-api/k-anonymity): 
        *   <em>In Q1 2024, for up to 20% of Chrome Stable traffic, excluding Mode A and Mode B experimental traffic, we will begin to check k-anonymity with the same parameters. 
 
            In Q3 2024, when the third-party cookie deprecation (3PCD) ramp-up process is planned to begin, k-anonymity will be checked for 100% of Chrome Stable traffic with the same parameters. </em>
        *   Will the experimental groups continue to be excluded? 
            (You are no doubt discussing the path forward with CMA, but any clarity on k-anon, which has been and I assume still is the next major PA privacy enforcement change to drop, will be welcome when you can provide it.) 



# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!! :) 


# Notes 


## Clarification on reporting values in Deals explainer: 
https://github.com/WICG/turtledove/pull/1237#discussion_r1700378453



*   David Dabbs
    *   The question in that series of comments - the read on section 1.2 is that those reporting values are mutually exclusive - but Paul said that one would get all of them. What’s the availability?
*   Paul
    *   (Presents “Deals explainer” #1237)
    *   A bit trickier. Orr had a proposal for making it simpler, but can’t without changing existing behavior. There’s a complex English description of the behavior.
    *   Today, without deal support, in today’s behavior, there are three reporting IDs - IG name, buyerReportingId, buyerAndSellerReportingId. If you put buyerAndSellerReportingId, you get buyerAndSellerReportingId. If instead you put buyerReportingId, you get buyerReportingId. If you put neither, you get IG name. Whichever one you want to get through to reporting is passed through k-anon.
    *   Originally, we thought about using buyerAndSellerReportingId and making that an array. There are two problems with reusing an old field. Not a great idea when designing an API - confuses people who used it before. Also, an exciting JS behavior, when you pass an array to a string field in WebIDL, it stringifies the array and passes it through as a string.
    *   So, we decided to leave buyerAndSellerReportingId alone and create a new field, selectableBuyerAndSellerReportingIds. buyerAndSellerReportingId and buyerReportingId are still optional string fields.
    *   New selectableBuyerAndSellerReportingIds.
    *   Before, you picked one. New behavior is a little different. We want to pick the selected_buyerAndSellerReportingId - the one you pick in generateBid - but also to be able to allow ad techs to specify buyerAndSellerReportingId and buyerReportingId. buyerAndSellerReportingId is not selectable, but still gets passed to both buyer and seller. For deals use case, this could be the seat ID. You could take the seat ID and append it to every selectableBuyerAndSellerReportingIds element, but that would bloat your IG. buyerReportingId could be private information that you don’t want to report to the seller.
    *   So, when you’re using selectableBuyerAndSellerReportingIds, you can pass these in as well. This is different from the behavior when you don’t specify selectableBuyerAndSellerReportingIds - there, you still get only one of the fields. When you do pass selectableBuyerAndSellerReportingIds, you get all of the reporting fields you provided.
    *   I’m going to add a chart - it is complicated. We have a few charts internally to make sure we’re all on the same page. Going to add a chart to show which ones you get and which ones you don’t get.
*   David
    *   To your point of private information, if I had access to the k-anonymous - when those win, all of the ads that get served are going to be k-anonymous - but all of the information I need will be in the URL. I’ll need to understand how I’m going to get them out.
    *   The actual renderUrl is not available to reportWin because the expectation is - you’ve got a buyerReportingId - you can channel anything you want through that.
*   Michael
    *   No, the renderUrl is always available.


## Request for nascent header join/leave capability to support <code>[fetchLater API](https://developer.chrome.com/blog/fetch-later-api-origin-trial)</code> (entered as [comment](https://github.com/WICG/turtledove/issues/896#issuecomment-2233667864) on #896)



*   David Dabbs
    *   Forward looking. Attribution reporting API group has hooked to be able to trigger on background fetches. Putting in a request - when Jeremy and all puts in the feature for PA to process IG events or IG API activations via headers - to be able to have fetchLater fetches process PA header in the background. That’s another way to take PA related processing off the critical path on an advertise or other site.
    *   I requested this feature as a comment on #896 - negative targeting. This is where there was a lot of discussion about processing IG joins and leaves and whatever via headers. When we have that header processing, we want it on fetchLater. 
*   Michael
    *   Can you clarify why you think that fetchLater is a good use case? As I understand it, the use case for fetchLater is - I want to know how long a user spends on my webpage - and I want to use a beacon to send it, but because I waited until the last second to send it, it’s possible that I don’t get my beacon. So, fetchLater is useful when I want to send a request just as a person is leaving the webpage.
    *   There’s nothing fundamentally I don’t like about this request, but I don’t understand why it makes sense.
*   David
    *   It’s retargeting. By virtue of visiting the site, you want to create an IG. I want to do this in the lightest possible way. Once there’s lightweight IG creation via headers, that’s how I prefer to do it. If it’s something in the background, then that’s how I’d want to do it.
*   Michael
    *   I don’t think fetchLater would be useful, since its goal is - don’t do this at all until the person is leaving the webpage. You could just issue a regular fetch.


## K-Anonymity 
According to the k-anon [doc](https://developers.google.com/privacy-sandbox/relevance/protected-audience-api/k-anonymity):



*   _In Q1 2024, for up to 20% of Chrome Stable traffic, excluding Mode A and Mode B experimental traffic, we will begin to check k-anonymity with the same parameters. 
In Q3 2024, when the third-party cookie deprecation (3PCD) ramp-up process is planned to begin, k-anonymity will be checked for 100% of Chrome Stable traffic with the same parameters._
*   Will the experimental groups continue to be excluded? 
(You are no doubt discussing the path forward with CMA, but any clarity on k-anon, which has been and I assume still is the next major PA privacy enforcement change to drop, will be welcome when you can provide it.)
*   David Dabbs
    *   Still on this timeline?
    *   Experimental groups - will they still be sent in headers? Or will the test cohorts still be excluded from k-anonymity?
*   Michael Kleber
    *   To the second question:
        *   Can we do something so that k-anonymity no longer applies to actual bidding functions but rather to reporting functions, which is what we need for actual privacy properties.
        *   That’s a whole infrastructure change that we haven’t put effort into implementing. You’re quite right that we could do this without damaging the privacy properties of the PA flows.
        *   If it turns out that this is really important to somebody, please comment on the GitHub issue. Maybe because k-anonymity thresholds are low right now, it’s not clear how important or urgent this is to anybody.
    *   To to first question:
        *   The answer right now is that we expect things to progress exactly as we talked about. We expect to increase the amount of traffic subject to the k-anonymity constraint later this year, sometime in Q3 is what that document says, and we expect that to still be in the group that doesn’t have group A/B testing labels to it. We’ll be doing that ramp up in the traffic that isn’t in those groups. No changes; all status quo.


## David Tam: Mode A/B traffic. How long will that stick around?



*   Michael Kleber
    *   We’d intended for this labeled traffic being available only during the first half of the year, during CMA testing, but there’s been some churn about how useful this labeling is.
    *   We know that we said so far that we’re going to end them in the end of July. We’ve extended them - with permission from Blink owners. They’re still a temporary thing. We’ll have to be patient a little longer to get resolution.
*   David Tam
    *   Limited amount of inventory that actually gets looked at by top-level seller.
*   Michael
    *   Question for GAM. The point of the labels and one of the reasons people have found them still useful is exactly so that there’s some traffic that everybody agrees that everybody expects a PA auction. My understanding is that GAM runs PA auctions outside of the labeled traffic. There’s two questions - one is how much traffic they’re running on, and how much traffic you can count their running on. The labeled traffic is the traffic you can count their running on, because they said they would do so. I don’t know what their future plans are.
*   David Tam
    *   How do I get an answer to this?
*   David Dabbs
    *   There’s a Google Ads experiments GitHub repository where they engage: try asking a question on https://github.com/google/ads-privacy 
*   Michael
    *   Not sure if there are GAM folks on the calls. I don’t see any of their hands. GitHub or other forums - I don’t know if there are other forums, where people who are all experimenting with PA API are having conversations that I’m not a part of.
*   Fabian
    *   Speaking from the buyer perspective - two additional reasons why the labels are useful. Useful for A/B testing. And the second reason is to save infra cost - you can use PA on those populations.
*   Michael
    *   These are both issues we’re aware of, and understand it’s useful for the ad tech community to decide something based on whether people do or don't have 3P cookies in the future, and looking into how to provide that capability. Probably won’t be these labels in the future.


## Negative targeting / headers



*   David Dabbs
    *   Jeremy - what’s your next steps on these? Explainer?
*   Jeremy Bao
    *   On the negative targeting for PA bids.
        *   Aren’t at the phase where we’re posting an explainer. Priority I’m currently working on.
    *   On the header bidding
        *   Not my priority yet
*   David Dabbs
    *   Doesn’t sound like you need more interest expressed for these, something the community wants.
*   Jeremy
    *   To make sure I get what you’re asking for - it’s allowing joinAdInterestGroup in HTTP response headers.
*   David Dabbs
    *   Yes, but with limits on IG creation. To paraphrase Michael - we (Chrome) could do that, but you can’t create a _whole_ IG. You could create a negative IG, which is lightweight, or a “shell IG”, which just has name, owner, updateURL, and minimal attributes - because it’s otherwise too long for a header. Is that right?
*   Michael
    *   Yes, there are technical limits on the size of an HTTP response header. Could use a header to create a small IG, and then use the update mechanism to fill it with ads and other attributes. 
*   David Dabbs
    *   If it’s supported in the JS API, and it makes sense and it’s lightweight enough.
    *   The other thing I’m asking for re. Header API processing is for Chrome to support deleting IGs via a header on the updateURL response. 
*   Michael
    *   If I recall the previous conversation we had about this, there’s some observation that - if your goal is to remove the IG, you could instead remove the cryptographic key, which is a field in the IG.
*   David Dabbs
    *   We covered that, but Orr pointed out that negative IGs are not updatable.
*   Michael
    *   Oh, that’s right, my mistake! So, what you’re saying, with the HTTP response header thing, in the updateUrl request, you want to be able to leave the IG that’s in the process of being updated, or being able to remove some other IG on the device, including possibly a negative IG.
*   David Dabbs
    *   That’s correct. Just remove an IG.
    *   Part of the scope of a header-based activation of those APIs.
    *   Jeremy - if you need a feature request, I could sketch that out.
*   Jeremy
    *   A feature request is still helpful. Different people asked for this feature for different reasons. Could you help write a feature?
*   David Dabbs
    *   Sure. To reiterate the conversation from a couple of weeks ago, I just want the expanded negative targeting to say, “I just want to negatively target this positive IG”.
    *   But Michael said that there are privacy reasons why this was not up for consideration.
*   Michael
    *   Not privacy - complexity. We already have positive IGs referencing negative IGs.
    *   Could reopen and change that, but as things work right now, you can only negatively target a negative IG.
    *   Do you have an answer for - what is the intended behavior if IG A negatively targets IG B which is on the browser, and IG negatively targets IG C, which is on the browser? Should IG A participate in the auction or not? The thing it’s looking for suppression is itself suppressed.
*   David Dabbs
    *   Didn’t give thoughts to transitivity.
*   Michael
    *   Having them be different things avoids the question. If you gave me an answer, I would then ask about what happens if there’s a cyclic chain. If we are going to rethink that restriction, we’ll need to get answers to all of these.
*   David Tam
    *   I was under the impression that there could only be one negative IG per positive IG.
*   David Dabbs
    *   Yes, In the current state, negative targeting with additional bids, you’re right. This convo has been talking to the expansion that Jeremy is proposing.
*   Jeremy
    *   Wrt issue #896 https://github.com/WICG/turtledove/issues/896, we’ll support negative targeting of PA bids - with a limit of 3. With negative targeting of additional bids, it’s also 3, though there’s a current discussion. There’s also a limit of 10 additional bids with negative targeting.
*   Michael
    *   If you have thoughts on how this feature ought to be used and how to appropriately give people enough flexibility to use them, please chime in on the issue.
*   David Dabbs
    *   I will take thoughts on these things and how they interact with the header mechanism and draft it into a document.
