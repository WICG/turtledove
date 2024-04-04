# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 4pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


### NOTE FOR EUROPEAN PARTICIPANTS: US clocks have changed, so this meeting will be 1hr earlier than usual (i.e. 4pm Paris time, not the usual 5pm) for the rest of March.


# Next video-call meeting: Wednesday March 27, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	 



1. Michael Kleber (Google Privacy Sandbox)
2. Brian May (Dstillery)
3. Roni Gordon (Index Exchange)
4. Matt Menke (Google Chrome)
5. Harshad Mane (PubMatic)
6. David Dabbs (Epsilon)
7. David Eilertsen (Remerge)
8. Isaac Foster (MSFT Ads)
9. Sven May (Google Privacy Sandbox)
10. Wendell Baker (Yahoo)
11. Laurentiu Badea (OpenX)
12. Fabian Höring (Criteo)
13. Sarah Harris (Flashtalking) 
14. Anthony Yam (Flashtalking)
15. Paul Spadaccini (Flashtalking)
16. Hillary Slattery (IAB Tech Lab)
17. Garima Mimani ( Google Privacy Sandbox)
18. Paul Jensen (Google Privacy Sandbox)
19. Qingxin Wu (Google Privacy Sandbox)
20. Tim Taylor (Flashtalking)
21. Youssef Bourouphael (Google Privacy Sandbox)
22. Arthur Coleman (OnlineMatters/ThinkMedium)
23. Denvinn Magsino (Magnite)
24. Jacob Goldman (Google Ad Manager)
25. Pawel Ruchaj (Audigent)
26. Luckey Harpley (Remerge)
27. Chris Nachmias (Flashtalking)
28. Orr Bernstein (Google Privacy Sandbox)
29. Don Marti (Raptive)
30. Kenneth Kharma (OpenX)
31. Tamara Yaeger (BidSwitch)
32. Shafir Uddin (Raptive)
33. Stan Belov (Google Ad Manager)
34. Matt Wilson (NextRoll)
35. Alexandre Nderagakura
36. Taranjit Singh (Jivox)
37. Alex Peckham (Flashtalking)
38. Owen Ridolfi (Flashtalking)
39. Sathish Manickam (Google Privacy Sandbox)
40. Alonso Velasquez (Google Privacy Sandbox)
41. Abishai Gray (Google Privacy Sandbox)
42. Garrett McGrath (Magnite)
43. Andrew Pascoe (NextRoll)
44. David Tam (Relay42)
45. Sid Sahoo (Google Privacy Sandbox)
46. Daniel Rojas (Google Chrome)


## Note taker: Orr Bernstein


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here:



*   Isaac Foster:
    *   Improved Introspection: https://github.com/WICG/turtledove/issues/1043
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Roni Gordon
    *   https://github.com/WICG/turtledove/issues/1088 - renderSize in scoreAd’s browserSignals
*   Laurentiu Badea
    *   https://github.com/WICG/turtledove/issues/1090 - component seller reporting in multi-currency auctions - request to pass incomingBidInSellerCurrency in win events.
*   David Dabbs
    *   Speaking of Brian’s reference to _crowdsourcing_, what is the team’s take on incorporating _Discussions_ in this repo (assuming WICG has greenlit their use)?
    *   [Small clarifications / consistency for arbitrary metadata values, #1054](https://github.com/WICG/turtledove/pull/1054)
Caleb R approved. Look good to you, Paul?



# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  First one one was last week.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Note, because of the time change in Europe this coming weekend, the time of this meeting will be different in European time.

Also, we won’t have a meeting two weeks from now (April 10), because it’s at the same time as the PAT CG being held in person here in Cambridge.


# Notes


## Improved Introspection [Isaac Foster]: https://github.com/WICG/turtledove/issues/1043 (also related: https://github.com/WICG/turtledove/issues/871)



*   Isaac Foster
    *   As we’ve gotten deeper into doing production work, can be hard to figure out what’s going wrong when a configuration is somehow wrong.
    *   Once cited:
        *   Priority vector - where doing something wrong, had to dive into local SqlLite DB
        *   Delegation denials - test server doing a thing and it wasn’t quite right
        *   IG being included in the auction
        *   And similarly, Private Aggregation API
    *   We can get logs from our user space code, console.log
    *   We can run the debugger on those for now
    *   Being able to get some info/debug/whatever logging from Chromium space code
        *   E.g. Made this decision based on priority vector, received delegation file with missing header
        *   Some appear in the console, but not all of them
    *   Could be interesting to get something in line with what we get with console.log and console.error to line things up
    *   Better introspection on what decisions are being made under the hood with the configurations we’re using. One way I can see is to inline this in the console log.
*   Paul Jensen
    *   Lots of ways we can surface this.
    *   DevTools is a typical place.
    *   We could log every decision we make, which is kind of a lot.
    *   We want to make sure that different pieces that are critical decisions are visible in DevTools
    *   For delegation, if we’re issuing network requests, that those are visible in the Networks panel in DevTools
    *   And we’re trying to add more on the IG log in DevTools
*   Michael
    *   Right now, when we join an IG or load one for an auction, those are printed in the DevTools Interest Groups panel. May we can add more detail?
*   Paul
    *   We’ve tried to add that recently. Maks - who’s not on the call - published an extension, demonstrate what can be seen and surfaced in an extension
    *   As an engineer, the most perfect thing is a priority-sorted list of things everybody wants.
*   Isaac
    *   Quite certain that there are application logs for Chromium. I would certainly not expect it to be the case that all of those are in there. But whether it goes into an application log or the console log, I don’t know. What I’m picturing is some new logging function - similar to standard logging APIs - info/debug/trace - and show the appropriate amount of detail at each. Top-level, if we could have some way to get those insights; slightly lower level, something close to log4j, etc; and then at the next level, where that goes.
*   Paul
    *   Four levels of logging
        *   DevTools - pretty UI for seeing it
        *   Below that - Chrome Tracing - can go to chrome://tracing. Added a number of events there, can figure out what’s taking the most time. Can see when different processes executing the different buyer/seller scripts are doing different things. Useful often for performance stuff.
        *   Third more verbose level - throughout the Chromium code, different logging messages - INFO/DEBUG/VERBOSE - hard to tie back to JavaScript
        *   Chrome is Open Source. You can build it, hook up your debugger.
    *   Sounds like you’re asking for something between DevTools and Tracing
*   Michael
    *   Sounds like Tracing is maybe the answer
*   Paul
    *   The more we add, the slower things get. Tracing doesn’t add much when it’s off; when you turn it on, it slows things down.
*   Issac
    *   Opening Chrome in a debugger - though it sounds like a great day to me - as more developers get involved, might not be a great solution.
*   Paul
    *   Issac - you had a list in your comment; anyone else, if you had anything else you had problems with - delegation, dot-products, etc.
*   Brian May
    *   Might be a good idea to crowd-source a list so that we can leverage the things people would get the most value from quickly.
*   Michael
    *   GitHub is a great place for us to collect information on what’s most useful and what we should devote our engineering resources on.
*   Brian
    *   Reuse this issue or create a new issue?
*   Paul
    *   Reuse is good. Maybe put each request into its own comment and people can thumbs-up those that they agree with.
*   Brian
    *   Is this going to cross-pollinate with other Privacy Sandbox efforts like Attribution Reporting API and Topics?
*   Michael
    *   There are no ARA folks in the room right now, so different set of people to answer. Answer will be similar - the more information we can get on prioritization of asks, the easier it will be to staff the right efforts - but I can’t speak for them.
*   Issac
    *   Two questions
        *   What does the logging framework look like and where does it go?
        *   And what do we log?
    *   Once we’ve solved the first one, then is it fair to say that this answer would apply to other Privacy Sandbox areas?
*   Michael
    *   Thankfully, this is a breath of fresh air - no privacy implications to knowing what’s happening in your own browser.


## renderSize in scoreAd’s browserSignals [Roni Gordon] https://github.com/WICG/turtledove/issues/1088


*   Roni Gordon
    *   Surprised not to see renderSize in the browserSignals
    *   Can’t see anything in the spec or explainer
*   Paul
    *   I tagged Garrett in that issue this morning; he would know more.
    *   renderSize is coming from the auctionConfig.
    *   Everything there should be surfaced in scoreAd.
*   Michael
    *   Things are surfaced in scoreAd if we explicitly make it so.
    *   But it doesn’t seem to 
    *   When we first created the flow, we assumed that sizes would be part of the blob that would be in scoreAd. If you adopt your own convention and make it part of your auction signals, it would be there. But now we’ve added it to the auction config, and we may have accidentally omitted propagating it to scoreAd.
*   David
    *   In this same issue, the current state of Protected Audience supports an IG owner adding all the size metadata. However, it doesn’t appear in the state of the IG that the buyer receives in generateBid(). We would love to be able to use the size metadata facility that we’re not obliged to use yet, but would like to use it.
*   Michael
    *   Difficulty, somewhat subtle.
    *   Right now, when we run the generateBid function, we can’t actually give you all the size data, because it would be an information leak. The reason is that when we run generateBid the first time, we can tell you what you put in the IG. But if you return an bid for an ad that’s not yet k-anonymous, then we run the auction a second time, and that second time, we filter out what is/is not over the k-anon threshold. We can’t just tell you what’s in your original IG at that point; we’d have to figure out how k-anonymity would interact. Not actually sure we could do that; if there’s an information leakage problem.
*   David
    *   We want to feature-forward. We’ll need to use the k-anonymous with sizes.
*   Michael
    *   The MS Edge version does not do this - where it runs the auction and run it again when it gets a non-k-anomyous bid. They allow generateBid to return multiple bids, and if none of those are k-anonymous, then it just doesn’t bid. If we use an approach like this
*   Isaac
    *   For multiple ads, if we’re doing server-side, interesting things with queues, if there’s a cache miss, if it were to win, would still get a +1. The odds for there to be a disaster would be low.
*   Michael
    *   Edge version also doesn’t have all of the renderURLs explicitly listed ahead of time. Things possible with server-side.
*   Isaac
    *   Hypothetically, can do the same thing on the client side, just have more misses. 
*   Michael
    *   Agree -there’s interesting stuff to talk about here. Might cause changes in the future if we discover that the tradeoffs in the Edge version are better than the tradeoffs we’ve made in the Chrome version.
    *   Thank you for bringing this up; we’ll explore this.
*   David
    *   In no way/shape/form should my request on the IG side slow down the implementation of how we get the size metadata.
*   Michael
    *   Agree. Getting it into scoreAd should happen. Getting it into bidding time requires more thought.
*   Paul
    *   Wanted to note that we have brought up the change to get generateBid to return multiple bids. Some may have noticed that this change is going into Chrome now, with Chrome feature and Intent. There’s a pull request in the explainer if you want to explore it.


## component seller reporting in multi-currency auctions - request to pass incomingBidInSellerCurrency in win events [Laurentiu Badea] https://github.com/WICG/turtledove/issues/1090



*   Laurentiu
    *   As a component seller, we need to be able to get original bid and modified bid in a known currency so we can calculate the amount subtracted by scoreAd 
    *   There are configurations where we cannot get one of the values - the original bid. Without this value we can’t calculate anything else.
    *   The table in the issue shows different currency configurations - sellerCurrency, perBuyerCurrency for that buyer, and bid currency from generateBid.
    *   perBuyerCurrency is not going to be set most of the time. The currency they bid with is not present in the igbid response. So we get a unitless value for their bid in reportResult. “The buyer bid 20 something; the winning bid was 0.75 dollars.” This information is available when the currencies match.
    *   This happens when the seller doesn’t set the currency in perBuyerCurrency.
    *   When the buyer bids in a different currency, we have no way to figure out the amount.
    *   Suggestions: add incomingBidInSellerCurrency to the reportResult data, either by itself ore place the bid when present.
*   Paul
    *   The reason for whether the currency is exposed or not exposed is whether it would reveal cross-site information.
    *   Things that are declared in the auction config - ahead of time - even things like per-buyer currencies - we can expose those things because they’re contextual data, and all contextual data is available in reportWin and reportResult.
    *   But things that depend on particular calculations in generateBid, we don’t know that those aren’t exposing cross-site identity.
    *   So a lot of which currencies get exposed is because of that.
*   Laurentiu
    *   The middle rows of the table are the most common
    *   The buyer generates the bid in USD or some other currency, which is not known at the time the auction config is created
*   Michael
    *   For a information leakage point of view, the only thing we can do.
    *   The auction config says, I have no idea what currency the bid is going to be.
    *   And then generateBid can choose whatever bid.
    *   If we propagated the information about what currency the bidder chose, it would be an open channel from generateBid - which knows cross-site identity - to scoreAd - which isn’t supposed to.
    *   300 different currencies worth of information.
    *   The design we arrived at when we talked about it here is to pass the information about what currency is used as part of the contextual information.
*   Laurentiu
    *   You meant reportWin and reportResult, right?
*   Michael
    *   Yes, that’s what’s not supposed to get that extra information.
*   Laurentiu
    *   We can’t really do billing if we don’t know these two values. In the first row, we see the bid was 20, the modified bid was 15, but a value with no unit is not useful for us.
    *   There are a few other site edge cases like this. I noted in the table the currencies scoreAd gets, and sometimes scoreAd doesn’t know the currency but reportResult does. This happens when the currency is configured in perBuyerCurrency, but generateBid doesn’t return a specific currency.
*   Stan
    *   Why wouldn’t you require your buyers to determine the currency ahead of time and make it available through perBuyerCurrency
*   Laurentiu
    *   Because you don’t always know what their currency would be ahead of time.
    *   If that’s the way that everyone should do, then we’re all set.
*   Michael
    *   Yes, the way that the API should be used is, the buyer should be told ahead of time what currency they should use. The fact that there’s a special field for the currency is a check that they’re behaving in the way they previously agreed to behave - a checksum - so that both the browser and the seller have a way to check that nothing has gone wrong, so that they’re not intending to e.g. bid in Japanese Yen and are instead bidding in USD. We added the currency as a checksum, but not as a new and potentially dangerous information channel.
*   Laurentiu
    *   If the top-level seller doesn’t specify the currency, then this leads to none of the component sellers get the currency in the report.
    *   If the component seller sets the currency USD, and the buyer sets the currency USD, but the top-level seller doesn’t set a currency, then we get question marks.
*   Stan
    *   Could you document that use case somewhere - the relationship with the top-level seller, and how it impacts the reporting of the component seller.
*   Laurentiu
    *   If top-level seller sets the currency, then the currency of the top-level seller should be propagated to the buyer.
*   Stan
    *   If it’s not propagated now, could be propagated.
*   Laurentiu
    *   The issue is that if the buyer doesn’t declare the currency in their igbuyer response, but we set it in perBuyerSignals, then that currency is not sent to generateBid
*   Stan
    *   Could use auction signals for that?
*   Michael
    *   perBuyerSignals should be a way to communicate that agreement.
*   David Dabbs
    *   But the challenge is that the way that perBuyerSignals is used - it’s some opaque JavaScript - but it’s 
*   David Tam
    *   Quite an important issue for the buyer. When we bill the client, we want to make sure that we’re paying in the right currency. We don’t usually see the signals from the top-level seller; we see them from the component sellers.
*   Michael
    *   perBuyerCurrencies in the explainer:
    *   https://github.com/WICG/turtledove/blob/main/FLEDGE.md#36-currency-checking
*   David Tam
    *   Have no way to tell what’s being sold by the seller.
*   Michael
    *   Still unsure which choice being made in which space is not being communicated.
    *   If the choice of what currency is known ahead of time, before the auction starts, then everybody should be able to communicate to everybody else what they expect. If everybody bids and modifies bids and attaches currency labels to all of the bids, then the browser can verify that what’s expected is what happened.
    *   I haven’t been able to follow the discussion well enough to determine if there’s a bug we need to fix, if there’s any case where the expectation is not being communicated.
*   Laurentiu
    *   So, for my ask, there’s no plan to expose the incomingBidInSellerCurrency currency, and the solution should be to demand or impose the currencies in perBuyerCurrency, propagate them in some way to buyers, and do not allow different currencies. Even though the way adtech works currently is that DSPs can choose to bid in other currencies and SSPs will perform the necessary exchanges 
*   Michael
    *   If there’s an expectation on currency established via perBuyerCurrency, and the bidder bids without a currency because it’s an old bidding script that doesn’t yet use currency, then you should go to the buyer and ask them to express the currency.
    *   If there’s no expectation on currency established via perBuyerCurrency, and the bidder bids with a currency specified, then that currency is not shared with the reporting functions
