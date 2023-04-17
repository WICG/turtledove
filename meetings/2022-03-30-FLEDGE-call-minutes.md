# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Mar 30, 2022


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Elad Tessler (Taboola)
3. Russ Hamilton (Google Chrome)
4. Brain May (dstillery)
5. Caleb Raitto (Google Chrome)
6. Chris Evans (NextRoll)
7. Paul Jensen (Google Chrome)
8. Andrew Pascoe (NextRoll)
9. Grant Nelson (TripleLift)
10. Michael Coward (Arcspire)
11. Alexandru Daicu (eyeo)
12. Matt Wilson (NextRoll)
13. Jeff Kaufman (Google Ads)
14. Nick Doty (Center for Democracy & Technology)
15. Tamara Yaeger (IPONWEB)
16. Christina Brauer (Capital C)
17. Boaz Yaniv (Taboola)
18. Leif Högberg (eyeo)
19. Matt Menke (Google Chrome)
20. Joel Meyer (OpenX)
21. Kevin Graney (Google Chrome)
22. Chris Starr (Hearst Autos)
23. Stan Belov (Google Ads)
24. Alex Cone (IAB Tech Lab)
25. Sangram Chavan (Neustar)
26. Jonasz Pamuła (RTB House)
27. Bartosz Marcinkowski (RTB House)
28. Benjamin Dick (IAB Tech Lab)
29. Peiwen Hu (Google Chrome)
30. Tim Hsieh (Google Ads)	
31. Supraja Sekhar (Google Ads)
32. Tao Liao (Google Chrome)
33. Don Marti (CafeMedia)
34. Michael Gorman (ShareThis)
35. Wendell Baker (Yahoo)
36. John Mooring (Microsoft)
37. Aditya Desai (Amazon)
38. Sergey Tumakha (Microsoft Ads)


## Note taker: Tamara Yaeger


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


## [Suggest agenda items here]



*   Experiment group ID
    *   https://github.com/WICG/turtledove/issues/191 https://github.com/WICG/turtledove/pull/266 
*   Component auctions and 2nd price auctions. [Paul Jensen]
*   Priority and perBuyerGroupLimits [jefftk]
    *   https://github.com/WICG/turtledove/pull/276  
    *   Interaction with multi-seller
        *   IG-level limits?  Timeout?
*   Provide Bid Filtering Reason to Buyers
    *   https://github.com/WICG/turtledove/issues/274
*   Access to data from FLEDGE OT for folks who are step removed. (bmay)


## Notes

Michael (Chrome): We have 3 agenda items, I believe proposed by John Mooring about experiment group IDs, how second price auctions should work proposed by Paul Jensen. PerBuyer Groups proposed by Jeff, more items being listed.



*   Experiment group ID
    *   https://github.com/WICG/turtledove/issues/191 https://github.com/WICG/turtledove/pull/266 

John: GroupID already been addressed. Split into per buyer signal.

Michael: Great, resolved.  Discussion on right syntax passing in something on per buyer level, 2 ways to pass, back and forth discussion on relevant API but not a concern here. Possible to do what you want either way.



*   Component auctions and 2nd price auctions. [Paul Jensen]

Component auctions and 2nd price auctions as proposed by Paul.

Paul (Chrome): As we talked a few calls ago, going to be passing auction signals along to reporting URLs and mechanisms. So we had talked back then about passing highest scoring price and second highest scoring bid price. Maybe weren’t thinking about component auctions, but how it interacts is interested. Bunch of different ppl may want to know different things. Looking for feedback on how top level component auction. There is the bidder in component auction, then intermediate sellers, then there are bidders in top level auctions.

Questions: for bidders in comp auction, when they find out highest and 2nd highest bid price, do they want to know the bids in the top or component auction? Probably about the ones in component auction? Then in the case of component (intermediate) sellers, do they want to find out about top level winning bid price? Assumption is yes. 2nd price in top level auction is this weird think where it could be coming from another auction. 2nd price is useful for 2nd price auction. Will this be a 1st or 2nd price auction and where should 2nd price come from? Do we want to do anything different for winner vs loser? Reporting on 2nd price bid? 

Joel (OpenX): I would love to see this start w 1st price auction, lots of complexities otherwise. Well defined surface that is smaller then expand. If you compete and lose, you should know, but not advocate for visibility in higher up.

Paul: Are you saying that component should be assumed to be 1st price as well?

Joel: It could be 2nd there but up to logic of component auction runner. 

(unknown speaker): The 2nd price important for participants. I feel there are 2 questions. Auction dynamics is 1st or 2nd? How to make available 2nd price to participants? Let’s distinguish.

Michael: One qs is how things are actually charged, 1st vs 2nd price? That’s a question of who should pay how much to whom? That is entirely in FLEDGE design. Seller gets to have whatever auction they want. They should be clear when buyers are participating. The qs of whether it’s 1st or 2nd, it’s not one Chrome wants to have an opinion on. Chrome is platform w goal to provide enough info and visibility so that auction runner has capability to run the auction they want. Q is what sort of reporting out of 1st and 2nd price does Chrome need to make available? So that sellers can run the auction they want and buyers can understand value. 

The next hand up is Jonasz.

Jonasz: I want to understand what the goal is, perhaps discussed in issue already. When we speak of component 2nd price auction, does it mean that each component auction is an isolated and passes only 1 bid which could be 1st or 2nd? Or aiming at a setup where all component auction participate in global 2nd price auction. Distinction is important. First scenario has undesired side effects.

Michael: When we first started talking about component auctions, each component auction runs entirely independently of other component auction. Each seller makes decision about participating buyers are in that component and has own decision logic on how relative bids are evaluated compared to one another and the winning bid. Happens entirely independently. Then component auction seller passes some info to logic that is controlled by the pub or whoever is resolving the various component auction winners are going to be the final winner or auction, or, passed out of device auction API. The discussion we had indicated that the seller of one component auction was very unlikely its price to be influenced by anything from other component auction. Skepticism around one auction seller trusting info from another seller to rep anything honestly as opposed to maliciously. Every component auction operates independently. Q is what info do they come away with?

Jonasz: Thanks for clarifying. One implication is that the highest bidder doesn’t always win. If we have component 2nd price auction, what is going to be passed to top level auction is info about the winner. LEt’s say winner submitted bid of $10 and 2nd price was $5, then overall winner and bid price is the winner and $5 because this is 2nd price. If some other component auction that has a winner that submitted $7 and was down to $6 they are going to win the top level auction,.

Michael: That’s exactly right. Component auction w higher top bid but less bid density could lose out to another 2nd price auction w higher bid density. 

Joel: I think this was alluded to but not many SSPs are fans of 2nd price auctions. 

Stan (): It’s a challenge of 2nd price auction competing in header bidding setup. Here API models facilitates biz relationships that exist today that exist w multiple exchanges representing multiple DSPs each. Exchanges compete w each other for pubs. Seems this is up to DSPs and exchanges to decide how to use these APIs. One small caveat here is how the currently proposed API for component auctions would even allow to pass 2nd price to top level auction to begin with. Mechanism does not exist.

Michael: Both buyers and sellers in component auction should be able to leard winning and 2nd price isolated from other component auctions. There is an open question of whether there should be a way for bid passed from component auction to top level auction to represent the price in the full bid landscape rather than passing along each individual bid, which prefers the 1st price way. We don’t ahve a way for each component auction to . Overall answer to outcome of top level auction should be the same as overall answer that is for whatever auction was the source of the chosen winner. If the top level auction is going to report the top and 2nd price, it should be the same as top price of auction that won, not the top price from one of them and the top price from another. 

Wendall: The notion of how auctions are designed has a lot of academic work in it, much funded by Google from early days of search marketing. Idea of who pays, incentives and setups, so participants will give true value and be penalized for now. Whether auction is sensitive or not to collusion, lots of work in that, industry has gone back and forth. Search industry is stuck with small changes around auctions scheme. Display gone back and forth on 1st and 2nd and other schemes based on reasons and dealings. I would guide Chrome team to come out w POV understanding that it will be controversial in the industry but that is right and operable. Then we can migrate because there will be changes in auctions scheme as time goes on. Hist is not done, things change, people like to fiddle w market maker. Chrome is in position of making that market.

Michael: I know this is where have disagreed, Chrome’s goal is to let buyers and sellers to do the market making and come up with logic as they see fit. Chrome’s goal is not to take a stance but to shift the tech to let the ppl to make set of decisions themselves. We’ll start by shipping something that may not enable full range, then through origin trials and later it’s possible for us to make more info available to buyers and sellers to further let them refine their strategies or whatever auction dynamics they want. Our goal is not to pick one, it’s to let you pick.

Jonasz: Just to summarize, current status is that we want to recreate possibilities that current ecosystem gives us, which gies ability to shoot ourselves in the foot. What if we don’t want to, can we create single global auction that behaves like a single global 2nd price auction? IF that’s possible, that would be desired for RTB House because historically we had issues w isolated 2nd price component auctions. It is not just minor nuisance that is edge case, but intros wrong incentives and dynamics for DSPs. Makes questions of what SSPs to bid with to win and win entire auction even though I’m not the highest bidder.

Michael: Nothing would make me happier than to have 1 auction. That is how we designed FLEDGE originally. Addition of component auctions was late addition that come from discussion in this group with necessities. Unsolved problem that prevented global auction is, who makes decision on who winner is. Aside from parallel component auctions we never had reasonable proposal, whichever one is the most money is not necessarily the right answer. Lots of secret sauce in biz deals that make it hard to pick the right answer in a global sense.

Jonasz: To add one more thing, not opposed to multi component auction. It is possible to create good dynamics but it would require passing more info to top level auction then back to component auctions I think. Multi component and 2nd price are compatible. But then 2 highest bids need to be passed to top level auctions. Top level then duplicates buyers then picks two highest bids confirming winner and price. 

Michael: I understand hierarchies of multiple 2nd price auction, but what you’re proposing require a lot of trust, seller of component auction trust that the seller in top level auction, the pub is acting in the interest of relaying accurate info between independent auctions rather than interest in the publisher which is to make the most money they have. A 2nd price auction is a great thing as long as the auctioneer is trustworthy.

Brian (Distillery): Seems intent is to give primary control of auction to pub. So equip pubs with info. Drifting over to taking control of things that should be pub’s domain or whoever is selling inventory domain. 

Grant (TripleLift): Is goal to not permit pub to get highest bid that they want?

Michael: I don’t think I understand

Grant: So, you made a comment on auction dynamics and encouraging high bids, but then comment about how a pub would act unscrupulously somehow to drive up price of inventory. Are we operating under assumption that pubs should not be able to get best price for their inventory?

Michael: Let me say, in the case of component auctions, just to describe the simplest, there are let’s say 3 diff parties involved acting as sellers. There is SSP 1 and SSP 2 that operate independent and each run an auction , every SSP picking buyers and setting biz terms and auction dynamics. Then there is the top level seller, which we think of as representing the interest of the publisher. SSP1, SSP2, and publisher. It is possible that the owner of the website publisher will hand off responsibility to some ad tech platform that might be the same as one of SSP1 or SSP2, seems a very likely outcome, but not guaranteed. The top level could also be an open source thing but some extremely transparent thing that is independent, not SSP1 or SSP2. The point was while SSP1 and SSP2 each have some sort of guarantee of relationship w buyer in own individual auction, where they can establish trust, we have heard that trust relationship between three diff sellers is not something we can count on. Cannot create auction that requires sellers logics to morally play nicely w each other. Need to build system that is robust to those parties not actually trusting each other.

Grant: I misunderstood that top level auction could be in pub interest but delegated to e.g. ssp or other party to manage on pub’s behalf. Makes sense, thank you.

Michael: When we started FLEDGE we made that mistake, made mistake that SSP is representing interest of publisher. Component auction is all about pulling that apart and have code acting in pub’s interest be separate.

Andrew: Seems that component auctions designed to replicate how the market functions today. If goal from Chrome is to let SSPs and DSPs and pubs figure out how auctions are supposed to run, then it would seem like the easy thing to do would each component auction return arbitrary JSON blob, could include anything, and that relationship between pubs and SSPs, their code would have to parse particularly and potentially different JSON blobs from SSPs. Is the concern privacy or reporting mechanism that comes out of it?

Michael: Paul’s earlier question was only about reporting and what info should be made available to make biz relationships. However FLEDGE is not designed to solution proposed, in FLEDGE every individual bid is evaluated separately. Each bid comes from a single interest group. Contextual info from user site and interest group info. If we shift that seller evaluating actually gets to see all bids in the world, that would be different from privacy POV. Here seller effectively gets to know global cross site info, from which they might develop some info to use that info to pick winning bid. That is different model from what we have in FLEDGE and different from privacy POV. FLEDGE model is somewhat conservative / safe in that is not allowing global profile step in the process.

Andrew: Understand about global profile but we can ensure that that global profile never leaves browser.

Michael: Agreed but there is diff between ad choices made based on 1 site vs all sites visited put together. If there is widespread agreement on purely on device profile maybe happen later but not right now.

Andrew: I understand having some kind of global profile directly in the browser is different than not, I guess this comes down to what we mean by privacy. I’ve been interpreting it as another human cannot identity and determine where I’ve been. Been operating that whatever is allowable by the API we are allowed to do. API’s job is to make sure info cannot be pieced together to identify. 

Generally APIs are created so that creativity on top of them by implementing in unique ways. 

Michael: Model that we have in FLEDGE right now in which the winner of a component auction is based only on info known about the user’s behavior on 1 site (where they were added to the interest group), that is the current FLEDGE model and the current FLEDGE position on what on device privacy and bidding looks like. If we were going to change things os that FLEDGE bidding could be based on many sites, not a lite decision, would require discussion about what does privacy mean question. For this discussion, I don’t plan to venture into changing to different model of privacy. Stan’s point earlier that current model does not do a good job supporting 2nd price auction feeding into top level decision based on pub payout is an interesting point and we could do a better job by passing data or giving component auction seller some opp to run an additional round of code to calc a pub payout before auction bids are handed of to top level auction. If there is something we need to do there to better enable component auction to have the right dynamics, we need to spec out work to support it. 

Paul: Since right now there may not be a way to do the 2nd price auction in the component auction that we maybe limit reporting to just the first price for now. At least that’s a reasonable starting point. 

Michael: The model we expected is to reporting 1st and 2nd price from component auction. Each buyer and seller’s reporting would be unaffected except for who winner and loser is. We should indeed spend more time figuring out whether we need to add something new in the API. 

Paul: We could report 2nd price in the component auctions

Brian: Regarding privacy, whether we should pursue model where browser guarantees or enables other to guarantee privacy is a long discussion. 

Andrew: With the topics API, the browser uses cross site info to provide info for bidding signals. Here we’re saying FLEDGE is not allowed. Hard to reconcile and navigate what to expect form Chrome and how to use these APIs for privacy guarantees. What is the policy? 

Michael: TOPICS API is an attempt by Chrome to figure out a way to make it possible to have some sort of cross site information / global interest info available for ads targeting purposes w sufficient guardrails around info that we aws the browser feel that we can make some sort of believe that the resulting info is not a privacy risk. Figuring out how to let enough info about interests appear that is useful while not letting so much info that it is privacy harmful is a fine balancing act. TOPICS API discussions were long story on long attempt how to balance those against each other. We are confident that allowing some sort of global interest like TOPICS w/out all the protections that we have built around it, that include not only anonymity goals but also a lot of work around curate taxonomy around topics that appear, and a lot of work on how much fingerprinting info is available, we couldn’t let TOPICS exist w/out that set of guardrails. 

Brian: Suggest that there is material diff between data available in FLEDGE and TOPICS API. 
