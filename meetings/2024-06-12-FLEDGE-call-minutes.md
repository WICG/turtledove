# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday June 12, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	 



1. Michael Kleber (Google Privacy Sandbox)
2. Luckey Haprley (Remerge)
3. Roni Gordon (Index Exchange)
4. David Dabbs (Epsilon)
5. Matt Menke (Google Chrome)
6. Orr Bernstein (Google Privacy Sandbox)
7. Paul Jensen (Google Privacy Sandbox)
8. Jacob Goldman (Google Ad Manager)
9. Andrew Pascoe (NextRoll)
10. Matt Kendall (Index Exchange)
11. Brian Schmidt (OpenX)
12. Fabian Höring (Criteo)
13. Isaac Foster (MSFT Ads)
14. Konstantin Stepanov (Microsoft Ads)
15. Victor Pena (Google Chrome)
16. Jeroune Rhodes (Google Privacy Sandbox) 
17. Russ Hamilton (Google Privacy Sandbox)
18. Harshad Mane (PubMatic)
19. Rickey Davis (Flashtalking)
20. Paul Spadaccini (Flashtalking)
21. Becky Hatley (Flashtalking)
22. Xavier Capaldi (Optable)
23. Jason Lydon (FT)
24. Tamara Yaeger (BidSwitch)
25. Warren Fernandes (Media.net)
26. Eubert Go (Microsoft Ads)
27. Maybelline Boon (Google Chrome)
28. Laura Morinigo (Samsung)
29. Koji Ota (CyberAgent)
30. Alexandre Nderagakura (Independant / Consulting)
31. Matt WIlson (NextRoll)
32. Jeremy Bao (Google)
33. Abishai Gray (Google Privacy Sandbox)
34. Matthew Atkinson (Samsung)
35. Kenneth Kharma (OpenX)
36. Arthur Coleman (IDPrivacy)
37. Kevin Lee (Google)
38. Denvinn Magsino (Magnite)


## Note taker: Tamara Yaeger


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here:



*   Isaac Foster:
    *   Brief revisit the “coarse information sharing” thing, we had talked about setting up time but never did, all got too busy…can even answer here
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Warren Fernandes:
    *   Shared storage-based analytics entity proposal: https://github.com/WICG/turtledove/issues/1115 [Shared Storage - PAAPI Analytics Extension Proposal](https://docs.google.com/document/d/1IbHq_XMpK4Q8YqbjSsve1E_Ux_fek_TlpP8vkXMyL38/edit?usp=sharing)https://docs.google.com/document/d/1IbHq_XMpK4Q8YqbjSsve1E_Ux_fek_TlpP8vkXMyL38/edit?usp=sharing 

*   David Dabbs
    *   API change to trigger a daily update right after a JoinAdInterestGroup event (#1191) 
https://github.com/WICG/turtledove/issues/1191 


# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

**Join Privacy Sandbox Developer Webinar: Protected Audience API Reporting**

The Google Privacy Sandbox team will be hosting our next series of webinars on the Protected Audience API. This is the fourth series of webinars covering the Protected Audience API and in this session, we will continue from the previous session and learn more about reporting. The first **Americas friendly session** is happening on** June 25th 3-4 pm ET**. A second **EMEA friendly session** is happening **June 26th 12-1 pm GMT**. 

To join, please register below: 



*   AMER-friendly: [Register Here](https://rsvp.withgoogle.com/events/protected-audience-webinar-4-reporting2-amer)
*   EMEA-friendly: [Register Here](https://rsvp.withgoogle.com/events/protected-audience-webinar-4-reporting2-emea)


# Notes

Michael (Chrome)

Check the doc for announcements; (1) MSFT Edge folks reg calls about ads select API (like PAAPI); (2) Upcoming webinars on PAAPI reporting - how PAAPI interacts w various reporting mechanisms that exist for learning what happened in auctions, check registration links.

We did PATCHY updates last week. Warren has shared storage analytics proposal, also discussed last week (?) , David has API change to trigger updates, which we haven’t talked about before, let’s start there.


## David Dabbs: API change to trigger a daily update right after a JoinAdInterestGroup event (#1191), https://github.com/WICG/turtledove/issues/1191

David (Epsilon)

Google ads is the original poster, Gianni said we should bring it to Wed discussion so I tried to put something on paper. We can have some interesting convos about it. We can open the last comment; basically I tried to put more descript to one of the scenarios that Gianni described, the basic goal is to take expensive IG ‘rendering’ off the main page by joining/planting a minimal set of attributes. Owner update  URL would be required. Means would be a new attribute since Gianni’s proposal to use 2nd parameter doesn’t work because legacy 2nd calling parameter is still supported. The approach would have to be some attribute, regardless of how it’s done.  The ask would be similar to the fetchLater API, which directs event to happen after timeout OR the page is unloaded, whichever happens first, to guarantee that update will be called.

Michael

Looking at the mechanism, it all seems like a good idea. HTTP header-based ways instead of code-on-page or JS API is in line with other parts of PSB. Triggering a daily update earlier than it would otherwise happen as mech for doing quick refreshes plays nicely the way you described it, being able to have minimal amount in HTTP response and defer other work to update that happens shortly thereafter, seems to work well together. 

Obviously fundamentally we can’t add API to say whether user is already in IG or new IG. So using HTTP header to update shortly thereafter has to behave same way as server sending join request, whether user is in the IG already or has just been added. We have to assure desired behavior to avoid unintentionally overwriting stuff. I don’t think it’s a bigger problem in this flow than how it exists today, but people shouldn’t take separation of concerns as if joint action is free; the joint action will have to do over-writing of group that’s already there if it’s in the browser up to this PATCHY thing from last week. 

David

To be clear the initial baseline ask to leave the header activation off the table because base ask is what everybody is already established doing today; access to JS and providing minimal attributes that allows you to use this pattern, and off the main page have the full attribute updates. Once that’s in place the header activation it’s a nice add.  To the comment about overwriting, if someone is coming to the advertiser for the first time or returning, you don’t know whether one is overwriting - that’s how PA is. 

Michael 
You said something about guarantee to get updated even if user navigates away, we should be able to do something where the user navigating away does not disrupt the queue; but there is no way if user closes browser that we’d still do that update, or do it when they re-open. 

David

If a user goes to a pub site and there are some tabs of auction where you’re eligible, then you get the update, as long as we are guaranteed that the IG was joined back on the advertiser site, we’re good.  
 
Isaac (MSFT Ads)

Minor question, if an update were to be queued and browser closed, Why would the browser not hold onto that for later? 
 
Michael

If you quit the browser it stops doing work. If it re-opens tabs it’s not going to send out network requests that it was thinking about before. 

David

And Fetch Later doesn’t have that?

Isaac

It sounds like there isn’t a mechanism currently to say that the queue comes back after a crash.

Michael

Right, I don’t think it would be desirable for a bunch of reasons.  For one example, if a network request causes a crash and then is called again it could be a cycle, makes user unhappy loop.

David

There is an earlier [issue](https://github.com/WICG/turtledove/issues/82) asking for a header-based API for other IG methods. #1191 only addressed joinAdInterestGroup omitting the others (leaveAdIntertestGroup, clearOriginJoined) because those seem straightforward; they’re basically passing in name + owner.

Michael

Haven’t thought about if it’s important to distinguish those APIs from others.

Warren has the shared storage based analytics entity proposal.


## Warren Fernandes: Shared storage-based analytics entity proposal, https://github.com/WICG/turtledove/issues/1115 

Warren (Media.net)

Two important points, buyers don’t get auction ____ so they wouldn’t know, (2) there is not explicit handshake / recognition from the seller that they are acknowledging it exists. Any suggestions on browser signals where this can be indicated or permissioned better?

Paul (Chrome)

If you have an old website, take info, give it to someone else you have to get permission to share that info. Both sides can get the request and approve in response to share information. Don’t have great iea how to do it w analytics, perhaps auction participants can denote who they’re ok sharing their data with (which analytics writers). It seems even harder w buyers than w sellers. We added sellers capability thing where buyers say it’s ok for certain sellers to get info like # of interest groups, etc. In this proposal, what is the idea for getting auction participants to opt into sharing info?

Michael

Is all of the info that we’re talking about all stuff that sellers already get access to? Buyers deliberately when they make bids and pass info, is the info a subset of what they’re already passing to sellers? 
 
Warren

Modeled on that process, info that is already available to sellers. If there is something in there that is not available sellers that would be an oversight. 

Michael

Buyers clearly know they’re passing info to the sellers. If sellers are the only parties we need appropriately empowered to say they are w the analytics entity to pass info that addresses a lot of the concern.

Warren

Like prebid analytics it’s the pub’s choice and if they enable they screen pub pages to some frequency, there is no other direct mechanism for them to be informed. 

Michael

From the precedent POV, I don't think it’s a compelling argument from the browser dev POV. The precedent is that everything that happens in prebid context in shared JS environ in which anybody w code on page can do anything, it’s a cesspool of lack of any control over info sharing that gives browser devs the heebie jeebies. The share of info should be much more explicit than info how it moves around in 1st party webpage that we have today. 

Paul

Thinking about trusted bidding and scoring signals to go to diff origins, we’re talking about opt-in from seller, my idea would be coming from their scoring signals fetch, a header that says analytics provider can get their info. 

Warren

In the case that isn’t permission granted from seller, would you propose that it should be rejected or should the analytics engineer not see them? 
 
Michael

(1) How does seller make clear they’re ok w analytics entity seeing what happened, (2) if pub page says that an analytics entity should see what the auction and seller doesn’t agree, who wins? Pub as owner of page is the one who wins? They can say that component seller can be on their page but disclose the analytics entity. On the other hand it’s plausible that some SSPs don’t want to work w some analytics entities. Whether pubs config of analytics entity overrides and you’re kicked out of the auction seems like a delicate issue where we might need to make it optional or support both ways and let pubs decide whether it’s a precondition, or whether they’re flexible w sellers who don’t report. We’ll have to figure out something that leaves control in hands of owner but that allows for diff possibilities in relation between page owner and various sellers.

Paul

iFrames have security principle that they can’t see what’s happening an in iFrame, so we probably can’t let someone outside an iFrame what’s happening inside. May not be able to differentiate whether seller participated in auction vs participated but didn’t want to share info. 

Michael

Permissions policy model says that iFrame is allowed to load but only if it agrees to certain criteria. 

Paul

We could add very specific permission for certain sellers

Michael

Seems like right semantics; seems like pub should have an option of having that as a way to specify. 

Warren

Same thing, possibly adding indication that pub can send that indicates behavior ; require analytics entity to see bids and any seller who is not ok w that has bids rejected. 

Paul

What do we do when there’s no bids in a particular auction and we don’t ever look at seller’s scoring script. Are we considering always fetching it? 


Michael

But if it comes from response header, the usual would be request header from script; this would be set of entities that pub is asking about. 

Paul

Tricky is if we wanted add anything we’d all have to update the version

Michael

We’ll think about details on how to enable it, but we’re all aligned. I think it’s good we don’t need to figure out a separate mechanism for buyer opt-in as well. We’ll continue in github issue.

Ok we’ve done the Warren issue and the David issue and Isaac had to drop, which means we have no more agenda items?


## Blink Launch Process

David

Question – I noticed over past week a number of intent to ships that got their LGTM x3, the render size, reporting timeouts and multiple bids, curious if those are things that are already there and will be ramped up? Is there was to find out if it’s only on the feature card? Which milestone will render them fully available? 

Paul

Our general way of launching has been to start w Chrome Canary channel, we enable 50% testing on Canary. Within week dev follows that. After we’ve done that a couple weeks and make sure we don’t cause crashes, we move onto Beta. 50% same thing, check metrics for a few weeks, then move to stable 1%. After 2 weeks on stable 1%, we get more data on each stage of rollout, if it looks good at 1% stable we go to 100% stable. We do that by landing a CO that turns it on by default, tip of tree, whatever branch that is. beyond that point it will have specific point of data how it lands, and it will be on forever unless we emergency turn it off. 

Through all these diff channels we try to have feat detect mechanisms available so you can tell what’s on and available. There’s also the Chrome status entries, we should probably update those more. The feature card - chromestatus.com (?) We should update that to say when something shipped but not sure we do. 

David

It sounded like one of them was being ramped to some % stable, but it may not be the case. It already has to go then you ramp it 100%.

Paul

If we do 50% canary, 50% dev, 1% stable, 100% stable, we have to have it approved by 3 of the Blink API owners. We can never get to 100% stable until we have the intent to ship approved by 3 people.  For more details see https://www.chromium.org/blink/launching-features/ 

David  

Also the trusted bidding signals, the cross origin had updates to fix the spec. It won’t affect it’s progress, just tweaks?

Paul

We ship change, sometimes we’ll forget something, the explainer….. (?)

Roni (Index)

How do we know it’s hit 100% stable?

Paul

Before we ramp it up to 100% stable, we’ll turn it on tip of tree, if you look at that CO you can copy paste the commit hash to the Chrome dashboard and it will tell you what release that particular CO went out to, we release a canary every day. Generally we will enable it by default on – imagine we put something today; the code would be there but it wouldn’t be on. Then let’s say intent to ship approved, + 6-8 weeks from now we would turn it on at tip of tree; 8 weeks is 2 milestones later, so that milestone would be on by default for everyone. Shortly after we enable by default by tip of tree, we’ll enable using controls for all branches. It’s on by default but we can really turn it on for all stable users when code is written. 

On the github issue, we try and say when it’s going to canary and then beta and then stable, that’s a specific place for each specific feature.

Roni

I keep track of every github issue, but for other that don’t it would be very helpful to see it somewhere together.

Michael

The webby answer here is that feature detection is better than on-by-default starting at milestone-whatever. Even if we say we’re turning something on by default in M-whatever, it’s not that you can just check something is sufficiently high milestone for it to be on because there are not the browsers that make separate decisions about thinks. If Edge turns out API selection and has some feature diffs, and you think it’s a high enough milestone, you might get it wrong. Feature detection is preferable because it’ll get the answer right every time as opposed to having edge cases.

Roni

Last week we were trying to figure out where the main plus delta is; I would love to know everything about everything, but if every auction includes 200 items out of which 197 have been stable we’re losing our signal to ratio.

Paul

Last week decided to prevent this sort of accumulation of long-term features, don’t know if we’ve done that yet. Thinking about how to ensure that list is not huge also, I think Pat’s concern was about sending it to servers. You could send a hash but you need a decoder ring. Otherwise the not-on-default version is what we’re talking about.

Michael

Sensible things that go well together

Paul

May be hard to implement, but… we can try to think about that. 

Michael

**NOTE: No call next week, Wed June 19 = Juneteenth US Federal Holiday.  See you June 26.**
