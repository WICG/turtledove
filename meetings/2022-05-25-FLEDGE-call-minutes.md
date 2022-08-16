# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday May 25, 2022


## Attendees: please sign yourself in!	

Regrets from Michael Kleber, who is on an airplane right now.  Thanks Paul for taking over running the call this week.



1. Paul Jensen (Google Chrome)
2. Peiwen Hu (Google Chrome)
3. Don Marti (CafeMedia)
4. Brian May (dstillery)
5. Sergey Tumakha (Microsoft Ads)
6. Russ Hamilton(Google Chrome)
7. Alexandru Daicu (eyeo)
8. Stan Belov (Google Ads)
9. Sven May (Google Chrome)
10. Katherine Wei (Zeta Global)
11. Matt Menke (Google Chrome)
12. Wendell Baker (Yahoo)
13. Benjamin Dick (IAB Tech Lab)
14. Fabian Höring (Criteo)
15. Sid Sahoo (Google Chrome)
16. David Tam (Relay42 )
17. David Dabbs (Epsilon)
18. Andrew Aikens (TripleLift)
19. Steven Avery (Google Ads)
20. Isaac Schechtman (IPONWEB)
21. Tamara Yaeger (Iponweb)
22. Alex Cone (IAB Tech Lab)
23. Sangharsh Kamble (Walmart)
24. Joel Meyer (OpenX)
25. Aditya Desai (Amazon)
26. Andrew Pascoe (NextRoll)
27. Jonasz Pamula (RTB House)
28. Ardian Poernomo (Google Ads)


## Note taker: &lt;?>


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here]



*   FLEDGE latency reporting to sellers [Issue 299](https://github.com/WICG/turtledove/issues/299) (Stan Belov, Google Ads)
*   FLEDGE API support for ad filtering [Issue 305](https://github.com/WICG/turtledove/issues/305) (Steven Avery, Google Ads)


## Notes



*   FLEDGE latency reporting to sellers [Issue 299](https://github.com/WICG/turtledove/issues/299) (Stan Belov, Google Ads)

Stan: Important to ensure latency impact from on-device logic doesn’t negatively impact user experience. Also need to be able to measure latency of FLEDGE. Can measure overall latency, but measuring component latency (trust signals fetch latency, number of interest groups, computation delay). Use case - What is latency impact of each buyer? – enables conversations between seller and buyer. Publisher could only support buyers that report soon enough. Could sellers receive info about component latency, broken down by buyer.

Paul: The issue listed several metrics (trusted signal fetches, latency and payload size, number of interest groups). Each bid receives number of ms required to calculate bid (computation time).

Stan: That is only scoreAd, so it is hard to report.

Paul: Could use debugging API for now?

Stan: There is a question of what that time actually indicates. If this is wall-clock time, it may not represent CPU cycles used by bidding function. That impacts latency, but also battery life, and slows down user’s device.

Paul: Right now is CPU time. Worklets are single-threaded. For other measures - web has APIs to measure performance - are broken down by Javascript context, separated by Origin. How does that apply to FLEDGE. Submitting a bid implies bidder has agreement with seller. For more granular metrics (metrics of trusted bidding signals) does bidder want to be measured?

Stan: In the current environment Sellers can measure and enforce bidding latency. Sellers want to measure and enforce latency of calls.

Paul: Giving latency about how long to create a bid is reasonable, but number of bytes in trusted signal request may not target latency. Don’t penalize bidders taking advantage of fast internet connections.

Stan: Clarify the concern

Paul: Current metrics are separated by Origin (security model of the web). Different worklets are different Origins. Bidders can refuse to bid (return 0 or negative bid). This may mean bidders don’t have agreement with seller.

Stan: Imagine 1 buyer takes 5 seconds for trusted signals call. What can seller do to detect and mitigate 

Paul: Can provide tools to allow bidders to expose this information to sellers. Analogy to CORS. Can standardize on special bid values (0 no bid, provide metrics. Negative don’t provide metrics). Or declare in interest groups. Could provide more metrics - wall clock, compute time, network request time.

Brian May: Can return information to seller about buyers that exceed a threshold. Just give error codes or timeout codes.

Stan: That partially works. Currently in FLEDGE there is no way to enforce trusted bidding signals time limit. Interested in whether a specific buyer often times out.

Paul: Bidding scripts can differ between interest groups within same buyer

Stan: Do interest group owners need separate bidding scripts for different scripts.

Alex: How are latency requirements in pre-bid?

Joel: Prioritize accordingly to latency, exclude slower bidders

Alex: It seems like a very similar use case.

Joel: High burden for seller. What can seller manage vs what can device manage.

Paul: Is there a summary statistic for seller? Overall buyer latency? Count of bids that didn’t want to be measured.

Brian: Analogous to automatically disqualifying bid requests in RTB if don’t respond in 100ms. Some notion of an impact score per buyer and/or per interest group. Don’t want anything from an interest group with impact above threshold. Should end users have equivalent sources of control?

Paul: End user part is complicated. We could put something together 

Alex?: Sellers need to know latency to make decision.

Joel: Budget metric. Different budgets per device.

Paul: Need to provide metrics. 

Brian: Need recourse for sellers to do something to keep out of future auctions. That needs to be fed back to buyers - so they know why not winning auctions. Go through sellers or direct to buyers. Two levels of feedback - exceed some threshold remove IG from participated, deeper view for trying to debug and fix things.

Paul: Number of interest groups user had joined - is that participating or overall.

Stan: Per-buyer basis. Similar to CPU time per interest group. How expensive was a given buyer. Need some sort of proxy for how expensive a buyer is – not per-interest group. 

Paul: Can calculate in scoreAd calls?

Stan: Would need reporting call for each scoreAd invocation. Not efficient. Aggregated reporting is what it was needed. Need path to inspect the latency metrics during the origin trials.

Paul: Browser could combine reporting – combine POST bodies with same reporting URL.

Brian: Several cases - Real-time feedback for immediate action.See variation over time (continuous feedback). Require frequent sampling.



*   FLEDGE API support for ad filtering [Issue 305](https://github.com/WICG/turtledove/issues/305) (Steven Avery, Google Ads)

Steven: Also concerned with latency. Observing majority of computation in browser is excluding ads based on requirements in bidder or seller – may exclude all ads. Overhead from generateBid too high. Move logic out of worklets to generalized logic browser can run. Buyer provides tokens classifying page. Some boolean function from trustedBiddingsSignals to filter ad. Seller can provide tokens and filter in auction config (publisher restrictions).

Paul: Running javascript is expensive. Chrome has been looking at time for different parts of auction. Context creation expensive - Javascript has a lot of builtins that need to be added. Filtering helps to avoid these steps when not necessary. Find middle ground of “safe to execute in browser” vs flexibility. Parsing in browser can be difficult to do securely. Example in Issue used JSON. Parsing JSON is something browsers can do. Do other bidders have ideas for how to express filtering requirements? Preferably not a new language.

Brain: Prefiltering would be good. Bidders are pipelined to do simple operations early then do expensive selections. How to find set of tokens that everyone agrees upon. 

Steven: Tokens are opaque.

Brian: How does seller communicate to buyer and buyer communicate to seller.

Steven: Seller provides page specification. TrustedSellerSignals provides Ad’s classification. Inputs and function provided by Seller.

Brian: That changes everything. How will that work in a FLEDGE world - don’t know when an interest group for a specific buyer will show up.

Paul: Is question how is buyer getting signals about the page, and seller getting signals about the ad?

Steven - Per-buyer signals can provide page info based on seller URL. Trusted seller server could do something with URL of the ad to evaluate it.

Jonasz: Could be useful. 1. Issue description does not mention contextual response. Decisions could be based on trusted seller response, contextual response,  and interest group data. 2. Could be used not only to skip, but also prioritize groups as ad campaign changes (resides on trusted server).

Steven: Mentioned contextual response. A priority score could be useful, but more complicated to spec out. Boolean functions simple enough. Linked to issue #302.

Paul: Boolean functions (proposed issue) or vector dot product.

Jonasz: Could be vector dot product. Would be good to solve both problems.



?[unknown speaker]?: Where would data reside?



Paul: Information comes from interest group, trusted servers, contextual information from auction config. Inputs could be vector-dot product, decision tree, etc.

Jonasz: perhaps vector product with sparse vectors, indices described as strings?

Alex: Bonsai from AppNexus- programmable bidding logic in language of its own. Could as Jeremie who built it about this issue.

Paul: Please comment on the issue. If have ideas submit them. Opportunity for speed improvements.
