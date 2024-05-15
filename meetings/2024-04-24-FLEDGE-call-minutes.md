# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday April 24, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	 



1. Michael Kleber (Google Privacy Sandbox)
2. Brian May (Dstillery)
3. Jacob Goldman (Google Ad Manager)
4. Patrick McCann (Raptive)
5. Peiwen Hu (Google Privacy Sandbox)
6. Joshua Prismon (Index Exchange)
7. Harshad Mane (PubMatic)
8. Laurentiu Badea (OpenX)
9. Sven May (Google Privacy Sandbox)
10. Paul Jensen (Google Privacy Sandbox)
11. Orr Bernstein (Google Privacy Sandbox)
12. Paul Spadaccini (Flashtalking)
13. Pawel Ruchaj (Audigent)
14. Sarah Harris (Flashtalking) 
15. Garrett McGrath (Magnite)
16. Abishai Gray (Google Privacy Sandbox)
17. Alexander Tretyakov (Google Privacy Sandbox) 
18. Alex Peckham (Flashtalking)
19. Alex Cone (Google Privacy Sandbox)
20. David Eilertsen (Remerge)
21. Christoph Armster (Remerge)
22. Denvinn Magsino (Magnite)
23. Risako Hamano (Yahoo Japan)
24. Leon Yin (Microsoft)
25. Taran Singh (Jivox)
26. Vedant Seta (Media.net)
27. Warren Fernandes(Media.net)
28. Matt Davies (Criteo | Bidswitch)
29. Isaac Foster (MSFT Ads)
30. Miguel Morales (IAB TechLab)
31. Stan Belov (Google Ad Manager)
32. Yanush Piskevich(Microsoft Ads)
33. Matt Menke (Google Chrome)
34. Konstantin Stepanov(Microsoft Ads)
35. Arthur Coleman (IDPrivacy/ThinkMedium)
36. Shafir Uddin (Raptive)
37. Russ Hamilton (Google Privacy Sandbox)
38. Brian Schneider (Google Privacy Sandbox)
39. Felipe Gutierrez (Microsoft Ads)
40. Antoine Niek (Optable)
41. Fabian Höring (Criteo)
42. Slawomir Jach (Ringier Axel Springer Poland)
43. Don Marti (Raptive)
44. David Dabbs (Epsilon)
45. Andrew Pascoe (NextRoll)
46. Bram Woolcott (TripleLift)
47. Daniel Rojas (Google Chrome)
48. Sid Sahoo (Google Chrome)
49. Elmostapha BEL JEBBAR (Lucead)
50. Aymeric Le Corre (Lucead)
51. Mihir Gandhi (Google Privacy Sandbox)
52. Hari Krishna Bikmal (Google Ads)


## Note taker: Orr Bernstein


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here:



*   Isaac Foster:
    *   TEEs Flexibility: KV Support + Inter TEE Communication https://github.com/WICG/turtledove/issues/1140 
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Guillaume Polaert (can be done async on github)
    *   [Addition of an analytics/reporting entity to enable centralised reporting.](https://github.com/WICG/turtledove/issues/1115)Things we could explore:
        *   Pinning down the exact issue.
        *   Where does Privacy Sandbox stand on the PAAPI vision and its constraints?
        *   Brainstorm a couple of potential solutions.
        *   Implementation proposal: `


# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.


# Notes


## Michael Kleber calls attention to the Privacy Sandbox timeline announcement from yesterday afternoon



*   https://privacysandbox.com/intl/en_us/news/update-on-the-plan-for-phase-out-of-third-party-cookies-on-chrome/
    *   Update to the timeline for the removal of 3PC in Chrome
    *   A subject we have discussed here in this meeting many times in the past
    *   There was ambiguity over the timing of the ramp up and how it might interact with any potential delays and what would happen during Q4 and whether it was possible to complete the removal before the ads industry wide freeze in late Q4
    *   This announcement addresses that, announces that we will not ramp up 3PCD before Q4, but rather will ramp up in early 2025 instead of the second half of 2024, subject as always to satisfying regulators.
    *   Everyone involved in market testing and giving feedback to us and the CMA that’s happening between now and June - that’s all proceeding, same timeline
    *   All that changes is that the eventual ramp-up will happen in early 2025
*   Brian May
    *   This is a great decision and adds structure that is needful
    *   Would it be possible to get the Chrome team to give some thought for how the ramp up process will take place so that we have an idea for at what rate cookies will go away and on what schedule.
*   Michael Kleber
    *   Nothing new to announce right now, but we intend to give more clarity on the ramp-up schedule before things start happening, don’t have that in hand yet.
*   Issac Foster
    *   Thanks for the announcement.
    *   Can we expect some precise updates on the extent to which the CMA footwork is modified as a result of this. Are there changes to the CMA process?
    *   Also - what’s the level of intent on the exact demarcation of the blue rectangle on the Chrome team site?
*   Alex Cone
    *   For the first question on envisioning changes - the short answer is no. The current state we’re in is Chrome-facilitated testing, which is still intended to end in the first half of 2024. CMA has been encouraging people to submit feedback as early as possible with June 30 as an end date. We think it’s important that they have time to take all results submitted during that testing window into consideration. As far as when we trigger the standstill, that will be something that is known once it happens, but there's no change in terms of the commitments and the Chrome facilitated testing period.
*   Isaac Foster
    *   It might be valuable to clarify some of this, exactly what is changing and what is not changing. It sounds like testing still ends at the end of CYQ2.
*   Alex Cone
    *   You mentioned the timeline on privacysandbox.com. We’ve updated that to reflect our intent to begin in early Q1, still subject to us addressing CMA concerns.
*   Michael
    *   One of the things we have discussed in this group before is the fact that the process after Google triggers the standstill is that the CMA has at least a 60 day window to come back with a response, and the CMA has the option according to those agreements to ask for an additional 60 days. So if they want, they have a 120 day window to make their decision. One of the concerns people in this room have expressed before was whether it was possible to get the ramp up completed by EOY if the CMA exercises that 120 day review window. One of the benefits of this early 2025 timeline is that we no longer need to worry about whether the CMA will take 60 days or 120 days, that we can start the process early enough that we can still target the timeline that’s depicted on privacysandbox.com.
*   Isaac
    *   So it’s still possible that you guys would trigger the standstill in 2024.
*   Michael
    *   Yes. The timeline on privacysandbox.com is the intended timeline for starting to get rid of 3PC, which is something that happens after the standstill period has already finished.
*   Isaac
    *   So, you are within your right to trigger the standstill in 2024, but you won’t start ramp up until 2025.
*   Michael
    *   I cannot say anything about when the standstill will happen, but we can keep to this timeline as long as it's more than 120 days before the end of 2024.
*   David Dabbs
    *   Quick net of what I heard the two of you say. The new blue bar that starts in 2025, the CMA mechanics would have to have transpired prior to that. Is that going to be in the report?
*   Alex
    *   The report will say [what’s on the blog post from yesterday](https://privacysandbox.com/intl/en_us/news/update-on-the-plan-for-phase-out-of-third-party-cookies-on-chrome/). Should that imply the date on the standstill? Yes, implicit in there is that the standstill will need to be completed before the ramp up begins.
*   David
    *   Might be good to just call it out.
*   Issac
    *   The reason I bring this up is that I’ve already fielded questions about this.
*   Brian May
    *   Deprecation can happen anytime from the end of the standstill period. It’s about making sure that there’s time to understand the results of the reporting.
*   David
    *   First thing is a request - regardless of when we start it, a request amongst a lot of the community is that it all doesn’t happen in one pop. Part of what helps the CMA and the market understand how this will play out - consider a measured titration.
*   Michael
    *   Going from 1% to 100% in one big jump would be bad for a lot of reasons. Speaking about Chrome as a web browser, any time we make backwards incompatible changes to the behavior of the web, we try to roll those out in a careful process to detect any breakages. In a sense, the entire history of the privacy sandbox has been about following that process and trying to be careful. As with any other large change, we will roll out that change in a slow, careful timeline. What you see on [privacysandbox.com/timeline](https://privacysandbox.com/timeline) now is a shaded area that covers all of Q1 and the beginning of Q2; a broad window across which cookie removal will happen.
*   David
    *   Other quick question - the asterisks - a lot of the other dates were based on the original timeline.
*   Michael
    *   I see no reason to believe those other dates will change. We’ll make sure that all plans are as well communicated as possible.


## TEEs Flexibility: KV Support + Inter TEE Communication https://github.com/WICG/turtledove/issues/1140



*   Isaac Foster
    *   Turning my attention to the genuinely private future, I started playing with the TEE. We have this KV server that you can sync data into freely, but can’t sync data out of.
    *   One of the things that’s always been changing from an ad tech side - there are these two functions - this one B&A server, and one KV server, and I’ve got the browser. I have to figure out how to shard data. But if I have a server that I can put data into - different clusters with different data - maybe less frightening, can scale in ways that we have experience doing.
    *   Can have multiple TEE servers that talk to each other. Blanket assumption that they have to be owned by the same owner. All TEEs that I own that can talk to each other. I can put one set of business objects into one and one set of business objects into another. Maybe what I’m looking at is to migrate existing domain structures into TEE. Really powerful if these B&A servers are associated with this set of KVs.
*   Michael
    *   There’s a whole separate call every other week hosted by the Privacy Sandbox team that is working on trusted servers. Maybe a better topic for that meeting, but some of those folks are here in this call.
*   Isaac
    *   B&A call is happening in 20 minutes. Happy to talk about it wherever is most relevant. But would need the Chrome team to say that they’d be willing to call out to a system that works this way.
*   Michael
    *   OK that makes sense.  Putting aside the question of how would one build a stack of servers in a TEE, we can address the question of whether it provides good enough privacy for browsers relaying that information.
    *   We have a set of TEE servers that do a collection of things but they don’t do other things. There are ones that get information from the browser, do some processing, and return results back to the browser. And then separately there are servers that do aggregation with noise and differential privacy. These are operations that we are comfortable with as a concept. If you want to know if we can let servers do entirely new things, like store browser data inside of servers, that would be a new capability. But it sounds like the question you’re asking is, can we arrange the servers in a topology that is compatible with the way we want to distribute load across servers, and still do the same kinds of operations as now.
    *   The thing we are concerned with from the browser side is the behavior of the implementations, to make sure that they’re only doing the operations we’re comfortable with.  So in principle, one server or many servers is fine, still accomplishes the privacy goal.
    *   BUT, once you’re talking about servers communicating with other servers, there’s a flood of new problems you need to solve. The TEE part of the server does some substantial amount of work to make sure the data it's processing cannot be observed outside of the TEE.
    *   With servers talking to other servers, there’s the potential for someone observing the pattern of network traffic between the servers, and that leaking user data.
    *   I'm not trying to say that what you want is impossible.  I'm just pointing out that it requires solving some new privacy leakage risks.  That’s the first part of the answer, and "here are the protections to address those leaks" would be the second part.
*   Issac
    *   Why does it not help that TEE servers all run in a public cloud?
*   Brian Schneider
    *   It still does not help because we don’t trust the network between the servers.
*   Michael
    *   If you’re running on an AWS TEE, we’ll assume that nobody can see anything that’s going on inside those layers of TEE protection. Even Amazon looking inside their own cloud cannot determine user data based on what they observe. For network traffic, by contrast, there are a lot of parties that could observe data being transmitted between different machines. All of these are a family of risks that need to be understood.
*   Issac
    *   If I am the ad tech, it would be significantly challenging for me to observe network traffic between two TEEs. The cloud provider - it might be different. If there is some service discovery that’s controlled at startup, connection management, connection/packet observability - is that type of thing something that could help?
*   Michael
    *   The sort of things that you’re saying is the start of a fuller answer, including some noise on the channels between these computers. The word we’ve used in a few contexts before is chaff - extra, fluffy, not real, noise communications that’s inserted in the TEE-to-TEE communications channels to make sure that it’s not possible to determine what one computer is choosing to communicate to another.
*   Brian Schneider (in chat)
    *   just by virtue of messages being passed from machine A to machine B, privacy can be leaked.  we take measures to avoid this sort of leakage.
    *   noising this communication is how kv data sharding works today
*   Peiwen Hu
    *   We don’t have a way to enforce TEE-to-TEE communication. We can only enforce that the client to server traffic has a TEE endpoint.
*   Michael
    *   I think this man-in-the-middle risk is what Issac was trying to solve by putting service discovery in the hands of the trusted code.
*   Isaac
    *   Yes, the services wouldn’t have access to curl.
    *   Would be a much better sell if I could say, we’re going to take our stuff, and this is a direction that we can make tradeoffs.
*   Brian May
    *   There are many channels of communication that are happening, and here we are focusing on only communication channels within the cloud.
    *   I can’t imagine any ad tech having the resources to monitor communication channels within a cloud, much less outside of a cloud.
    *   I think it would be important to start with a feasibility study to understand what it would cost an attacker to collect meaningful data and based on that decide if this is a concern that warrants further consideration.


## Warren Fernandes’ proposal for the analytics entity:



*   Warren Fernandes
    *   https://docs.google.com/document/d/1dmtOXo1WAWmPXOoi0B1CR8RfqcAek0nU3kgcFyyDT8Y/edit.
    *   We can potentially talk about this in next Wednesday’s meeting

General announcements



*   Michael Kleber
    *   Google group you can join to get this meeting to show up on your calendar.
    *   To be added to the calendar invitation, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/
