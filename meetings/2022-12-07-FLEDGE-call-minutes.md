# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 4pm UTC (during winter).

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Dec 7, 2022

(Note we are skipping Nov 23 due to US Thanksgiving)


## Attendees: please sign yourself in!	



1. Matt Menke (Google Chrome)
2. David Dabbs (Epsilon)
3. Brian May (dstillery)
4. Martin Pal (Google Chrome)
5. Paul Jensen (Google Chrome)
6. Russ Hamilton (Google Chrome)
7. Bartosz Marcinkowski (RTB House)
8. Sangharsh K (Walmart)
9. Stan Belov (Google Ads)
10. Sven May (Google)
11. Caleb Raitto (Google Chrome)
12. Joel Meyer (OpenX)
13. Marco Lugo (NextRoll)
14. Orr Bernstein (Google Chrome)
15. Andrew Aikens (TripleLift)
16. Jonasz Pamuła (RTB House)
17. Alex Cone (Coir)
18. Zheng Wei (Google Ads)
19. Andrew Pascoe (NextRoll)
20. Michael Kleber (Google Chrome)
21. Matt Davies (Iponweb / Bidswitch)
22. Przemyslaw Iwanczak (RTB House)
23. Bartosz Łoś (RTB House)


## Note takers: Martin Pal


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here]

Items remaining from previous call:

1. Paul Jensen (Google Chrome): FLEDGE auction performance
2. David Tam (Relay42): Two questions, first regarding permissions policy and second, about removing browsers from interest group via trusted server




## Notes

Paul: Welcome to the FLEDGE bi-weekly, run under auspices of W3c/WICG. If you are participating, please join W3C and WICG. Also, please remember to sign in. Today’s agenda is two carry-over items from last time.

### Paul Jensen (Google Chrome): FLEDGE auction performance

Item 1: Paul to talk about FLEDGE performance. Continuing to look at performance, latency of running fledge auction. Exploring a bunch of optimizations, some of them launched. 

Launched: GroupByOrigin execution mode allows reuse of js execution context. generateBid doesn’t need a fresh context, can reuse context from the same origin.

Launched: Ability to specify priority vector for interest groups. If you don’t want an IG to run, set its priority to be negative. Priorities can be sent from the trusted bidding signals server. If the dot product is negative, the IG will not participate in the auction, saving computation time.

Launched: Parallelize execution of parts of the fledge auction with the contextual request. Saves time on parts of the auction that can run concurrently with the contextual call.

This is a big change, especially for sellers. Appreciate feedback. 

Smaller changes to make auction run better. Added pre-connect for buyer trusted signals and seller trusted signals. We figure out how many worker processes we need. These processes initiate requests to trusted servers. Initiate establishing http connection ahead of time.

Make some arguments passed into worklets lazy, so that they don’t need to be created if not used.

Jonasz: do we have metrics to show these improvements help?

Paul: We do have some metrics. Some improvements are only in canary, waiting for metrics to come in. For many optimizations, local testing gives 

Go to developer tools, look at worker processes. You can see Chrome downloading, compiling and executing scripts.

Locally, we have seen these optimizations help.

Metrics are harder to read. Some optimizations require opt-in from adtechs. For example, GroupByOrigin requires opt-in. Priority vector must be set up by adtech first.

Joel meyer: Just read through the issue. We have buyers passed in the contextual response. Curious what other sellers thought.

Paul: Thanks for the feedback. It would be lovely if we could know the list of buyers before the contextual response.

Jonasz Pamula: For the pre-connect optimization, will it work out of the box?

Paul: should work out of the box. Benefit will vary depending on connection quality. On a bad connection, can save 3-5 roundtrips (DNS lookup, TLS handshake). Downside is low, as we’re pretty sure we’ll need to talk to these servers. Mostly the cost of Chrome starting up a process.

David Dabbs: you mentioned wanting feedback from sellers. Why? DO you expect sellers to do work?

Paul: Parallelization optimization needs seller feedback. Buyers don’t need to change, but sellers need  to make a change. Some inputs to the auction will be provided at a later time via Promises, but some runAuction tasks can start executing early.

Knowing list of buyers would let us know which IGs are eligible to bid, how many worker processes need to be started up.

David Dobbs: The set of buyers should be relatively static and configured in advance? 

Joel Meyer: maybe an exclude list?

What is the cost of spinning all of this up, assuming everybody bids, fetch signals optimistically, then potentially drop bidders early. 

I don’t think we can get the exact list of bidders early – it is coupled with perBidderSignals.

Paul: If you determine some bidder should not be in the auction, can fix things later.

As an example, perBuyerTimeout could be specified as a Promise. If timeout is 0, bidder can be dropped.

Can you predict? How often is it going to happen that you’ll need to remove a bidder from an auction? If late removal is rare, this optimization is still worth it.

Joel: The reason bidder would be excluded if there’s no relationship – can’t bill. Also, if there’s no perBuyerSignals, the buyer has no need to bid.

Paul: Can the list of buyers be provided in the AuctionCOnfig?

Joel: we build the list of buyers in the contextual response.

Paul: Can you give me an overestimate / superset?

For this list of buyers, we’ll need to fire up processes, request trusted bidding signals, prepare to run generateBid. 

Joel: seems you’d like a ‘registered buyers’ list hosted at a well-known location

Michael: It would help us if you could walk us through the sequence of steps when you are buying via pre-bid. We on Chrome don’t know how that sequence looks like. 

Is there a ‘slowly changing’ list of sellers on this page, and can each seller know a slowly changing list of buyers. If such sets were known we cna optimize.

If it turns out such list can’t be had, some optimizations will be unavailable.

Joel: Let me get back on this in 2 weeks.

### Topic 2: David Tam (Relay42): Two questions, first regarding permissions policy and second, about removing browsers from interest group via trusted server

David Tam: had questions on agenda. Unfortunately dropped off the thread.

Questions: one on policy, one about removing user from IG via a trusted server.

David Dabbs: 

Michael: removing IG via trusted server: can use filtering logic via the trusted bidding signals server. Can also use daily update mechanism to remove all renderUrls from the IG – the IG won’t participate in bidding in that case.

David: don’t recall if the spec says whether the browser is going to abide by cache control? Or disallow caching and fetch fresh data 

Cache control http headers

Paul: yes, you can use cache control headers 

Matt Menke: the request contains the site. Hence caching across sites is not useful

Paul: If there are multiple adslots, caching across adslots will work.

Paul: Any other topics?

Bartosz Los: RTBHouse is not using the hostname in a TBS request. To improve caching, we’d prefer to not include hostname in the request. 

Paul: Are you saying that not being able to cache across sites is a problem?

Bartosz: We are not using publisher info. We’d like to reuse cache across pubs. We’d like a control to remove hostname from the request.

Paul: What TTL would you want if caching were allowed across pubs?

Bartosz: Ideally, we’d like to specify TTL per key. Some can persist, some must be real-time. For some keys this could be 1-2 hours.

Brian May: There’s a subset of things where a long TTL makes sense, and some things need real-time fetch, so it isn’t clear that it will help with pre-auction connection optimization.

Bartosz: Another reason why a cache per key would be a bit more effective is the fact that TBS request is not 100% deterministic (batching, adding to new IG etc.) so not always we are able to take advantage of the network cache (Cache-Control in HTTP header)

Paul: The order of keys and IGs should be fixed, as long as the set of IGs is fixed

Martin: Are your keys shared across interest groups, or are keys per-IG?

Bartosz: We have different types of information (some per-campaign and some per-user) and in the future they would be represented as separate keys

Martin: If you have a key with a certain string, can it appear in multiple IGs?

Bartosz: Right now every IG has a different key in our current setup.

Paul: HTTP cache is a complicated part of Chrome. What if we had a separate http request per key

Matt: That would allow combining cross-site data. 

Stan: can you repeat?

Matt: you could include user history as the key. TTL for key 1 is 5 years, value is foo.com. Key 2 is Bar.com ttl is 5 yrs. This could allow building cross- site history..

Paul: is there a current precedent?

Matt: 3p cookies going away

Matt: even with trusted server. You can build a cross-site history.

Can’t allow bidding script to have access to keys from both foo.com and bar.com. A cache could end up holding data from multiple sites.

Paul: If the value is cross-site cacheable, you’d want to declare the key as cross-site

David Dabbs: Cache timeouts should be capped at 30 days. The problem is cross-site

Stan Belov: With a TTL mechanism, the problem is if the key is publisher agnostic

Paul: would it be feasible to mark keys as “site-independent” vs site-specific?

Bartosz: I think this would be beneficial. Haven’t measured it though. 

David: General question: how can we accelerate this. There are 3 groups who are testing this at scale. Can we identify things that will allow more people to test. Perhaps remove friction on publisher side? Also, what friction can be removed on the buy side. 

Joel meyer: Testing at scale requires coordination with a bunch of parties, each of which has their own testing roadmap. What version of Chrome? Shadow vs real serving, prebid vs rtb. Perhaps if we could clearly mark requests as test request. 

There are ~5 parties involved in every impression: publisher, SSP, DSP, advertiser, Chrome and all need to coordinate.

David: adtech is complicated, coordination is hard. It feels there’s a mountain of coordination friction. It feels insurmountable to run it (even if we assume the utility is there)

Stan Belov: Can we start a doc that lists friction and pain points that should be addressed to help adtechs at scale

David; Issue 57. How is google ads going to play. Relationship between Chrome and Google ads. How widely is this going to be. Ideal relationship: one uber-auction, fledge participates in that. 

Matt Davies: Product team. We can help with nor

Get everything up to speed. Maybe we can be the solution. Connected to almost everybody. If OpenRTB

Help coordinating DSPs and SSPs

Matt is from BidSwitch. App and Web. 

Standing in for Isa Schappman(?)

Brian: make a list of players, who needs to interact with whom.

David Dabbs; How much work will fall on ad ops teams. Can we reduce toil for ad ops.

Joel Meyer: GAM is doing testing behind the scenes. Looks like one large pub is making money already. [Can share a doc](https://support.google.com/admanager/answer/12052605). The problem is bidding in chains. Testing is happening, but hasn’t permeated the ecosystem.

Stan: having a list of pain points would be helpful for adtechs.

Brian: such a list could be useful for Chrome so they know what can be optimized.

Paul: We have put out a number of devrel docs to help developers. We’d like to hear about needs and gaps.
