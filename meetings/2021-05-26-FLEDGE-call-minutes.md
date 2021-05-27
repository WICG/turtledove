# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 4pm UTC.

That's 11am Boston = 8am California = 5pm Paris time.

(This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload)


# Next meeting: Wednesday May 26 2021

**_NOTE: May 26 meeting will only be half an hour.  Sorry!_**


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Paul Farrow (Xandr)
3. Brendan Riordan-Butterworth (IAB Tech Lab / eyeo GmbH)
4. Allen Fung (ShareThis)
5. Julien Delhommeau (Xandr)
6. Brian May (dstillery)
7. Russell Stringham (Adobe)
8. Newton (Magnite)
9. Rémi Lemonnier (Scibids)
10. Wendell Baker (Verizon Media)
11. Andrew Pascoe (NextRoll)
12. Fred Bastello (Index Exchange)
13. Nathan Jia (Nielsen)
14. Jonasz Pamuła (RTB House)
15. Przemyslaw Iwanczak (RTB House)
16. Michael Coward (Arcspire)
17. David Tam (Relay42)
18. Christa Dickson (Meredith)
19. Vincent Grosbois (Criteo)
20. Grzegorz Łyczba (OpenX)
21. Jeff Kaufman (Google Ads)
22. Matt Wilson (NextRoll)
23. Phil Eligio (EPC)
24. Erik Anderson (Microsoft)
25. Afolabi Adenuga(Nielsen)
26. Don Marti (CafeMedia)
27. Valentino Volonghi (NextRoll)
28. Amelia Waddington(Captify)
29. Les Cochrane (Audigent)
30. Larry Price (OpenX)
31. Robert Ries (Nielsen)
32. Tamara Yaeger (IPONWEB)
33. Brad Rodriguez (Magnite)
34. Vishnu Prasad (Nielsen)
35. Kelda Anderson (Microsoft)
36. Ryan Arnold (P&G)
37. Viraj Awati (Amazon)
38. Tanya Moldovan


## Note taker: Brendan Riordan-Butterworth


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.



[<img src="https://i.insider.com/5fc55f5350e71a00115580e8?width=700" alt="Screenshot showing location of the Raise My Hand button">](https://www.businessinsider.com/how-to-raise-hand-on-google-meet)


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

 		


## [Suggest agenda items here]



*   Held over from last time:
    *   https://github.com/WICG/turtledove/issues/177 (aggregate attribution reporting API & FLEDGE?)
    *   https://github.com/WICG/turtledove/issues/116 (FLEDGE : Answering contextual requests with a generate\_bid() function)

Standing Agenda Item: Open Issues, or Issues that Need Opening

https://github.com/WICG/turtledove/issues?q=is%3Aopen+is%3Aissue+label%3AFLEDGE


## Notes

Michael Kleber: Preamble, WICG membership required, location of notes, letting people in.  

MK: Agenda items held over from last time.  Let’s get started.  

[Issue 177](https://github.com/WICG/turtledove/issues/177)

MK: Jonasz Pamuła

Jonasz Pamuła: For those of you without context, the issue is about extending FLEDGE with additional reporting mechanism.  This need has been identified through 93 and other items.  The proposed reporting mechanism won’t be enough to optimize our bidding models.  What’s missing is “Report Win” doesn’t have access to all bidding signals, by design.  We need an additional reporting mechanism with access to all bidding signals, most likely some flavor of aggregate reporting.  Earlier proposal was “custom reporting token”.  The Attribution Reporting API has been updated since then with details.  I have to ask, can extend the FLEDGE spec with additional agg reporting mechanism?  If so, can it align with Agg Attribution Reporting API?  Especially with Bring-Your-Own server mode, so that each party can create aggs on their own, to facilitate adoption and early testing?  

MK: Good, thank you for the summary.  Let’s see.  

MK: The idea seems right on, the intention for FLEDGE is event level signal available, but the reliance on full data on aggregate reporting.  Hooking histogram up.  Thumbs up on the high level, but there are details to be worked out, especially carveouts during the ramp up / startup phase.  

JP: I am happy to hear general idea sounds good.  The reason we keep bringing this up is that the details about how this will be implemented are really important to determine what models can be used.  It will take time to design models, build them, test them, and all that.  The sooner the FLEDGE spec is extended, the better.  

JP: Issue 177 proposes to use the pieces from Attribuition Reporting API, and to produce the agg reports in 4 different situations, so we can receive for Impression Level Data - after an auction is won, we have a chance to generate report and send to agg server.  Also other situations - after on-click, for conversion, and perhaps for lost auctions.  These are the 4 scenarios (Win / Click / Conversion / Lost).  Sound good?  Can we propose a pull request that implements this?  

MK: All makes sense.  For the 4 detailed times when you might want to use things, producing inputs to the agg API in both Generate Bid and in something that you do during Generate Bid that only get reported after Click report happens, those sound exactly like what you’d expect.  Number 4 also sounds correct.  During Generate Bid, say what you want the histogram to be updated if loss occurs.  

MK: What requires more thought is number 3, being able to dynamically produce the context signal from within Generate Bid.  This is the thing that is attached to the event level conversion data.  And that might require more privacy thought.  After click / conversion / ???  The attribution source context is available inside the fenced frame.  We don’t want arbitrary info channel from Generate Bid to the Fenced Frame.  And also, we don’t want it to be a channel to Event Reporting.  This is asking for Attribution Source Context to include both UUIDs - from the content, from the ad.  I don't’ think this would be allowed, since it might allow cross-site linking.  

JP: The threat is a technical side effect of what we’re trying to achieve.  We want agg reports based on data, but not joined at the event level.  

MK: What you’re saying is.  Your goal is to get the Agg Conversion API to join up more signals to the Conversion data later, because you want to aggregate data later.  

JP: Yes, the use case is to optimize conversion rate models that use impression level data as input.  

MK: What we need to do then, in order to support #3, is to figure out how to attach information to this ad impression (and click, if it happens) so that it is available to the Agg Conversion API.  Make sure the mechanism doesn’t allow a UUID link.  

JP: Sounds great.  One question remains.  What is next steps?  Would it be useful for us to propose pull request?  How can we make it happen?

MK: Do pull request.  In the spirit of the explainer: Indicate what the goals are.  The exact details / implementation details are not currently written down, so don’t get too much more details than everything else around the space.  Yes, I think that pull request is fine.  

JP: Last question - could this work in the Origin Trials, in the BYO Server mode?  If not, it might turn out that the Agg Server integration might happen much later, which is a bad outcome.  

MK: Yes, agree, getting access to this info is important in any origin trial.  Recognize that this is a reasonable requirement.  

Remi Lemonnier: I think this is very important, training bidding model is important, so this is good progress.  

[Issue 116](https://github.com/WICG/turtledove/issues/116)

Remi Lemonnier: This is a bit of an old issue.  The context is that we think media buyers need to be able to have bids that vary based on user signals (simple, like freq cap, and more complex).  I think my last question: will you be able to provide as a use case that you are concerned about, if we had a generate bid context, and another, would it be OK if there is 1 bit of information leaked?  How can we mitigate the risk?  

RL: The second point is whether the criteria is, even if this is very expensive and requires stuff upfront, should we reject any cases where 1 bit of information for cross-site is rejected?  Or is there a budget allowed about previous data.  Understanding the cost of getting bits from cross-site allows rational decisions.  

MK: I think that the answer you give at the end is what makes sense.  1 bit leak of information that the surrounding pub page can learn, like “was it a targeted interest group ad that won?” allows a pub to learn 1 bit of information about the user.  That’s unfortunate, would love to be rid of it, but it’s a matter of balancing privacy against utility.  Allowing this functionality is important to adoption, migration.  Making it hard to exfiltrate data is important to continue allowing it.  Understanding better how data can be exfiltrated, and bit reuse, probably should be avoided.  

RL: Why is the generic case more risky than the single-mode design?  Can we get some clarification?  

MK: Reading through the latest comment.  In a world where the SSP is also a DSP, and the SSP is willing to say that the only bids that it’s willing to look at are it’s own, in that case it’s already possible for the SSP to trigger the leak of the 1 bit of information (if they’re in the interest group).  

RL: Yes, it’s a very unlikely case, but it’s a way that info can leak.  Is it different if we make it generic?  

MK: The point is, the SSP that rejects all bids except those from a DSP, that seems like surprising case.  The browser could notice and indicate as suspicious.  The browser tries to ensure that data isn’t being exfiltrated.  Building in browser oversight to understand whether the auction is competitive?  The browser could hope to have an opinion on.  The one-bit leak where the contextual bit get handed into something, where the browser received 2 different ads from a server, 1 using on-device state, the other not using on-device state.  The browser doesn’t have additional signal in this case.  

MK: I think the unregulated access to a bit is fairly different than the existing tech.  But if there is a minimum threshold on things like number of auctions, and the auction seems to be performed fairly, make sure that the browser can be evaluated as fair, then there’s path for the generalized single bit leak.  

RL: There is probably a way to find metrics that support safety of the proposal.  

MK: Yea, there’d need to be a way to ensure that the generic single bit couldn’t be used to get information across.  

MK: I have to leave now, but you’re welcome to stick around!  
