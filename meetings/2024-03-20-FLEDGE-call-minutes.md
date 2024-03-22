# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 4pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


### NOTE FOR EUROPEAN PARTICIPANTS: US clocks have changed, so this meeting will be 1hr earlier than usual (i.e. 4pm Paris time, not the usual 5pm) for the rest of March.


# Next video-call meeting: Wednesday March 20, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	 



1. Michael Kleber (Google Privacy Sandbox)
2. David Dabbs (Epsilon)
3. Roni Gordon (Index Exchange)
4. Arthur Coleman (OnlineMatters)
5. Andrew Kwok (Criteo)
6. Shankar Venkataraman (Jivox)
7. Harshad Mane (PubMatic)
8. Tim Hsieh (Google Ad Manager)
9. David Eilertsen (Remerge)
10. Paul Spadaccini (Flashtalking)
11. Becky Hatley (Flashtalking)
12. Paul Jensen (Google Privacy Sandbox)
13. Orr Bernstein (Google Privacy Sandbox)
14. Sven May (Google Privacy Sandbox)
15. Shivani Sharma (Google Privacy Sandbox)
16. Garrett McGrath (Magnite)
17. Patrick McCann (Raptive)
18. Jacob Goldman (Google Ad Manager)
19. Alex Peckham (Flashtalking)
20. Chris Nachmias (Flashtalking)
21. David Tam (Relay42)
22. Laurentiu Badea (OpenX)
23. Brian Schmidt (OpenX)
24. Taranjit Singh (Jivox)
25. Przemysław Iwańczak (RTB House)
26. Shafir Uddin (Raptive)
27. Daniel Rojas (Google Chrome)
28. Jason Lydon affiliated with the Flashtalking East SIIIDE gang
29. Sathish Manickam (Google Privacy Sandbox)
30. Hillary Slattery (IAB Tech Lab)
31. Maybelline Boon (Google Chrome)
32. Zach Edwards (Victory Medium)
33. Martin Karlsch (remerge)
34. Miguel Morales (IAB TechLab)
35. Russ Hamilton (Google Privacy Sandbox)
36. Alexandre Nderagakura
37. Tim Taylor (Flashtalking)
38. Sarah Harris (Flashtalking)
39. Caleb Raitto (Google Chrome)
40. Matt Menke (Google Chrome)
41. Andrew Pascoe (NextRoll)
42. Owen Ridolfi (Flashtalking)
43. Wendell Baker (Yahoo)
44. Luckey Harpley (remerge)
45. Alonso Velasquez (Google Privacy Sandbox)
46. Anthony Yam (Mediaocean)


## Note taker: Orr Bernstein


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here:



*   Fabian Höring:
    *   Short term options & MACRO replacement for VAST/Native workflow
    *   Initial Chrome Demo: https://privacy-sandbox-demos.dev/docs/demos/instream-video-ad-multi-seller/
    *   https://github.com/google/ads-privacy/tree/master/proposals/fledge-formats-rendering
    *   https://github.com/WICG/turtledove/issues/265 \

*   Anthony Yam (For 3/20, need Michael K and Anthony):
    *   Revisit / discuss this issue https://github.com/WICG/turtledove/issues/1028#issuecomment-1942746203 
*   Isaac Foster:
    *   ~~clarify deprecatedReplaceInUrn breadth of support~~
    *   Improved Introspection: https://github.com/WICG/turtledove/issues/1043
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Roni Gordon
    *   https://github.com/WICG/turtledove/issues/1088 - renderSize in scoreAd’s browserSignals
*   Laurentiu Badea
    *   <span style="text-decoration:underline;">https://github.com/WICG/turtledove/issues/1090</span> - component seller reporting in multi-currency auctions


# Announcements

The Microsoft Edge folks are starting up every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  First one is 24hrs from now.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.


# Note

[Michael Kleber]



*   Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


# Fabian Höring: Short term options & MACRO replacement for VAST/Native workflow



*   Initial Chrome Demo: https://privacy-sandbox-demos.dev/docs/demos/instream-video-ad-multi-seller/
*   https://github.com/google/ads-privacy/tree/master/proposals/fledge-formats-rendering
*   https://github.com/WICG/turtledove/issues/265
*   Andrew
    *   I can talk about this.
    *   Discuss VAST video proposals 
    *   Glad to see Tim is here, as the originator of one of the proposals
    *   Implications for kanonymity when we want to render native ads or video ads
    *   You had said that macros replacement won’t work for hostname - is there a way to restore this functionality?
*   Michael Kleber
    *   I said something incorrect when we talked about this last time
    *   Stated that you could not use deprecatedReplaceInURN for this.
    *   Turns out that was wrong - two different macro formats - one involving curly braces and one involving percent signs.
    *   Turns out that you can do the hostname replacement today.
    *   Changing the hostname of a render URL is a big deal - party doing the bidding has less confidence on what’s going to render, because everything can change all the way down to the render URL.
    *   The way we’re talking about allowing macro substitutions in the future is specifying these directly in the config. Then, it’s possible for the seller to make sure that the macro replacement is something that they intend, and not that some other script on the publisher page changed some part of the render URL.
    *   Some risk of other parties meddling with the data that seems riskier than we want for the Protected Audience API.
    *   We would like to support this idea of being able to have the same render URL come from the buyer and have it rendered by lots of different sellers who can do the appropriate kind of transformation to make VAST style video rendering actually work
    *   Exact details are still somewhat in progress. Several designs that will certainly work. We will ship a way of specifying those transformations that is safer than what is possible to do right now.
*   Andrew
    *   Totally get the security aspect of this. If we put this in the context of one of the proposals from Tim on how to do native ad or video ads, there’s an existing risk in potentially malicious sites.
*   Michael
    *   Definitely an extra risk that we can make better
*   Roni
    *   Wanted to understand the nature of the safety tht you’re talking about for the replacement. The auction config is not guaranteed to be unmodified.
*   Michael
    *   Quite right - the auction config is a point of risk, because the auction config passes through the publisher page, and as many people in this call know, the JS environment of the publisher page is a potential cesspool of third party scripts that can monkey patch the environment and modify things.
    *   The basic thing we can offer now is that the ScoreAd function can see the auction’s config. That page gets to look at its auction config, and if the config looks fishy, like weird surprising macro replacements proposed, it can choose to not bid in this auction.
    *   That’s not possible using the current mechanism using deprecatedReplaceInURN API, which runs entirely after the auction is over, no scripts that run there, no opportunity for seller to verify that the replacement is one that they’re happy with.
    *   Future allows seller to be more confident while still allowing URL hostname replacement to solve the video rendering problem.
*   Roni
    *   Have yet to speak to the proposal, but that makes sense for now.
*   Patrick McCann
    *   Idea of an immutable auction config is unusual, not something that we anticipated.
*   Michael
    *   Agree that nobody should pretend that the auction config is immutable. Even in the browser, it’s a JS object, serialized and deserialized. Agree that doing a checksum is not going to work, but if you have a specific field you want to validate, you can look for surprising values and decide if it’s been modified in an unexpected way.
*   Tim Hsieh
    *   While deprecatedReplaceInURN right now can replace hostname, we shouldn’t depend on it right now because it’s likely to be replaced in the very near future?
*   Michael
    *   You’re right that it’s possible in one particular way to do the hostname replacement right now. That’s a way that we feel is unsafe to us. We want to talk about offering a safer way, and when we do so, we should probably improve safety by removing the unsafe way. At some point in the future, we’ll want to make sure that everybody is using the safe version instead of the unsafe version. We do our best to remove any functionality as responsibly as possible, lots of communication involved.
*   Tim Hsieh
    *   Can we assume that there will be some way of specifying a macro in which the hostname can be changed by the seller later? In the short term, we can use deprecatedReplaceInURN(). In the future, we will use the safer way. 
*   Michael
    *   Yes.


# Anthony Yam (For 3/20, need Michael K and Anthony): Revisit / discuss this issue https://github.com/WICG/turtledove/issues/1028#issuecomment-1942746203 



*   Anthony presenting.
    *   (TODO: If Anthony sends a PDF of the presentation, link it here.)
    *   Joint Interest Groups and creative
    *   Had a chat about this in GitHub
    *   Overall idea here is that - if you look at IGs today - the ownership is the DSP
    *   We posted an issue about this #1024 - didn’t get any feedback
    *   Why is the IG owned by DSP? The feedback was that this doesn’t really mean ownership, it’s just who is doing the bidding
    *   The owners are the advertiser or the publisher
    *   Should be able to delegate responsibilities such as buying media to a buyer and displaying creative to creative parner
*   Michael
    *   Apologize that the relevant field in IG is called owner
    *   It’s been called owner since we proposed anything remotely resembling this API in 2020 - it’s not a very good name.
    *   It’s not easy to change names in APIs once they’re heavily used.
    *   It should be called “bidder” or “buyer”
*   Anthony
    *   Aren’t looking to change everything Hopefully want to make it add additive.
    *   Ad structure
        *   Advertiser -> Campaign -> Line item/ad group/placement -> Ad -> Creative.
        *   That’s the part that’s missing from the plan is the creative.
        *   We have ad URLs and ad components, but the creative is kind of missing right now.
*   Michael
    *   I am bad at naming things.
    *   I would have said that the thing you’re calling creative here is the thing we’ve called renderURL. Best analog for that concept inside PA right now.
*   Anthony
    *   RenderURLs are 1:1 with ads right now.
    *   It’s not the naming, but there’s another level here.
    *   Maybe this is not how it should always be, but that’s kind of how things work right now.
    *   There tends to be this structure overall.
    *   We’re proposing that there should be a separation between creativeLogicURL and creativeURLs.
    *   creativeLogicURLs should have access to publisher data, key-value stores, IGs, shared storage
    *   creativeURLs instead of - or in addition to - ad URLs.
    *   I know it’s late in the game here. If you’re a DSP that don’t use creatives, you can still use adURL, but otherwise, we can add creativeURLs.
    *   This should be a responsibility for ad server or DCO partner to specify things like their product idea name image and lay that all out and make that visible for the user.
    *   Finally, on shared storage - selectURL - that’s where we’ve been pointed to. But why are we doing that twice? Why are we making PA IGs in one place and then again later using shared storage? Why can’t creative ad server be able to access IG data? In the context of a PA auction, why shouldn’t we access publisher data and shared storage and other things like that?
*   Taranjit Singh (in Google Meet chat @11:42 AM)
    *   gist of it probably...the logic to pick the bid needs to be different from the logic to pick the creative...DSP can own/assist in former case but advertiser would need independent control e.g third party ad server to pick the right "dynamic" creative which has nothing to do with bidding logic.
*   Alonso
    *   Trying to understand the taxonomy
    *   You’ve made a distinction between ads and creative
    *   Is it fair to say, is there a many-to-many relationship between ads and creatives?
    *   Also, what’s an example of metadata? How would that work with creatives?
*   Anthony
    *   Yes, there’s typically a many-to-many relationship between ads and creatives.
    *   Creative metadata - would be another level of being able to affect the buying. Ads can have schedules or other things like that on them, bid factors. On a creative, geography, and audience, and things like that.
*   Alonso
    *   You said factors that affect bidding logic is part of ad metadata, but not part of the creative metadata?
*   Anthony
    *   Could be a world in which the creative is part of the bidding factor, everything’s working together, maybe this is the opportunity or not.
*   Alonso
    *   So, the value of having this creative object as its own has to do with reusability? Maybe a lot of where the value comes from is having a different creative object. Do you agree with that statement?
*   Anthony
    *   Yes.
*   Shankar
    *   Add to what Anthony said
    *   Creative side - to give you more examples of what the creative metadata could look like - e.g. product images, or even personality images. For example, Kobe Bryant was used in a Nike ads, after the disaster happened, all Kobe Bryant images were removed from those ads for the next five days or so, but the ads could keep running.
*   Michael
    *   One of the last things Anthony said resonated with me, which is that creative selection happens, and then bid price happens that’s informed by creative selection. That’s more similar to how we in this group have been talking about bidding, because we’ve heard that … (VC crashed)
*   Roni
    *   Would you agree that the logic to pick the bid would precede the logic to pick the creative?
*   Shankar
    *   This is how it works today
*   Roni
    *   
*   Anthony
    *   In an ideal state, it would be joint.
*   Michael
    *   And we’re back!
    *   Had a comment about the relative order of choosing the bid and choosing the creative is a little odd and something of a historical anomaly. My goal is not to fix problems with ad tech history, but rather to make things work given where they are today.
*   Anthony
    *   Maybe, where we should eventually go is that there’s an AI that looks at everything at the same time. But that’s for the future.
*   Michael
    *   For the privacy point of view, there’s an essential difference between the bidding functionality and the creative choosing functionality, which means as a browser, we have to keep the two things separate. In terms of bidding, we want to keep the amount of information leaving the browser small. In terms of creative selection, lots of stuff is leaving the network anyway, e.g. to load the creative. Choice of render URL based on one site of information. K-anonymity is the tool we have available right now to do this, but it’s not by itself a complete answer, and that the bidding selection is based only on one site’s worth of data is an important layer of the privacy protections.
    *   Some discussion happening right now on how we might enable dynamic creative selection. Happening in the context that our colleagues at Microsoft have proposed a slightly modified version of the Protected Audience auction that they’re interested in incubating and experimenting with inside of Microsoft Edge. One of the differences between the protected audience API in Chrome and Edge is the ability to do dynamic creative selection at auction time. Being able how to still do k-anonymity is one thing they’re still trying to figure out how to do. No slam dunk solution for how to make that change and still keep things private.
    *   Separation of concerns question of how can we let one party be in charge of bidding logic of an IG and a different party be in charge of the creative selection logic of an IG.
*   Anthony
    *   Why is there this presumption that there’s many sites worth of information?
    *   We’re only asking for information from publisher and information from advertiser.
    *   That’s two sites, it’s a small number of sites.
*   Michael
    *   In PA world, it’s true that you get to make bidding decisions based on two sites worth of data. But at the same time, we need to make sure that the stuff that leaves the browser and goes to untrusted servers, like rendering and reporting, reflects the user’s identity on both of those sites.
*   Anthony
    *   That’s why we would propose that when you do something like selectURL, and want to access information, you can only do that based on one site.
*   Michael
    *   Yes, but there’s still two site’s of information. We need to avoid joining up the two sites of information about the user. Even after the auction is over, if I later visit Nike, and then I visit NYTimes, we don’t want to join up those two pieces of information. The privacy goals of the PA is that, even after the ad renders, you can’t join up those pieces of information. If you get to choose your render URL based on knowing who I am on both sites, that adds a lot more risk to being able to joining those two sites of information. K-anonymity helps with this.
*   Anthony
    *   Happy to narrow it down to 8 URLs. Shouldn’t we be able to access the publisher data and IG data and then narrow it down to a small number of URLs?
*   Michael
    *   The way it works down is the narrowing down from many URLs to 8, that’s done based on one site of information. Going from 8 to 1 can be done on multiple sites of data.
*   Anthony
    *   This whole thing is DSP centric. If you have a DSP, you can access all the publisher information from multiple advertisers, and you have access to the creative. Why can’t there be two parties that are working for an advertiser to achieve this goal?
*   Michael
    *   I think we have a lot of flexibility in having different parties perform different parts of the role of picking an ad and bidding based on the ad. This is not how we’ve built it, but there’s no philosophical problem with it. But today, reducing to a small number of URLs is based only on one site of data, and then doing actual selection based on a small number of URLs based on multiple sites of data.
    *   I understand that making the separation of concerns is exactly the direction that will make this compatible with the privacy goals.
*   Taranjit Singh (in the Google Meet chat @11:56 AM)
    *   any specific challenge wrt allowing the update/population of same IG by different/two parties (DSP and creative partner for respective sections within IG json) whitelisted by the advertiser? the renderUrl can be treated the same - as it would happen in today's case if DSP were to populate both the sections. 


# Michael Kleber: Announcement that The Microsoft Edge folks are starting up every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.



*   The Microsoft proposal that I mentioned earlier is also starting up having meetings that are happening every other week. People who are interested in the Protected Audience API might very well be interested in the Microsoft version of this. I would encourage you all to check this out.
    *   https://github.com/WICG/privacy-preserving-ads/issues/50
