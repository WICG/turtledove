# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 4pm UTC (during winter).

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Jan 4, 2023


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Eyal Segal (Taboola)
3. Brian May (dstillery)
4. Michael Dragatski (Taboola)
5. Matt Menke (Google Chrome)
6. Paul Jensen (Google Chrome)
7. Jonasz Pamuła (RTB House)
8. Fabian Höring (Criteo)
9. Julien Aimonino (Criteo)
10. Lionel Basdevant (Criteo)
11. Tamara Yaeger (Criteo)
12. Sven May (Google)
13. Kevin Lee (Google Chrome)
14. Marco Lugo (NextRoll)
15. Orr Bernstein (Google Chrome)
16. Bartosz Marcinkowski (RTB House) 
17. Stan Belov (Google Ads)
18. Peiwen Hu (Google Chrome)
19. Don Marti (CafeMedia)
20. Wendell Baker (Yahoo)
21. Matt Davies (Criteo) 	


## Note taker: Tamara Yaeger


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here]



1. Duplication - Following https://github.com/WICG/turtledove/issues/411
2. 


## Notes


### 1.Duplication - Following https://github.com/WICG/turtledove/issues/411

Paul (Chrome): I responded to this issue, but Michael D from Taboola not on the call.

Michael (Chrome): No one on call from Taboola? We can have a discussion about #411 but maybe not if ppl raising issue not here to participate.

Brian (Distillery): Worthwhile to frame the discussion for those who have not read the issue, good to discuss and come up w ideas around.

Paul: The issue talks about if you have webpage w multi ad slots what to do about duplicate ads. Trying to figure out how does buyer prevent showing ad multi times in multi times on 1 web page. Couple comments on how this is done today, looks like there are 2 different ways  - (1) combined ad requests where 1 request goes out for multi slots (2) if same ad bids for multi slots, they are de-duped and one is chosen but is not displayed in multi slots on page. Issue is getting to how to do this in FLEDGE how to isolate ad slots. Several FLEDGE options for each ad slot, not immediately obvious how they communicate. One solution is if each auction is run sequentially then an interest group can use previous wins info gets passed to generate bid. If they won previous they can decide not to bid for that ad. That’s the sequential option so slower, could prioritized by preference (most to least visible). Other option is sellers combine info on ad slots on page, when someone bids they can decide what is most valuable and only bid on that. More “concurrent” option. Auction can make holistic decision on all ad slots on page. Consecutive vs concurrent decision making.

Michael: Previous wins mech is the way we intended to support de-duping when originally wrote FLEDGE proposal. If there are ways it is not meeting needs, would love to hear what features are lacking and what needs supporting.

Brian: It’s not just about de-duping, also not bidding against yourself or interfering w dynamics of auction. In isolation you can have effects in local view that impact global interaction and intended effects. Want to know when we are competing with ourselves.

Michael: How competing w yourself interacts w multi slots? What’s the dynamic?

Brian: If you can’t see what’s going on on the page and bidding on your local view, you may bid higher than if you knew you had other option. If I have a banner ad and another ad on a page, I may prefer banner ad and do things differently.

Michael: To understand, if you think about set of ad slots on page going through sequential decision making process that previous wins would support, there is question of while biding is happening for ad slot #1, can you be aware of upcoming auction for ad #2 and choose not to bid on slot #1 because anticipating ot bid on slot #2 for same page? Is that what you’re describing? That use case can be addressed by the auction info that is passed by the seller including list of all ad slots on the page w expected auctions in the future. Seller passes arbitrary contextual info, if they want ot make things clearer by passing # of ad slots, there is certainly channel for seller passing that info in. Dont know the best way to use that info but it would be possible to do if seller chose to do so. If you’re talking about reacting to outcome, then sequential is problem because you cannot know outcome of future outcome. 

Brian: Referring more to the first case, I think that if the seller provides info it’s helpful, but we probably want to have standards around what the seller provides so it’s known what to expect on a given website for FLEDGE auctions. 

Michael: What info seller provides standard sounds like good idea but in realm of standards established by Ad tech industry, sellers and buyers should agree what info is passed, which can change any time if they agree. Browser’s job should be to provide channel, but choice of info up to auction participants. 

Brian: That’s reasonable, we should note it somewhere. From the perspective of current ecosystem dynamics, it might be unclear how that works – what the FLEDGE analog for mapping available slots on a page are. Also, do we have interest in exploring how someone would buy all ads on a page?

Michael: Indeed, great question. Gets into multi simultaneous auctions and how auctions affect each other. Not designed into FLEDGE or putting resources into figuring out accommodating. FLEDGE is one auction as a time based on info on previous wins. If roadblock situation is important part of how things ought to transact, could be a new feature request. Not in first version of FLEDGE. 

Brian: Perhaps seller channel could do something along those lines like communicating # of ads on page. 

Michael: Problem of doing that in existing info channels is there is no way now for seller when running auction #2 to get info on what they chose as decision for auction #1 in same page. 

Brian: I was thinking the seller configures the page to indicate everything is available to a single buyer so that buyer can say they want to buy everything.

Michael: Is roadblock the right term?

Brian: It’s the term I’m familiar with.

Michael: Do those happen on RTB or direct pub / advertiser deal that happens outside the oRTB channel?

Brian: I’m not currently involved with the world of bidding, in my experience mostly roadblocks are direct deals, sometimes programmatic companion ads. Similar thing on a smaller scale.

Michael: So roadblocks would entirely be supportable by contextual info? But really if there is a roadblock then there wouldn’t be a FLEDGE auction at all. Of you can say you have roadblock deal if first slot on page is willing to pay more than entire roadblock, then happy to let FLEDGE ad win first slot. If no FLEDGE ad wins, then contextual winner will be rb and won’t run FLEDGE after. If there is demand for interest group, then we would need new sort of logic. My inclination is not to tackle unless someone posts a feature request with revenue goals, then can try to figure out API extension to accommodate use case. Would prefer not to attack if not highest value thing to add to FLEDGE.

Brian: As long as we note we discussed it for others to review, that's what matters.

Michael: Any more discussion about ad slots on page and ad buying decisions? Is here a race condition problem w using previous wins to get info abt ad slots. Are we appropriately guaranteed in browser when 2 auctions run consecutively that 2nd ad slot will get info about 1st?

Matt (Chrome): As long as first one resolves that should be fine. 

Michael: As we move to ability to handle auctions in parallel, what happens if two kicked off at same time but only resolve promise of one auction, is prewins computed early in setup stage, or will it wait for promised contextual information?

Matt: Populated at start of auction. If parallel, each owner will have contextual view of world, even if both concurrent auctions complete, bids may not be consistent between different owners. If you don’t strictly wait til the end. 

Michael: We might need to think about semantics of previous wins w multi auctions and see if there is spec guarantee we want to make on visibility for this use case, or if any changes would be beneficial for consistent view. 

Michael: Wish you all excellent 2023, see you in 2 weeks :)  [[Update: actually no meeting Jan 18; see you Feb 1 instead]]
