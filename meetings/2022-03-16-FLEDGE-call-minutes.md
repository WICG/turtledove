# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time (but 4pm this week!!) = 3pm UTC (during summer).

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Mar 16, 2022


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Brian May (dstillery)
3. Caleb Raitto (Google Chrome)
4. Paul Jensen (Google Chrome)
5. Chris Evans (NextRoll)
6. Matt Menke (Google Chrome)
7. Andrew Pascoe (NextRoll)
8. Russ Hamilton (Google Chrome)
9. Jeff Kaufman (Google Ads)
10. Matt Wilson (NextRoll)
11. Valentino Volonghi (NextRoll)
12. Christina Ilvento (Google Chrome)
13. Joel Meyer (OpenX)
14. Christina Brauer (Capital C)
15. Wendell Baker (Yahoo)
16. Brad Rodriguez (Magnite)
17. Supraja Sekhar (Google Ads)
18. John Mooring (Microsoft)
19. Bartosz Łoś (RTB House)
20. Philippe de Lurand Pierre-Paul (Google Chrome)
21. George London (Upwave).
22. Daniel Rojas (Google Chrome)
23. Jonasz Pamuła (RTB House)
24. Alex Cone (IAB Tech Lab)
25. Ben Dick (IAB Tech Lab)
26. Don Marti (CafeMedia)
27. Stan Belov (Google Ads)
28. Aditya Desai (Amazon)
29. Sergey Tumakha (Microsoft Ads)
30. Fred Bastello (Index Exchange)
31. Alexandru Daicu (Eyeo)


## Note taker: Alex Cone


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

 		


## [Suggest agenda items here]



*   [Browser Bandit to Curb Computation · Issue #268 · WICG/turtledove · GitHub](https://github.com/WICG/turtledove/issues/268)
*   Experiment group ID
    *   https://github.com/WICG/turtledove/issues/191
    *   https://github.com/WICG/turtledove/pull/266


## Notes

Michael Kleber: Intro [issue 268](https://github.com/WICG/turtledove/issues/268)

Andrew Pascoe: offhand comment in priority scores discussion; browser wouldn’t be able to evaluate everything so what if advertisers injected a priority score to be evaluated by browser (that was in issue 79); the could be updated through the trusted server mechanism, but the scoring might relate to the page context which would give way to a need for a background process; issue 79 sort of kicked the can down the road; Instead what about a Bandit approach: there’s some prior on the probability that a given ad unit would win an auction, browser keeps track and plugs into \_\_\_\_ (need to check the issue 268 description for further summary). Based on Google paper. Thompson sampling. Better than trying to plug in priority scores. Could be a “contextual bandit” per page category. “I think this ad will win on this page about this type of topic.” This is the less fancy way. Fancier would be a GLM. Bigger ask as doing ML in the browser. Don’t need to be a particularly large model. Frequency for example could be part of a ML. Computationally quite simple. Balances utility and exploitation. A note on wins and losses: browser could take note of ghost wins and losses, by evaluating what would have happened had every participant been computed, even if it can't run all the bids in real time

Jeff Kaufman: I want to make sure I understand the problem we're trying to solve. We expect there will be a zillion interest groups in browsers, and trying to run an auction on all of them each time may cause computers to catch on fire, so we want to try to run generateBid() less often but ideally still get the result we would get if generateBid() ran every time. 

Andrew Pascoe: “that is correct”

Jeff Kaufman: In Issue 78 we were talking about having buyers provide scores, which maybe could be updated in generateBid(), and it could run for the highest-scoring interest groups.  And here you're proposing that buyers not need to provide scores, the browser does the work, and maybe we get the benefits of really well done scoring without the buyer having to do anything at all?

Andrew Pascoe: “that is correct.” How does this ad seem to be bidding from a browser POV. This takes a data centered approach to say these are the things that seem to influence the win the most, let’s evaluate those

Stan Below: couple of questions: (1) some sort of score would be determined off the historical probability of winners and losers

AP: it’s bayesian, with no data it’s uniform distribution, with a % chance of winning auction; would update prior to get a posterior 

SB: I’m somewhat worried about how noisy the data on historical wins and losses would be on a single browser instance. If you think about this on an ad buyer pov…they probably want frequency capping, pacing (spreading budgets more easily) and the effect of this could be that ads stored in any one browser could result in over exposure plus fluctuation in win rates

AP: in the case of f-capping and pacing in order to know you’ve hit it you’d need to run generateBid() you’d get a no bid that you could chuck as a loss. What is the probability that if I were to evaluate this that it would contribute to auction decisioning. Re: pacing - the most complicated piece, GLM, addresses this. The browser would be keeping track of this feature. The browser would need to train somehow. Counterpoint is that we’re not going to know frequency and recency anyway. If we go priority score route those get noisy too and possibly inflated. This (268) is more data driven to get to the heart of the problem

SB: f-capping might keep ad from winning

AP: yeah, that’s probably what you want. Browser not trying to evaluate. There’s a chance the browser will be like “oh I got a random number that was high enough to include you in the auction.” If you win it shifts priorities back up. It’s a very adaptive process. Could also be that we add something around time windows.

Brian May: it’s an interesting proposal. We need to be careful about how much we hand over to intermediaries. We should be very clear about what kinds of things in today’s stack we’re going to impact. We’re putting people into a position similar to SEO. Google makes an ML change and industry has to rebuild/retool.

AP: Agree, but if we’re moving forward with FLEDGE, we’re already ceding this ground. If we’re taking the view that the browser is running the auction and there are a lot of ad units, the browser has to decide. Something has to be an input to this. Could be provided by the outside, but it’s probably less valuable that way because it’s effectively inputs while flying blind. Will be a scramble to figure out what is working best. Browser would still be in the position to evaluate the scores (prior issue 78). Fundamentally the same problem either way. New proposal is just more efficient.

BM: Anytime the browser is deciding for the participants to the auction it makes it very hard to work with.

MK: as a browser engineer I tend to agree with BM. It’s a lot of decision making handed to the browser and there are lots of ways that could go badly. I’m cautious and skeptical about this. Not saying no, but it really should be simple and easily understandable heuristics when prioritizing computational resources. Example of cache performance behind the scenes, but we generally prefer to leave the type of decisions that AP’s proposal is talking about in the hands of the infrastructure. A completely deterministic solution like previously discussed priority scores is most preferable. Perhaps handing the determination to the seller is more desirable still from the browser taking on this type of decision-making. There are a lot of other things MK would prefer to see the browser do than this.

Russ Hamilton: a couple points. One of the things someone mentioned is that the state of the internet might change if there was rate limiting. You would need to time discount win and loss count. Could create perverse incentives to create new interest groups. New IGs would have a higher priority than old unless something was just winning all the time.

AP: you only get to create an interest group if you’re on the advertiser site. Are you suggesting the tech would be stuffing IGs?

RH: yes. Also echoing MK as someone who works in the browser we’d prefer to put control into hands of the browser. AP had suggested sites could cite a lot of owners. Seller would have to be complicit in that and may actually be what the seller intended to have happen.

Brad Rodriguez: allow controller of page to specify algo (or none) or logic that would be used that might go into a controlled data store

AP: saw people chuckling when originally proposed. Knew then that it was complex. It doesn’t have to be as complex as it sounds. Open to controls over the Bandit Algo.

BR: would help with BM’s concern that browser changes the algo. Pubs could make choices on their own priorities.

AP: interesting. I like that.

Don Marti: Similar point to BR’s point. The browser’s job in this case is to try to buy for the user the maximum ad supported content for the minimum attention and computational resources. If we do have an idea like this multi-armed bandit proposal that looks good it might be productive to enable a browser extension to run it. Users could try alternate versions of ad decisioning, and report results, which would help inform future browser defaults. Is there interest in a browser extension API?

MK: interesting idea, but not prepared to comment on this. If this is subject to the control of the seller of the auction perhaps making it available to extensions might make sense. Given that you framed it as an optimization problem for the user, we might even know what the answer to that is. “[The power of two choices in randomized load balancing”](https://www.eecs.harvard.edu/~michaelm/postscripts/mythesis.pdf), Michael Mitzenmacher's Ph.D. thesis: evaluate a random two interest groups, pick whichever of them bids higher.  I'm pretty sure most people here would not like that answer, but worth thinking more about what the optimization problem is that we're really trying to solve.  [Don Marti comments in reply: Enabling extensions here would make it pretty straightforward to run the UX research [on whether that actually _is_ a good idea!]]

Alex Cone: A couple of comments.  (1) What Andrew proposed is smart, given the circumstances we're living in — trying to make a determination of priority scoring outside the browser might be folly — but I want to agree that complication of the black box that starts to happen (even if we know the algorithm) becomes hard for the practitioners.  People who need to answer Q's from publishers about "why aren't my ads delivering?" are in an awful position if they need to understand browser algorithm innards, could be very onerous.  Please take into account the poor frontline support people who will need to answer these questions. (2) Re publisher control over this logic: there are demands on both buy-side and sell-side here.  Are we saying that all the control should live on the publisher side, where the ads are being served?  Seems like the buy-side decision-making should have influence also

AP: R (2), maybe two sets of parameters, provided by each of buyer and seller — hadn't really thought about the fact that this might be controllable, could be interesting.  Re (1), I think this is actually much simpler to understand than our usual ML algorithms!  Maybe some people are disturbed by the non-determinism, but that doesn't bother me: law of large numbers applies quickly here, it will all average out to the right thing.

Brad: Publisher chooses who to make requests to, and the choice of who to make requests to will affect who can bid and monetize.  Requires buy-in from the buy side.

Paul Jensen: Thanks for writing this up. Remember the multi-armed bandit problem and went down a rabbit hole. I like this application here. Talked about buyer priority before. I think something where buyer could look at win rates across large sets of users vs on device, there may not be enough signal on device (on browser). There may not be a lot of past signal on IGs. Maybe there’s a hybrid. Not super thrilled about the browser making decisions. Browser gives developer content and control historically. A buyer might set a fixed priority based on past knowledge with IG, but also have adjustments to that. An IG that wins on the browser might be an indication of wins in the future. If this wins, bump priority by x. Maybe there are even f-capping reasons to bump priority up and down. Could even be a negative amount to depress the priority over time. Could even help for f-caps. We’ve heard IGs are most effective early. There’s a recency bias. Perhaps that could have a control / modifier as well.

AP: not so concerned because of bayesian approach. When you don’t know much, you want to explore. Try to collect data to make better choices in the future. Hybrid approach suggested by PJ could be interesting. Advertiser should have control of overall priority through “ghost mechanism.” If your recency is going up, you would evaluate in this ghost thing and your priority would go down. If you feel like it’s been a while since you’ve shown an ad, you could up generateBid(). Not really worried about browsers not picking up an ad due to Bandit approach. Tangentially, this is another problem with the priority scores. I don’t know all the IGs a user has been added to. As users cross advertisers IGs how do I weigh priority across advertisers and IGs. It’s a very strange field to be operating in.

Russ Hamilton: as the owner of the interest group, you don’t know which are more valuable?

BM: you do know, but you are competing with yourself blindly. It would be good to get insight so that you don’t step on yourself.

MK: doesn’t add up. Advertiser priority scores should be exactly what you think on average an IG would bid. Whatever the highest bidding IG is on average is going to be the one that wins the auction. You aren’t forced to bid with best guess at the time you added user to IG.

AP: if the optimal strategy is it should be the average bid, the browser should know that.

RH: browser doesn’t know. They know a local estimate, not a global value.

MK: but it might be different for different folks so the browser’s view might be better.

AP: this is actually what the power of programmatic is. It’s user level. Individual data at browser level is most important a lot of time.

BM: it seems this is something advertisers and publishers should work out together. The publisher is determining what types of algos they are going to use. Advertisers can pick publishers with the best strategy.

MK: Not sure I understand how to operationalize what BM just described.

BM: advertiser creating IG. Publisher buying against IG?

MK: The thing that concerns me more is that we’re trying to save computational resources by not even letting the buyer’s IG run code. If we’re talking about something before that I am not sure how that could even be expressed.

Jonasz Pamuła: Plus one to Paul. Who should be in charge of prioritizing IG. Doesn’t have to be just a browser or just the seller. The buyers are in the best position to prioritize across their own interest groups (in a white box fashion). Let's create a system that promotes a competition between the buyers to do the prioritization as well as possible. The seller, in turn, could be in position to dynamically decide which buyer gets a chance to process an interest group next. Really about how high one bids and how quickly it computes. If a bidder bids high and finishes quickly, maybe the seller decides to give the buyer another shot. If a buyer bids low and takes a long time the buyer may not have another chance. Make incentives for buyers to prioritize bidding logic and efficiency.

MK: See you all in two weeks when we return to the normal sync between US and EU clocks
