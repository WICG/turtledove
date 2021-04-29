# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 4pm UTC.

That's 11am Boston = 8am California = 5pm Paris time (the winner of the [doodle poll](https://doodle.com/poll/eht2imrtvec69hxx?utm_source=poll&utm_medium=link)).

(This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload)


# Next meeting: Wednesday April 28 2021


## Attendees: please sign yourself in!

1. Michael Kleber (Google Chrome)
2. Brian May (dstillery)
3. Brendan Riordan-Butterworth (IAB Tech Lab, eyeo GmbH)
4. Remi Lemonnier (Scibids)
5. Jeff Kaufman (Google Ads)
6. Michael Lysak (Carbon)
7. Adrian Mason (Xandr)
8. Erik Anderson (Microsoft)
9. Newton (Magnite)
10. Valentino Volonghi (NextRoll)
11. Michael Coward (Arcspire)
12. Wendell Baker (Verizon Media)
13. Nathan Jia (Nielsen)
14. Basile Leparmentier( Criteo)
15. Ryan Arnold (P&G)
16. Gwen Gillingham (Nielsen)
17. Vincent Grosbois (Criteo)
18. Lahat Abu (DoubleVerify)
19. Martin Gruau (Captify)
20. Luke Whittington (Glimpse)
21. Larry Price (OpenX)
22. Matt Wilson (NextRoll)
23. Kale Smith (Nielsen)
24. Vishnu Prasad(Nielsen)
25. Anthony Molinaro (OpenX)
26. Don Marti (CafeMedia)
27. Stan Belov (Google)
28. Viraj Awati (Amazon)
29. Kelda Anderson (Microsoft)
30. Andrew Pascoe (NextRoll)
31. Joel Meyer (OpenX)
32. Brad Rodriguez (Magnite)


## Note taker: Brendan Riordan-Butterworth


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.

[<img src="https://i.insider.com/5fc55f5350e71a00115580e8?width=700" alt="Screenshot showing location of the Raise My Hand button">](https://www.businessinsider.com/how-to-raise-hand-on-google-meet)



# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

 		


## [Suggest agenda items here]



*   JeffTK: JS shim
*   Publisher Trusted agent fleshing out (open issue 114 https://github.com/WICG/turtledove/issues/114 )?
*   ML challenge https://github.com/WICG/turtledove/issues/171 
*   Issue on bidder currency https://github.com/WICG/turtledove/issues/166 
*   How is contextual data defined in FLEDGE?
*   Are we calling this thing TURTLEDOVE or FLEDGE now? - brad


### Standing Agenda Item: Open Issues, or Issues that Need Opening

https://github.com/WICG/turtledove/issues?q=is%3Aopen+is%3Aissue+label%3AFLEDGE

Notes Start Here.

Michael Kleber: Shared link to [previous notes](https://github.com/WICG/turtledove/tree/main/meetings).  

MK: Process guidance.  Open to WICG members, details on this.  

MK: I don’t have a specific item to spend time on this week.  Happy to take open issues that we haven’t spent time resolving and talking about on GitHub.  Also cover topics that are not adequately discussed on GitHub.  Happy to take suggestions.  


### JeffTK: JS shim

MK: Jeff Kaufman, talking about Fledge Shim, perhaps.  There is work for JS Shim for Parakeet proposal.  These are probably interesting paths to solving easy access to advertising interest on the topic.  

Jeff Kaufmen: Google Ads is working on Fledge in pure JavaScript.  Goal is to make it feature complete, as possible, based on what’s doable in JS.  Question: would this be useful, how would you use it? 

JK: There is risk about exfiltration.  Someone might submit bidder code in final version that tries to exfiltrate.  How could we prevent this in browser JS code only?  Is this even an issue, if you’re using a hosted version of the shim?  Since you would open source the bidder logic.  If, for example, the bid is a dot product between (A)/(B), open sourcing wouldn’t disclose too much about plans.  

JK: Would this type of open sourcing approach be something people would be interested in?  

MK: Thanks for the intro.  

Michael Lessic: Do you want to flesh out the PUblisher Trusted Agent (issue 114?)

Remi Lemonnier: Question was not about the shim, it was something else.  

MK: Anyone want to respond about the shim?  

Brad Rodriguez: Thanks for intro on this.  It looks good.  The Magnite team is working on similar shim, trying to get to the same place.  We’re hoping to run a test on real traffic (not full real world) once it’s built out more.  Question about the DSP Bidding Logic / Seller Scoring Logic being contributed via GitHub (or other vetting place) to verify that it’s not sending data off the client.  

BR: For any trial, we want to participants to be working together.  There is possibility of leakage.  Code might be able to phone home about something like “Nike Runners on LA Times!”, but the risk of this leakage, and the impact of this leakage, is small enough that it’s OK during trial.  The controls will be important in final.  

BR: We’re looking to build out what would emulate real world, but understand there’s priorities.  Would love to hear about how different people are planning on using the shim, so that we can converge on similar workflow.  

MK: I think that the approach that Brad was talking about, people agreeing to cooperate for testing, and that it’s only through agreement that you have assurance that they’re not being sneaky, that’s probably the way that we’ll have to go until the browser can offer assurance of enhanced privacy/security.  

MK: We want to have a version that exists as part of the browser that provides protections that the JS Shim version can’t do.  There are some fundamentals that can’t be turned off in the browser, so the privacy features.  

BR: Is there a timeline for the Fenced Frame?

MK: No.  Being worked on, but no public commitment.  

BR: Linked?

MK: Fenced Frame is a required component of FLEDGE.  It is what allows the page and an ad to be loaded while keeping the ad targeting information from being available to the surrounding page.  It will be necessary for FLEDGE, PARAKEET, MaCAW.  

MK: The earliest version that might be made available in the browser, some privacy features, might be able to be made available ahead of the Fenced Frame availability.  

MK: We are going to need both an implementation of an API, and Fenced Frame.  But they’re not tightly coupled, API might come before Fenced Frame, they will eventually be interrelated.  

Basile Learmentier: Thank you.  It’s a question.  The problem of FLEDGE, there is a lot of grey area yet, not well defined API yet.  How do you think you will be able to keep up with what’s happening here, so that you have a functionally similar / on track specification that will mean less confusion for implementing?  Additional question about timeline - when is the code, mock implementation, etc, going to be available?  

MK: How soon can we have something available is at odds with how consistent can we make things.  This is tension, making something available faster is better option, and this means that the JS version will be faster, though this might be inconsistent.  And implementations will highlight any inconsistencies.  

BL: Should we do it with the Shim, or how else can we get to clarity in the gray areas?  Can we have CHrome support in these areas? 

MK: GitHub issues are best place for this.  Consider special labels to make sure they’re correctly attached to things like the JS Shim or other components.  

Vincent Grobois: I work at Criteo with BL as well.  I have a different question?  

Michael Lysak: A little bit additional follow-up.  Could the readme be updated with further API on the PUblisher’s Trusted Agent, so that there’s easier comparison with each other?  

MK: Yes, we do want to go there.  Once we have the resource available!  Engineers will have to work through the open issues.  Lots of future work!  

BL: Do you know when there will be more bandwidth?  Chrome announced the end of third party cookies, pretty strict timeline, and can you please have more people on the Chrome side?  Some issues just seem to be sitting there.  Should I start filing tickets against Chrome directly?  It’s really starting to be a problem working with the type of delay we’re seeing in getting in responding to the questions.  

MK: It’s not just me, there’s a fair amount of work already.  CHromium commits, you can see the relevant code already being submitted.  

BL: On FLEDGE?  

MK: Yes

BL: Oh, I may have missed that.  

MK: We will try to add links.  There are engineers working on getting stuff committed.  

https://chromium-review.googlesource.com/q/fledge

MK: Yes, that’s the right group of engineers.  

MK: Yes, there is work being done for the initial version.  This is the simple version described in the initial document.  Then we’ll add the additional features that have come from this process.  

Remi Lemonnier: I was going to ask same question - who can we talk to about these things.  We can hope it might be completely OK, but at the same time, we’re trying to communicate risks.  Could we be doing discussion at the same time as implementation on your side?  

RL: I understand there’s a lot of things being studied, do we have to expect that some part of things like PARAKEET will be integrated into FLEDGE?  

MK: In terms of doing everything at the same time, that’s a limitation on based on resources.  Right now, engineers are working on the code hat has just been linked (above).  Once taht’s done, we can have them doing more work in the remaining grey areas.  

MK: In terms of the interaction between FLEDGE and PARAKEET - both of us have indicated the desire to make this a successful state for us to work towards.  Edge folks want to make the code part of Chromium.  A lot of PARAKEET API seems to be reusing FLEDGE API as much as possible, and that’s the goal.  

RL: Can we expect that PARAKEET be part of the final solution?

MK: Not sure yet.  Incubation about finding the right idea.  TURTLEDOVE became FLEDGE based on an entire year of open discussion.  PARAKEET came after the year-long process.  Haven’t gotten a unified solution yet.  

Vishnu Prasad: From Nielson, mutlitouch attribution.  QUestion about volumetric data (clicks, and impressions).  Without 3rd party cookies, will we still get information like timestamp and creative/campaign information even without cookies.  Will the pixel request still fire?  

MK: Not sure the scope of your question.  

VP: I feel that FLEDGE is really the answer for post-3rd-party-cookie world.  Hence want to understand your perspective on this.

MK: The answer in FLEDGE is that it describes event level reporting still being available at ad render for the winning creative. So for the initial version of fledge it’s likely that this functionality will be present.  This doesn’t solve all of privacy, there’s still some aspect of information leakage.  Move to aggregate reporting is more important in the future.  

Brian May: There seems to be a lot of interest knowing where things are at with regards to where FLEDGE is at in terms of development.  Could we have a standing thing?

MK: We’re doing it as things are coming up, but we’re not at a regular cadence.  


### Publisher Trusted agent fleshing out (open issue 114 https://github.com/WICG/turtledove/issues/114 )?

Michael Lysak: Talk about next issue?  114. 

ML: I think that we really fleshed out the details.  But I noticed it wasn’t added to the explainer.  Does anything need to happen ahead of this?  If one piece of information is missing, there’s risks across the industry in terms of fraud detection, analytics, publisher contracts would be affected.  What’s the process for adding to the readme?  

MK: Once we have the initial, successful engineering implementation, then the engineers will move towards other things, like 114.  

ML: Would FLEDGE release without the agent?  

MK: This is a prioritization question.  I’m not in a position to say what the full list of these.  

ML: How would this work if it’s not available? If fledge launches without the agent, analytics companies will have to move to an auction running model, and auction running companies have to move to a server model. If they cannot move fast enough there would be contract disputes and publishers could suffer a great deal of lost functionality.

MK: In the initial design, the worklet is the same between publisher and auction.  There is value in having a separate worklet.  The first implementation will be on this initial design.  

ML: Can we broadcast this better?  The people who currently operate as trusted agents and those who run auctions would have to make changes to who’s running what. 

MK: Understood, but I don’t have a prioritization of the issues. 


### ML challenge https://github.com/WICG/turtledove/issues/171 

Basile Leparmentier: Announcement and question.  We at Criteo, along with a conference on machine learning, offering an engineering challenge.  The goal is to show differential private implementations.  This is a big topic, so it’s to learn and help setup the right parameters.  We would be very happy if you can provide us with a level of feature complete when the competition is ongoing.  

BL: The good thing about science is you learn while you do it.  Should start early May, you are all welcome to participate in the challenge, how to do models in differential private environment.  

MK: CHarlie Harrison is the right person, especially for picking parameters that are in a plausible range.  I would like to see more than 1 value for Epsilon.  Aim for “here’s what we can do with epsilons of these values”, covering a range, understanding the impact of different values of Epsilon. 

BL: It’s more expensive, and multiple values make it easier to cheat.  Once it’s done, then release the information, model, and then the impact of different epsilons can be determined with subsequent analysis.  

BL: We aim to make information public, post-challenge.  

Remi Lemonnier: Question: Source of differential privacy?  Is it going to be implemented in the API, or is it linked to the value of epsilon?  

MK: For the Aggregate Reporting API, it’s differential privacy based reporting.  

RL: UNderstand there’s a potential of different epsilons, but the underlying model also has an impact.  Are we still thinking of using mutliparty computation (MPC)?  

BL: MPC is about avoiding leaks.  Our challenge won’t do MPC.  

MK: The MPC vs single server is an implementation detail, not relevant to this question.  However, what we’re probably talking about is central vs local differential privacy.  Central DP is better data.  This should be discussed in the Conversion Measument sessions on Mondays.  


### Issue on bidder currency https://github.com/WICG/turtledove/issues/166 

Vincent Grosbois: Next point on the agenda?  

VG: Not that complex, but blocking.  Currency of the bid?  We have some method that returns a bid value.  The question is that the system - each pub might be a different country, using different currency.  Don’t see a simple way to do conversion in FLEDGE.  The main bid function does Euros, but how do we keep track of all the different transaction rates?  Is there an easy way to solve this issue?  Something that should be take into account in the API itself?  

MK: This is a big question.  Discuss more in GitHub.  

VG: Great , please pay attention on GitHub!  
