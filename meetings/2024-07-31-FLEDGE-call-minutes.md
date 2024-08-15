# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday July 31, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!	 



1. Michael Kleber (Google Privacy Sandbox)
2. Brian May (Dstillery)
3. Roni Gordon (Index Exchange)
4. Paul Jensen (Google Privacy Sandbox)
5. Omri Ariav (Taboola)
6. Fabian Höring (Criteo)
7. Harshad Mane (PubMatic)
8. Tim Taylor (Flashtalking)
9. David Dabbs (Epsilon)
10. Anthony Yam (Flashtalking)
11. Alex Cone (Google Privacy Sandbox)
12. Denvinn Magsino (Magnite)
13. Paul Spadaccini (Flashtalking)
14. Becky Hatley (Flashtalking)
15. Kevin Lee (Google Privacy Sandbox)
16. Yanay Zimran (Start.io)
17. Matt Kendall (Index Exchange)
18. Isaac Foster (MSFT Ads)
19. Felipe Gutierrez (MSFT Ads)
20. Josh Singh (MSFT Ads)
21. Laurentiu Badea (OpenX)
22. Laura Morinigo (Samsung)
23. Matt Davies (Bidswitch | Criteo) 
24. Tamara Yaeger (BidSwitch)
25. Orr Bernstein (Google Privacy Sandbox)
26. Brian Schneider (Google Privacy Sandbox)
27. Herschel Wiseman(Google Privacy Sandbox)
28. Arthur Coleman (IDPrivacy)
29. Jeremy Bao (Google Privacy Sandbox)
30. Pooja Muhuri (Google Privacy Sandbox)
31. Alex Peckham (Flashtalking)
32. Stan Belov (Google Ad Manager)
33. Lydon, Jason (Flashtalking)
34. Matthew Atkinson (Samsung)
35. Bharat Rathi (Google Privacy sandbox)
36. Andrey Prokofyev (Google Display & Video 360)
37. Maybelline Boon (Google Privacy Sandbox)
38. Sarah Harris (Flashtalking) 
39. Andrew Pascoe (NextRoll)
40. Sathish Manickam (Google Privacy Sandbox)
41. Abishai Gray (Google Privacy Sandbox)
42. Ricardo Bentin (Media.net)
43. Garrett McGrath (Magnite)
44. Victor Pena (Google Privacy Sandbox)
45. Shafir Uddin (Raptive) 
46. Alonso Velasquez (Google Privacy Sandbox)


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
    *   Request for nascent header join/leave capability to support <code>[fetchLater API](https://developer.chrome.com/blog/fetch-later-api-origin-trial)</code> 
(entered as [comment](https://github.com/WICG/turtledove/issues/896#issuecomment-2233667864) on #896) 

        Chrome is [migrating](https://issues.chromium.org/issues/40236167) keep-alive request handling from the “renderer” (front-end) to the “browser” process (back-end). [Explainer](https://docs.google.com/document/d/1ZzxMMBvpqn8VZBZKnb7Go8TWjnrGcXuLS_USwVVRUvY/edit#). Attribution Reporting API (ARA) supports event and trigger header registrations on background requests, and this will move to the browser. ARA team has [extended that hook](https://issues.chromium.org/issues/40242339#comment48) so that <code>fetchLater</code> API requests will also be able to set ARA headers. Requesting that Protected Audience header join/leave capability also supports <code>fetchLater</code>.  

    *   K-Anonymity 
        According to the k-anon [doc](https://developers.google.com/privacy-sandbox/relevance/protected-audience-api/k-anonymity): 
        *   _In Q1 2024, for up to 20% of Chrome Stable traffic, excluding Mode A and Mode B experimental traffic, we will begin to check k-anonymity with the same parameters._
 
            _In Q3 2024, when the third-party cookie deprecation (3PCD) ramp-up process is planned to begin, k-anonymity will be checked for 100% of Chrome Stable traffic with the same parameters._
     *   Will the experimental groups continue to be excluded? 
 
         (You are no doubt discussing the path forward with CMA, but any clarity on k-anon, which has been and I assume still is the next major PA privacy enforcement change to drop, will be welcome when you can provide it.) 

*   Roni Gordon:  Update on the deals proposal
    *   https://github.com/WICG/turtledove/pull/1237/files, related to https://github.com/WICG/turtledove/issues/873#issuecomment-2196882982
    *   Timeline for implementation?


# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!! :) 


# Notes 


## Low entropy client hints on Trusted Bidding Signals requests ([#1031](https://github.com/WICG/turtledove/issues/1031))



*   David Dabbs
    *   Client hints on the Trusted Bidding Signals request
    *   Requesting an update on client hints on real-time reporting
*   Paul Jensen
    *   Didn’t see a lot of motivation as to why folks wanted it
    *   Read over the Trusted Key Value Server trust model
        *   [FLEDGE Key/Value service trust model](https://github.com/privacysandbox/protected-auction-services-docs/blob/main/key_value_service_trust_model.md)
    *   The basis of it is that there are three design principles
        *   Overall flow: “The browser needs to trust that the return value for each key will be based only on that key and the hostname, and that the server does no event-level logging and has no other side effects based on these requests.”
    *   Why would we want to send more data to it?
*   David
    *   Omri Ariav may be able to answer this
*   Omri Ariav
    *   Common use case for advertisers who have an operating system they want to target.
*   Michael Kleber
    *   The bid is calculated at a particular time, a bunch of different opportunities for information to flow into the environment where you calculate your bid. At IG join time, you get unrestricted access to all of the information about this particular user and site. Another time is the contextual flow of information into the bid at the auction that the auction is taking place. You already have control, know everything you want to know, at these two times. You can use this information to decide what ads to put into the IG, and again, if you want to do some sort of cross-checking between the IG time and auction join time, the contextual information from that side of the auction is an opportunity to do so. Maybe the Trusted Bidding Server is not the best place to talk about that becoming available. On the other hand, what I heard as other people were discussing this, is that this is a question not just about the TBS, but also about the periodic IG refresh, and there it’s a very different question.
*   David
    *   Suggestion you just made makes a great segue to that. Recent discussions of people pursuing the shell IG joining pattern, if you’re not making those decisions at join time and deferring that to the update and centralizing.
*   Michael
    *   You could, at the time you’re making an IG, you can annotate in that IG, e.g. in the IG name, you can make available to you for updates in the future, e.g. by including user agent.
    *   However, not much reason not to make those low-entropy signals available at update time.
*   Omri
    *   Wondering if it’s possible to have a formal explanation on the ticket.
*   Michael
    *   Yes, I can do that.
*   Omri
    *   Where would we put these signals in the IG? In the name of the IG? Or is there a field or object?
*   Paul
    *   Yes, I think that’s the place to put it.
*   Michael
    *   For the TBS, you can always include it in a key. Including it in the name of the IG is relevant as a workaround for what David mentioned about wanting to know that information at IG update time.
*   David Dabbs
    *   Or you could put it in the update URL. You have to build the update URL.
*   Michael
    *   Oops, yes, that’s absolutely right, update URL, not IG name.
*   Paul
    *   David, you were talking about update to the user agent client hints. What’s the user case for that one?
*   David
    *   Language header, the client hints on the version. Yes, we could bake that into the update URL. If we’re not targeting a certain version, we could have not joined the IG then. If it’s not just Chrome and the version - if it’s mobile or not - if you have a line item and you want to do the bid, or more in the IG about whether it’s a mobile device. You might be producing information like keys and other things that will impact other decisions later. Mobile device or other device - similar user case for real-time reporting, have opportunity to aggregate everything, can you see if the behavior is aberrant on a particular device.
*   Brian May
    *   Important to have some information about device capabilities that naturally comes through this channel.
*   David
    *   Want to emphasize that the ask here is solely low-entropy UACH. Not looking for the granular stuff.
    *   Last thing to follow up - not the original request in 1031, which we can’t abide - but should I open a separate request for the update URL? I think I already did comment on the real-time postbacks and you have said there’s already a request for that.
*   Brian
    *   I’d like to vote for having a separate issue so that people who are interested can find it.
*   David
    *   A separate one for each? A request to provide on updateURL and [real-time reporting] postbacks?
*   Paul
    *   Different ones. I haven’t looked at how easy to implement.
*   Michael
    *   We’ll have to do our due diligence.
*   Paul
    *   As Michael said, you can do it through the keys for trusted bidding signals or through the URL for updates.
*   David
    *   It does, but it adds to IG overhead, have to jam all that stuff into the update URL.
*   Michael
    *   Possible to work around it today, more convenient if it had direct native support directly from the browser. We generally put higher priority on feature requests that are not otherwise possible over things that are possible but may be awkward to do it today, so may not be the first thing on the queue.
*   David
    *   So, priority on real-time reporting postbacks because Chrome is in complete control there so API users have no work-around.
*   Brian
    *   Suggestion to capture these exceptions on how the browser normally communicates be called out.


## Update on the Deals proposal: https://github.com/WICG/turtledove/pull/1237/files, related to https://github.com/WICG/turtledove/issues/873#issuecomment-2196882982



*   Roni Gordon
    *   Following up on gaps to the original proposal. I saw an explainer out there. Just wanted to make sure I’m connecting the dots. End of the changes, curious about when we can try it out.
*   Paul
    *   Have started work on implementation. In the coming weeks, it should be testable. I don’t think the first CL has landed yet, but we’re working on that right now.
    *   I did put up a PR to solidify that. I know, Roni, you’d asked for that, because we had Leeron’s original proposal and made some changes to that.
    *   If you’re looking closely, there were a couple of ergonomic renames of things, we’ll post on GitHub just to explain it a bit more, may not be worth getting into now. If you asked for a timeline for implementation, first change should be landing very soon, and I suspect all of it will be landing soon after that. We’ve been working on it for a couple of weeks. We’ll post back on the deals thing with the couple of ergonomic renames, and then when it’s available for testing in canary.
*   Michael
    *   As a reminder, things we land to Chrome make it very rapidly to Chrome Dev and Chrome Canary, and then once a month gets promoted to Chrome Beta channel. Chrome 129 is going to move to Beta on or around August 21; that's the earliest that something can land in Beta, but if you’re super eager to try things out before they get to Beta/Stable, try them out in Dev/Canary channels.
*   David
    *   You’d said the work is underway. Is there a buganizer item for that? Re the ergonomics, one thing that came up is, it looks like we’re putting “seat” in a particular field; is that the name of the field name?
*   Paul
    *   I don’t know if there’s a buganizer if you’re interested in following along on the CLs landing. In terms of naming things “deal” or “sealID”, we tend not to be proscriptive. We tend to build infrastructure, and people can use it however they want, can innovate from there.


## Request for `updateURL` processing to support the leaving/joining of negative interest groups via the [nascent header feature](https://github.com/WICG/turtledove/issues/896) 
(entered as issue [#1228](https://github.com/WICG/turtledove/issues/1228))



*   David Dabbs
    *   Can we negatively target a regular interest group. Orr had said, why don’t we just create a shadow negative interest group? But it seems to be more ergonomic that, at update time, we could either join or leave an negative IG. The wrinkle I found is, I’m going to be joining some groups, put next to nothing in them, going to do all of the heavy lifting at update time. Want to be able to either add the negative shadow at update time, or leave it, so it’s effectively not impacting the IG that wants to anti-target it.
*   Michael
    *   The existing work that we’ve talked about in this group is equivalent header. Very much in line of Privacy Sandbox using HTTPheaders instead of needing to run JS. The “interest group header joining” may already be in progress?
*   David
    *   Met with Jeremy, seems there’s still some discussion about it.
*   Jeremy Bao
    *   Still making progress.
*   Michael
    *   You’re saying one particular request/response on which it’s helpful to have the header thing is the request/response on which one would join a negative interest group. One important property of an IG is that a page that a person is visiting when they join an IG - it’s an essential element of the nature of an IG, the time is also very important to an IG, related to the potential longevity of an IG. So the idea of joining of an IG when a person is not actually visiting a page - because it’s an HTTP response happening in the background - presents some problems.
*   David
    *   What about leaving an IG? That’s what I’m really interest in.
*   Michael
    *   If we’re just talking about leaving an IG, then it’s much easier to say yes to.
*   David
    *   Extra-territoriality of the IG update call. Deleting or leaving would smooth out the wrinkle. If I’ve deferred the hard decisions to later, but then I call the update for the one that I join on the shadow.
*   Michael
    *   Just to make sure I understand the flow you’re imagining. The shell IG flow you’re imagining. While the user is visiting the site, you make two different shell IG joins - one for regular IG, one for negative IG.
*   David
    *   Actually three. We’re closing on the opportunity to join lazily. The IG - excluded positive - that’s the one you really want (if you could anti-target it, you would); then the IG that wants to negatively target that. And then, you need that actual negative IG, that is the shadow of the one that also may exist. Turns out the second positive IG - you can’t join it - you need to delete its negative shadow, that the other groups may be negatively targeting. That’s why three or more.
*   Michael
    *   Almost makes sense. Let me repeat it back.
    *   What you’re envisioning. Instead of a single IG, you’re envisioning a flock of three IGs.
        *   IG X - functions like a regular old IG, that has a bunch of ads and potential bidding on those ads.
        *   Negative IG - which you add somebody to anytime you add them to IG X, which is an IG that serves that the user is in IG X, so that some other IG may choose to negatively target.
        *   IG that actually takes up that negative IG on its offer. Exactly the complement of the first positive IG. Going to show to people who are not in X.
    *   Person just joined X, so now you can target people in X and people not in X.
*   David
    *   And their status may change - may be updated.
*   Michael
    *   Not completely sold that it makes sense to join IGs in sets of three in the way you’re describing. You might often want to have negative IGs that are at a different granularity than just people not in a single IG. Not sold on the creation of a positive IG that negatively targets the negative IG, but nothing inherently wrong with how you’re describing using the flow.
*   David
    *   But that’s why Jeremy is expanding the positive IGs to be able to negatively target a negative interest group.
*   Jeremy Bao
    *   Proposal already out to allow negative targeting a PA bid. In the positive IG, you can specify the negative IG you want to anti-target. I’m trying to understand with your three group proposal, what’s the additional thing the current proposal cannot solve?
*   David
    *   At the join site, if you’re deferring the full evaluation of information until update time - what the shell approach entails - because of that, you need the three IGs.
*   Michael
    *   I’ll try again to repeat this back.
    *   If you don’t want to do any decision making about how to use IGs; you merely want to record events and use those events later, then, anytime you observe an event X, you propose creating two IGs
        *   One for ads you want to target that event X has happened
        *   And One for ads that want to target that event X has not happened
        *   And now you need to create three IGs - the negative IG, the regular IG that doesn’t negatively target the negative IG, and a regular IG that does negatively target the negative IG.
    *   Everything you’re talking about happens on this same site where user did event X. If you’re talking about a bunch of stuff that all happens on a single site, you only need one IG for all of that. This person is Nike customer 123456789, then you can send the fact that event X happened to that user, and then when you get your periodic daily update, you can make all the decisions you want at that point, and it does not require a large flock of interacting IGs.
*   David
    *   This is true, but the differentiator is that the positive IG that, in the ideal world, would be negatively targeted, is something that could be joined on many sites, but the one that’s actually negatively targeted, could only ever be joined on a single site.
*   Brian
    *   Jeremy - could you add a link to the proposal you mentioned?
    *   If we provide a capability for someone to do something, we need to provide a capability for them to undo that something if they did it by mistake. So, if we allow them to delete a negative IG, we should provide a capability to restore an accidentally deleted negative IG.
*   Jeremy (in chat)
    *   https://github.com/WICG/turtledove/issues/896
*   Paul
    *   Another way to phrase that - maybe we have a capability to disable that temporarily. Maybe remove the key.
*   David
    *   No way for the IG that’s negatively targeting the other one can know about its state or whether its active. It’s only the update to the positive IG that might be joined in various places that needs the negatively targeted shadow.
*   Michael
    *   What Paul is suggesting is that instead of deleting the negative IG, delete the key that makes that negative IG work, and then you’ve disabled the negative targeting capability.
*   David
    *   But they can’t have updateURLs.
*   Jeremy
    *   But if we allow negative IGs to be updated, that may make your use cases simpler?
*   Michael
    *   Will make the delete/undelete operation simpler.
*   Orr
    *   Indeed, negative IGs can’t be updated; was considered but decided against allowing update because it’s a lot of network traffic for a lightweight object that has no fields that typically need to be updated.
*   Brian
    *   If it can be turned off instead of destroyed, that would solve things for me. The use case I’m thinking about is, somebody doesn’t know what they’re doing, wipes out a subset of negative IG population, and we’d like to recover from this mistake.
