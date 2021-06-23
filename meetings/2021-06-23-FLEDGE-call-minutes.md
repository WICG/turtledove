# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 4pm UTC.

That's 11am Boston = 8am California = 5pm Paris time.

(This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload)


# Next meeting: Wednesday June 23 2021


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Brian May (dstillery)
3. Brendan Riordan-Butterworth (IAB Tech Lab / eyeo GmbH)
4. Remi Lemonnier (Scibids)
5. Erik Anderson (Microsoft)
6. Allen Fung (ShareThis)
7. Luke Whittington (Glimpse)
8. Valentino Volonghi (NextRoll)
9. Russ Hamilton (Google Chrome)
10. Phil Lee (Google Ads)
11. Afolabi Adenuga (Nielsen)
12. Fred Bastello (Index Exchange)
13. Michael Coward (Arcspire)
14. Shivani Sharma (Google Chrome) 
15. Paul Farrow (Xandr)
16. Les Cochrane (Audigent)
17. Brad Rodriguez (Magnite)
18. Matt Wilson (NextRoll)
19. Wendell Baker (Verizon Media)
20. Andrew Pascoe (NextRoll)
21. Przemyslaw Iwanczak (RTB House)
22. Larry Price (OpenX)
23. Matt Menke (Google Chrome)
24. Paul Jensen (Google Chrome)
25. Ryan Arnold (P&G)
26. Russell Stringham (Adobe)
27. Phil Eligio (EPC)
28. Romain Quéré (Xandr)
29. Caleb Raitto (Google Chrome)
30. James Fung (Neustar)
31. Don Marti (CafeMedia)
32. David Tam (Relay42)
33. Amelia Waddington (Captify)
34. Aditya Desai (Google)
35. Mehul Parsana (Microsoft)
36. Christa Dickson (Meredith)
37. Vishnu Prasad (Nielsen)


## Note taker: Brendan Riordan-Butterworth


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.



[<img src="https://i.insider.com/5fc55f5350e71a00115580e8?width=700" alt="Screenshot showing location of the Raise My Hand button">](https://www.businessinsider.com/how-to-raise-hand-on-google-meet)



# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

 		


## [Suggest agenda items here]



*   Valentino Volonghi, from last meeting: "Two topics we can get to later:
    *   [Issue 175](https://github.com/WICG/turtledove/issues/175), need feedback, especially understanding feedback and latency.
    *   Second topic: how is billing going to work?  The exchange/SSP takes care of it now.  When the auction takes place on device, how does that get out?
*   [Issue 116](https://github.com/WICG/turtledove/issues/116): answering contextual auction with a bidding function depending on user signals (Remi Lemonnier, Scibids)

Standing Agenda Item: Open Issues, or Issues that Need Opening

https://github.com/WICG/turtledove/issues?q=is%3Aopen+is%3Aissue+label%3AFLEDGE


## Notes

Michael Kleber: It’s all our responsibilities to make sure notes are accurate, so please review, especially if you speak.  Also other housekeeping activities.   

MK: Suggested Agenda Items.  Some items that Valentino brought up at the end, one added today - Issue 116 by Remi Lemonnier.  

MK: Feel free to add additional items, no guarantee we’ll get to them today.  Go ahead Valentino.


## [Valentino] [Issue 175](https://github.com/WICG/turtledove/issues/175), need feedback, especially understanding feedback and latency.

Valentino Volonghi: Real time feedback is the primary discussion point.  There is the possibility that FLEDGE would have 3-or-4-events level of noise for real time, but no fleshed out the discussion yet?  Any more thoughts on this?  

MK: Charlie Harrison?  He’s the person to have in this discussion.  I think the answer is we don’t have a proposal that’s public.  Seeing if we can bring Charlie in on the topic today.  

MK: Best answer at the moment: Understand that some kind of near-realtime signal is valuable.  There are interesting discussions about how to do that have happened with the academic community, it’s come up at some summits.  Relatively realtime monitoring feedback in a way that preserves privacy at low latency, high volume.  There isn’t a proposal to publish yet.  Nothing more to offer, other than we’re actively investigating into how to provide this.  

VV: Do you have a sense as to whether this is “feasible and working out the details” or “unsure of even doable”?  

MK: What’s not yet clear is the levels of noise.  How can noise be added in a way that preserves privacy, especially with realtime.  This opens up timing attacks, above and beyond the normal ranges of bits.  

MK: So how you put all these together, figure out the appropriate way to prevent this from being usable for realtime tracking is a challenge.  It does seem like “how much noise” is the core question, and that affects how useful this ends up being.  Discussion about how much noise is acceptable in use-cases would be useful in order to determine work on this.  

VV: Is there an open place where people can provide feedback (about what latency / noise tolerances would be acceptable)?  

MK: No good issue at this point.  May be able to follow up with a pointer.  

VV: Would be great to have discussion in the open.  It’s a delicate subject for many.  

MK: Agreement.  Not sure which meeting this would be better (or the other weeks).  

Paul Jensen: Thanks for this item.  Can you break down the latency requirements for the 6 different use cases in the issue 175?  

VV: Yeah, most of the use cases are good with 5 minutes.  Where it may be a little more challenging is frequency capping / page level freq capping.  If you want to block it (from advertiser perspective), you want to do it as fast as possible.  Also, cost resolution, level of buying, the more latency the more wrong it gets over time.  Big impact on how deals are structured.  

MK: Thank you!  Adding more information to 175, writing this down would be solid, other people can add more use cases.  175 seems like a good place for that investigation to happen.  

MK: For the thing you just said, fullpage takeover, can you explain more?  How is it “errors”?  

VV: It’s not a bug, it depends on advertisers.  Retargeting tends to buy all ads on the page, since it’s convenient.  Some customers don’t like this approach (be in every slot), so ad networks will limit the amount of ad units bought on the same page.  This is done in realtime, and managed on the servers, with blocking the buys in realtime.  Is an artifact of having multiple channels to buy.

MK: Could on-device decisioning help with this, making it so that the browser can say “hey, don’t sell the same ad to the same user in the same page”?  

VV: Yeah.  

MK: OK, this moves out of the aggregate, and into the browser decisioning.  

Brian May: Is it fair to assume that anything production of FLEDGE would be gated on resolving these?  (unclear audio)

Brad Rodriguez: [Issue 98](https://github.com/WICG/turtledove/issues/98), extending auction to provide multiple ad slots.  This aligns to the discussion that’s been going on just now?  Calling it out in the specific case to handle what VV has raised.  Probably not the first iteration of FLEDGE.  

MK: This looks slightly different - “I want to buy all” rather than “I don’t want to buy all”.  The VV scenario suggests that the subsequent ad slots take into account what has happened in previously delivered slots.  98 is potentially whole page optimization, while VV is sequential.  

BR: It is possible that 98 doesn’t require whole page optimization, it might be greedy (sequential) rather than holistic.  

Brian May: Is it fair to assume that any release of FLEDGE would be gated on some of these reporting issues, and if so, what would they be?

MK: Some sort of aggregate reporting is necessary for FLEDGE.  The simplest possible implementation is event level reports, which we already committed to doing for wins.  If nothing else is possible, we could say everybody gets event level reports, even though is not private at all, so this will not reflect long term reality, so won’t be helpful for testing.  We do need to have some form of agg reporting so that FLEDGE can be considered a good privacy function.  

MK: How much of these is TBD, we don’t have a position aside from the BYO Server discussion.  


## [Valentino] How is billing going to work?  The exchange/SSP takes care of it now.  When the auction takes place on device, how does that get out?

Second Topic - Billing!

VV: Working through details yet.  Out of all the blocking issues in FLEDGE, Billing is going to be the real blocker. DSPs are going to have trouble developing the infrastructure to pay tens of millions of publishers.  

VV: With questions about the accuracy of billing, and who will be responsible, and who is selected - all of these are open questions, and will require lots of business process.  

VV: In today’s world, exchanges are rewarded for the quality of data that they share in the session.  If the browser generates a bunch of worklets, and the exchange that develops the best on-device data is rewarded.  The browser makes a bunch of determinations, and then assigns the value to the exchange, who then also needs to assign value.  Publisher and DSP need to be paid as well.  

MK: Pulling that apart a little.  There are several different issues that are all coming together.  First of all. Who pays money to whom?  That’s not a question that the browser should have an opinion on.  It simply offers infrastructure.  Everyone making a business of it should get to make those decisions.  Browser is  a platform to support the variety of decisions that companies make when it comes to business deals.  

MK: Then the reporting interfaces make it so that those terms can be fulfilled.  

MK: on the second, the question about multiple exchanges with different quality of information being made available to the bid system, is that multiple auctions by multiple sellers on the same page, supervised by a higher level auction of the winners?  We have talked about something like this, [issue 59](https://github.com/WICG/turtledove/issues/59) might be the canonical discussion.  This is a hard design problem, determining the hierarchy of auctions on device is tricky.  What you said is that the browser could make a determination among them?  This is not the goal of the browser, we would want SSPs on device, but some other seller representing the publisher who runs the auction between the SSPs.  We expect arbitrary business logic.  

MK: That said, this is something that we haven’t adequately designed, especially if Winner of AUction A is also a participant in AUction B.  Lots of complications!  As a browser person, this is an ads industry type of question!  I’d like to listen to a discussion about hierarchies of ad decisioning could happen.  

VV: Haven’t yet read 59.  Will give it a read.  Need to flesh out a specification here.  

Andrew Pascoe: My perspective, I think it’s fine for browser to be agnostic about auction, how winners get selected.  However, supply can come from multiple sources.  Multiple bids through multiple SSPs.  In the browser, in that case, I would expect that the contextual requests that each SSP sends can be enhanced with extra data.  When we send request back, it has to be found in the browser, but then the browser needs to make multiple…  The DSP question about who-pays-whom depends on what set of contextual data was used to generate the bid.  

AP: The browser has to log who won, where did the data come from.  This might be on the JS to log and send.  

AP: Figuring out who won, and what price, having the browser take some responsibility on that makes resolving discrepancies easier.  

MK: Example: The GenerateBid on the interest group might get called twice, once in the context of each of the auctions if two systems were on page.  These two invocations would have different data, since each SSP has different insight.  Each SSP would chose a winner from their own on-device auction, but then we get to issue 59 (hierarchy of auctions).  

MK: When we’re dealing with hierarchies, we need to know who’s a winner and who’s a loser.  Not just local auction, but the global overview - your auction failed to win against others, even though your initial bid was success.  

AP: We know that the browser has to know the final winner, since it knows to render the ad.  That’s where we 100% know who won, and, in theory, looking for more thoughts on the browser enforcing similar reporting standards across the DSPs and SSPs.  Make sure that things like IG origin is this DSP, and so on.  “You won under this SSP” and “You won on this network thanks to this DSP.  

MK: Rendering an ad, we know something is happening, but we don’t know that this is the right thing.  Delayed render can affect what a win actually is (examples presented).  Need industry input!

Joel Meyer: Quick note. Issue 202 for Multi SSP support.  Could be closed for issue 59.  However, it would be useful to start with a clean sheet.  A couple of different scenarios.  How do we make progress so that we’re all aligned?  

MK: Yes, 59 has a bunch of data mixed in.  I may be mixing stuff in that is unfairly mixed up.  Is working with multiple SSPs the same as some other prebid scenarios?  Not sure if we’re looking at the same or different scenarios? 

Brad Rodriguez: What we’re dealing with the is the multi-exchange problem.  The primary thing that PreBid.JS solves is allowing multiple parties to bid on the same ad slot.  Lots of what has been brought up aligns with this discussion.  

JM: Hard to speak for PreBid, because it’s a consortium.  Prebid is an answer to a problem, and alignment woul be cool, but ultimately we need a solution.  

Mehul Parsana: In the hierarchical auction, overlaying reporting, it woul be hard.  If multiple SSPs register wins on their own systems, then it’s inefficient, and value flows out where it didn’t before.  

MK: Yes, what VV brought up feeds into both of those.  


## Third Topic - [Issue 116](https://github.com/WICG/turtledove/issues/116), Remi

Remi Lemonnier: This has been discussion a few times before.  Interest group auction is great for some things.  But there are prospecting campaigns.  Advertisers need some kind of data based on cross-site data to support prospecting campaigns.  116 was a way for a DSP / media buyer to ensure the contextual auction, instead of the more complex user auction.  

RL: This system was more exposed to privacy tracks.  Allows the disclosure of 1 bit about the user.  This is already possible to get 1 bit of information about the user.  Question: MK, for you, do you identify differences between the GenerateBid function in FLEDGE, versus what we have proposed?  We have been trying to think about privacy attacks that are specific to our proposal.  

MK: I’d like to differentiate some use cases.  Cross-site freq cap vs A/B testing.  In A/B Testing, we understand, in principle, about how this should work.  Ben Savage has documented this, early on.  You should be able to give the browser 2 different ads and allow 1 of those two ads to render.  The pub page doesn’t know what ad has been rendered.  Cross-site freq capping does present the link of a single bit of information (like user identifier).  If there are 30 different ad campaigns, and I select a random selection of them on Site A, trying to show you all on Site B and having the failures leaking information about user.  

MK: Also a way to leak under/over cap.  

RL: What’s the difference of this happening?  You have the same thing.  1 bit leak is a worry.  

MK: It’s harder to do this on-device, and more detectable on device.  Comparing FLEDGE to Turtledove?  More effort to go through for FLEDGE.  Specifically for on-device frequency capping, if you say "show ad A if it's under freq cap and that can happen inside a Fenced Frame, otherwise show ad B which renders without a Fenced Frame", then the over-cap condition immediately leaks the bit of information.  

RL: We didn’t imagine such a failover / fallback.  

MK: Determining what the fallback is if an ad is over cap, that’s the key to dealing with this type of leakage!

BM: CMA Comments?

MK: No.  
