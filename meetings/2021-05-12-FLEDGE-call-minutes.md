# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 4pm UTC.

That's 11am Boston = 8am California = 5pm Paris time.

(This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload)


# Next meeting: Wednesday May 12 2021


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Allen Fung (ShareThis)
3. Cory Underwood (Search Discovery)
4. Paul Jensen (Google Chrome)
5. Michael Lysak (Carbon)
6. Michael Coward (Arcspire)
7. Remi Lemonnier (Scibids)
8. John Bryson (Resonate)
9. Benny Lin (Bombora)
10. Jonasz Pamuła (RTB House)
11. Andrew Pascoe (NextRoll)
12. David Tam (Relay42)
13. Brendan Riordan-Butterworth (IAB Tech Lab / eyeo GmbH)
14. Wendell Baker (Verizon Media)
15. Phil Lee (Google Ads)
16. Brad Rodriguez (Magnite)
17. Nathan Jia (Nielsen)
18. Brian May (dstillery)
19. Noah Harris (Klickly)
20. Luke Whittington (Glimpse)
21. Erik Anderson (Microsoft)
22. Piotr Banaszczyk (Clearcode)
23. Phil Eligio (EPC)
24. Jeff Kaufman (Google Ads)
25. Joel Meyer (OpenX)
26. Don Marti (CafeMedia)
27. Martin Gruau (Captify)
28. Viraj Awati (Amazon)
29. Christa Dickson (Meredith)
30. Fred Bastello (Index Exchange)
31. Gang Wang (Google)
32. Julien Delhommeau (Xandr)
33. Ryan Arnold (P&G)
34. Matt Wilson (NextRoll)
35. Les Cochrane (Audigent)
36. Afolabi Adenuga (Nielsen)
37. Tanya Moldovan 
38. John-Marcus Phillips (Nielsen)
39. Vishnu Prasad	(Nielsen)
40. Aleksei Gorbushin (Walmart)
41. Kale Smith (Nielsen)
42. Przemyslaw Iwanczak (RTB House)


## Note taker: Michael Lysak + Joel Meyer


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.



[<img src="https://i.insider.com/5fc55f5350e71a00115580e8?width=700" alt="Screenshot showing location of the Raise My Hand button">](https://www.businessinsider.com/how-to-raise-hand-on-google-meet)



# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

 		


## [Suggest agenda items here]



*   Update on implementation status (Paul Jensen)
*   Multi-tiered auctions - Currently Fledge seems to support only a single tier of auction, while publishers today use Prebid to manage multiple tiers. Can we discuss if/how/when Fledge will support multiple SSPs needing to resolve auctions for a single ad slot? Related issues: https://github.com/WICG/turtledove/issues/73, https://github.com/WICG/turtledove/issues/59 
*   https://github.com/WICG/turtledove/issues/177 (aggregate attribution reporting API & FLEDGE?)
*   https://github.com/WICG/turtledove/issues/116 (FLEDGE : Answering contextual requests with a generate\_bid() function)

MK: Please sign in if you’re attending. Please use the “Raise Hand” function to join the speaker queue. Please be sure you have joined the WICG.

MK: First agenda item is an update on implementation status and then we’ll get to other items.

PJ: “””

As of 5/12/2021 here’s where Chrome’s FLEDGE implementation is at:

This functionality is available from Chrome versions 91.0.4472.49 or 92.0.4493.0 when the --enable-features=FledgeInterestGroups,FledgeInterestGroupAPI flag is passed to Chrome.  Using the latest canary version of Chrome will get you the best debugging experience.

All APIs are only accessible from frames with HTTPS origins, and all URLs and origins passed to the APIs must be https.

joinAdInterestGroup() and leaveAdInterestGroup() are implemented.

For now, this is the implementation now to facilitate testing but is not indicative of final implementation and is expected to change, the origin of the frame calling the API must match the interest group owner.  The interest group owner must be specified as an origin (explainer says it must be a site).

Feature-Policy and .well-known delegation mechanism is not implemented.  For now, this is the implementation now to facilitate testing but is not indicative of final implementation and is expected to change, any frame can call the API.

These APIs have various argument checking (i.e. arguments are present and parsable), but there is no further feedback as to whether the APIs succeed or fail as this might pose a cross site identity risk.  Think of it as a write-only API.

Interest groups are stored in an SQLite database.  This is similar to how cookies are stored. Interest groups are retained for the minimum of either the duration specified by the joinAdInterestGroup call or 30 days. Join and bid counts for interest groups are calculated over a 30 day period, and the win history is limited to the past 30 days.

Periodic/daily updates via the “dailyUpdateUrl” are not implemented.  We are working on an API to initiate manual update of interest groups attributes via the “dailyUpdateUrl”.  It is not complete.  This is the implementation now to facilitate testing but is not indicative of final implementation and is expected to change.

runAdAuction() is implemented.

“seller” attribute must match the “decisionLogicUrl” attribute’s origin.

“interestGroupBuyers” attribute selects the interest group owners who can participate in the auction.  Specifying the wildcard for interestGroupBuyers is not supported.

Note that the argument names to all APIs were changed to camelCase as opposed to the originally proposed underscore\_separated names to match other web APIs.

trustedBiddingSignals are fetched and then biddingLogicUrl is fetched and the bidding scripts are run.

decisionLogicUrl is fetched and the scoring scripts are run.  The sell-side analogue of trustedBiddingSignals, called trustedScoringSignals, is not implemented yet.

Note that the fetch of the decisionLogicUrl is performed as an uncrendentialed, CORS-enabled Fetch request from the parent page.  Interest group fetches are not done with CORS enabled as they were advertised in a same-site context previously.

Note that biddingLogicUrl, decisionLogicUrl, and trustedBiddingSignals all require a “X-Allow-FLEDGE: true” HTTP response header.

These fetches and scripts are run in a separate process to provide a security boundary between the bidding and scoring scripts, and the site’s renderer process where the site’s JS scripts are run.

The browser compares the scores and picks the winner, if there is one.

The runAdAuction() API returns a Promise for a URL.  For now the URL returned is the renderUrl of the winning ad, this is the implementation now to facilitate testing but is not indicative of final implementation and is expected to change.  Eventually this will be an opaque URL.

Currently event-level reporting is implemented, this is the implementation now to facilitate testing but is not indicative of final implementation and is expected to change.

If there is a winning bid, the seller’s reporting script is loaded and executed.  The reporting script is provided a sendReportTo(“https://example.com/report”) API which takes a URL as an argument.  This URL is fetched after the reporting script completes.

If there is a winning bid, the bidder’s reporting script is loaded and executed.  Again, a sendReportTo() API is provided.

For now there is no k-anonymity enforcement, this is the implementation now to facilitate testing but is not indicative of final implementation and is expected to change.


### “””

MK: End of prepared remarks, let’s move to questions.

BR: Can we update the proposal to reflect the change to CamelCase in the implementation? BR is happy to a PR, PJ - do you have a pref?

PJ: No preference, either is fine.

BR:  Noticed there was no support for wildcard buyer enablement, that could make initial testing hard. Is there a technical reason for that?

PJ: Two reasons: 1) we just chose not to implement it yet, and 2) need to think about security implications.

BR: Reporting looks like it’s being called at the end of the auction but not at render time, in the original workflow, we envisioned that a render could be when reporting gets triggered so that you don’t end up with an auction that’s won but isn’t rendered. Is it possible to move to the reporting call on render?

PJ: This is an area that is likely to change. For now you can delay the reporting call, but this is likely to change.

BR: Thanks for the update, Paul.

MK: Next hand is Michael Lysak.

ML: Paul, one question for you and then a general question. You said folks were filing bugs, where is that happening?

PJ: CRbug.com (link here). Create bug and assign it to me or put Fledge in the name.

ML: Second question is more general - group seems to be straying from WICG guidelines where work happens after design, but here design is happening during implementation. Last week you had said we could not clarify or discuss the api because the chrome team had not developed the prototype yet; Is that by design?

MK: API design is often informed by implementation so that’s kind of what’s happening here. As the proposal works through the standardization process, then the implementation in Chrome might end up needing to change due to the standardization process. This is prototyping going on in Chrome and doesn’t necessarily reflect a design about the final state. Everything can and will change - this is not an implementation of a standard, just a prototype of an idea that is helpful in understanding the concept.

ML: Makes sense. If Chrome version is a prototype, could we then move to specify the details of the API in the working group github so it can be more well defined and be discussed?

MK: We can definitely have all kinds of discussions and it will take some time for the prototype to match up to the design. The spec will also lag behind because we haven’t started the spec.

ML: Makes sense - must have been a misunderstanding of what you said last week

JP: Thanks for the summary, Paul. To what extent is this implemented in Chromium vs Chrome? We would like to have an env where we can experiment with this and we’re wondering to what extent we can rely on Chromium.

PJ: It’s all in Chromium. There are several different parts of Chromium and almost all our implementation is in the Content side, which is in chromium.

JP: Sounds good, thanks.

BM: Thank you Chrome team for implementing and delivering the update. Is there any documentation that reflects what was talked about today? Or is it all just going to be in the proposal?

PJ: Things are changing very rapidly so it’s hard to keep it well documented in a way that isn’t quickly obsolete.

BM: Mentioned it because several things were mentioned that aren’t implemented yet and it would be helpful to know when those things change.

MK: Queue is empty, going to wait a minute in case anyone has more thoughts.

ML: Wanted to second the last point that it would be useful to know when things are changing, The existence of the publisher trusted agent changes the workflow and is needed for some publishers and businesses and would really like to know when that’s added.

MK: The thing PJ referred to about a better communications channel is in relation to what IS implemented, and ML is asking about what WILL be implemented. We can figure that out. Something better than copy/paste into notes on what is done can be improved.

J-MP: Wanted to see if aggregate reporting api is next on agenda.

MK: Order has multi-tiered auctions next, but doesn’t have to be that way. Then aggregate reporting, then 116. Can change order if it flows more naturaly.

J-MP: Wil hold for later int he call.

MK: Unless other objection, we’ll move on to the next topic. Multi-tiered auctions. Currently Fledge only supports ….


### There are links to issues 73 and 59. I think issue 59 is canonical. I have commented a great deal, and others. 

Joel Meyer: Today, publishers use multiple ssps, they makes decisions, hand it to prebid which makes ultimate decision. Need a way to resolve between opaque pointers. Have chrome given this some thought as to how this might work?

Rodriguez: Agreed will follow up.

MK: Sounds like there are 2 issues maybe? One way to think about htis like what I said in issue 59 is we need a fledge style auction with someone’s auction logic that has separation between buyers and sellers running auction, then another fledge style auction, in which winner of one is entrance into the next. Auction number 2 has a seller. This could go on indefinitely. That is one possible way of thinking about it. I think it has tricky questions we have not designed for. We ought to be able to know whose auction is being bid into. Does it now get a new chance to bid, or a new second bid? How do you know the bid is similar to that of auction 1? There seem like some hard questions there.

This is seperate from the result of the auction like Paul was saying? 

That strikes me as thing 1. Seems different from a single dsp that has a single interest group and wants to pick 1 form among a small number of ads whose interest groups are owned by the same person. Not a series of auctions, just a pre auction stage in which a single buyer can do some ranking among its own interest groups. You know the seller. I think this is a different case. Is this correct?

JM: I’m not sure that is the case. Lets say the seller needs to choose contextual or interest based groups to win. HOw to we allow punisher to make a decision between the two groups that are making the contextual versus interest decision. This is not an auction issue, its a decision process that fledge seems to kill. It is currently functionality in today’s ecosystem and we do not wish to lose it. 

MK: What is the difference between how the contextual and interest compete versus how different parties compete? I don’t understand.

JM: The reason for issue 73, how do you except these apis to work with ssps and sellers. 1 seller does not represent all demand. I think the api should try to represent how the industry works today. Dsps have placed bids, who gets to represent them and how? How does the publisher agent fit in?

MK: IT sounds like openx wants to have multiple dsps run into an ssp auction, and then compete with the winner of another ssp, and then have a final auction?

JM: yes

MK: This is the first of the 2 situations I was talking about. It is not clear who decides which bid where and there is design work here that I should not be doing. I think the ad industry needs to discuss the use case that needs to be recorded. 

JM: I think we’d all love to see the api and workers become more crip more quickly.

BR: You mentioned competing with the winner of the first auction, but we aren’t looking for the full featured fledge auction, but a resolution of auctions based on price. A second scoring function rather than a second full auction. It is more normalized.

MK: That is more like the original turtledove proposal, but it was rejected. 

BR: Not intending to backtrack on the concerns, but there are ways to do this in fledge. Fledge already ranks based on score to determine if it can serve. It is not our intention for the browser to steal control and pick one, but the function could treat them equally and rank similarly. So rather than having 1 huge auction, 2 layers, 1 represented by a single scoring function and another layer represented by a different scoring function.

MK: clearly more discussion is needed. worth having an hour in 2 weeks?

BR: will try to document something. 

MK: I dont feel like I understand the problem atm from the browser perspective.

Mehul P: Dsp choosing a top winner an intermediate server is still choosing a top winner, and it cascades right now in the current world. Regarding the 2 issues you mentioned, they are kind of different flavor of same topic. They have partial signal so they can’t make a decision upfront. They are waiting on the browser. I hope this make it crispr in terms or the problem.

MK:ok.

Julien d: Wanted to ask to clarify the usecase. Today it makes sense in prebid to have this auction competition. I do not understand why the fledge auction initiated by openx is different. In both auctions, in theory it should be the same buyers using the same logic which is pulled from the same interest group, the only difference is the rank option. Why would it be different for a different ssp/dsp? I understand auction is run by the browser, why is this usecase still relevant?

Joel M: That is a good question, but you are assuming adtech is efficient. THe ssp has responsibilities to the publisher. Even in a fledge world someone has to do this job and enforce ad quality, brand management, etc, it not always price (and that has been extensively discussed). Publishers get more revenue working with different entities, they have uniquenesses. There is a publisher benefit, and that is why. In a fledge world, it does not make sense to have just 1 entity do that.

Julien: I get your point. In a prebid world it makes sense. In practice this makes sense. In fledge isn’t the demand the same? Why do you need multiple ssp if the adcall will be the same? It makes sense today due to differences. Maybe 1 ssp will bring additional value. Can you think of a concrete example of different demand?

Brad R: It is similar to how prebid works within fledge. Different ssp has access to different demand due to different buyer relationships. Even if browser holds all the different demand, there has to be trust, possibly a paid relationship.  In theory every ssp could call every buyer, but that is not realistic. Different relationships, strengths, and weaknesses exist. This creates competition and gives buyer access to more inventory. 

Julien: I get your point. I disagree. Yes there are these different relationships. I think long term that won’t exist, but I agree you are right, initially this will be the case.

BRad: We should discuss in web adv, publishers can give you even more use cases.

Joel: negotiated rates is another example. players may not have the same access and that is a concrete use case.

MK: I have a deep suspicion that publisher concerns might be violated with a multi-auction model, which makes it complicated. I think discussing in web-adv business group is the way to go.

MK: Issue 177, can we discuss in 3 minutes

Jonasz: not enough time. can we have weekly meetings?

Mehul: Depends on overlap between fledge and parakeet.

Jonasz: 177 is fledge specific, not parakeet. if we could handle in github would also be good.

MK: google I/O come!

