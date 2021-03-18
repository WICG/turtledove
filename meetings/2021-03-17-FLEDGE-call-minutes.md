# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 4pm UTC.

That's 11am Boston = 8am California = 5pm Paris time (the winner of the [doodle poll](https://doodle.com/poll/eht2imrtvec69hxx?utm_source=poll&utm_medium=link)).

(This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload)


# Next meeting: Wednesday March 17 2021


## Attendees: please sign yourself in!	

1. Michael Kleber (Google Chrome)
2. Michael Lysak (Carbon)
3. Valentino Volonghi (NextRoll)
4. Brian May (dstillery)
5. Allen Fung (ShareThis)
6. Matt Wilson (NextRoll)
7. Don Marti (CafeMedia)
8. Les Cochrane (Audigent)
9. Jeff Kaufman (Google Ads)
10. Paul Jensen (Google Chrome)
11. Newton K. (Magnite)
12. Amanda Carlton (DoubleVerify)
13. Raj Belur (Amazon)
14. Jeffrey Wieland (Magnite)
15. John-Marcus Phillips (Nielsen)
16. Pat Moore (Bloomberg)
17. Ryan Arnold (P&G)
18. Philip Lee (Google) 
19. Travis Teo (Adzymic)
20. Anthony Molinaro (OpenX)
21. Larry Price (OpenX) 
22. Basile Leparmentier(Criteo)
23. Przemysław Iwańczak (RTB House)
24. Erik Taubeneck (Facebook)
25. Joel Meyer (OpenX)
26. George London (Upwave)
27. Paul Marcilhacy (Criteo)
28. Stan Belov (Google Ads)
29. Saul London (Google Ads)
30. Kelda Anderson (Microsoft)
31. Vinicio Haro, Bloomberg
32. Wendell Baker (Verizon Media)
33. Danilo Girello (GDB)
34. Benny Lin (Bombora)
35. Supraja Sekhar (Google Ads)
36. Vincent Grosbois (Criteo)
37. Grzegorz Lyczba (OpenX)
38. Julie Karasik (NAI)
39. Stephen Campbell (Nielsen)
40. Greg Bougeard (Teads)
41. Arnaud Blanchard (criteo)
42. Christa Dickson (Meredith)
43. Thomas Mouron (Teads)
44. Brad Rodriguez (Magnite)
45. Chris Evans (NextRoll)
46. Viraj Awati (Amazon)
47. Julien Delhommeau (Xandr)
48. Martin Gruau (Captify)
49. Kate Dye (district m)
50. Nicole Lesko (Meredith)


## Note taker: Joel Meyer


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.



[<img src="https://i.insider.com/5fc55f5350e71a00115580e8?width=700" alt="Screenshot showing location of the Raise My Hand button">](https://www.businessinsider.com/how-to-raise-hand-on-google-meet)



# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


### Open Issues, or Issues that Need Opening

https://github.com/WICG/turtledove/issues?q=is%3Aopen+is%3Aissue+label%3AFLEDGE


#### Seller validation of creatives: Issues [#106](https://github.com/WICG/turtledove/issues/106) vs [#108](https://github.com/WICG/turtledove/issues/108) (addressed by PRs [#147](https://github.com/WICG/turtledove/pull/147) v [#148](https://github.com/WICG/turtledove/pull/148)).  Which approach is better?


#### #114/#135 data clarification (similar to above but publisher side)

Michael Kleber (Google Chrome): [Administrivia] Welcome back. First on the agenda is open issues or issues that need to be opened. Have spent some time going through issues and made headway and then lost ground. Roughly 30 issues still to be caught up on at this time. Looking forward to having more Chrome engineers to help in the near future as development starts on FLoC origin trial. Until then, please be patient and have mercy.

Michael Kleber (Google Chrome): Issue #106 and #108 are a pair of alternate approaches to the same issue.

Supraja Sekhar (Google Ads): Publishers work with multiple SSPs. Issue #106 is about the ability of Sellers to validate the data associated with the winning ad candidate. Issue #108 is a proposal that sellers can bring their own trusted servers and fetch it in realtime. We believe this simplifies things by removing the burden of fetching/syncing the seller metadata with a bunch of SSPs. (Joel: need to understand this better and revise notes accordingly.) TL;DR: Can Chrome provide a way for sellers to fetch the metadata?

Stan Belov (Google Ads): Addresses a specific use case, would like to hear from sellers and buyers if there is interest in this.

Michael Kleber (Google Chrome): Thanks, great summary. Two different approaches to the problem of how sellers can get sufficient data to rank and evaluate the bids. The idea that this would be cryptographically signed meta-data provided by the DSPs predates Fledge and comes from a different bird (Tern?). The K/V approach is 

Michael Lysak (Carbon): Wanted to talk about a very similar issue involving trusted server access in issue 114.

Michael Kleber (Google Chrome): Hold and come back.

Brian May (dstillery): Does it make sense to have both approaches because it seems like the non-server approach may be faster in some situations and the server approach brings more utility. Both could be useful.

Michael Kleber (Google Chrome): Can see value in both approaches. The crypto sig thing makes sense only for the winning ad since it’s expensive and do it post-auction when you’re validating (106). The 108 mechanism provides the ability to influence the ranking pre-auction. Is there a different use case that 106 would be useful for? 106 seems limited, if possible would be nice to discard 106 and use 108.

John-Marcus Phillips (Nielsen): Agrees with Brian that there are use cases that 106 would address that 108 doesn’t, particularly in the case of audiences. 108 can address use cases around the experience I want my audience to have, where 106 can only do the post-validation part. Is it possible to modify 108 so that it can meet that use case as well?

Michael Kleber (Google Chrome): Can you please describe what you think can happen in 106 that wouldn’t happen in 108?

John-Marcus Phillips (Nielsen): To ML’s point, I think issue 129 may speak to this with more clarity but won’t go into it now. The use case aligned with 106: only once an auction has been won do I want to confirm that the user was the audience I intended to reach? Eg - was the user male/female or of a certain age? This doesn’t need to happen on all ad candidates, but only matters to the buyer once the ad has been rendered and won.

Michael Kleber (Google Chrome): After an auction winner is declared, what happens?

John-Marcus Phillips (Nielsen): Buyer wants to reach a certain audience, after the auction how do I make sure I reached that audience?

Michael Kleber (Google Chrome): Okay, so it’s expensive to perform that so I only want to do it on winners?

Stan Belov (Google RTB): Do you envision this only happening locally or do you require network access to do that?

John-Marcus Phillips (Nielsen): I think it would only happen locally.

Stan Belov (Google RTB): SO there would be some local means of validating this.

Michael Kleber (Google Chrome): Are you assuming that someone else would be providing this information and validating it is expensive so it only happens post auction?

John-Marcus Phillips (Nielsen): Yes.

Michael Kleber (Google Chrome): Interesting, makes me think of a proposal by Jeff K that is about a way for trusted signals to propagate to the seller without worrying that they get tampered with along the way. That might be a lower overhead way of accomplishing the same thing. Not sure of exact issue.

Michael Kleber (Google Chrome): If other thoughts on use cases for 106 are identified please post in 106. 119 might be the relative issue.

Jeff Kauffman (Google Ads): Might work, not sure though.

Michael Kleber (Google Chrome): 

Erick Taubeneck (Facebook): The histograms on the private conversion measurement API may be able to solve this issue as well.

Michael Kleber (Google Chrome): My understanding is that this isn’t a reporting focused request, it’s a bidding time focused request that I want to short-circuit the rendering if this isn’t the right audience.

John-Marcus Phillips (Nielsen): Premise is correct, conclusion that this is a pre-bid signal isn’t necessarily true. This occurs at impression time when the scoring function is run.

Michael Kleber (Google Chrome): If it’s a post render signal then I don’t think we need the 106 mechanism, which is a way to bail out if fraud is detected. Sounds like a different use case.

Michael Kleber (Google Chrome): Any other comments on 106 v 108? None brought up… moving on to Michael Lysak issue.


## #135 https://github.com/WICG/turtledove/issues/135 \
FLEDGE: Discuss Every bit of Data

Michael Lysak (Carbon): During last meeting we talked about data, what data goes to what worklet to identify the data flow. Raised 114 to talk about this. Hoping we can identity the key points of confusion in where the data is. Hoping we could get a publisher worklet because there are certain signals that a publisher agent can get from Prebid.js. Where does it exist in this ecosystem? Is it from the aggregate API? Exposed by browser or another party? If another party we need a separate signal. To narrow this down, can we discuss which data are available to be sent to servers at an event level in each worklet?

Michael Kleber (Google Chrome): Sure. Part of the confusion is that the TD high-level approach and the Fledge explainer are talking about two different points in time and they are somewhat mixed together, apologies for the confusion. TD in general is aimed at a long-term end state in which no information is made available on an event-by-event basis to a server. An end-state in which all logging is done through an aggregate measurement service. There is much discussion about what mechanisms for logging will come to fruition and we need to get to resolution. But in the end-state, no information will be provided on an event by event basis because any info is a source of privacy leakage. That’s the long-term goal. However, it is understood that this will be a very difficult transition for adtech so we won’t move to that end-state at the moment that we first start Fledge and get rid of 3P cookies. Fledge has a carve-out to provide an event level logging option during the transition to provide limited information about what ad was the winner. There is not an API that provides a server txn level info on all bids, ads, or IGs as that would allow someone to join cross-site identity. We are talking about a way to allow k-anonymous information (who won?) with the page level information. So there’s only high-fidelity information only on the page info, something the pub already knew.

Michael Lysak (Carbon):: Today, a pub using Prebid.js, can identify information about the different SSPs participating to help optimize their business. In a Fledge world, how can I do that? It can still be useful for a pub to know this, so in Fledge how can a pub know this?

Michael Kleber (Google Chrome): This sounds like a use case that can be met using aggregate information. If you have five bidder adapters and you have the bid landscape information you can look at the aggregate information to determine which bidders were ‘useful’ what percentage of the time. In the Fledge design I expect that information to come from the person running the auction. It sounds as though the desire is that another party independent of the auction runner can get access to this information and do some level of aggregate reporting to make it available to the pub. Is that right?

Michael Lysak (Carbon):: Pretty close. I’m not saying any 3P script can do this, just the pub themselves or someone acting on their behalf.

Michael Kleber (Google Chrome): The way I would think about adding this is to use the Fledge configuration object and add a script that identifies an independent auditor who can see the buyer and seller info and provide aggregated data on what happened.

Michael Lysak (Carbon):: Would meet most of our needs, but I’m concerned about the winning CPM info, for instance. Some Pubs separately tally up all the bids and keep their own records to do revenue resolution at the end of the month (Joel: this is discrepancy resolution). Is there a way to give pubs access to the unmodified winning CPM? Presumably the bidder is able to do this, can the pub do the same?

Michael Kleber (Google Chrome): End-state does not include an event by event stream and everything will be done via aggregate reporting. The noise introduced will be very very small. Enough to hide the contribution of any one event.

Basile Leparmentier (Criteo): Also has a question on bidding. There is huge variation in what we pay for a given ad. One thing that Michael Lysak (Carbon): raised that is very important is that for resolution we have two sources: buyer and seller (issue 130). Is that still the case in the future with the aggregated reporting API? If there is a point of failure, in some cases only the buyer or the seller will know what happened. In today’s world the biggest loss is a transaction, not all the reports (Joel: summarizing heavily). I think this is a huge huge issue.

Valentino Volonghi (NextRoll): Super quickly adding something. My ask to talk about 2nd price auction touches on this as well. It’s an issue of auction dynamics and accountability.

Michael Kleber (Google Chrome): Let’s keep 2nd price auction question separate and focus on the two books question. The Fledge design does specify that there are two books: a buyer and seller both have an independent opportunity to log what they think happened.

Basile Leparmentier (Criteo): Everything goes to the Google server. So if there’s a bug, how do we know what happened? Everything goes through Chrome. If there is a bug in [diff parts of Chrome] - how do we solve it?

Michael Kleber (Google Chrome): Let me talk a little bit about the structure of the aggregated measurement machinery we’ve put together. Charlie here? Nope. Stepping back from this discussion and moving to aggr reporting which is totally separate infra. The idea of how we think aggregate reporting works is that anytime in the browser (including w/in the worklets) there is a way to get information out and that is to send an aggregated message that is an encrypted blob to the party who is doing the reporting. If there is a Criteo worklet that is reporting on the outcome of an auction (or any party), they learn what happened in the auction by doing some JS computation or something. Then they say “I would liek to log this number.” That gets encrypted and sent to that party. The individual event is not something that can be decrypted. The point is that on an txn-by-txn basis those can’t be decrypted. But you can take them from a larger window and hand them to an aggregated decryption service and get the total back. You can perform different operations on it.

Basile Leparmentier (Criteo): Really? We get an actual realtime data?

Michael Kleber (Google Chrome): Yes, might be a small delay or something, but you do get the records and you can send them off to the magical MPC service that gives you totals back, or multiple things that can be totalled. But fundamentally, you still get the data back the way you used to. You have to pass it to a service to get the totals, but the encrypted event blob is there. You do have some noise.

Mehul Parsana (Microsoft): What party does the data go to? Does it go to the DSP or the SSP?

Michael Kleber (Google Chrome): I see this as both parties report to their own endpoints and maybe even a pub focused endpoint. Everybody gets their own metrics (yay!). 

Mehul Parsana (Microsoft): But there’s only one impression event... 

Michael Kleber (Google Chrome): I think you’re confusing this with the conversion measurement API, which gives the DSP/advertiser (a single party) the information. It’s their transaction. The on-device auction is different, it involves multiple parties and each of those parties gets an opportunity to do aggregate reporting.

Mehul Parsana (Microsoft): I get that, but there is only one party that gets to see the outcome (the Pub?) when they individually get their data they are telling someone that this user was there. You’re saying that there are multiple reporting origins?

Michael Kleber (Google Chrome): Yes.

Mehul Parsana (Microsoft): In the reporting API that wasn’t clear.

Michael Kleber (Google Chrome): The conversion API doesn’t go into detail on this as it’s a single party. The Fledge explainer is also somewhat thin on details (in section 5). And it says that DSPs and SSPs are both bidders and auction runners and they each get to do reporting, but it’s not explicit.

Michael Lysak (Carbon): Just wanted to finish up on the topic, it sounds like I need to follow up on 114 with a response that details the business needs of a publisher trusted agent.

Michael Kleber (Google Chrome): Yep. Valentino - are there new questions about 2nd price auctions that are different?

Valentino Volonghi (NextRoll): Just wanted to say that I think 2p auctions would help with some of these issues. If the bid price is a vector of privacy, then 2p removes the vector of information and it becomes possible to transmit bid price through a proxy and send the price to multiple parties, which takes us back to the traditional adtech model. At a high level it would allow us to say that this campaign on this pub spent this amount of money.

Michael Kleber (Google Chrome): I believe further discussion coming in web-adv, we can talk about it there.


## Stan Belov on Google's Bid Request Simulation Experiments

Stan Belov (Google RTB): As some of the people on this group may know, the Google OpenRTB team is running multiple experiments, including a server-side Fledge simulation. Wanted to make people aware in case they want to participate. Info available on Blog post on how to become part of the simulation if you work with G as an RTB partner. Link:

https://ads-developers.googleblog.com/2021/01/announcing-new-real-time-bidding.html

Michael Kleber (Google Chrome): Thank you all for joining, next get together in two weeks. Folks in EU will be back on same TZ. Looking forward to further engagement.
