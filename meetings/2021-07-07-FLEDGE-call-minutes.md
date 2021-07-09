# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC.

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday July 7 2021


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Basile Leparmentier (Criteo)
3. Brendan Riordan-Butterworth (IAB Tech Lab / eyeo GmbH)
4. Brian May (dstillery)
5. Russ Hamilton (Chrome)
6. Don Marti (CafeMedia)
7. Ryan Arnold (P&G)
8. Allen Fung (ShareThis)
9. Michael Coward (Arcspire)
10. Christa Dickson (Meredith Corporation)
11. Bill Landers (Xandr)
12. Jonasz Pamuła (RTB House)
13. Tamara Yaeger (IPONWEB)
14. Vincent Grosbois (Criteo)
15. Fred Bastello (Index Exchange)
16. Przemyslaw Iwanczak (RTB House)
17. Jason Xie (Adobe)
18. Afolabi Adenuga (Nielsen)
19. Erik Anderson (Microsoft)
20. Julien Delhommeau (Xandr)
21. Tanya Moldovan
22. Adrian Swift (Nielsen)
23. Remi Lemonnier (Scibids)
24. Phil Lee (Google Ads)
25. Chris Evans (NextRoll)
26. Valentino Volonghi (NextRoll)
27. Andrew Pascoe (NextRoll)
28. Viraj Awati (Amazon)


## Note taker: Brendan Riordan-Butterworth


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.



[<img src="https://i.insider.com/5fc55f5350e71a00115580e8?width=700" alt="Screenshot showing location of the Raise My Hand button">](https://www.businessinsider.com/how-to-raise-hand-on-google-meet)



# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

 		


## [Suggest agenda items here]



*   [Paul Mrcy & Basile Leparmentier ] Chromes announced here an updated timeline for the retirement of third-party cookies and the Privacy Sandbox. Could we get some details from Chrome about the roadmap?
    *   What is the tentative start date for the FLEDGE Origin Trial? Will it be tested brick by brick, starting earlier, or the entire proposal at once, starting later?
    *   Will the Measurement APIs be up and running by late-2023 when Chrome will stop supporting 3P cookies? Or will that be a two-step process (with an event-level reporting to start) such as described in the FLEDGE explainer?
    *   Etc.
*   Workflow integration with existing system (adserver, SSP, etc.) https://github.com/WICG/turtledove/issues/203 (Julien Delhommeau - Xandr)

Standing Agenda Item: Open Issues, or Issues that Need Opening

https://github.com/WICG/turtledove/issues?q=is%3Aopen+is%3Aissue+label%3AFLEDGE


## Notes

Michael Kleber: Intro and housekeeping.  Under auspices of WICG, so make sure you’re a member.  Also make sure that the notes reflect what you’re saying, it’s collaborative.  

MK: I think we’re ready to get underway.  Look at the suggested agenda items.  

MK: Criteo, first suggested item?


## Topic 1  [Paul Mrcy & Basile Leparmentier ] Chromes announced here an updated timeline for the retirement of third-party cookies and the Privacy Sandbox. Could we get some details from Chrome about the roadmap?

Basile Leparmentier: The question relates to the big news that was announced a couple of weeks, with the Google Chrome announcement to delay the 3rd party cookies changes.  What does this mean?  Specifically around the Origin Trials, and the timelines of those.  Can you comment more about the strategy for the origin trial?

MK: A few different parts to the answer, reflecting the few different questions.  We have not yet published a start date for the origin trial for FLEDGE yet.  We plan on continuing to update the main Privacy Sandbox, I believe monthly.  That will be a main place for communicating timelines, on privacysandbox.com, so that’s where to look, don’t think there’s anything about FLEDGE OT there yet.  

MK: About “everything” or “some”, the first version of OT will have a minimum amount of stuff, and will be missing a lot of what people want.  Expect this is similar to what other APIs go.  The question about what a MVP for the API looks like is still TBD.  I will point out that (as per discussion in last week’s PARAKEET meeting), there is already part of an implementation through JS ([google/fledge-shim](https://github.com/google/fledge-shim)), like for [PARAKEET](https://github.com/microsoft/PARAKEET/tree/main/Polyfill) in Edge.  

MK: You don’t need to wait for them in browser, but you can use the API in the browser as implemented through JS.  These JS versions don’t have the same privacy protections, but do express the functionality.  

BL: Measurement API: Is it going to be bundled in it, or when the stable version, will it be bundled with that?  The one that will be usable for FLEDGE, will a measurement API be available when FLEDGE is final?

MK: We want all of the APIs, including an aggregate measurement API before the 3rd party cookies go away.  Actually, before then, so that there’s time for migration and all that.  That’s the intention of the new/updated timeline.  Making sure that there’s path to some of the data.  Doesn’t answer the question about what measurement APIs will be available at the earliest time of what APIs will be available at the start of the trials.  But before the end of 3PC time is when the APIs will be available.  

BL: When you presented FLEDGE, you said you might be able to update the doc to reflect the new stuff.  But is it going to be a new doc, or a big change, to account for things like SPARROW?  

MK: A lot of effort recently has been trying to implement first version of spec.  There will be updates to the doc as we make changes and implement things.  This will be a living document, yes!

Vincent Grobois: Comment about origin trials.  Basically, what is the minimum set of things to run an origin trial.  Question/discussion.  Trying to understand the exact () to determine the Origin Trial.  In the Chrome Canary version, there’s a lot of FLEDGE functions already available?  The only real missing piece is some implementation, when a display is done with FLEDGE we get notification of this display.  Do you know if this notification will be implemented sooner, and second question, once the break of reporting is implemented, what are the other blockers preventing this from being origin trial?  Seems like everything is in place to run it!

MK: Yes, everything is being worked on!  You can see relatively recent docs that speak about this.  Not changes to the FLEDGE explainer, but other docs in the repo that include things like specifics about WORKLETS and FENCED FRAMEs.  Maybe [include link](https://github.com/WICG/turtledove/blob/main/Fenced_Frames_Ads_Reporting.md).  https://github.com/WICG/turtledove/blob/main/Fenced\_Frames\_Ads\_Reporting.md

MK: That’s a pointer to some of the work / rendering / reporting in that context.  Maybe that’s part of an earlier answer to the explainer not being updated - it’s in other interrelated docs.  In terms of things being available in CANARY - you can turn on to test on your own device, but this is different from origin trial.  

MK: In the current partial implementation in CHROME there’s no UI, so there’s no way for a user to see what’s going on about ads and other things.  Developers have the tools they need, but they’re only 1 of the 2 major users, and users of Chrome need UIs.  

Matt Menke: What about versioning?   Can we make sure we have that?  

MK: Yes.  Versioning!  

VG: For the reporting, to understand better.  In the “sendreportto()” in the reportwindow function, this is the next thing to be implemented in Chromium, yes?  The link you sent is for the full implementation, a more advanced form of reporting.  Do you think that, for the Origin Trial to be started, that the full version of the reporting to be implemented, or can we do a simplified version?  

MK: Not sure, don’t have an answer yet.  The features required for an Origin Trial are not yet locked in.  

Jonasz Pamula: In terms of Reporting, there is an open P[ull Request (on Turtledove)](https://github.com/WICG/turtledove/pull/194), you can take a look at that and make comments.  There’s another one but that’s less relevant. 

BL: I am happy you are talking about UI.  We haven’t talked about it for a while!  It’s an important part of FLEDGE, user control is important!  When you have a good idea of what it would look like, can you share, especially big ideas, like how do you intend to surface data about ads?  Really happy that you’re talking about this, was concerned it had been forgotten.  Things about what interest groups led to what decisions, surfacing this info to the user, cool.  

MK: Yes, UI is important.  Don’t have any mock-ups yet.  Will share when they’re available!  

MK: There are fewer UI devs in Chrome than other engineers.  So it’s coming along slower.  The UI is something that will evolve over the origin trial, so starting simple (meeting minimum reqs) but expect it to change based on feedback.  Ongoing development.  

Brendan: Is there a UI philosophy that is going to guide this?

MK: The section of the original TURTLEDOVE explainer that says a bit about the [UI philosophy](https://github.com/WICG/turtledove/blob/main/Original-TURTLEDOVE.md#user-interface-controls).  

https://github.com/WICG/turtledove/blob/main/Original-TURTLEDOVE.md#user-interface-controls

MK: That's from about a year ago, gives some of the thinking about the API originally.  Definitely room for improvement from people with expertise.  

Brian May: Maintaining / Capturing interests.  Should this be a separate item from TURTLEDOVE, so that the Interest Management component can be common to TURTLEDOVE / PARAKEET, and others?

MK: This is a hard question.  The API might be tightly bound to what you are wanting to do with the interests.  There might not be commonality that transcends these different intended uses.  There are some similarities for the FLEDGE and PARAKEET APIs, but that’s because some proposals try to solve very similar problems.  


## Agenda Item 2: Workflow integration with existing system (adserver, SSP, etc.) https://github.com/WICG/turtledove/issues/203 (Julien Delhommeau - Xandr)

Julien Delhommeau: Thank you!  The question is about solving workflows of integrating in a FLEDGE world.  My understanding is that there will be a regular, cookieless auction happening before FLEDGE.  At the end of this auction, a second, on device auction (FLEDGE auction), and then a decision on page about which auction to select.  Today, the ad server is the count of record, the ad server is the brain.  Under the FLEDGE proposal, the Ad Server is no longer the final decision, which makes things like pacing hard.  Ad servers are making decisions based on having all the information.  Has this issue of not having all the information been raised before?  How will the existing workflow change to interoperate with FLEDGE?  How do you intend to have Google Ad Manager run alongside FLEDGE?  

MK: A Chrome answer first.  Yes, your question is more for ads team!  But here’s the Chrome perspective.  

MK: The model for FLEDGE is that the same party that used to make the final decision is still able to make the final decision.  We’ve only changed where the decision happens.  Historically, the pub ad server (on a server) would make the decision.  Now the pub ad server (in JS on the client) will make the decision.  The same person (publisher or someone working for them) is making the decision.  So we’re not trying to change the person, but changing where the decision happens.  

Basile Leparmentier: Just to say, one thing that is interesting, the auction is very closely made by Google, since the first part auction?  

Vincent Grosbois: At this time, in the Chromium codebase, the sorting of which bid is the highest and which to return is done by Chromium itself.  Computing the cost and determining the winner is done by Chromium code at this time.  

MK: I don't think that's right.  The expected flow is that every one of the interest groups has a chance to bid, and does.  Chromium doesn’t evaluate the bid, it’s handed over to a JS bundle of code supplied by the publisher ad server, and that code is responsible for evaluating which of the bids is the best.  The code takes all the info about each individual bid and assigns some “goodness” score, and whichever ad gets the highest score.  

LB: Once the score is decided, the price is the decision, and this doesn’t account for second price auction.  

MK: Yes, we want to support first and second price auction.  This doesn’t change who is the winner, what you’re asking is when the ad does the reporting, is the second price bid a number that is available to the reporting code?  As long as we make both the first and second price available to the reporting code, then that decision can be made by the code author.

VG: If this is available to the JS code, then the JS code the code can make decision about what the price.  But right now it seems hardcoded in Chromium, and the browser makes the decision.  If it’s open to much more wide kind of auction, it has to be taking into account carefully, so that the auction still makes sense.  If you allow the JS to say things like the different prices that people pay might be higher.  

MK: Yes, we need to make enough information available to everyone so that fraud and other malicious behavior is detectable, like lying about the second price.  The question about what to do in those situations is more about the business relationship between those parties.  As a browser we want to make sure enough information is available, balanced against privacy requirements, so that malicious actors can be detected and handled.  

Remi Lemonnier: About the auctions, the first auction, then the FLEDGE auction.  My understanding about TURTLEDOVE was that the browser would initiate one, is it only that Chrome will focus on the interest based auction, and that the context auction would be generally left alone?  

MK: Yes, the contextual (or the auction that doesn’t use cross-site data - only contextual and first party data) auction happens in the same way as today.  There is no new API planned for Chrome to enable that, because we don’t think we need to add anything new.  If there is a need for something new, then let us know.  There is desire for some sort of persistent ID for first party sides (see [CHIPS](https://github.com/WICG/CHIPS) proposal), how can people do buying using partitioned data.  If there are other things necessary, let us know.   

Julien Delhommeau: Thank you!  The browser will make the decision about whether to make the choice between the two systems.  Moving from the server to the client is a challenge!  Lots of questions about how this would work, has that discussion happened?  

MK: If there are people working on ad servers, definitely feel free to comment.  There is a GitHub repo about results.  

Philip Lee: not much to comment, working on Trusted Servers, not on the things like pacing, budgeting, etc.  

JD: I have o[pened GitHub issue](https://github.com/WICG/turtledove/issues/203) on this, open to comments on this, to help clarify.  

PL: Will forward the issue, open to other people coming in.  

MK: Some ad campaigns happening now charge on displays, viewability.  Since ad systems currently handle viewable, there should already be infrastructure in place to handle the conditionality of ads at the moment, right?  

MK: This brings us to the end 

Other Topics

Brian May: Anything on the impact of Cookie Delay on FLOC?  

MK: The initial end date of FLOC is coming up in the next few days.  We’ve gotten lots of feedback.  The feelings on the potential and shortcomings.  The fact that there’s a change in the timeline means we have more opportunity to iterate, so we will take this to iterate based on this feedback and come out with more.  

BM: The timeline is relatively the same?  

MK: This origin trial is ending, we’ll probably create a successor version based on feedback.  The [Privacy Sandbox](https://privacysandbox.com/) site will have updated timelines.  

Fred Bastello: Question about FLOC as well.  Since you have all this feedback, will you present back to this group (here, or in Web-Adv BG)?  If so, when?  

MK: Yes, feedback from Origin Trials is discussion that either happens in public (news / twitter / notes in Web-Adv BG) or in private (if they feel like doing so).  

FB: Part of extending the 3PC is to get more feedback, yeah?  

MK: Feedback that Origin Trial participants send to Google directly won’t be made public in a surprising way!  The main summary of that feedback will be the next version, and you can surmise based on what features (and the reasoning for these changes) are available in the next version.  

MK: The whitepaper from Firefox / Mozilla published was a strong example of the type of public feedback, that we take seriously.  

MK: Thanks all!
