# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday June 5, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	 



1. Jason Lydon (FT)
2. Paul Jensen (Google Privacy Sandbox)
3. Don Marti (Raptive)
4. Brian May (Dstillery)
5. Patrick Mccann (Raptive)
6. David Dabbs (Epsilon)
7. Garrett McGrath (Magnite)
8. Matt Menke (Google Chrome)
9. Sven May (Google Privacy Sandbox)
10. Matt Kendall (Index Exchange)
11. Roni Gordon (Index Exchange)
12. Isaac Foster (MSFT Ads)
13. Laurentiu Badea (OpenX)
14. Kenneth Kharma (OpenX)
15. Jacob Goldman (Google Ad Manager)
16. Aymeric Le Corre (Lucead)
17. Alexandre Nderagakura (Independent / Consulting)
18. Harshad Mane (PubMatic)
19. Jeroune Rhodes- Google Privacy Sandbox
20. Sid Sahoo (Google Chrome)
21. Alonso Velasquez (Google Privacy Sandbox)
22. Orr Bernstein (Google Privacy Sandbox)
23. Fabian Höring (Criteo)
24. Sathish Manickam (Google Privacy Sandbox)
25. Wendell Baker (Yahoo)
26. Alex Peckham (Flashtalking)
27. Abishai (Google Privacy Sandbox)
28. Victor Pena (Chrome)
29. Russ Hamilton (Google Privacy Sandbox)
30. Laura Morinigo (Samsung)
31. Tal Bar Zvi (Taboola)
32. Guillaume Polaert (Pubstack)
33. Arthur Coleman (IDPrivacy/ThinkMedium)
34. Pawel Ruchaj (Audigent)
35. David Tam (Relay42)
36. Yanay Zimran (Start.IO)
37. Koji Ota(CyberAgent)
38. Maybelline Boon (Google Privacy Sandbox)


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
*   Tal Bar Zvi (instead of Omri Ariav for this meeting)
    *   Follow up on easing domain restrictions (https://github.com/WICG/turtledove/issues/956) 
        *   (Roni) https://github.com/WICG/turtledove/pull/1156 resolves https://github.com/WICG/turtledove/issues/813 
*   Warren Fernandes
    *   Follow up on the proposal to support an analytics entity (https://github.com/WICG/turtledove/issues/1115)
*   Patrick McCann (_from Google Chat_)
    *   https://github.com/WICG/turtledove/pull/1156
    *   Can we expect a reasonable cap on the \* at:[ ](https://github.com/WICG/turtledove/pull/1156/file)for number of features returned in `queryFeatureSupport('\*')`?


# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

**Join Privacy Sandbox Developer Webinar: Protected Audience API Reporting**

The Google Privacy Sandbox team will be hosting our next series of webinars on the Protected Audience API. This is the fourth series of webinars covering the Protected Audience API and in this session, we will continue from the previous session and learn about reporting. The first **Americas friendly session** is happening on** June 25th 3-4 pm ET**. A second **EMEA friendly session** is happening **June 26th 12-1 pm GMT**. 

To join, please register below: 


*    AMER-friendly: [Register Here](https://rsvp.withgoogle.com/events/protected-audience-webinar-4-reporting2-amer)


*    EMEA-friendly: [Register Here](https://rsvp.withgoogle.com/events/protected-audience-webinar-4-reporting2-emea)


# Notes


## Patch’y Updates When on Joining Origin: https://github.com/WICG/turtledove/issues/1162



*   Isaac Foster
    *   Today, if you join an IG (ignoring cross-site) on example.com, you put in - among other things - an updateUrl and possibly ads and other things. Later, the updateUrl is called. UpdateUrl semantics are patchy - only updates keys you specify. Later on, the user comes back to the page - the code on page, because it doesn’t know what IGs have been registered, what their state is, rejoins the IG. Could make calls to a service for coordination, but would be nice if you could do patchy update instead of overwriting everything at IG join.
    *   Has come up in two contexts - internally, and talking with some folks about delegation scenarios. Broadly relevant.
    *   Proposal: leverage the joining origin. Allow patchy semantics joinAdInterestGroup if it’s joined from the same joining origin. Could rejoin an IG and then build from there.
    *   Intention is that this would only be applicable under the same joining origin, not suggesting we allow this across sites.
*   Brian May
    *   You create an IG. Whatever state it’s in, you call an update server, which provides some updates to it. If you then create the IG and it calls the update server again, won’t it have everything it had?
    *   If all IGs could have unique state, very difficult to understand how to interact with an interest group - if IGs are not uniform across all expressions of them.
*   Issac
    *   Are you referring to a distributed systems case where the IGs are kind of meant to be semantically the same across browsers, but they will definitely not be updated at the same point?
*   Brian
    *   An IG is an entity shared across all browser instances in which it appears, hopefully has a common definition across all browsers.
*   Paul Jensen
    *   Possible that an IG is different across different devices depending on when they were updated.
*   Brian
    *   But if two IGs hit the update server at the same time, they should have the same understanding of what the IG is.
*   Paul
    *   Could make your update server give back different results at different times. We’re not enforcing that it’s giving the same response every time. Could be adding ads to your IG as you go along. If an IG is updated at this time, and another at a later time, they could have different sets of ads.
*   Brian
    *   Difficult to reason about what it is you’re interacting with if two IGs have the same name but very different ads.
*   Isaac
    *   Problem of - can we effectively touch an IG on a page without changing features that require a call to the server - you prefer not to do that in some cases. In a multi-billion node distributed system, add a timestamp to make sure things are synced, but if you’re putting in something that’s meant to be synced in a more interesting way, that would be challenging.
*   Paul
    *   If you break the original problem into two pieces, interacts with what Brian mentioned. a) extend the lifetime, and b) not lose all the ads that may have been downloaded during an update.
    *   If you look at (b), if you rejoin the IG, which deletes the ads, but then immediately call update, could get ads back, so they’re not lost. May run into rate limiting if we updated the IG recently. But the expiration date is a different problem, and the patchiness/partial updating is kind of a different problem.
    *   The thing we’re trying to preserve - one IG contains one site worth of data - and this can be broken when we don’t do these complete overwrites. I linked to a couple of GitHub comments where this is discussed. Could have one IG that’s joined from lots of different sites, and then it becomes a product of multiple sites.
    *   We’ve encountered this multiple times, and the solution we’ve come up with is - when IGs are joined from a different site, it’s often an error, and so it’s joined anew when joined from the second site.
*   Isaac
    *   Without making a different call - different companies are going to setup different update strategies - modify the bidding signals or even the trusted signal URL - if you happened to join from different sites, then yes, it would go back to overwrite semantics.
*   David Dabbs
    *   The use case you described is an IG that’s only going to be joined on an advertiser site. But can’t you set a CHIPS cookie, and so you don’t drop the IG with a second join?
    *   Lightweight, scaffolding at join time - mechanism to ensure that the updateURL gets called - already called the update URL and is ready to go. That proposal might provide some means to get what you want.
*   Isaac
    *   Like the idea of not having to make the call. If it’s a matter of asking, please run the update if it hasn’t been run. Certainly could set something in cookie or local storage.
*   Brian
    *   It occurred to me that the person creating the IG could call the update server and create the join based on what they got back from the update server; don’t need the browser to do it on their behalf. Once an IG is updated, we probably want to have a “don’t update this IG for a specific period of time”, so it’s not constantly being refreshed, either on purpose or by accident. If the semantics of an IG creation was to call the update server immediately.
    *   I would want to try to get the IGs to be as consistent as possible across these billion nodes; can’t treat the IG updated today the same as I would the IG updated yesterday.
*   Isaac
    *   Setting local storage is fine. Having developers not need to do that - if you’re on a site, you’re navigating around - but having to make those calls on every page load instead of just being able to say, call this update unless you did so in the last two hours or whatever.
*   Paul
    *   David - you’re right to point out that what Issac is asking for is something you could do today using first-party storage. Good that we’re asking for things that are asking for convenience.
*   Roni Gordon
    *   In the interest of not trying to design it in real time, since I’m not a buyer, we have updateIfOlderThanTime, Brian has don’tUpdateIfOlderThanTime. What can be patched, what can’t be patched, and how do we do it we can leave to GitHub.
*   Paul
    *   What we’re trying to prevent is that an IG contains data from multiple sites. Any update is OK because the updateURL comes from the original site. Priority vector is a little different because it’s not exposed; but in terms of joining and updating, what we’re trying to maintain is that it’s from one site.
*   Brian
    *   Am interested in the concept of TTL. You don’t want the browser getting crowded. But do want IGs that are renewed as long as there’s a certain amount of activity related to them. Audience extension and other sorts of cross-site advertising without the cross-site data.
*   Paul
    *   We have a 30 day TTL, and it can be extended by rejoining it, but not update. Need some kind of limit on the TTL and we need to prevent mechanisms that allow it to be updated.
*   Isaac
    *   Have to drop for a different meeting. Thanks for the conversation.
*   Paul
    *   Something like this could be possible as long as we have a mechanism for preventing it from accumulating state from multiple sites. Could have something like “updateOrDelete” - would delete the IG if it had been created from a different site.
*   David
    *   Brian - to the points you’ve been making - the focus should be achieving a patchy update whether they have a conception of an IG that you do that has the IG being consistent - or whether the contents of the IG may be different based on what was on that first party site at join time. About a year ago, updateURL was removed from the k-anonymity calculus - which allows people to apply it in different ways.
*   Paul
    *   To Brian’s comment - have to assume that if ads for campaigns are changing, the potential for different versions of the same IG on different devices is possible. Can’t have a perfectly distributed coherent DB across all devices. Signals like when it was updated or what version of it has been kept on that particular device could help.


## Follow up on easing domain restrictions (https://github.com/WICG/turtledove/issues/956)



*   Tal Bar Zvi (instead of Omri Ariav for this meeting)
    *   With Taboola, following up on question on a GitHub issue
    *   Owner domain - used both to download most static files - that contain GenerateBids, ScoreAd, reportWin, reportResult, and also downloading web assemblies.
    *   And then there’s trusted server - now BYOS - that has to use the same exact DNS name as the prefix, and this kind of server usually costs more because of the dynamic call.
    *   The first is static resource, the second is dynamic resource, and they have to have the same domain name.
    *   I know you addressed this subject recently in this issue that Roni created. Is this the same issue?
*   Paul
    *   Yes, Roni filed #813. We talked about it on previous calls. It did boil down to that differentiation of resources in the interest group. Some static (CDN), some some more dynamic as you called them. Isaac mentioned a few calls ago that this was their motivation for asking on #813.
    *   We’ve been working on the solution for this. Pushed out the solution to Canary Chrome at 50% earlier this week. Should be on Chrome Dev Channel soonish maybe next week or something.
    *   Detectable with some feature detection mechanisms
    *   Should address the issue that #956 was asking about, where you might want to serve some things from static origin and other things from dynamic ones.


## https://github.com/WICG/turtledove/pull/1156


## Can we expect a reasonable cap on the \* at:[ ](https://github.com/WICG/turtledove/pull/1156/file)for number of features returned in `queryFeatureSupport('\*')`?



*   Patrick McCann
    *   Speaking of feature detection - you guys plan to cap the number of features it returns, we’re just kind of worried about sending the whole thing over the wire.
*   Paul
    *   We don’t plan to cap things.
*   Patrick
    *   In a year, is it going to be 200-300 features?
*   Paul
    *   Multiple ways we can address things. For example, could ignore things you already know about. In terms of keeping the list smaller and simpler, when we go about shipping these different features, we roll them out for testing on the early channels - canary, dev, beta, then stable some percentage, and then we turn them on by default. After that, all versions of Chrome will have them on supposedly forever. If you’re already getting the Chrome version, could omit those features that are enabled by default. Could only find out about experimental things that are enabled only for a fraction of traffic.
*   Roni
    *   As of this version, these things are available. That’s not formally available either. If I can’t infer which versions are available - what can I figure out.
*   Paul
    *   We do have the feature detection page in our repository. Put in links for the intent to ships (I2Ss), but could also put in links to the version of the CL that added it on by default.
*   David
    *   Back in the dawn of time, the release notes were useful, but you guys don’t do that anymore.


##  (Back to) Follow up on easing domain restrictions 



*   David
    *   Separating the reporting from the bidding logic. Now that we can separate host, can we take that out of the calculation?
*   Paul
    *   One of Isaac’s topics
*   David
    *   Can address another time.
*   Paul
    *   Isaac was looking at something like - many versions of a bidding script with fewer versions of reporting script - might be easier to reach k-anon with fewer versions of a reporting script.
*   David
    *   If separating out different scripts for bidding and reporting, could support this.


## Follow up on the proposal to support an analytics entity (https://github.com/WICG/turtledove/issues/1115)



*   Warren Fernandes
    *   Was hoping to get some feedback on the analytics entity proposal.
    *   Got some feedback last time.
*   Paul
    *   Some of the rows in the doc were aggregate statistics
    *   Events - when we wanted to report them.
*   Warren
    *   If we write to shared storage instead of another reporting output gate, could take advantage of a lot of existing tooling, just reading out of shared storage.
    *   Followed up on proposal - effectively on the GitHub page
*   Paul
    *   Could take a look at it tomorrow.
    *   We’ve been thinking more about integrations between Protected Audience and Shared Storage
    *   Josh filed https://github.com/WICG/turtledove/issues/1190 (Consider adding ability to read Interest Groups in Shared Storage worklets) - more related to cases of shared storage pulling directly from Protected Audience
    *   Warren - did you want to push stuff from 
*   Warren
    *   Want Chrome browser to automatically push a structured JSON object that could be picked up by some entity that could - on behalf of all sellers.
*   Paul
    *   So a push instead of a pull.
    *   Haven’t had a chance to look further, will very soon.
