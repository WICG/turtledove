# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC.

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Aug 4 2021


## Attendees: please sign yourself in!	

Regrets from Michael Kleber, who is on vacation this week.  Paul Jensen has offered to host the meeting instead.



1. Paul Jensen (Google Chrome)
2. Wendell Baker (Verizon Media)
3. Allen Fung (ShareThis)
4. Caleb Raitto (Google Chrome)
5. Basile Leparmentier (Criteo)
6. Brendan Riordan-Butterworth (IAB Tech Lab / eyeo GmbH)
7. Russ Hamilton (Google Chrome)
8. Newton (magnite)
9. Shivani Sharma (Google Chrome)
10. Christa Dickson (Meredith)
11. Brad Rodriguez (Magnite)
12. Julien Delhommeau (Xandr)
13. Vincent Grosbois (Criteo)
14. Przemyslaw Iwanczak (RTB House)
15. Matt Menke (Google Chrome)
16. Andrew Pascoe (NextRoll)
17. Mehul Parsana (Microsoft)
18. Stan Belov (Google)
19. Aleksei Gorbushin (Walmart)
20. Bill Landers (Xandr)
21. Jason Xie (Adobe)
22. Gang Wang (Google)
23. Jonasz Pamula (RTB House)





## Note taker: &lt;please volunteer>


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.

# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

 		


## [Suggest agenda items here]



*   https://github.com/WICG/turtledove/issues/212
*   https://github.com/WICG/turtledove/issues/203 

Standing Agenda Item: Open Issues, or Issues that Need Opening

https://github.com/WICG/turtledove/issues?q=is%3Aopen+is%3Aissue+label%3AFLEDGE


## Notes

[Shivani of Chrome] ...on Fenced Frames. The ad will be rendered in the Fenced Frames.  Concerning the UnloadHandler; it will not be supported.  But, this is a solicitation of comments on that.

[Basile] asking for a recital of the load/unload scenario.  The goal is to be able to show products in the fenced frame, especially nested fenced frames.  Absent that capability, there is difficulty.

]Shivani]. On the UnloadHandler only, of course the Fenced Frame will continue. Only the Unload Handler will be removedin the proposal.  The reliability is dubious on Frames anyway, so there should be a reliability guarantee of the interface to the frames, so it need sto be removed.

[Basile] There should be an event OnClose for measurement.

[S] We have thought about that, including visibility and timing of such.  An Observer API can do this now but that will change in the future.  There is other thinking developing here for the advertising use case, but the implementation may be different.  It won’t use UnloadHandler.  That prposal is forthcoming.

[Paul?] Will FencedFrames have Network access?

[S] yes.  But there are considerations about whether there was user interactin in the FF.

[Paul] All this, measurement features.,should be moving to a Reporting API in any case.

[Julian] Measurement uses Network in today’s implementations, but it would be good to ensure that this use case is continued.  Especially considering the use of the passage of time in sending signals back outward.

[S] Measurement and reporting will be a separate channel, not part of the Fenced Frame (FF).  So UnloadHandler isn’t a good place for hooks to support the measurement use cases.

[Basil] Cookies were good because they allowed permissionless innovation.    These new APIs do not have the permissionless aspect.

[Paul] Part of the art here is to consider all use cases.

[Jonasz] There don’t seem to be any strong uses for UnloadHandler.  But what is more important is the use case of nesting aspect of FF and “mashup” composition of advertising creatives at the edge, in the client. (That is: support for "Ads Composed of Multiple Pieces" in the FLEDGE explainer.)

[Shivani] These can be recorded in the issues for treatment.

[Vincent] The problem with nested FF is unclear.  Let’s say the main frame is a FF.  But in the product case, do the subframes need to be (MUST) be FF?  Can’t they just be “regular?”  Soliciting a response,rather than proposing a solution.

[Shivani] The iframes within the FF can communicate with each other so the K-Anonymity guarantee cannot be given. 

[Paul] Yes, in product-level advertising, that is the consideratin.

[Jonasz] In product-level advertising, the goal is to select advertising creatives for each product independently (from the k-anonimity perspective).  The microtargeting and k-anon guarantees are the consideration.

[Paul] How does the product-level advertising flow work inside FLEDGE?

[J] Considering the information flow in the event sequence?  [yes]  The advertiser calls joiinInterestGroup and a list is provided.  Later in generateBid, a choice is made to which products to include.  These selections are positioned in the nested FF.  This allows the k-anon threshold for each product rather than for the whole of the set of products.   

[Paul] One interest group, and one bid? (generateBid)

[J] Yes, one creative and a number of products is returned from generateBid.

[J] So the rendering occurs with products (plural) inside the single creative.

[Paul] We should investigate this further in a separate session.

[Paul] closing.

IPaul] Consierning Issue 203.

[Julien] Considering FLEDGE integratiion into an advertising workflow.  From a discussion of some four weeks ago.  A single partner will run the FLEDGE auction, but generalizing out to having multiple participants and a meta-auction among the SSPs, there are other considerations [in the issue?].  Soliciting clarification on the question of how the auction is run, the multiple auctions cascade and compose.  When?

[Gang Wang]  There are many parts of the question.  Considering timing (latency budget).  For Google Ads, change is managed to develop and test.   There is no announcement yet on timing or production features.

[J] Is the work flow basically correct?

[G] We (Google) are considering the multi-tier auctions are being studied to understand how Google Ad products will fitthis in.  

[J] No clear answer yet?  

[Stan] What do other industry participants think of the use ases in the issue?

[Paul] There is another issue that covers it.

]someone] Issue 73 and Issue 229.

][Paul] more work is needed for clarification

[Brad] Hearing more about workflows from industry and other design work.  Can  you [Stan?] elaborate?  I can facilitate summarization and discussion here.

[Stan] Specific use cases are in the Issues.  One of the considerations is the privacy model that is supported by the work flow; the work flow among multiple sellers.  Deferring to Paul (of Chrome et al.) for the form of the discussion.

[Paul] Yes.  The Github Issues system is the right place.

[Brad] Are the changes proposed to “the design” of FLEDGE as a whole or for specific aspects?

[Stan] I was considering multiple sellers in the auction.

[Paul] same.

[Brad] So, just in general, hypothetically, any other changes.

[Paul] There are issues filedon the multiple sellers cases.

[Brad] more input is needed from practioners.

[Paul] Yes. More thought and elaboration of details.  Ther are a number of GitHub Issues which Chrome Team has been reviewing.

[Stan] In Prebid and other multiple seller scenarios, would it be helpful if prebid experts explained to Chrome Team?  Would it be helpful?

[Paul] Yes, a deep dive would be helpful.  Here or separately.

[Brad] OK.  Some materials should be prepared to make it useful.

[Paul] There are two aspects of the issues.  How will FLEDGE work; and how other sellers wil integrate.  Do the multi-tier auction issues cover it?

[J] Multi-sellers is important, yes.  To understand the work flow is the consideration. Need to understand how the auction is actually run.  Understanding the decision logic in teh auction will be reported out because there is no reporting out from the auction in FLEDGE.  Will there be a script running the auction observation or will there be another method?  What runs client-side and what runs server-side and how are the two matched up.

[Matt] A multi-tier auction could be developed in client-side scripts.  

[someone] all in teh client?

[Matt] Yes, maybe.

[Paul] This needs to be investigated further to ensure the privacy characteristics of the composed auction.

[J] When will the clarity happen?  FLEDGE testing might start at end-of-month in Q4?  We need to know Google Advertising’s position on how FLEDGE will compose the workflows.  Is there any hint on the time expectation?

[Stan] No time line to convey. Hearing the feedback is important.

[J] Will you (Google?) participate in teh FLEDGE origin trials?

[Gang] Google ads committed to support Chrome Sandbox API. We intend to participate in FLEDGE and other Sandbox API origin trials. Details TBD .

[J] Will the workflow answers be available by the time that Google participates in the FLEDGE origin trials?

[Stan] It’s too early now to sketch out the details of participation in the origin trials.

[Paul] that completes the formal agenda.  AOB?

[Aleksei] How will testing work with FLEDGE and also with PARAKEET.  Integration tests.

[A] Do the browsers expect to develop integration tests for this capability.  They occur in the nightly biulds, but the question is about external availability.

[Paul]  Yes there is continuous integration against tests.  Origin trials are ways to test in the field.  Are there specific tests at issue here?

[A] We want to know if a certain kind of capability can happen, specifically targeting people in a way that is similar to the “old way.”  The scenario is an A/B testing of reach of interest groups between theo old cookie way nad the new FLEDGE way.

[Paul] Original trials can be used for this sort of in-the-field use case.

[A] Does one hav eto turn on a flag?

[P] Yes, they are opt-in.  Also there are 3rd party Oriign Trials

[Vincent] One can test without the Origin Trial by activating the feature.  There is a link with the recipe for the process to perform local-box testing.  The flags must be enabled manually for the testing (“opt in”).

[A] ok.  Thanks.

[Paul] There are references in this meeting series’ notes from May 2021.

[Paul] AOB?

close.
