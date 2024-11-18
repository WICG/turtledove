# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Nov 6, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!



1. Michael Kleber (Google Privacy Sandbox)
2. Brian May (Individual CLA commitment)
3. Shivani Sharma (Google Privacy Sandbox)
4. Roni Gordon (Index Exchange)
5. Pooja Muhuri (Google Privacy Sandbox)
6. Sven May (Google Privacy Sandbox)
7. Paul Jensen (Google Privacy Sandbox)
8. David Dabbs (Epsilon)
9. Matt Davies (Bidswitch | Criteo) 
10. Garrett McGrath (Magnite)
11. Harshad Mane (PubMatic)
12. Luckey Harpley (Remerge.io)
13. Matt Kendall (Index Exchange)
14. David Eilertsen (Remerge)
15. Orr Bernstein (Google Privacy Sandbox)
16. Laurentiu Badea (OpenX)
17. Tim Hsieh (Google Ad Manager)
18. Alonso Velasquez (Google Privacy Sandbox)
19. Sathish Manickam (Google Privacy Sandbox)
20. Abed Islam (Google Privacy Sandbox)
21. Peiwen Hu (Google Privacy Sandbox)
22. Donato Borrello (Google Ad Manager - Video)
23. Jeremy Bao (Google Privacy Sandbox)
24. Diana Torres (MLB)
25. Abishai Gray (Google Privacy Sandbox)
26. Shafir Uddin (Raptive)
27. Koji Ota (CyberAgent)
28. Isaac Foster (MSFT Ads)
29. Yanush Piskevich(Microsoft)
30. Arthur Coleman (ThinkMedium)
31. Andrew Pascoe (NextRoll)


## Note taker: Orr Bernstein


# Agenda

_Note: If you tried to join the meeting but were denied, please try again.  Sorry about that_


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here (meetings with no items may be canceled)



*   [Alonso, Google Privacy Sandbox] Proposals for changes on forDebuggingOnly and Real-Time Monitoring APIs based on user choice state for 3p cookies: 
    *   https://github.com/WICG/turtledove/issues/632#issuecomment-2455842300
    *   https://github.com/WICG/turtledove/issues/430#issuecomment-2455844071 
*   [Shivani Sharma, Google Privacy Sandbox] Gather initial feedback from ecosystem partners on instream video support with FFs: https://github.com/WICG/turtledove/issues/1318
*   [Peiwen Hu, Google Privacy Sandbox] Quick announcement: will talk about passing contextual signals to trusted key value server in the protected auction server WICG call happening right after this meeting. https://github.com/WICG /protected-auction-services-discussion/issues/27
*   


# Announcements

Privacy Sandbox folks are holding every-second-Wednesday meetings in the hour after this meeting to discuss the trusted server elements of Protected Audience work. For more information see https://github.com/WICG/protected-auction-services-discussion/issues/27.

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!


# Notes 


## [Peiwen Hu, Google Privacy Sandbox] Quick announcement: will talk about passing contextual signals to trusted key value server in the protected auction server WICG call happening right after this meeting.



*   Peiwen Hu
    *   Another WICG meeting for servers - includes B&A server and Key/Value Server for PA.
    *   Will talk about passing contextual signals from PA flow to Trusted K/V servers
    *   Would like to get feedback on people's interest and what people think about.
    *   If people would like to discuss in this forum, happy to discuss in future session of this forum as well.
*   Isaac Foster
    *   To clarify, this is the plan for the conversation in the meeting one hour from now?
*   Peiwen
    *   Yes.
*   Michael Kleber
    *   A few other meetings
        *   Meeting on the server-side of PA development, every other week including today, immediately following this one.
        *   Another call, every other week from the Microsoft EDGE folks, on Thursdays at 11:00 am eastern time (24 hours after this meeting), but not this week - the opposite Thursday.
*   (Link to that meeting is here: https://github.com/WICG/protected-auction-services-discussion/issues/27)
*   Isaac Foster
    *   Not available at this week's servers meeting, but in strong support.


## [Alonso, Google Privacy Sandbox] Proposals for changes on forDebuggingOnly and Real-Time Monitoring APIs based on user choice state for 3p cookies: 



*   https://github.com/WICG/turtledove/issues/632#issuecomment-2455842300
*   https://github.com/WICG/turtledove/issues/430#issuecomment-2455844071 
*   Alonso
    *   (Presenting https://github.com/WICG/turtledove/issues/632#issuecomment-2455842300)
    *   When Chrome detects that 3PCs are allowed in the current site, FDO and real-time monitoring - it's possible for these APIs to be unsampled or unnoised.
    *   The downsampling algorithm will stay in place, the cooldown algorithm will stay in place.
*   David Dabbs
    *   Is this a sort of, here's what we intend to do if we don't hear otherwise?
*   Alonso
    *   Like everything else, it's a proposal.
*   Roni Gordon
    *   I don't understand the third paragraph of this update.
    *   What does it actually mean in practice when you allow 3PC in the “current impression site”?
*   Michael Kleber
    *   The easiest way to describe this is in terms of how Chrome works right now - no need to make guesses about how the UI will look in the future.
    *   Right now, in Chrome, it's possible for users to have 3PCs turned on or turned off. Even if a user has 3PCs turned off, there's a little eye icon in the top-right corner of the URL bar in which users can enable third party cookies for the current site, where enabling 3PCs might fix the current site. That expression takes into consideration the full context of whether 3PCs are enabled for that site based on that combination of settings.
    *   Note, Alonso - you were pointing to two different things - FDO, sampling; and also real-time monitoring.
*   Alonso
    *   (Presenting https://github.com/WICG/turtledove/issues/430#issuecomment-2455844071)
    *   Real-time monitoring - when 3PC are allowed, the noising algorithm will be turned so that noise is set to zero.
    *   Ad tech, you'll be able to see a particular contribution.
*   Roni Gordon
    *   Equivalent signal planned for FDO, so we know if we need to be sampling or not?
*   Alonso
    *   With sampling, the algorithm will handle sampling. Do you mean that you would adjust sampling on your own?
*   Roni
    *   Not currently in cooldown/lockout state, but it's just about understanding what the nature of this is going to be so you could understand the data set. But with UCC, some user agents will be sending it all the time, and others will be subject to lockout/cooldown. Trying to make sense of this duality.
*   Alonso
    *   Today, the API has a flag that tells you whether this particular contribution would have been sampled, and this exists because we launched it in June so that ad tech could use that to understand, with the current settings for lockdown/cooldown, whether this works for analysis. Paul, if this same signal continues to be available, would this answer Roni's concern?
*   Paul Jensen
    *   Not sure, because it would indicate that a user is in lockdown/cooldown.
    *   Checking the 3PC presence might be a way to indicate, or could maybe expose it as a feature detection, but that might be redundant.
*   Alonso
    *   Roni, you're just looking at impression side, right?
*   Roni
    *   No, not necessarily. Would have to check, but do we get our 3PCs attached to these sendReportTo callbacks?
*   Paul
    *   Before the seller constructs an auction config, could check for the presence of 3PCs and indicate to determine if FDO is going to be downsampled or not.
*   Roni
    *   That's my question - will FDO provide this, or do we need to pass it back to myself?
*   Michael Kleber
    *   Not sure how to do that. In aggregatable reports case, it's easy for us to add another URL parameter. But for fDO, the URL is in the hands of the ad tech; could pass the signal to the JS environment and let the ad tech use that.
    *   I think the question is, when a caller gets a bunch of reports, some are downsampled and some are not, so they can't aggregate that without knowing which are which.
*   Paul
    *   But we can expose that in the API, right next to the worklet.
*   Roni
    *   Yes, can I get a signal that says, alongside whether the device is lockdown/cooldown, to indicate that this is unsampled.
*   Michael
    *   Yes, this seems reasonable and something we should take as a feature request.
*   David Dabbs
    *   In your fourth paragraph, mentioned cookie choice state change as the lever - that's the, I want to turn them off globally? Just want to understand the "globally" vs "site".
*   Alonso
    *   There are possible scenarios that the API will detect from one state to the other.
*   David Dabbs
    *   I think you answered it; if I joined some IGs yesterday, and then today I turned off 3PCs, then it's smart enough to know that these IGs will expire in 90 days.
*   Michael Kleber
    *   For these kinds of questions, don't have an answer nailed down, should wait until we get more of the engineering.
*   Brian May
    *   What if users turn the cookies on and then back off; do we know how this would work?
*   Michael
    *   Again, for such corner cases, we should wait for the engineering to happen and we can read back what happens. That paragraph sounds like users changing their global browser controls, and what happens when users change their settings for one site, it's not obvious what the right answer should be.
*   Arthur asked by chat
    *   what is the timeline for the engineering?
*   Alonso
    *   TBD
*   Roni
    *   Independently of when you get it on your TODO list, is this tied to user choice, tied to that decision, or implemented independently?
*   Alonso
    *   Evaluation and changes in algorithm - all of the logic that you see today would run. If the PA API team is able to implement this earlier, we'll share that.


## [Shivani Sharma, Google Privacy Sandbox] Gather initial feedback from ecosystem partners on instream video support with FFs: https://github.com/WICG/turtledove/issues mi/1318



*   Shivani Sharma
    *   As mentioned in the issue log, seeking ecosystem feedback - assumptions made while creating this design. First of the hopefully more discussions that we'll have on in-stream video support within Fenced Frames.
    *   Four aspects of the design that it goes into.
        *   1. Based on the assumption that there's a requirement to have a UI coordinated between content and the ad that goes in; transitions between ad/content and back, similar look and feel of controls, and also when users interact with the controls, e.g. adjust volume, should apply to both ad/content. The first two parts of the design - Media and Control - are related.
        *   2. How does VAST actually go to the player? Last part is about VAST tracking events and reporting in general.
        *   3. What we developed in PA for non-video ads or banner ads and applying it to video in-stream. In this case though it's not just the renderURL, but rather a combination of the video player and the VAST XML.
        *   4. Reporting in the current state, what this issue is suggesting for instream video, currently do it via FFAR; already has support for component sellers, top-level sellers and buyers as different entities. Would like to have FFAR being used for post-render reporting.
    *   Any questions?
*   Roni Gordon
    *   Curious about the third part. Looking at the non-fenced frame proposal. Generally speaking, the buyer has no role in providing the video player whatsoever. Donato mentioned it on the GitHub issue as well. Not sure that actually works.
*   Shivani
    *   Would like to hear from buyers as well. Let's say there was some coordination between buyer and video players that are integrated with PA and FFAR. Let's say there were two or three different video players, and they could add them to their renderURL.
*   Roni
    *   I'm representing seller. Could have different video player for different sellers, so can't necessarily have baked into renderURL.
*   Shivani
    *   How does this impact on k-anonymity? Would have impact on the privacy model.
*   Roni
    *   Why the original macro
*   Donato Borello
    *   Some information, e.g. volume. But otherwise, video player is a black box - why publishers don't have to care about what video player a seller is using. All video players can conform to the API.
*   Shivani
    *   Video player and VAST, and jointly k-anonymous. If we're talking about seller providing video player, would have to be very careful about privacy. If it's one seller, multiple video players, would have to be careful. Definitely can't be user-specific, would need to be global in some way.
*   Donato
    *   What if it's selected in ads.txt, so that it can't be personalized by user and can't be hard-coded by the publisher?
*   Shivani
    *   Will take this input and see how it can fit in the design in a privacy-safe manner
    *   First and second are about how the media player is synchronized, bit rate/volume - communicated on the start/end events. Underspecified what these rate limits look like; intentionally underspecified as we're still looking at how the privacy characteristics look. Limited number of bits to communicate, e.g. volume. Still TBD.
    *   If the user interacted with the fenced frame at the beginning or have autoplay granted, that's when the data coming into the fenced frame are available to the fenced frame.
*   Patrick McCann
    *   Criteria for autoplay being granted on?
*   Shivani
    *   Autoplay criteria is something the browser already has. Combination of user activation media engagement index.
*   Patrick
    *   So, not changing, whatever they are now?
*   Shivani
    *   Yes, that's correct. If the fenced frame doesn't get autoplay, then the fenced frame doesn't even get that data unless/until user activation.
    *   Second part is about the controls. In today's world, same user controls used for the content being played and ad being played. In this proposal where fenced frame is a separate boundary, there are inputs about the controls needed from the context, and from the ad itself. Combination of these two - which is why we need a separate privacy boundary - which is why it's in a Fenced Frame.
*   Matt Kendall
    *   Is the intention that VAST redirects are not supported?
*   Shivani
    *   If it's the VAST XML that needs to be wrapped by different entities.. in this proposal, we're not suggesting that seller and component seller be able to wrap the VAST XML. Today, that's used for reporting. Instead, they could register a beacon in their worklets, and reporting would happen for all of the registered events and entities.
*   Matt
    *   Thinking specifically about the ad itself, would need to be a media file, can't be another VAST document, is that correct?
*   Shivani
    *   Yes. VAST XML is provided by the buyer.
*   Matt
    *   My understanding of how this works today is that there's a daisy chain of VAST XMLs, fairly common how this works today, might be a challenge for this proposal.
*   Shivani
    *   If root XML is the buyer's, and it has all of the daisy chain inside it.
*   Matt
    *   So, same origin restriction?
*   Shivani
    *   No, but for the root XML, if it's provided
*   Donato
    *   Chaining in theory works, but not in practice, because you can't inject the renderURL.
    *   Because publisher or seller can provide the player they want, don't have to worry about the daisy chaining. Most likely seller or component seller will need to set the set of events they need to trigger. Otherwise, FFAR would be difficult to coordinate between groups.
    *   If component seller is using registered beacons, need to have engagement about what beacons would be set.
*   Shivani
    *   IAB standard could be used to coordinate what these beacons should be.
    *   Any other questions? Please add to the GH issue.
*   Alonso
    *   Sooner we get feedback on how feasible this is, the more we have internally a chance to figure out what changes we need to do. Please give the GitHub issue a read.
