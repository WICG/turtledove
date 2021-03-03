# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 4pm UTC.

That's 11am Boston = 8am California = 5pm Paris time (the winner of the [doodle poll](https://doodle.com/poll/eht2imrtvec69hxx?utm_source=poll&utm_medium=link)).

(This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload)


# Next meeting: Wednesday March 3 2021


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Phil Eligio (EPC)
3. Allen Fung (ShareThis)
4. Brian May (dsitllery)
5. Paul Marcilhacy (Criteo)
6. Amelia Waddington (Captify)
7. Newton (Magnite)
8. Erik Taubeneck (Facebook)
9. Jeff Kaufman (Google Ads)
10. Paul Jensen (Google Chrome)
11. Mehul Parsana (Microsoft)
12. Brian Schmidt (OpenX)
13. Remi Lemonnier (Scibids)
14. Joel Meyer (OpenX)
15. Basile Leparmentier (Criteo)
16. Christa Dickson (Meredith)
17. Nicole Lesko (Meredith)
18. Martin Gruau (Captify)
19. Jonasz Pamuła (RTB House)
20. Brad Rodriguez (Magnite)
21. Arnaud Blanchard (Criteo)
22. Tim Holmes-Mitra (Glimpse)
23. Tamara Yaeger (IPONWEB)
24. Andrew Pascoe (NextRoll)
25. Wendell Baker (Verizon Media)
26. Pat Moore (Bloomberg)
27. Larry Price (OpenX)
28. Isaac Schechtman (IPONWEB)
29. Greg Bougeard(Teads)
30. Vedant Seta (Media.net) 
31. Thomas Mouron (Teads)
32. Vinicio Haro (Bloomberg)
33. Theophile du Besset (Scibids)
34. Michael Lysak (Carbon)
35. Vincent Grosbois (Criteo)
36. Gil Sommer (Connatix)


## Note taker: Joel Meyer


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.



[<img src="https://i.insider.com/5fc55f5350e71a00115580e8?width=700" alt="Screenshot showing location of the Raise My Hand button">](https://www.businessinsider.com/how-to-raise-hand-on-google-meet)



# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


### Open Issues, or Issues that Need Opening

https://github.com/WICG/turtledove/issues?q=is%3Aopen+is%3Aissue+label%3AFLEDGE

Issues are accumulating faster than Michael can keep up with, he hopes to catch up in the next few weeks.

**Erik Taubeneck (Facebook)**: will there be a dedicated call for FLoC?

**Michael Kleber (Google Chrome)**: Not as much discussion around FLoC, probably would be good for web-adv to have a session on FLoC. Has been some discussion, but mostly related to the DV360 FLoC paper. Hasn’t been as much discussion around the FLoC origin trial.

**Basile Leparmentier (Criteo)**: Issue [#132](https://github.com/WICG/turtledove/issues/132) - What are the constraints on the Javascript running in the browser? It will have impact on the complexity of the logic we can put into the buyer worklet(?).

**Michael Kleber (Google Chrome)**: We don’t have a sense of this yet, but if there are thousands of scripts running ML models on many signals then it can definitely overwhelm the browser. Do you have any data you could share on the # of interest groups that you expect to be in the browser given that IGs only last for 30 days? Trying to get an approx sense of size.

**Basile Leparmentier (Criteo)**: Criteo has spent a lot of time thinking about how to create intelligent IGs when we thought there would be no signals available we thought there would be a lot more, but now we think that we can get by with less as there are user signals, reducing the need to have many interest groups as we can accomplish it with a single IG now. … The number of IGs depends on how flexible they are and how much data we can communicate with the trusted server. In short, I don’t know because it depends on a lot of other factors.

**Brian May (dsitllery)**: My experience with adtech is that if you provide a resource to us we will pile as much into it to extract as much value as we can. I’ve thought about this and it seems like we may need to impose some per actor restrictions.

**Michael Kleber (Google Chrome)**: The intention of TD/Fledge is that you should be able to use one IG for multiple different advertisers (for example). Desire is that an IG corresponds to an audience, which could work for multiple different campaigns/creatives/etc (see OB TD). Goal is to lead to a smaller set of IGs. Agrees with Brian that a shared resource (browser) requires a limit to avoid tragedy of the commons. Welcomes suggestions on approaches that could impose a limit/cost on the shared resource.

**Basile Leparmentier (Criteo)**: The more that we can do server-side the better, because we can optimize our spend to achieve our goals. Basically allows us to make it a business decision. Tragedy of commons could happen on trusted server as well, but expects that there will be a cost model. On the browser, not so much. This is an argument for doing more on the trusted server.

**Michael Kleber (Google Chrome)**: Agrees that the more we can allow to happen on the server the more we can compute. The trade-off is that the more we do on the server the harder it is to find a trust model that works. How can we know that the browser can trust the server. The model proposed in Fledge very deliberately chose to give the TS the smallest amount of data possible - eg realtime budgeting info (to enable somehting that \*can’t\* be done only on the browser). Would be happy to move to more info being made available as the trust model can be developed. KV lookup version can be imagined with a privacy preserving infrastructure - it is well understood how to allow values to be looked up without the server knowing the keys. Allowing servers to do unrestricted computation is harder.

**Brian May (dsitllery)**: Has the chrome team given any thoughts to where the restrictions in the browser might be set to give us an idea?

**Michael Kleber (Google Chrome)**: We haven’t given any thought to that, this group would be a good place to do that and figure out what the needs are.

**Basile Leparmentier (Criteo)**: What you say about KV approach makes sense and from a browser trust standpoint makes sense, but from an adoption standpoint it might make sense to do a more gradual move from purely server-side to more browser side. Rather than a big revolution in which everything is moved to the client, let’s start by moving a bit. … Some discussion around data sizes and the complexity of moving from server to browser. I do agree with Brian that we need to know some sense of the limits.

**Remi Lemonnier (Scibids)**: Issue [125](https://github.com/WICG/turtledove/issues/125) - Related some to previous discussion. What are the restrictions on the values stored in the KV? Is it required to be very simple, or can it be JSON that includes a model? Do you see any privacy problem with this approach?

**Michael Kleber (Google Chrome)**: Thanks for the question, I agree that there shouldn’t be any problem with the KV server returning JSON that includes numbers, strings, structured data, etc to support the logic you want to run on the browser. In combo with BL question, BM comment on size restrictions, the goal is that the data isn’t too large. Same concern about utilization of resources. 1KB json? No problem.

**Remi Lemonnier (Scibids)**: Thanks. Second question on [131](https://github.com/WICG/turtledove/issues/131) - can you provide clarity on post-view conversion measurement and how important it is relative to Fledge. On the Fledge agenda, is it a requirement to find a solution to this in order to move forward?

**Michael Kleber (Google Chrome)**: This isn’t really the right forum for going into the Privacy Sandbox topic as Charlie Harrison isn’t here, there’s a separate meeting for that every other Monday. We certainly do want to support post-impression conversion measurement but supporting it does present some challenges. The PS approach to event level view conversions is to associate the fact that a conversion happened with all the data that was available at the time the ad won the auction. In a contextual auction that make sense, but in a Fledge auction we can’t do that because making all the data available would allow you to join IG and context, which violates a fundamental requirement of Fledge. The info that you would be able to join to a conversion event would have some k-anonymity requirements on it. So we’re thinking about it, but privacy is important.

**Mehul Parsana (Microsoft)**: The conversion depends on C and S together (see PARAKEET), so if you separate the reporting on those can you actually do what you want?

**Michael Kleber (Google Chrome)**: (clarifies C&S) - This is a good question. What exactly can you learn after the fact? The TD/Fledge model is that you can never learn C&S together. You can learn either one of them associated with a K-anonymity version of the other during the Fledge trial when we’re allowing trusted server communication. In time I would like us to move to a model where you only ever get aggregated reporting and C’ & S’ type of information (?). But Fledge is a compromise for now as we move that direction. 

**Michael Lysak (Carbon)**: I feel like we’re circling around the data itself. Can we get more details specifically on the data, the data in each signal. The specific of what data is available at what point in time isn’t clear and I’m having a hard time knowing what I can actually do.

**Michael Kleber (Google Chrome)**: It sounds like I need to clarify the explainer as I believe the information you want should be in there, but I’ll take a look.

**Wendel Baker (Verizon)**: Having roof work done. He will now stay dry (congrats). Thanks for doing the Q&A every week, seems like it would be a stressful thing. I’m wondering if there’s a diff format we can use for the w3c meetings. A lot of us are trying to figure out how to go to market and we have two minds: 1) how will we break it, how will it not work, resources, etc and 2) let’s pretend this all works, but what’s the go to market on this? As an engineer working on this type of things in the past I realized that some things were obvious only to me. Could half the meeting be an update on the go-to-market stuff (origin trial status, dates, etc). And the second half could be the deep dive on technical details. How do we take our billion dollar business through this transition?

**Michael Kleber (Google Chrome)**: Agrees that we’re trying to serve two audiences here and it might be worth pulling that apart. Will need to think more on how to do that. Part of the answer is temporal - right now at the beginning a lot of the discussion on what will break, how will it work, etc, informs the design and we’re still very open to changes to the design. Fledge was an attempt to pull all the feedback together but I don’t pretend that I got it all right. I think the refining of the design needs to happen before the go-to-market planning does. The design changes will impact the g2m. The g2m conversation might be a different set of folks as well. The G folks here are engineers (I’m not the only one, PJ here as well) and the Chrome engineering side effort is probably the wrong set of folks for the g2m and buyer/seller interaction conversation. That conversation will probably require more G Ads people. But not entirely sure where that meeting happens. Is it w3c or a more ads industry focused group?

**Michael Lysak (Carbon)**: Can I offer a suggestion? Could we have more time to discuss because conversations get cut short, don’t get to the full queue. And we’re trying to address two things: privacy and business, but can’t discuss them in isolation. Can we have more time? And there are a bunch of other meetings (mostly IAB - go check them out). Very interested in the data available.

**Michael Kleber (Google Chrome)**: This seems related to the question WB was asking. Having two different meetings make sense. Still not sure who the right person is to host the other conversation.

**Brian May (dsitllery)**: Fairly easy to suggest it’s not browser makers to run that meeting, agrees it would be G Ads folks.

**Michael Kleber (Google Chrome)**: G Ads already thinking about this, running experiments, etc. Not currently running public meetings but will talk to them to see if they are.

**Brian May (dsitllery)**: Also worth noting that we have G2M issues but they are unique because we’re taking an existing market and moving it, not just making a new one.

**Michael Kleber (Google Chrome)**: Will talk to G Ads folk and post poll on GitHub on times to have a more ad-focused meeting. Will still be present in Ads focused meeting as it’s important to have awareness across.

**Brian May (dsitllery)**: Seems like there are several use cases that will need to be looked at. Would be good to have a list ahead of time.

**Brian May (dsitllery)**: Origin trials are coming up, is there an explainer for FLoC coming out?

**Michael Kleber (Google Chrome)**: Yes, we do plan to publish more info soon.

**Brad Rodriguez (Magnite)**: Related to what Wendell said, we’re trying to figure out when and how many people we dedicate to working on a Fledge origin trial. High level timelines would really help in planning. Just a high-level overview would be useful at the start of the meeting to provide a sense of how it’s progressing. Will it happen this quarter, year, decade. Just need some signals to help plan for development.

**Michael Kleber (Google Chrome)**: I don’t have any actual timeline to share but Chrome’s development is very in the open and if it were happening I could point you at it. We are not at that stage and I think we are looking at later this year for people to be able to look at testing this but I can’t given an indication of what quarter to look at. As soon as I do know I will share it. Chrome is on a six week release schedule, so generally it’s six weeks dev, six weeks beta, so at least twelve weeks notice (two milestones) in advance.

**Brad Rodriguez (Magnite)**: Are there milestones or dependencies that can be shared with the group that you’re waiting for?

**Michael Kleber (Google Chrome)**: There is definitely design work that’s needed to be done, trust models, FF rendering, etc. Those things need to be pulled apart so that the basics of the API are clean and have minimal interdependencies. Still working on that.

**Michael Lysak (Carbon)**: I feel like I’ve been trying to create discussions on GH issues about privacy leaks, etc, but it’s been quiet. But the conversations on the meetings are much more fruitful. Can we have more time to discuss these things?

**Michael Kleber (Google Chrome)**: GH is the right place, I’ve just been really busy - sorry. Other meetings: w3c web-adv business group. That group has an issue listing other meetings worth checking out: issue [#104](https://github.com/w3c/web-advertising/issues/104) in web-adv.

**Michael Kleber (Google Chrome)**: Thank you all for coming. See you next week.
