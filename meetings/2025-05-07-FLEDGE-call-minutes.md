# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California and 5pm Paris time.

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday May 7, 2025

Note no call on April 30, because no agenda

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!



1. Michael Kleber (Google Privacy Sandbox)
2. Paul Jensen (Google Privacy Sandbox)
3. Brian May (unaffiliated)
4. David Dabbs (Epsilon)
5. Garrett McGrath (Magnite)
6. Kevin Lee (Google Privacy Sandbox)
7. Orr Bernstein (Google Privacy Sandbox)
8. Patrick McCann (Raptive)
9. Sven May (Google Privacy Sandbox)
10. David Eilertsen (remerge)
11. Alex Cone (Google Privacy Sandbox)
12. Jacob Goldman (Google Ad Manager)
13. Matt Menke (Google Chrome)
14. Fabian Höring (Criteo)
15. Miguel Morales (Tech Lab)
16. Isaac Foster (MSFT Ads)
17. Andrew Pascoe (NextRoll)
18. Matt Kendall (Index Exchange)
19. Lydon, Jason (Innovid)
20. Owen Ridolfi (Innovid)
21. Karin Gamus (Innovid)
22. Don Marti (Raptive)
23. Youssef Bourouphael (Google Privacy Sandbox)
24. Martin Pal (Google Privacy Sandbox)
25. Xavier Plantaz (Google Privacy Sandbox)
26. Abishai Gray (Google Privacy Sandbox)
27. Jeremy Bao (Google Privacy Sandbox)
28. Lionel Basdevant (Criteo)
29. Bharat Rathi ( Google Privacy Sandbox)
30. Tim Hsieh (Google Ad Manager)
31. Shafir Uddin (Raptive)
32. Pooja Muhuri (Google Privacy Sandbox)
33. Sathish Manickam (Google Privacy Sandbox)
34. Eyal Segal (Taboola)
35. Manny Isu (Google Privacy Sandbox)
36. Alex Peckham (Innovid)
37. Martin Karlsch (remerge)
38. Alonso Velasquez (Google Privacy Sandbox)
39. Zahiar Ahmed (Innovid)
40. Alberto Medina (Google Privacy Sandbox)
41. Alex Turner (Google Privacy Sandbox)
42. Kristina Baron (Eyeota, a Dun & Bradstreet Company)
43. Sid Sahoo (Google Privacy Sandbox)
44. McLeod Sims (Media.net)
45. Trenton Starkey (Google Privacy Sandbox)
46. Peiwen Hu (unaffiliated)


## Note taker: Manny Isu


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which  can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here (meetings with no items may be canceled):



*   Alexandra Peckham: Can we talk about [the announcement from 4/22?](https://privacysandbox.com/news/privacy-sandbox-next-steps/)
*   McCann - https://github.com/WICG/turtledove/issues/1436
*   McCann - https://github.com/WICG/turtledove/issues/1435


# Announcements

Privacy Sandbox folks are holding every-second-Wednesday meetings in the hour after this meeting to discuss the trusted server elements of Protected Audience work. For more information see https://github.com/WICG/protected-auction-services-discussion/issues/27.

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!


# Notes 


## Topic #1: Alexandra Peckham: Can we talk about [the announcement from 4/22?](https://privacysandbox.com/news/privacy-sandbox-next-steps/)



*   [Alex Cone]: Will no longer pursue a new User Choice prompt for 3PC. Will keep as is. We want to hear from the ecosystem about where they see value in the work done and potential value in work yet to be done.
*   [Alexandra] I want to get a sense of understanding on what the next steps are and also, what are your instincts on the future of the API? Is it platforms first or partners? What is truly the plan? What should we expect in the coming weeks / months?
    *   [Alex Cone] The current plan is to use current channels which include this forum, also IAB Tech Lab (meeting on 19th May). Also, lots of 1 on 1 with leaned-in partners. We will be at a whole lot of events in upcoming weeks. Also, we will be at Cannes talking to industry leaders. We may also host or have someone host a special gathering, but the details are yet to be determined. Basically, it will be our normal forums.
    *   [Kleber] Hopefully, you can give us feedback about your thoughts, understanding that you are still digesting the news.
*   [Isaac Foster] There are different APIs within the sandbox - Do you have any specific comments or thoughts on ARA and its continued participation?
    *   [Kleber] This meeting is more focused on PA API, so we do not have folks who work on ARA in the room. But all the APIs already shipped in Chrome are in the browser, and they remain there. As with all APIs, the effort invested is driven by how much it matters to the ecosystem. As for what kind of ongoing work should happen on existing APIs, that is exactly the feedback we are looking for and hope to get from you. We want to invest our efforts in what matters to the ecosystem. The upcoming meetings we have are about cross browser initiative and not about ARA specific or Chrome specific things. That work will proceed as planned - collaboration with our colleagues across the  browser ecosystem and push that forward.
    *   [Isaac] Can you help adtech understand what you mean that the work that is out there will continue to be out there and will get support based on what the ecosystem needs?
    *   [Kleber] As folks may be aware, any change that happens in the browser involves a runway of public discussion before we actually implement it. We do not do things in Chrome by surprise. Any API removals or breaking changes would only happen after a lengthy process of ensuring no websites break and how we can move folks to other options.
*   [Eyal Segal] What about the 1% (Mode B users) of the Chrome deprecation - what happens to it?
    *   [Kleber] We do not have an answer to what will happen at this time. To remind folks, we had set up a 1% experiment with 4 different 0.25% sub groups, started back in late 2023, and we turned off 3PC in those browsers
    *   [David Dabbs] Is there a 400 day max expiry on cookies?
    *   [Kleber] Maximum lifetime of cookies is 400 days, so there aren't cookies from before that experiment started any more. The question is for those folks, their browsers have 3PC turned off because we turned it off as part of an experiment and that was based on the expectation that there will be turning off 3PCs for more people. But now that we are not doing it, what will happen to those folks?  Not sure yet.
    *   [Pat McCann] What about Storage Access Headers?
    *   [Kleber] Those are for labels to indicate whether folks are part of the experiment or not. The labels are still arounds and at some point, we will wind them down
    *   [Pat McCann] There is a feature about publishers as it relates to SAH - Can you talk more about it?
    *   [Kleber] No update since we talked about it in this group last month.  Will need to get back to you on it. Need to check with Johann.


## Topic #2: McCann - https://github.com/WICG/turtledove/issues/1436



*   [Pat McCann] There are different Chrome profiles… Can we make IGs more viable; i.e. To access them in Incognito and rescue some of our efforts?
    *   [Kleber] Incognito is complicated in a lot of ways. As a very rough approximation, opening up a tab in Incognito mode is the same as installing a brand new browser and removing the browser once you are done. Anything that carries over info about a user's browser experience will run counter to the goal of Incognito. Having said that, this is not a definite answer so maybe we should be thinking about how our APIs can interact with Incognito in a privacy safe way. My instinct is that if we are to bridge that barrier, it will be more likely in the aggregate measurement point of view 
    *   [Pat] How about the Web View regular browser area
    *   [Kleber] Right now, IGs do not transit the barrier between Android Web View and Chrome because there is no way to keep that private.
    *   [Pat] Aren’t there scenarios where cookie use is unavailable?
    *   [Alex] So you see coverage as a key motivator for your own investment? I think this is in the realm of the discussion we are trying to encourage over the next few months
    *   [David] It will be great to make this something that is viable in scale - like one dynamic open web 
*   [Alex Cone] To the point of identifying places where there is no coverage today, have you been thinking about the APIs as a gap filler on cookie match rate?
    *   [Fabian] Interesting question - It will be very complicated to leverage this because of side by side traffic, cookie sync will always work for someone. If the auction has many SSPs then probably some of them will have a cookie match for us, and also some will have a cookie match for other buyers.
    *   [Alex] Higher value to you means where you won't ever be competing with 3PCs. Correct?
    *   [Fabian] Yes
    *   [David Dabs] I would rather defer and move on to the next topic


## Topic #3: McCann - https://github.com/WICG/turtledove/issues/1435



*   [Pat McCann] Is there a way to modify PA so that the directfromsellersignals header can…?
    *   [Kleber] There are 2 aspects to this question. On a technical level, is there a natural way for us to say here is the comms channel with privacy properties to use for some kind of information and not use it for others? That is very hard because the browser does not understand that concept. The design of the API has always been to build information-passing channels. The information that flows is generally left 100% to the decision of the adtechs, and has seemed like the right choice because adtechs get to innovate on what information can be passed around. The other side is that it seems to me (instinctively), that if the goal here is the comms channel and here is a specific thing you cannot use the channel for, then a great way to achieve it is a legal/policy side enforcement. We on the browser side do not have to change anything at all. A big part of the PS is understanding technical enforcement vs policy enforcement.
    *   [Isaac] Do you believe that no adtechs should be able to hide the price?
    *   [Pat McCann] Not sure. Hiding the price was the mechanism of the outcome - one viable TLS in the market. Many entities wanted to do TLS, but we prevented them from doing so, functionally. Responding to the earlier question from Kleber, I am not sure if it belongs to the policy realm
    *   [Kleber] We do not actually know if the directfromseller signals is only being used for that specific use case, or if it is being used for other purposes. We would need to understand this before we go about removing it. If it is being used for other purposes, then we need to design/implement a substitute before we remove it. It seems like a very difficult thing to me. However, if everybody agrees that this should be removed and there are no other usages, then we have a valid discussion on how to proceed about removing it but I am afraid this is far from the case right now.
    *   [David Dabbs] Not sure why it is a good thing for the publisher to allow for it. 
    *   [Kleber] I do agree that we have had discussions in this group in which other non GAM players have wanted to do the same contextual bid flow in the auction without the details of the bid being visible to other parties… we enabled that exact same flow that was requested by other non GAM SSPs, you can do it using the Additional Bids feature of PA. What you are asking for does seem like a legitimate use case that others have been asking for, from a technology pov. 
    *   [Pat McCann] The TLS may not have any reason to receive anything like encrypted price

[David Dabbs] Going back to the beginning of the discussion, are you turning it around to the industry to say you tell us how you want us to make this useful?



*   [Alex] Okay, so is whatever you tell us the only thing to decide where we would end up with the APIs? The answer is no. But would we be a good product team to just do what we think is best without consulting with you? Absolutely not. So we want to come to you and listen to you to help inform what our roadmap should look like and this seems like a natural thing for us to do.
*   [David Dabbs] I don’t see a clear use case
*   [Isaac] Clarification on what the market feels about “this” - I wonder if there is room for exploration on what “this” means. I am curious - Is this meant to be limited to what we have been doing but moving it forward?
    *   [Kleber] It is definitely larger than that for sure. The question applies very broadly to all the PS APIs
*   [Brian May] One problem - The governance structure with PS is not well distributed. Need to figure out how we get adtech to the table with ground rules

## Comments on chat: 

Michael Kleber
8:04 AM

* Please sign in: https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit?tab=t.0

Patrick McCann
8:13 AM

* who's going to i/o

Patrick McCann
8:14 AM

* Alex: will the sandbox team be at I/o

David Dabbs
8:15 AM

* Place for that Q would be in the ARA repo

Patrick McCann
8:17 AM

* I love the topics api
we dont have a call anymore, so saying it here

You
8:18 AM

* Thanks, Patrick. Will pass on the message to the Topics team.

Isaac Foster
8:24 AM

* i believe it was like, jan 4th 2024...i remember having to do work for that

Patrick McCann
8:25 AM

* i got enrolled when i bought a new chromebook mid experiment :)

Patrick McCann
8:28 AM

* https://privacysandbox.google.com/blog/storage-access-headers-133

David Dabbs
8:33 AM

* We'd need to get the basic features scaled, like video, before expanding to incognito

Andrew Pascoe
8:39 AM

* yes. i long suspected IGs would be stickier than 3pcs.

Isaac Foster
8:50 AM

* i think you'd have to get to the point that "The API" manages much more than it does atm
which brings its own challenges

Patrick McCann
8:56 AM

* one interesting thing i was excited about in paapi: it would potentially be impossible to spoof the url and the user directed action
