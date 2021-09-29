
# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC.

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Sept 29 2021


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Brian May (dstillery)
3. Chris Evans (NextRoll)
4. Newton (Magnite)
5. Caleb Raitto (Google Chrome)
6. Erik Anderson (Microsoft)
7. Pierre Tsai (NextRoll)
8. Andrew Pascoe (NextRoll)
9. Brendan Riordan-Butterworth (eyeo GmbH)
10. Matt Wilson (NextRoll)
11. Isaac Schechtman (IPONWEB)
12. Vincent Grosbois (Criteo)
13. Gaby Jenkins (Google Chrome)
14. Wendell Baker (Yahoo)
15. Fred Bastello (Index Exchange)
16. Jeff Kaufman (Google Ads)
17. Matt Menke (Google Chrome)
18. Don Marti (CafeMedia)
19. Christa Dickson (Meredith)
20. Antoine Bisch (Google Chrome)
21. Joel Meyer (OpenX)
22. Anthony Molinaro (OpenX)
23. Phil Lee (Google Chrome)
24. Phil Eligio (EPC)
25. Jonasz Pamuła (RTB House)
26. Lukasz Wlodarczyk (RTB House)
27. Przemyslaw Iwanczak (RTB House)
28. Sergey Tumakha (Microsoft)
29. Tamara Yaeger (IPONWEB)
30. Aswath Mohan (Microsoft)
31. Aditya Desai (Google)
32. Julien Delhommeau (Xandr)
33. David Tam (Relay42)


## Note taker: Joel Meyer


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

 		


## [Suggest agenda items here]



*   Questions about FLEDGE Origin Trial #225 (by Andrew Pascoe @appascoe)


## Notes

[MK usual housekeeping items.]

Michael Kleber (Chrome): New video setup makes it hard to see as many people, apologies if I miss facial expressions or raised hands. One agenda item has been suggested by Andrew Pascoe regarding Fledge Origin Trial in #225. Are there other items to put in the agenda? Notes doc ahead of time is good, but realtime is also okay. No other items suggested, moving to Andrew.

https://github.com/WICG/turtledove/issues/225

Andrew Pascoe (NextRoll): If you haven’t seen the issue, at NextRoll we’ve been chatting with our partners on Google Ads side (AdX) who have recently posted a means of enacting the Fledge Origin Trial. As we get into that, we have several questions for the Chrome team. How do we want the trial to be structured and what the expected volume would be. Structure matters because it would be helpful for us all to agree on the best way to run a test for comparable results. And a lot of the questions have to do with scale. There are four questions in the issue from NextRoll:



1. What percentage of browsers does the Chrome team expect to enable the OT on?
2. What’s the participation rate that you expect to see on the SSP & Pub side?
3. (Related to first two) What is the population for the test itself? NextRoll would like to see the population be everyone who is opted into the trial, and then we could do a cookie split on the opt-in population. That would give us a lot of control to balance the control and test groups. But it would also required that cookies be included on any traffic coming from a browser that is opted in.
4. Are there any plans on the Chrome team side to make sure we get sufficient volume to have valid test results? There are factors outside just browser opt in (SSP and DSP participation, for example).

That is the set of questions. Would be good to discuss them.

Michael Kleber (Chrome): Thanks for bringing up the issue, that’s a good set of things to think about. Let me start by saying that there are a lot of different ways in which browsers have made new features available. OT is one of the ways we have of making things available, 3P OT is also one. As Andrew and others have pointed out, the fact that this particular new API only makes sense when different parties are working together, especially the SSP and DSP, because DSPs need to put people into IGs and SSPs need to run on-device auctions that draw on those IGs, that makes it a less good fit for an OT. In this case, some sort of more global switch makes a lot of sense. I expect when we get to the point where we are asking people to do some integration testing, when the entire system is running e2e, then some sort of global fraction of the population turned on sounds like a better approach than an OT where individuals/companies opt-in. I can’t answer what percentage of browsers that would be, we’re not there yet. It would involve a conversation with other people as well (blink owner). Before the time when we start the broader trial, it will be possible to turn on Fledge on your own browser for the purpose of in-house testing, etc. But testing in the real world would be helped by some sort of global opt-in.

Michael Kleber (Chrome): You mentioned cookie splitting. That is interesting. If Chrome opts-in some random set then you don’t have a control group to test against. But if we want different companies to coordinate with each other and all be in Fledge experiment together (or not) we don’t want each company doing a coin flip for each cookie. So maybe some kind of explicit opt-in group A and opt-in group B and we all agree which group is test and which group is control seems reasonable. I can’t actually think of a precedent for when that has happened before inside of chrome so that will take some research on my side. But I do want to make sure we setup the test so it is as measurable as possible.

Michael Kleber (Chrome): When we receive a bid request, will we receive the cookie in the request? The answer is that the contextual request would have the cookie, but the request from inside Fledge (the IG request) would not have the cookie - it will be uncredentialed just like in the final version. Otherwise the test will be unrealistic because it doesn’t reflect the final version.

Andrew Pascoe (NextRoll): Okay, I see that people have raised there hands, but one follow-up question. If a browser is opted-in to the test group (I like the idea of a control and test w/in chrome itself) - assuming a browser is tested into the test group, unless a pub is participating… if they’re not participating, do we assume that the SSP doesn’t get any communication from that browser. Or if they are, the cookie must be sent somehow. It ties into what Stan wrote on the AdX proposal, which seems like a given bid request has a classic blob and then a Fledge blob. I agree that including the cookie isn’t an accurate simulation, but the thing is that if an SSP or DSP is using cookie information to bid on Fledge, they’re only playing themselves and invalidating their own test and making it look better than it otherwise would be. As an industry we want the most accurate results on perf/spend hits. Any DSP or SSP should have in their software that if they’re bidding in a Fledge context they’re not going to tie this to an identity. Maybe that was a bit rambly.

Michael Kleber (Chrome): No, that is a good point. The only thing I’d say in response is that unfortunately a bidder who decides to bid based on having the cookie, even when the SSP is thinking they should be testing the cookieless scenario (context only), unfortunately they’re not only hurting themselves, they are hurting others because they out-compete others and mess up their ability to experiment as well. I think there is a lot of subtlety to how we can get some kind of coordinated activity across the ecosystem.

Vincent Grosbois (Criteo): I just wanted to ask for some clarification about this topic/issue, because for me it’s getting very confusing. I asked a few questions on the issue. I will rephrase them. To me, for Fledge, if i exclude the part about contextual bidding, the part that is relevant for Fledge is addUserToInterestGroup and runAdAuciton that is purely pub side. When you are talking about the OT on fledge we are discussing some purely Fledge things and the OT part of Chrome is going to be limited to exposing those two methods to a percentage of the browser. I’m getting really confused about a Fledge bid request that comes into an SSP. To me those layers are up to the pub to define who they want to do that. And secondly, actually, to my knowledge it was not really defined yet in public so I’m not clear on the separation of a different layer that’s happening. THe discussion about how the SSP is getting the bid request and what you mentioned about a fledge blob and non-fledge blob is completely dependent on the OT that chrome is running. What chrome will do is only enable the JS function and that’s it.

Michael Kleber (Chrome): I think you're right, let’s try to pull these pieces apart because some things are getting conflated. The point of this question is that from an SSP POV, in today’s world we issue some sort of request that contains both context and cookie info which triggers RTB with cookie matching so everyone downstream gets an opportunity to do that kind of third party cookie based bidding. For an SSP that does return ads based on context, as well as on 3P cookies, in the eventual future with no 3p cookies, there is still some request from the browser to an SSP (w/fan out to other buyers) that wants to buy purely on contextual info. The description of how that works i not particularly present in the Fledge explainer. It’s been discussed here, and in Parakeet, but the point is that even that part of the ad flow system needs to be aware that an experiment is going on, if we want to experiment with how the entire ecosystem will work when the 3p cookie is not available anymore. So that there can be awareness that the ecosystem pretends that cookies are no longer available so we can simulate the eventual world where context/IG have been separated. Does that answer the question?

Vincent Grosbois (Criteo): to me it’s not clear that the goal of the OT is to test the contextual part of the implementation. To me I wasn’t aware that this was a goal of the OT, I thought it was a technical test of the new Fledge functionality to make sure IG stuff works. To me it wasn’t the goal that we were trying to have an assessment of the impact on eCPM, etc. And I’m not sure how much you want the involvement of SSPs and so on, because as long as on your browser you have Fledge available you can be a single pub and you can just test the APIs yourself. I feel like the big part we’re not discussing is how this will be integrated into the different pub flows, though maybe this isn’t the right group. TO me the goal of this group is to discuss purely Fledge and the pub/SSP integration path is totally separate.

Michael Kleber (Chrome): I think you’re quite right that the first goal is to test the flow e2e and insure the bidding functionality, IG api works successfully. And this is why I was saying that there are several different ways that new features become avail in chrome. And as Andrew pointed out, the browser could make it much easier for those who want to run a global A/B traffic split exercise. That might not be the first thing that the browser does. The API validation and then ecosystem comparison both make sense, there’s a roadmap of validation layers that is useful.

Pierre Tsai (NextRoll): This is following up on the spirit of the 4th question from Andrew at NextRoll. As we move toward the OT we’re trying to plan for how we want to approach it. The cookie split question is relevant and another detail we’re trying to figure out is if we should set an IG for every user that visits an advertiser site regardless of if they are in the OT. We want to plan for the eventual roll-out of Fledge broadly. What we’re concerned about is the phasing as we move off the simulation testing we’re currently doing to the OT, we’d like to know when we can achieve equivalent scale on the OT side. In your answer to Andrew you said that when we get to a place of testing for multiple parties, do you have a sense of when that will happen? Or should we expect that everything will land in November?

Michael Kleber (Chrome): We don’t have a timeline for when everything will be available and if it will be all at once or phased, but I do understand that being clear about what we’re turning on at various stages will be helpful. We’ll communicate when we have it, but can’t say anything more right now.

Andrew Pascoe (NextRoll): Wanted to reply to what VG had to say. Yes - I agree that this group is talking about the Fledge spec itself, and I think being able to test on your own is super useful, but I’m not just interested in how people integrate, but what I really care about that as an industry, as we test, we’re making sure we get the cleanest test possible with enough scale to have statistically relevant results that are comparable and maybe aggregable in a meaningful way. That requires that we all agree on how this will work because there are so many actors in the space.

Michael Kleber (Chrome): I think that VG is correct that a lot of what you’re asking about is best thought of as industry coordinating with other parts of the industry and the browser's goal is to make that possible and then get out of the way. My goal is to figure out things that I can do to make it easier for you all to adopt it and understand it collectively. That will absolutely require some set or at least one SSP to run auctions, or else some number of sufficiently large pubs to run auctions in this way w/out an SSP being involved exactly as you say. But as the browser we want to make that as easy as possible for those who want to do some global testing together, then take a back seat.

Vincent Grosbois (Criteo): Andrew, I’m totally on board with the goal of what you’re trying to do because it’s the same issue for us, but we’re trying to find as much supply as possible to run a Fledge auction. Actually, I’m asking you if there is a forum or space to discuss the integration of Fledge. To me it wasn’t this call or this repository, but it’s definitely something we should do. I’m wondering if it doesn’t exist?

Michael Kleber (Chrome): Great question, it seems to me like there are two sell-side parties that we would want to have as voices in that convo. One is Google Ads because they’re running simulations now. The other is the Prebid folks, who have been very active in our previous discussions about being part of the sell-side here who might have thoughts about what the future is as the API becomes more available. Maybe there is a different forum not run by browsers that is a better place for that discussion to happen, but I don’t know what it is.

Andrew Pascoe (NextRoll): I also don’t know. The Google Ads team has their proposal in a repo where we can have conversations. But I think for NextRoll oru communication basically happens here. Unless theres is some other forum for interacting with Chrome? I just want to have the convo here.

Brian May (dstillery): With respect to testing, Angelina Eng at IAB TL is putting together a group of people to test the sandbox and other tech coming together. She is trying to engage SSPs, pubs, whoever wants to be involved in testing. The other thing is that while we on this call probably have the capacity to understand the difference between a testing phase looking at pipes and one looking at impact, I think we need to be careful about what people are going to see if during the pipe testing we don’t get clean results. When I say clean results I mean people not participating in the auction who are outside of the test. I think we need to be careful about what people might say about what Feldge will do to the future of the industry given the potential to misunderstand it.

Michael Kleber (Chrome): I agree, there is a lot of risk for tests that are not representative of the future to have their results reported and misconstrued as what the future of ad tech is like. It’s on all of us to be very clear when we talk about the results of tests, but maybe successful communication is hopeless. I don’t know…

Michael Kleber (Chrome): Okay. Any other hands up? None to see. I think we’ve fully addressed that topic. Angelina is not here right now but does show up on the privacy CG. An IAB Tech Lab group seems like a good venue for further ad tech discussion.

Michael Kleber (Chrome): Any other topics? Have we covered everything from the original question? I think so.

Brian May (dstillery): I will send Angelina an email and let her know we talked about her group in this group.

Michael Kleber (Chrome): I will try to get the notes up sooner so IAB folks can see the record of this discussion and think about it themselves and in discussion with the rest of you. Alex (Cone) said he has pinged her already, so great.

Michael Kleber (Chrome): Any other topics for conversation? [silence] Okay, then I have nothing more to bring up. Perhaps we can be done and give everyone time back. Thank you all for coming. See you in two weeks.
