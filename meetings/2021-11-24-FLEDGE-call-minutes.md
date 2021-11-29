# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 4pm UTC (during winter).

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Nov 24 2021


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Jonasz Pamuła (RTB House)
3. Aleksei Gorbushin (Walmart)
4. Paul Jensen (Google Chrome)
5. Brian May (dstillery)
6. Sven May (Google Chrome)
7. Matt Menke (Google Chrome)
8. Michael Coward (Arcspire)
9. Brendan Butterworth (eyeo GmbH)
10. Bartosz Marcinkowski (RTB House)
11. Przemyslaw Iwanczak (RTB House)
12. Don Marti (CafeMedia)
13. Łukasz Włodarczyk (RTB House)
14. Bartek Łoś (RTB House)
15. Richard Anton (Amazon)
16. Stan Belov (Google)
17. Andrew Pascoe (NextRoll)
18. Brad Rodriguez (Magnite)
19. Christa Dickson (Meredith)
20. Matt Wilson (NextRoll)
21. Martin Pal (Google)
22. Vincent Grosbois (Criteo)
23. Ryan Arnold (P&G)


## Note taker: std::pair&lt;Michael Kleber, Paul Jensen>


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

 		


## [Suggest agenda items here]



*   ~~(Jonasz Pamuła) https://github.com/WICG/turtledove/issues/233, caching of bidding logic~~
*   (Jonasz Pamuła) Detecting generateBid timeouts? (issue: https://github.com/WICG/turtledove/issues/239)
    1. an API for finding out "how much time is left"?
*   (Paul Jensen) Discuss FLEDGE worklet language needs
*   (Andrew Pascoe) FLEDGE debug API?


## Notes



*   (Jonasz Pamuła) Detecting generateBid timeouts? (issue: https://github.com/WICG/turtledove/issues/239)

Jonasz: Topic initially was about timeouts, but issue is a bit wider than that.  In FLEDGE the only way to report something during/after an auction is to use reportWin / reportResult, so no way to report errors within generateBid() or learn whether the function times out.  If we deploy bidding function that times out 10% of the time, we won't know it's happening, won't have a way to tune the function to trim it down to succeed more often.  One suggestion: add a reportLoss() at event level that operates on subset of inputs

Brian: Timing issues across many browsers (tens of millions at least), so meaningful reporting across the population will be very challenging.  Maybe something like percent of population where something like a timeout happens, but reporting would need a lot of data to identify which slice of traffic is generating the errors.  need something robust across all devices to have any consistency

Paul Jensen: To prevent information leaking in/out of worklets, we expect to need to restrict information leaking into/out of worklets.  Browser already restricts those, to avoid side-channel communication.  Already restrict what's available in worklet environment.  May be possible to offer an inaccurate timer — what accuracy would you need?

Jonasz: Paul's answer relates to the second question I was going to ask: having a timer inside generateBid() would be useful, so we can perform an amount of computation appropriate to what time budget the seller gave us.  But my original question was about reporting — not about access to timers inside the bidding function, but status information inside reportLoss() to indicate that (i) you bid successfully but didn't win the auction vs. (ii) your bidding function crashed vs (iii) your bidding function ran too long and was timed out.

Paul: Should be easy to offer status function about number of msec allowed to run for.

Jonasz: That can already be provided by publisher in auction signal, so that's not a new vector.

Michael: Status of worklet (lost/ran out of time/success) could be provided in browserSignals to reporting wins and losses.  How that gets to network is important to consider, e.g. aggregate reporting.  Perhaps report could only get high level outcomes, not full context.  Event level logging is one mechanism assuming we take into account privacy risks.  Event level with restricted inputs is one way to account for privacy risks.

Brian: Seems like we have two contexts: on-browser context, where worklet needs to know what's going on (which can be rich flow of data browser->worklet, maintain statistics about past outcomes?) vs data that gets exported to the developer (where maybe want an interactive reporting system?)  Interactively drilling down to errors in different categories

Michael: In browser API to provide drill down info falls into the event-level vs aggregated discussion.

Brian: Have to talk about when we get into billing.  Can’t query every browser.

Michael: Proposed API contains event level logging for auction winner.  Jonsaz is pointing out that we don’t have that for losing bids.  Winning bids can have more information than losing bids.  Winning bids are k-anonymized, losing bids are not.  The set of all stored ads would definitely be a fingerprint.

Stan Belov: Follow-up question: what you described seems different from what's currently in the FLEDGE explainer, in terms of what information would be available.  What use cases do you have in mind, and what information would you need to support them?

Jonasz: Inputs could be restricted because of privacy considerations.  Status of invocation is a very important piece of information we want to get.  If we only get that, it's much better than nothing!  Global counter of successes and timeouts would be a significant improvement.  If we can slice that by some dimensions, e.g. to learn "we timed out on older phones but not on newer laptops", would be even better.  Access to "perBuyerSignals" from reportLoss would be useful.

Stan: Would you also care about what bidding function ran?  What version of code you're running would very much impact outcome here.

Jonasz: would be useful, yes.

Brian: Comment about "what version of code" raises a question about what you do when you get that information?  How do you adjust in response?

Jonasz: Of course it depends on how much information you have!  Do you need to optimize, or look for errors, or fraction of timeouts?  Can globally make computation lower, but if we have more dimensions to slice, then response can be much more precise.

Paul: Probably a fair bit of difference in performance between phone and laptop etc.  What if the bidding worklet got some state?  e.g. bidding function gets "Did this bidding worklet time out the last time it ran on this device?"

Jonasz: Would be useful for on-device control.  If providing exact msec left isn't acceptable, then an API like "is there at least 3ms left?" could be useful already — compute in chunks, return the best answer so far when your time is almost up.

Brian: Is there some kind of history per-IG-owner that can be persisted over time, adapt based on experience?

Michael: At present, interest group gets to learn about previous wins (e.g. for freq capping).  The privacy model for an interest group is not supposed to bid on global profile of all sites visited, instead it’s limited to site where user was added.  If interest group could store information every time it got to bid, it could build up global profile which is similar to 3rd party cookie model which is different from FLEDGE’s model.  

Brian: I was thinking about something related to counters that would let the worklet have some idea of how often it had timed out — not full information about past behavior and context.  If we don't make worklets adaptive in that way, there's a lot of risk of excessive resource usage.

Andrew: Question about the last comment about privacy model: I thought we could have as much detailed information as you want inside the IG, that detailed information may not leave the browser (subject to K-anon, DP, reporting, etc).  The proposal SPURFOWL about maintaining a timeline or trail history that stays in the browser, or a proposal from Google that was similar [maybe Shared Storage?].  I'm surprised that you're concerned about data being passed into the bidding function, provided it stays in the browser.

Michael: You're right that the essential Privacy Sandbox model that we want is that information stays in the browser.  But beyond that, interest group bidding being based on site where you were added and site where ad can be shown has advantages about what browser knows about what ad is shown, and user gets these same advantages.  Look at these comments from the original TURTLEDOVE explainer about possible UI controls:

https://github.com/WICG/turtledove/blob/main/Original-TURTLEDOVE.md#user-interface-controls

Points out advantages of interest groups being stored on browser is the insight the browser has into why someone sees an ad.  Browser can tell user why they are seeing a particular ad (i.e. activity on other site where user was added).  If interest group bidding is based on global activity on all sites, then the transparency the browser can offer to users is substantially weakened.  Long term answer to what info is available at bidding time is undecided.  Would be a change in privacy and storage model from what we have today if we allowed global profile availability.

Andrew: So this is not a privacy concern, it's an interpretability question?  On the advertiser site, arbitrary logic can be used to determine all sorts of crazy segmentation approaches.  And actually seeing an ad is a function of what's in the bidding logic, whether the site is worth it, etc.  One of the other things we've said is available in FLEDGE is that I could visit the advertiser's site, and an IG could be added to show ads for a completely different site; that pulls on the interpretability of the logic — "I saw an ad for a toaster because I went to a site about video games" doesn't seem helpful.

Stan: Reflect on Brian's previous comment of worklets over-using resources if they always just tiem out: Could be useful to seller / publisher also.  Seller should also be able to make use of that information, who might make choices about which buyers are invited to run on the page.

Brian: Re. where IG came from, interpretability by the user: It seems like different types of data could be treated differently, keeping success rates / timeout rates handled in a way that's different from adding IG data and bidding based on it.  Giving worklets some persistent store of their own would trigger a privacy concern if someone can figure out a way to exfiltrate information about what's happening on the browser, which is what's addressed by k-anonymity and aggregation.  Seems like there should be a way to give worklets some limited space associated with an IG and remember what it's done previously.

Michael: A lot of subtlety here.  Ability to record arbitrary information is a significant change. FLEDGE mechanism has access control where site that they’re on must cooperate with the person adding them to interest groups.  Right now this might be available for ease of adoption, but this can be restricted to prevent “audience stealing”.  If interest group gets to record arbitrary information from previous bidding opportunities, it circumvents the protection against “audience stealing” by adjusting bid amounts based on previously visited sites.  The fact that which information from which sites gives privacy and user experience considerations and business considerations that are different from what we’ve talked about and we need to consider these implications.  Can have open and collaborative discussion about what behavior can affect bidding behavior.  There are people with different privacy and business model views that will make this tricky.

Don Marti: I think there's also a performance consideration: if IGs can check for whether or not a user has visited a particular other site, then sites creating IGs may generate a lot more, speculatively, in the hopes of someone going go to a valuable site later — so lots of IGs that don't actually get used except when they can sometimes catch users visiting a different site.

Michael: Yes, lots of resource discussions.  7 mins left.  Reviewing agenda.

*   (Andrew Pascoe) FLEDGE debug API?

Andrew: Engineer playing with FLEDGE APIs.  Pass in flag to start using FLEDGE APIs.  Would like another flag to enable debug mode to dump everything into browser, at least until UI developed.

Michael: No privacy concern with developers investigating their machines.

Paul: What does "dump into the browser" mean?

Andrew: Suppose we add an IG to the browser.  We don't get success or failure response.  Any way to add a JS call to see the IGs in the browser?  See on-device state, with some additional flag, so that we can see the results of having invoked the APIs?

Paul: I'm trying to think of parallels in the browser — nothing directly like that.  Dev Tools certainly has that kind of visibility.  We have added debug capabilities to Dev Tools, e.g. to have breakpoints inside of worklets.  There are command-line ways to dump browser store, and we're working on some console logging, e.g. for things that go wrong.  Dev Tools doesn't have a way to turn on extra logging afaik.

Andrew: Right now our engineers are playing with "we've downloaded Chrome and playing with it", but not gone into Dev Tools, maybe need to dive in more.

Bartosz Los: Even now it's possible to do extra debugging!  Right now we sometimes add patches to Chromium, to measure compilation or running time of worklet.  Writing extra code for extra debuggability is possible but hard, would be nice to have that behind a flag!  Maybe there are use cases that are common to everyone that Chrome could address with a debug flag.
