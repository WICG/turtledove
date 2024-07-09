
# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday June 26, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	 



1. Paul Jensen (Google Privacy Sandbox)
2. Sven May (Google Privacy Sandbox)
3. Brian May (Dstillery)
4. David Dabbs (Epsilon)
5. Ricardo Bentin (Media.net)
6. Matt Davies (Bidswitch | Criteo)
7. Roni Gordon (Index Exchange)
8. Youssef Bourouphael (Google Privacy Sandbox)
9. Garrett McGrath (Magnite)
10. Jacob Goldman (Google Ad Manager)
11. Matt Kendall (Index Exchange)
12. Aymeric Le Corre (Lucead)
13. Matt Menke (Google Chrome)
14. Alonso Velasquez (Google Privacy Sandbox)
15. Sathish Manickam (Google Privacy Sandbox)
16. Orr Bernstein (Google Privacy Sandbox)
17. David Tam (Relay42)
18. Yanay Zimran (Start.io)
19. Denvinn Magsino (Magnite)
20. Gianni Campion (Google ads)
21. Arthur Coleman (IDPrivacy)
22. Laurentiu Badea (OpenX)
23. Abishai Gray (Google Privacy Sandbox)
24. Lydon, Jason (FT)
25. Becky Hatley (Flashtalking)
26. Anthony Yam (Flashtalking)
27. Owen Ridolfi (Flashtalking)
28. Rickey Davis (Flashtalking)
29. Itay Sharfi (Google)
30. Manny Isu (Google Privacy Sandbox)
31. Tamara Yaeger (BidSwitch)
32. Warren Fernandes (Media.net)
33. Koji Ota(CyberAgent)
34. Maybelline Boon (Google Privacy Sandbox)
35. Matthew Atkinson (Samsung)
36. Andrew Pascoe (NextRoll)
37. Alex Peckham (Flashtalking)
38. Tal Bar Zvi (Taboola)
39. Luckey Harpley (Remerge)


## Note taker: Orr Bernstein	


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here:



*   Isaac Foster:
    *   Trusted Scoring Signals for Financials: Via [Comment](https://github.com/WICG/turtledove/issues/824#issuecomment-2198376904) on [Roni Original](https://github.com/WICG/turtledove/issues/824) 
    *   Brief revisit the “coarse information sharing” thing, we had talked about setting up time but never did, all got too busy…can even answer here
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736 \

*   Alexander Tretyakov (Google Privacy Sandbox)
    *   Hybrid protocol – modify v2 protocol so that a single request can go to both BYOS and TKV servers. This is meant to be a temporary measure, and when the enforcement starts only TKV mode will be supported. The benefits of the hybrid protocol are:
        *   enables piecemeal migration. AdTechs can migrate pieces of their logic and asses the cost/latency and other parameters before fully committing
        *   allows keeping parts of the logic that are not currently supported by TKV in BYOS, while moving the rest to TKV.
*   Roni Gordon
    *   We’re observing **biddingDurationMsec** values in production well above the default 50ms timeout (i.e. with the default auctionConfig values) – some as high as 322ms – can you think of any explanation for this behaviour?
        *   Haven’t yet logged an GH issue since we haven’t been able to reproduce in controlled tests, where the time spent in generateBid() seems to match the fDO QSP
    *   Can we discuss the implications of the tweaks to the deals proposal?
        *   https://github.com/WICG/turtledove/issues/873
*   Warren Fernandes
    *   Continued discussion on the addition of analytics entities to PA auctions: https://github.com/WICG/turtledove/issues/1115
    *   New mechanism for seller opt-in proposed.
*   David Tam
    *   There are 3 parties that can create IGs, the buyer (advertiser), the buyer’s delegated tech partner and the publisher.  I would like to understand how a buyer would be able to bid for publisher curated IGs - https://github.com/WICG/turtledove/issues/1196 
*   Gianni Campion (briefly discuss 0 ads API for joinIG
    *   https://github.com/WICG/turtledove/issues/1191)
*   Yao Xiao
    *    discuss header api for joinIG https://github.com/WICG/turtledove/issues/82


# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

**Join Privacy Sandbox Developer Webinar: Protected Audience API Reporting**

The Google Privacy Sandbox team will be hosting our next series of webinars on the Protected Audience API. This is the fourth series of webinars covering the Protected Audience API and in this session, we will continue from the previous session and learn more about reporting. The first **Americas friendly session** is happening on** June 25th 3-4 pm ET**. A second **EMEA friendly session** is happening **June 26th 12-1 pm GMT**. 

To join, please register below: 



*   AMER-friendly: [Register Here](https://rsvp.withgoogle.com/events/protected-audience-webinar-4-reporting2-amer)
*   EMEA-friendly: [Register Here](https://rsvp.withgoogle.com/events/protected-audience-webinar-4-reporting2-emea)


# Notes


## Roni Gordon: We’re observing biddingDurationMsec values in production well above the default 50ms timeout (i.e. with the default auctionConfig values) – some as high as 322ms – can you think of any explanation for this behaviour?



*   Roni Gordon
    *   Have observed. Bidding_duration_msec is the time that generateBid spends? Having trouble reconciling how this can be significantly higher than what we would see based on the timeout.
*   Paul Jensen
    *   Asked the PA team how the bidding_duration_msec could be higher than the timeout. Largely historical reasons. The way it works is the perBuyerTimeout - which defaults to 50 ms - added way back in the beginning. In the implementation/spec, you can see that that one is tied to how much time it takes that one to execute the generateBid JavaScript, does not include the time for setup - create a JS execution context, initialize 7000 built-ins in JavaScript, then move those into the JS context. Can add a lot more time; these are all part of bidding_duration_msec because it’s all “the cost of generating the bid”, but for the timeout, we only enforce that on the generateBid call in JS. All measured using different timers, browser is running in a very multi-threaded environment, some of these threads are getting preempted at random points. This is the reason why there’s a bid difference between bidding_duration_msec, which includes the setup, and the timeout, which doesn’t.
*   Brian May
    *   Timeout is the max amount of time that the code can run for, but the bidding duration is how long it took including a bunch of setup. Not based on the same thing.
*   Paul
    *   If you use a debugger - attach via Chrome Developer Tools - the timeout would pause, because otherwise the timeout would always be exceeded because of a breakpoint; but the bidding duration msec is just a timer, so it would report the time even when it was paused at that breakpoint.
    *   While on the subject of per-buyer-timeouts, it was one of the first things we added to PA. We’ve sort of moved away from that to some degree. It made the browser opinionated on how many IGs you should have and how fast each of them should be. Wanted to make the browser less opinionated - maybe you have one IG that’s particularly important and you want to spend more time calculating the bid there because maybe it’s a bigger bid. So moving away from per-buyer timeout, and towards cumulative IG - controls the whole of all the interest groups, how long they can spend in aggregate. There’s a devrel website that describes the differences between these, and how the cumulative timeout gives people a bit more freedom.
*   Brian (by chat)
    *   https://developers.google.com/privacy-sandbox/relevance/protected-audience-api/latency
*   Roni
    *   I thought this was a measure of the time spent on device in the generate bid function alone – seems like it also includes all the initialization pre-work required to get to execution of generateBid. It would be great to elaborate on this in the spec on what’s it’s measuring.
*   David Dabbs
    *   In the spec, it says the reporting timeout is clamped to 5 seconds. Is there something similar for the per-buyer cumulative timeout? Seems like it should be mentioned in the explainer.
*   Matt Menke (by chat)
    *   There's no max for cumulative timeout
    *   By default there is no timeout
*   Paul
    *   The difference in incentivization is perhaps the reason why one is capped and one is not capped.
*   Matt
    *   For reporting, it’s a background thing. You will typically navigate to a different page, maybe by clicking on an ad. Don’t want the reporting to keep on going for an excessively long time.


## https://github.com/WICG/turtledove/issues/873



*   Roni Gordon
    *   Trying to ascertain whether or not its possible to have a dealID visible in the reporting.  In other words, can you have a ‘split-brain’ situation where scoreAd seems dealID bids but reportResult doesn’t?
*   Paul Jensen
    *   To refresh folks, the proposal that Roni put up on March 13 - issue 873 - taking an existing field that’s part of the ad part of the interest group - existing field is called buyerAndSellerReportingID - a string - and making it into an array of strings, and allowing the buyer to pick one of them and to have that chosen deal passed along to scoreAd and reportResult. So, as Roni noted, the same thing is available at scoreAd time - to assess the score if there’s some discount - and at reportingTime - so that we can account for how the desirability score was arrived at, and to keep track of who’s buying with a particular deal, keep track of that. Like buyerAndSellerReportID, it will have a k-anonymity reqiuirement on it. Could appear in scoreAd, but would only be visible for reporting functions if it’s k-anonymous with the URL and interest group owner. Part of Leeron’s proposal was to give buyers a way to say that the buyerAndSellerReportingID being passed through to reporting is required. You an imagine, if it’s not k-anonymous, without the deal ID, the bid might not make sense at a later time when the reports are benign processed, so we expose this boolean to the buyer to only report to scoring if it’s k-anonymous.
*   Roni Gordon
    *   The way k-anon works for everything else - you have to end up with the seller choosing to select the ad in order for the k-anon to be ticked; otherwise, it never starts. What I wanted to confirm is that the same thing is happening here; you can score a deal bid with all the required parameters.
*   Paul
    *   We give the buyer the ability to throw away a bid if the deal ID is not k-anon, you want to give the seller the same. We could expose a boolean, and seller can require that boolean to always to be true, that the deal id has to be k-anon to be used.
*   Roni
    *   Yes, but also, if a seller rejects deal bids that are k-anon, it will never be k-anon – how so would that dealID ever reach the threshold?
*   Brian
    *   Can we do this retroactively, at the point at which k-anon is reached, that we can report a deal ID that was not previously reported because it wasn’t yet k-anon.
*   Matt
    *   Can’t convey whether it’s k-anon, or you could game k-anon. Need some way to say, if it’s not k-anon, don’t let it win. Specifically if it’s not k-anon because of the deal ID, but would be k-anon.
*   Paul
    *   What if we allow everything to be scored, even if it’s not k-anonymous. Two winners - the k-anonymous winner and the non-k-anonymous winner. Sometimes you only have one or the other. For the non-k-anonymous winner, we could increment the k-anonymity count, and we’ll do it for the k-anonymous winner as well. This is what Leeron had planned for deal support, was at the bottom of his proposal. What I had suggested earlier of exposing that boolean to scoreAd, could still increment their k-count.
*   Roni
    *   If it doesn’t meet the threshold, it’s discarded from the auction. Optional is whether or not it gets to the seller or not, if it gets to the seller, score it as normal. Just wanted to make sure that this is the behavior we’re getting, and not that reportResult is getting the winner without deal ID.
*   Paul
    *   Yes, we could always expose the boolean. We don’t want to expose whether it’s k-anon early to avoid gaming. Want to know the highest non-k-anonymous winner so that we can increment its k-anonymity count. So I think we can expose that boolean to sellers to ensure that they can choose to only allow winners that would be reported correctly.
*   Roni
    *   Seems like something we’d always want, but I’ll take the boolean in scoreAd, because for forDebugOnly, it’ll help make it obvious what’s happening. Absent k-anon enforcement, as many scoring bids I score is how many bids I submit to the upstream seller. As soon as we include k-anon, that number could be anything. The scoring counts are way off. It would be nice to see that if I submit a non-desirability score of non-zero.
*   Paul
    *   I think we have a private aggregation on whether a bid is not used because of k-anon.
*   Roni
    *   The current proposal with tweaks?
*   Paul
    *   We already have the boolean for buyers, so what we’d change is to expose that boolean to the seller so that the seller could reject any bid from an IG that doesn’t have that boolean set, since it can’t guarantee that this bid would be correctly reported. 
*   Brian May
    *   What does the boolean say?
*   Paul
    *   The boolean is provided by buyer and says that they want their bid to be thrown away if their bid - with the buyerAndSellerReportingID - is not k-anonymous. If it’s false, it can continue onwards.
*   Brian
    *   Can we simplify that it’s reportable to all parties? Boolean that says that this thing is going to be reportable to everybody.
*   Paul
    *   I think this is for everybody.
*   Brian
    *   Language there that this is about deals.
*   Paul
    *   Not sure I totally understand, but I think we’re aligned. Maybe if you could put it on the issue, I could better understand. And I’ll try to add more detail on the issue.
*   Giani Campion
    *   Where is the boolean available? Does it cover buyerReportingID as well?
*   Paul
    *   Indicates that the bid should be thrown away if the reporting ID is not k-anonymous. It covers both buyerReportingID and buyerAndSellerReportingID.


## Continued discussion on the addition of analytics entities to PA auctions: https://github.com/WICG/turtledove/issues/1115 

New mechanism for seller opt-in proposed



*   Warren Fernandes
    *   Following up on the conversation from last time; request for some sort of mechanism for both the seller to be aware that they had analytics available. Proposed header that could be set on the call to the seller decision logic file that included a list of analytics entities that were present and another head they could respond with. Additional flag that the sellers could host on the well-known URL to indicate whether they had opted out of being observed by the analytics entity.
*   Paul
    *   Sounds like you made the updates we’d talked about before. Did you want to discuss a part of that, or is this a request to take another look?
*   Warren
    *   The latter. Is this good to go, want some feedback. Not really sure how to proceed at this point, so any feedback on that would be great.
*   Paul
    *   I can take another look at that. May be less available in the next couple of weeks, but will try to take a look at it in the coming month, or to find someone else who can take a look sooner.


## There are 3 parties that can create IGs, the buyer (advertiser), the buyer’s delegated tech partner and the publisher.  I would like to understand how a buyer would be able to bid for publisher curated IGs - https://github.com/WICG/turtledove/issues/1196



*   David Tam
    *   As I understand it, there are three parties that can create an IG - buyer, delegated buyer, or publisher. For publisher, I assume that the publisher wants to monetize an IG and sell it to interested buyers. How would a buyer actually buy an IG?
*   Alonso
    *   We don’t have my product peers on the call who have been thinking about this from the publisher side of things, but it’s a known request. I do see issue 1196, we’ll tag it for follow-up here. We’ll follow up.
*    David
    *   Is it right that a publisher might want to sell an IG to a DSP?
*   Alonso
    *   You’re showing a new use case. If you have any hypotheses, please feel free to post them to 1196. 
    *   In this use case, an audience, by the publisher - what’s the scope of that audience? A little more clarify on the use case helps us. A lot of what we do is to ensure that we’re not creating  cross-site profiles. If you have those ideas, please add them to 1196.
*   Matt Davies (by chat)
    *   what about other 3rd party entities?
    *   Data providers, Pubs, other entities
*   Brian May
    *   Original FLEDGE proposal was that someone else could create an IG and cede ownership. This sounds like a different use case.
*   Alonso
    *   Would like to better understand the delta in these use cases.
*   Paul
    *   The other thing that would help us understand the use case better is understanding how the seller or delegation of the IG would work. Today, it’s possible for whoever creates the IG to maintain control, still do things on another party’s behalf, could still grow/shrink/control the bidding directly. Sounds like something less tightly coupled; many of the behaviors are controlled by the new owner.
*   David
    *   Yes, if you’ve got multiple publishers, want to basically create a single IG and monetize that. Another use case is to funnel activity through a PA audience.
*   Paul
    *   PA doesn’t limit the sites that you’re joining from. Could have a third party iframe that joins from all different sites. The publishers could kind of pick that, join IGs from different sites.
*   Brian
    *   Is this about giving publishers a mean of getting credit when an iG they created is monetized by somebody?
*   David
    *   Essentially, yes. It’s an interesting use case where we can buy publisher-created interest groups. Some way of informing the buyer that this is the IG, would you like to buy it?
*   Brian
    *   Maybe a different direction - could inform  the creator of the IG that somebody has bought against it.
*   David
    *   Because the seller initiated the auction, from my view, it just needs to be added into the auction confirm.
*   Matt Davies
    *   It could be useful for publishers, but what about other types of ad techs? The second you start adding in third parties to the mix, e.g. BidSwitch, they’re an interested party in the mix.
*   Brian
    *   It’s not about the creation; it’s about ownership once created.
*   Brian
    *   Bidding logic, reporting API callouts - all of that, who’s getting those calls.
*   Alonso
    *   Issue [1196](https://github.com/WICG/turtledove/issues/1196) is timely to galvanize the sell-side, looking into buy-side as well which is[ issue 1028](https://github.com/WICG/turtledove/issues/1028)  If you have thoughts about that, I’d love to hear your thoughts in a future WICG.
