# Protected Audience (formerly FLEDGE) WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 4pm Paris time = 3pm UTC

**NOTE THAT Europe clocks have changed but US clocks have not!  So this week's meeting is at an usual hour for Europe folks!**

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Nov 1, 2023


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Brian May (dstillery)
3. Sven May (Google Privacy Sandbox)
4. Paul Jensen (Google Privacy Sandbox)
5. Roni Gordon (Index Exchange)
6. Youssef Bourouphael (Google Privacy Sandbox)
7. Orr Bernstein (Google Privacy Sandbox)
8. Matt Menke (Google Chrome)
9. Laurentiu Badea (OpenX)
10. Harshad Mane ( PubMatic ) 
11. David Dabbs (Epsilon)
12. Andrew Kwok (Criteo)
13. Antoine Niek (Optable)
14. Xavier Capaldi (Optable)
15. Alex Cone (Google Privacy Sandbox)
16. Caleb Raitto (Google Chrome)
17. Mahdi Sadjadi (Yieldmo)
18. Leeron Israel (Google Privacy Sandbox)
19. Alex Johnston (Yieldmo)
20. Andrew Pascoe (NextRoll)
21. Marco Lugo (NextRoll)
22. Sid Sahoo (Google Chrome) 


## Note taker: Orr Bernstein


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


## Suggest agenda items here:



*   Roni Gordon
    *   K/V lookup timeouts - https://github.com/WICG/turtledove/issues/814
    *   Macro substitutions - https://github.com/WICG/turtledove/issues/817
    *   API versioning - https://github.com/WICG/turtledove/issues/823
    *   Sensitive signals - https://github.com/WICG/turtledove/issues/824
*   Isaac:
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Buyer/Seller Reporting Questions: https://github.com/WICG/turtledove/issues/682#issuecomment-1710965068
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736


# Notes



### K/V lookup timeouts - https://github.com/WICG/turtledove/issues/814

*   Roni Gordon
    *   Want to understand timeouts
        *   About how one defines timeouts in an auction config and what they’re governing.
        *   Where is the KV time considered? It happens in between the function calls but in the same worklet. Paul provided a clarification on the bug.
        *   Roni is trying to better understand how to configure it. Similar question for the seller KV time. No notion of “cumulative” because there aren’t multiple sellers in a component auction.
    *   Paul Jensen
        *   When we started thinking about different ways to do timeouts, we had limits on the number of interest groups and the amount of time each can take in generateBid and scoreAd. Also protects against a potential privacy leak.
        *   For performance, the more important thing is wall clock time - overall time, not just time for each one. We want to be less opinionated about how long each one takes. So that’s when we moved to the cumulative timeout. Also, the trusted signal call can take quite a while. So, that’s why, as we moved to cumulative, we included the total wall time an interest group takes.
        *   As for why there isn’t a trusted seller signals timeout, nobody has asked for it. There is a 30 second timeout, but hopefully nobody reaches that. How many times it gets invoked is sort of under the control of the buyers. If we get 10 bids, we could make one trusted seller signals call for all ten bids, or we could make ten requests, depending on how those requests come in. Also, the seller has an overall control for timeout - the abort signal they can pass in.
        *   Roni, what kind of timeout are you looking for?
    *   Roni
        *   KV needs to respond in a certain amount of time, and needs to reflect the potential timeouts. Nothing prevents a slow seller KV call from taking over the auction.
    *   Paul
        *   You can use the abort signal to control total time of the auction. The real question is, if we added a seller trusted signal timeout, would you use it?
    *   Michael Kleber
        *   Usual process is that the timeout is applied to other parties. Could be that the top-level seller could apply a timeout on the component sellers.
    *   Roni
    *   Michael
        *   There is not a timeout right now that covers the seller KV lookup. If all of these are maximally slow, the whole auction might take 30 seconds plus a little bit. [From chat: actually, could be a minute.] The top level seller would be highly incentivized to not work with component sellers that take so long.
    *   Roni
        *   So, incentivized to return the KV as quickly as possible to reduce the likelihood of timing out the auction. If the timeout is for the KV call, then the KV call times out and it’s like the bid isn’t there anymore. If the signal times out, the generateBid call could decide what to do in the absence of that signal.
        *   If I set just the cumulative timeout, do I still set the interest group timeout?
    *   Paul
        *   You probably do want to set both timeouts, but set them to the same value. Per-buyer timeout to the same as the cumulative one.
    *   Roni
        *   The spec sets a default value of per-buyer timeout, so not specifying it means you get that default.
    *   Michael
        *   Default value of 50 ms, maximum allowable value of 500 ms.
    *   Roni
        *   So, I have to specify both and hope their generateBid is fast.
        *   Is there any indication that this timeout has happened? That I’ve run out of time because I’m too slow?
    *   Paul
        *   Per-function timeout, there might be a way. But the cumulative one is trickier, because the bids before the timeout you get, but the bids after the timeout you don’t.
        *   We do have private aggregation for times - your trusted bidding signals and your generateBid times. Don’t know if we ever added a value to the enum to indicate that generateBid hit its timeout.
        *   At the beginning of generateBid - while you have forDebugOnly reporting - you can specify at the start of generateBid, you could specify that it reached the end of generateBid, and then to see if these numbers diverged.
    *   Matt Menke
        *   With cumulative timeouts, we kill the worklets, so it doesn’t even start generateBid for those.
    *   Brian May
        *   These things are implying that there are resource constraints on the browser, and that the browser is taking preemptive action if it hasn’t reached a point where it needs to do something to show an ad. Have to figure out if we’re in a situation where the same couple of interest groups are getting evaluated, and there’s starvation of interest groups lower down. Unlike in a server, where we have pretty tight control over what happens between the time we get a request and the time we show an ad.
    *   Paul
        *   Happy medium is that seller specifies a cumulative timeout. Buyer optimizes how they want to do trusted bidding signals, could spend more time there and less in on-device generateBid script or vice versa.
    *   Brian
        *   Context that a buyer has control over. No way for a buyer to get a sense of what the situation on a given browser is going to be, how to handle to the competition for resources across buyers.
    *   Michael
        *   The seller is the one who controls how these resources are shared using the cumulative timeouts.
    *   Brian
        *   But the buyer can’t optimize because they don’t have enough information to see, for this auction, what the situation is.
    *   Michael
        *   Buyers can prioritize interest groups; higher priority interest groups go first. If the seller can tell that the device is a slow device, and they want to allocate more time for the auction.
    *   Brian
        *   As a seller, I don’t have a good way to optimize calls, and if there are too many interest groups, how to prioritize them.
    *   Paul
        *   Buyer should prioritize the interest groups, how to prioritize. The seller should be worrying about who the buyers are, what the resources are, and what the latency acceptable to the publisher is.
    *   Michael
        *   Seller won’t know which buyers have an interest group on the device, so they can’t decide how to allocate resources among those buyers that will participate in the auction. Don’t want this to be in JavaScript because that would delay the start of the auction until after we’ve started a JavaScript environment, but can do this in a declarative way.
    *   Brian
        *   If there are too few resources on the browser, then if I have more things than I can do in the amount of time, then I might visit the same first 10 interest groups every time, and never see the following 30 interest groups.
    *   Michael
        *   Yes, buyers get to prioritize their interest groups.
    *   Brian
        *   Will run into a situation soon where interest groups will be starved as a result of this.
    *   Michael
        *   Yes, if some buyers are not going to be able to run at all because of cumulative timeouts, do sellers get to prioritize which buyers get to run or is there some randomness there?
    *   Brian
        *   On the server side, we give campaigns that tend to be paid attention to less an opportunity to squeeze themselves in by making space.
    *   Matt (via chat)
        *   Buyers are run in the order they’re specified in the auction config. We can't create an infinite number of isolated processes, each with its own JS thread. So there's a limit of buyers that we process at once.
    *   Michael
        *   If there’s insufficient resources on the browser, 
    *   Roni
        *   Somewhat of an awkward position. We don’t play favorites with buyers. If I put somebody first, it’s not just that they’re first because I chose to alphabetize my list. I’ll need to randomize the order of buyers, which I didn’t know I need to do.
    *   David
        *   Request from the chat - Matt put some interesting comments - could he pull that together in a concise way, in the spec or howto doc. Reading the chat, it’s not too clear.
    *   Sid Sahoo
        *   Could Paul explain how the top level seller timeout works with component sellers today?
    *   David
        *   One seller timeout, which governs the scoreAd of component sellers.
    *   Paul
        *   Matt - does per-buyer cumulative timeout apply to component sellers?
    *   Matt
        *   Each auction is mostly run independently so that their fields don’t affect the other auctions.
    *   Sid
        *   How does the top-level seller timeout play with the component auctions? Can the top-level seller influence the component auctions?
    *   Paul
        *   The auction configs may be passing through the top level seller. So they could add an abort seller in each component seller config if they wanted.
        *   Roni - is there a feature request here besides improving our documentation?
    *   Roni
        *   Curious about how timeouts for component and top-level sellers. There’s a limit for the number of IGs; if a buyer hits that limit, how do they know? Do we know how these different clocks fight with each other? Right now, it seems fine but a bit vague. Haven’t seen any examples of this abort signal. I don’t know if the intent of top level sellers is to inject something that I can’t see.
    *   Brian
        *   What can a buyer understand about the environment in which their bid is happening? If we have many different sellers and different sellers are applying different constraints, hard to have any idea of how the different seller constraints are going to impact the buyers. In today’s world, a bunch of publishers go through an SSP, the SSP delivers bid requests to a bunch of DSPs. They have a relationship, know what to expect. In the Protected Audience world, a bunch of stuff happening on the device on the browser locally, much smaller context to figure out the rule. With timeouts as an example, how do I set my timeouts? With some sellers, the timeout might be long, with others the timeout might be short.
    *   Michael
        *   Are you saying that as a particular interest group, you want some more global sense about what opportunity the interest group has to bid with multiple different sellers?
    *   Brian
        *   Different from what I said. When I’m putting things together as a buyer, trying to anticipate the environment in which this thing is going to be sent. How as a buyer can I tell how my campaign is going to perform across 50 different sellers? If I’m the first one in a seller’s list, I’m going to have lots of resources; if I’m #50, I’ll have less.
    *   Michael
        *   That is a control that doesn’t exist right now; no timeout that applies to all buyers collectively.
    *   Paul
        *   The only people bidding are those that have interest groups, and that might not be everyone. With today’s world, there’s a lot of play between buyers because they all go every time. But in PA, there may be less resource competition because only some buyers will have IGs on that device.
    *   Brian
        *   Different devices may have more or fewer buyers with IGs; how do they optimize with the uncertainty of this?
    *   Michael
        *   Browser signals is an opportunity to tell buyers about these kinds of things.
    *   David
        *   In the chat, Matt said that buyers are serviced in the order that they’re specified in the component seller’s auction config. Is this true for the order of component sellers in the top-level auction?
    *   Matt
        *   There is an ordering, but today there’s no global timeout.
    *   Michael
        *   If the limit is reached, it aborts the entire auction. Taking too long does not mean that some people get to go and others do not get to go, which is how it works today.
        *   The only constraint is that we won’t wait for a network request more than 30 seconds. If your KV server request takes longer than that, we’ll drop it on the floor and the call to generateBid will get no information back.
    *   Laurentiu Badea
        *   Time elapsed means if the page runs multiple auctions (many ad slots) the chance to complete drops proportionally.
    *   Matt
        *   We use processes for bidders and sellers, shared by all auctions running simultaneously. So, auctions can have an effect on each other. Cumulative timeout is shared. Running scripts at once in the same auction or different auctions will share the same process.
    *   Paul
        *   A lot of this is limited by how many processes we have and how many processes we share. In the last 15 years, a rise in the number of cores. In many ways, consumer devices have been limited by the single threaded nature of programs. We’re doing a better job of using all the cores in Protected Audiences. Doing more work might actually not mean more wall time.

### Notes from chat (some captured above):

David Dabbs: Matt is that documented somewhere in the explainer, or does one have to inspect the chromium source? Or spec?

Matt Menke: With cumulative timeout, we don't even run generateBid, (And we don't cancel any currently running call).  With cumulative timeouts, we don't hit the beginning of generate bid.

Laurentiu Badea: https://github.com/WICG/turtledove/blob/main/FLEDGE.md#35-filtering-and-prioritizing-interest-groups

Matt Menke: Buyers are run in the order they're specified.  In the auctionConfig interestGroupBuyers array

Roni Gordon: isn't that done in parallel?

Matt Menke: We can't create an infinite number of isolated processes, each with its own JS thread. So there's a limit of buyers that we process at once. (On desktop - on Android, they all run in the same process, sharing a single JS thread)

Matt Menke: My suggestion (as someone who admittedly has no clue) is just to randomize buyer order. Note that there's no inter-buyer timeout currently.  So I'm not sure how important order actually is between buyers

Harshad Mane: But Seller does not know how many IGs each buyer has so prioritizing buyers in random order may not be optimal

Matt Menke: I'm not sure order actually matters - we do run them in the order provided, but there's no global timeout, only per-buyer timeouts. So unclear if running first is actually an advantage (or disadvantage)

Laurentiu Badea: time elapsed means if the page runs multiple auctions (many ad slots) the chance to complete drops proportinally

Roni Gordon: why would the time against those configured timeouts be impacted by other any runAdAuction calls?

Marco Lugo: I did observe that with 1k IGs doing heavy computation everything broke down but that was last year before several improvements come in. Maybe an edge case but it relates to what was said.
