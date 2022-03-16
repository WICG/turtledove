# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 4pm UTC (during winter).

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Mar 2, 2022


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Matt Menke (Google Chrome)
3. Paul Jensen (Google Chrome)
4. Brian May (dstillery)
5. Jeff Kaufman (Google Ads)
6. Wendell Baker (Yahoo)
7. Michael Coward (Arcspire)
8. Joel Meyer (OpenX)
9. Philippe de Lurand Pierre-Paul (Google Chrome)
10. Christina Ilvento (Google Chrome)
11. Chris Evans (NextRoll)
12. Sergey Tumakha (Microsoft Ads)
13. Caleb Raitto (Google Chrome) 
14. Peiwen Hu (Google Chrome)
15. Garima Dhingra (Google Ads)
16. Anthony Molinaro (OpenX)
17. Isaac Schechtman (IPONWEB) 
18. Stan Belov (Google Ads)
19. Supraja Sekhar (Google Ads)
20. Erik Anderson (Microsoft Edge)
21. Fred Bastello (Index Exchange)
22. Aditya Desai (Amazon)
23. Alexandru Daicu (Eyeo)
24. Andrew Pascoe (NextRoll)
25. Tim Hsieh (Google Ads)
26. Christina Brauer (Capital C)
27. Brad Rodriguez (Magnite)
28. Przemyslaw Iwanczak (RTB House)
29. Bartosz Marcinkowski (RTB House)


## Note taker: JoelPM


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

 		


## [Suggest agenda items here]



*   [Supraja] Passing filtering reason to buyers
*   [Jeff] Passing auction signals securely to buyers on device.
*   [Stan] Second price reporting support.


## Notes

Michael Kleber: [Insert WICG boilerplate and meeting stuff here]

MK: Nothing on the agenda currently. Was out of office, had a wonderful time in New Orleans. Still catching up - has it been quiet or are there agenda items that people wish we were talking about?

Brian May: In last meeting we discussed working across different venues in order to come up with a consistent model. Any feedback internally on how to do that?

MK: Have flushed a lot of state due to vacation, what are the different venues?

 \
BM: Android is working on this, Chrome is working on this, server-side issues - etc.

MK: I have pointed out to the Android team that these meetings are happening and they are welcome to join, and that there is interest from this group in discussing it with them. Android has not historically worked under WICG, but they do have a form for feedback that is their standard way of hearing from the outside world. I don’t see any Android folks here today, but if I missed anyone let me know. Don’t see anyone. (Reminder to sign-in on the doc to those who haven’t.) Don’t have a better answer right now. This forum is a good place to talk about Fledge and when we have discussions about things involving both servers and the way browsers interact with them, and on-deivce computation this is a good place for it. We are highly motivated to end up with a shape for the API that makes it as easy as possible for those who are doing stuff here and on Android in-app ads and not have to repeat work. We certainly will try to carry things that we hear in this venue over to Android folks. But the other channels that there are for giving feedback to them seem like a necessary separate part of the conversation.

BM: If we come up with a specific set of APIs and they have a certain meaning in our conversation, what if Android interprets it differently? How would we reconcile that difference in interpretation?

MK: That’s the goal of writing standards. You have to write them down clearly enough that another implementer can understand them and implement them. Differences based on the environment are to be expected, but where the env isn’t a factor it should be the same.

Christina Ilvento: If there are Android specific questions, would it make sense to get them on the agenda in advance and ask them to come?

MK: We shouldn’t expect Android to be here regularly like the Chrome folks are, but if there are specific Android questions, having them on the agenda might make it feasible for us to get an Android person here. Great point. Having nothing on the agenda makes it more likely that no Android people will attend. See Fenced Frames/Shivani as a similar example.

Brad Rodriguez: I wanted to ask if it is true that Google intends Fledge to be a w3c standard, or if its’ some other type of standard not under the auspices of w3c.

MK: The w3c is the natural standards venue for adding APIs to Javascript. There are other types of standards that are in play for other parts of the privacy sandbox. Some are standardized under IETF, e.g. network comms and cryptographic protocols (Trust Tokens).

BR: Just to clarify, are you trying to drive Fledge to become a w3c standard? With Android, seems like this might be a higher level conversation?

MK: I think there is no natural alternative venue that would replace w3c in the process we are currently going through. Stuff that happens on Android doesn't generally have a home in a public standards body, so there’s not a natural replacement that uplevels to cover android as well. I will point out that the w3c’s PATCG (recently formed, first meeting) was explicitly chartered and scoped to include things that cross the boundary between web and other platforms like OSes, etc. I think that the w3c is an appropriate home as best as we understand things. No plan to move elsewhere, and no obvious elsewhere.

BR: Great.

BM: With respect to the question of having agenda items for Android folks, I’m not really sure how to think about Android. I wonder if we could have the Android folks look at what we’re doing and give an overview of how things might be different for them from what has been contemplated for Chrome/Fledge.

MK: Sure, that seems reasonable. I will convey that and if we get a bite from them, perhaps it will be on the agenda in the future. Keep your eyes on this doc.



*   [Supraja] Passing filtering reason to buyers

Supraja Sekhar: I added an agenda item about passing the filtering reason from the scoreAd function from the seller to the buyer. Is that something we’re planning to actively support or should sellers try to find a different way to support this.

Stan Belov: I think this may be in reference to the debug reporting functionality discussed in the first OT.

SS: Also discussed having a macro that might be substitutable with filtering reason.

MK: Anything to add Paul?

Paul Jensen: Just to clarify, this is a signal that would come from the seller's scoring worklet and you’re asking how we can channel that to a loss report from a bidder? 

SS: Yes. It could go into loss reporting or the debug API - either would work. As long as there’s the ability to pass an enum about why they were filtered.

PJ: I think initially with event level reporting there may be ways that the seller be able to correlate that with a specific bid so the bidder can ask the seller for the histogram of reasons they were rejected. In terms of passing directly, the granularity makes sense for an enum to avoid passing sensitive info. What kind of granularity in that signal makes sense?

SS: I guess the rough ballpark would be 20-25, but maybe lesser. The granularity would be: 1) filtered due to policies, 2) pub blocked the category, etc… I can think of maybe 10 reasons why.

MK: Is there an Issue in which we’re developing a list of reasons for this hypothetical enum?

SB: Not sure there’s an issue, but I don’t think we’ve captured the loss reason.

SS: Previously just a discussion in this forum. Happy to create an issue after the meeting.

MK: Yes, an issue with a set of enum values would be extremely helpful. All the people here can chime in, and when we know the set it’ll help inform what the API should look like and how the buyers & sellers collectively learn what the outcome looks like.

SS: Sounds good.

MK: We are definitely interested in this, it seems like a natural set of outcomes that should be reported. We just need to flesh it out.

SS: Thank you.

BM: This raises in my mind a larger question about how do I, as a Fledge participant, know if I am being included in a Fledge auction - due to the disconnected nature.

PJ: How does a buyer know?

BM: Yes, if I install a bunch of IGs - how do I keep track of what’s going on?

PJ: We have several different reporting mechanisms that are meant to keep track of that. The bidder worklet is allowed to invoke two different reporting functions during its execution to record either win or loss, and the worklet is also able to invoke the debugging API

BM: Are those time-delayed?

PJ: For now, they are not. We are considering some rate limiting so they may be delayed a slight amount (1s) if there are tons of them - Chrome can’t send out 100s of network requests all at once.

BM: So would it be accurate to think of these as sort of equivalent to pixels?

MK: Seems like a reasonable analogy, but this is better, because you get it when you lose as well. Pixels don’t provide much if you don’t get to render in the browser.

PJ: These are the mechanisms we hope people will use to find out info as they participate in auctions.

MK: Job of this group is to put together the set of outcome states and on the Chrome side we will think about how to plumb them through to the reporting mechanism?

MK: Any other topics? Not opposed to ending early if possible.



*   [Jeff] Passing auction signals securely to buyers on device.

PJ: I think last call we had agenda items we didn’t get to. Jeff Kaufman has items on securely propagating signals to the auction.

Jeff Kaufman: That’s right, we didn’t get to that. It is reasonably common that there is info that one SSP has on a page that they’d like to make available to themselves and their bidders, but not everyone else. For example, your SSP prides itself on having viewability data and providing that to bidders - but you don’t want others to be able to get that info. Not a huge huge issue, but still somewhat important. In the current model, all the info gets exposed. And a lot of it depends on context, so there’s not a way to get it from a trusted server. Currently there’s no way to pipe information from a contextual ad response to the sellers worklet. Is this functionality that other SSPs would be interested in? How much is this valuable to others outside of GAM?

BM: When we discussed last time I wondered if something like CHiPs or Storage Access API could be used to accomplish this?

JK: I don’t think that would really be possible in this case. We have info attached to a specific slot on a specific page at a specific instance in time, so attempting to use storage doesn’t really seem like the right approach.

JM: Prebid already does this today by just obfuscating stuff, but it would be great to have a really secure way of doing this. Different proposals have timing implications.

BR: Timing issue does seem like a concern, but it would be great to have a way to use this asynchronous call to pass this information. But I think this needs to be done in plain Javascript anyway, so if you can see the SSPs JS, then probably you figure out [stuff]. But it would be great to have a mechanism that is fair. The goal seems like a good goal.

MK: I want to make sure we’re talking about the right thing. Specifically, a channel for providing information from their server to their auction, under their control. To the buyers and the seller ranking/scoring script in their auction, while not exposing it to other parties. This seems like a more pressing concern now that we have Component Auctions and multiple sellers running on the same page.

Andrew Pascoe: Sorry, I feel a little confused. Not sure I totally understand the problem. I think there’s a way around it. The SSP needs to conduct a contextual request to all of the buyers. Isn’t there a path where SSPs could package up things they know about the page in their contextual request to the DSPs, the DSPs could take that into account with their own package in the contextual response that they could use in their own bidding worklets? How would any other SSP have access to that info in the contextual request?

SB: It would still be populated as part of the Auction Config in perBuyerSignals, which would necessarily be part of the publisher’s page. It’s just a different part of the configuration where these signals go.

AP: When the auction gets instantiated a bunch of JS has to provide info?

JK: Anything that passes through the pub’s JS context is available to anyone who is running there.

AP: But the SSP still gets the network call from the page, right? Maybe I’m thinking about a different stage?

MK: I think you’re right, but if SSP1 has some special sauce from ML model and they pass that in the server-to-server real-time bid request, then the buyer passes something back that ends up on the page, another SSP or buyer could observe that perBuyerSignal that came back from SSP1 that got put in the AuctionConfig. So it basically reduces to the obfuscation model mentioned before, because it’s technically visible to any party on the page. So if parties are putting effort into trying to steal information from each other, then having a secure browser channel would prevent that.

AP: Sounds similar to an issue from a year and a half ago from Criteo about bidding logic being visible to anyone. The general response then was that transparency is a good thing for the market. It would be good for everyone, including users. What makes this fundamentally different?

JK: I think users being able to tell what’s happening and external people standing up Chrome and telling what’s happening - that’s all good. But when one party can take info from one party and use it to their own advantage is a lot less useful. Take for example the contextual bid - if they have a floor that they use to make a decision between IG and context, another party could take that floor and use it to impact their own bidding strategy.

MK: Thanks Jeff, I agree - very different situation from having JS visible.

BM: I think the way to think about this is to say that anything I do today on the server side should be partitioned in the client in a Fledge world.

SB: In the current Fledge proposal it’s already partially partitioned, but the data - the inputs - are not fully isolated. They can be observed on the pub page.

BM: Is there a fundamental barrier to doing that?

MK: That’s what’s being discussed in issue #119. Isolation of signals to the appropriate context. I think the answer is that there’s no off-the-shelf mechanism that addresses this need. Jeff is looking to understand the importance of this feature so he can appropriately encourage us browser engineers to work on it.

JM: The options in that Issue are interesting.  Need to discuss here, or can we just continue evaluating there?  Seems like we are agreed here that there is value.

JK: Happy to keep talking on ticket, glad we've raised visibility.

MK: Okay, great. Thank you for bringing that up, Paul and Jeff. Good discussion. Any other topics?



*   [Stan] Second price reporting support.

SB: I will just bring it up, no solutions or suggestions in mind, but I think there’s still an open question of whether Fledge could provide some sort of second price from the auctions. Some early Fledge doc may have mentioned something about a second price being made available. Perhaps part of aggregate reporting, could it be made available in the origin trial?

PJ: Happy to jump in here. The Explainer does talk about getting that value into some of the reporting worklets and I think we’re close to having some kind of a solution. There might be two areas where this could be percolated into the reporting. If they could be divided into winners and losers in the auction. The winners have the reportWin and other fn, those fns receive browser signals - so the browser could forward that information. In terms of the loss reporting, as I mentioned earlier, in the first OT we have those debugging URLs that can be declared in the bidding/scoring worklets that can be fetched afterwards. But at that point you don’t have the post-auction signals. [SB: you don’t know the winner or second price]. So if you’re going to convey the winning price and second price, those must be filled out later. We could put those into a URL and parameterize it. We’re thinking of solutions along those lines. In a previous call we discussed signals people might like, and 2nd price was one of those. For the winning ones, in the browser signals, for the losing ones, in the parameterization of the reporting URL.

SB: Does this need to be captured in an issue, or is it clearly understood from the Chrome side?

PJ: The explainer talks about this a bit, but doesn't propose an API. We may want to update the explainer to have more details in this area. It’s more a discussion of how the API works.

SB: Would appreciate it if the explainer had more details. Thank you.

Matt Menke comment in chat: [One problem here is if other bidders that the seller doesn't want to deal with are added to an auction, they potentially would get second price when they're rejected by the seller for being unknown bidders.]

SB: Responding to Matt’s comment. This sounds like a nuance as to who is able to get the second price? Maybe some of these nuances could be discussed on GH.

PJ: I believe Matt is referring to the fact that the caller of runAdAuction may not be the seller, it may be someone else. So this may be a leak to unintended parties.

SB: Isn’t the seller who invokes the runAddAuction?

MM: What if a party invokes the runAdAuction who isn’t the seller, and extends the config with a party who isn’t authorized? A spy.com bidder could get access to reporting by doing this.

SB: I think I understand. I believe what you’re referring to is the integrity of the AuctionConfig that the seller has provided. We discussed at length the security of AuctionConfig, here we refer to the integrity of it. Perhaps this could be tied to issue #119?

MK: I think that’s only sort of right. It sounds like you’re suggesting that the AuctionConfig has been meddled with, but what if some other party invokes runAdAuction with a manipulated AuctionConfig? I think this may relate to the Auction Outcomes reporting discussed earlier. One outcome could be a scoreAd result that is “you don’t belong here!” eg - “rejected from auction with prejudice” which should result in a reduction of info that party gets at reporting time.

BM: For the seller too, or just the rejected buyer?

MK: I think the seller in the auction should be able to learn that there was an auction in which they were designated as a seller but something very surprising happened to them. The Seller gets to see the entire AuctionConfig in their reporting.

SB: If the AuctionConfig came from the seller's domain (verified by the browser) the seller could be more certain that the AuctionConfig hasn’t been interfered with.

MK: But the AuctionConfig is in the pub page as noted earlier. and so there are no guarantees on how info moves around. Lots of third parties in the same environment mean the publisher page might be one giant cesspool of … cess.

SB: What if the AuctionConfig could be specified via a URL.

JM: Sounds like you made an interesting leap that the auction config is the entity being protected, this is shared with Jeff's comment before, maybe that is worth investigating on its own.

SB: Maybe getting the auction config into the auction securely is indeed its own goal, but would have latency implications.


### Other

Peiwen Hu: Hi, Peiwen from Chrome, working on the trusted server. We have just published an API on the trusted server. It won’t impact in any way the First Origin Trial but would love to see feedback. It’s in the same GH repo as the Fledge explainer.  https://github.com/WICG/turtledove/blob/main/FLEDGE_Key_Value_Server_API.md

MK: Thank you all for coming, look forward to seeing you all in the future.
