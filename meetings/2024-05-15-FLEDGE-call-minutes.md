# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday May 15, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	 



1. Paul Jensen (Google Privacy Sandbox)
2. Brian May (Dstillery)
3. Sven May (Google Privacy Sandbox)
4. Orr Bernstein (Google Privacy Sandbox)
5. Wojciech Biały (Wirtualna Polska Media)
6. David Eilertsen (Remerge)
7. Andrew Pascoe (NextRoll)
8. Harshad Mane (PubMatic
9. Shafir Uddin (Raptive)
10. Patrick McCann (Raptive)
11. Alex Cone (Google Privacy Sandbox)
12. Aymeric Le Corre (Lucead)
13. Wendell Baker (Yahoo)
14. Antoine Niek (Optable)
15. Garrett McGrath (Magnite)
16. Jacob Goldman (Google Ad Manager)
17. Yanay Zimran (Start.io)
18. Russ Hamilton (Google Privacy Sandbox)
19. Abishai Gray (Google Privacy Sandbox)
20. David Tam (Relay42)
21. Laura Morinigo (Samsung)
22. Brian Schmidt (OpenX)
23. Warren Fernandes (Media.net)
24. Matt Davies (Criteo | Bidswitch)
25. Sathish Manickam (Google Privacy Sandbox)
26. Alexandre Nderagakura\*
27. Elmostapha BEL JEBBAR (Lucead)
28. Michael Kleber (Google Privacy Sandbox) (second half only)
29. Alonso Velasquez(Google Privacy Sandbox)


## Note taker: Orr Bernstein


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here:



*   Isaac Foster:
    *   Patch’y Updates When on Joining Origin: https://github.com/WICG/turtledove/issues/1162
    *   Attestation Process  https://github.com/privacysandbox/attestation/issues/53 
    *   Brief revisit the “coarse information sharing” thing, we had talked about setting up time but never did, all got too busy…can even answer here
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Warren Fernandes
    *   Tweak to the signalValue returned by aggregate reporting functions to support cross-component auction loss reporting: https://github.com/WICG/turtledove/issues/1161
    *   Scope reductions to the analytics entity proposal discussed last week: https://github.com/WICG/turtledove/issues/1115 \

*   Patrick McCann
    *   InterestGroupBuyers as promise: https://github.com/WICG/turtledove/issues/1093 


# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.


# Notes


## Warren Fernandes: Scope reductions to the analytics entity proposal discussed last week: https://github.com/WICG/turtledove/issues/1115 



*   Warren Fernandes
    *   Linked a version of the doc that’s scoped down, has just the aggregate-only version of the prior proposal. https://docs.google.com/document/d/1j1OZlrU1OTMmUaqt8OzidZzNOHsJezAfTy2athIij0I/edit#heading=h.xhlrw7f4erxq 
    *   The prior proposal - two new functions
        *   Event level reporting for auctions
        *   Captures other data at an aggregate level
    *   The new proposal has just the second function
    *   First table suggests comparison to prebid analytics
*   Paul Jensen
    *   List of events, aggregated
    *   What’s are the metrics you’re interested in learning about at that time?
*   Warren
    *   Function receives these events, can do analytics reporting using the standard aggregated reporting mechanism
*   Paul
    *   So, there’s a piece of JavaScript that receives one of these for each event type.
*   Warren
    *   Suggested implementation section in the document.
    *   New analytics.js file
    *   /.well-known/pa-api-analytics/permissions would include the list of analytics to be reported by the auction
    *   The entity provides its own JS. It has one function in it: reportOutcome
    *   A couple of cases
        *   Could have a winner, in which the structure referenced in the doc is provided.
    *   Uses the standard histogram function to send these over to private aggregated service
    *   Each component seller’s winning bid is provided to the reportOutcome function, both the winning component seller and all other component sellers
*   Paul
    *   The analytics script would only be able to exfiltrate using the aggregation API?
*   Warren
    *   Correct.
*   Paul
    *   Seems very seller-specific
*   Patrick McCann
    *   This would be helpful for us, but seems like it maybe even has too much detail.
*   Paul
    *   Some of this data is very internal too; for example, desirability may only be meaningful to the seller. Some of this is already available through aggregation - the browser already exports it to private aggregation. 
*   Patrick
    *   But the publisher can’t report it.
*   Paul
    *   Say someone can already run JavaScript; a lot of this is information they can already report back.
*   Warren
    *   Also reports information on bids that didn’t happen, for example, “noBidReason: timeout”. When an entity doesn’t provide permission to provide these analytics, would be conveyed in the reportOutcome call.
*   Brian May
    *   Seems worthwhile to prioritize aggregates by what’s most valuable. 
    *   What keys do you propose?
*   Warren
    *   Not proposing a specific set of keys, just a mechanism to get that data to new entities, who can then report it however they want.
*   Patrick
    *   MVP would be event counting. Just knowing how often each entity was activated.
*   Paul
    *   By activated, do you mean bidding/scoring, or winning?
*   Patrick
    *   Both. An analytics provider could run an aggregation API in a TEE and also count how often various partners activated and won.
*   Wojciech Bialy
    *   Looking at those events from publisher POV, what’s important to us is that this provides some mechanism for cost reporting. Winning ask, and the ones that started to deliver. “noBidReason: timeout” is possible to discern for top-level seller. If possible, would like to see the difference between bid won and bid delivered. Cannot discern what cases the auction won but the ad was not attached to the page.
    *   Do you see this being called once with all of the events from the auction, or once per event?
*   Warren
    *   Proposing that it’s called once, so that it can be aggregated. Also, if it’s firing in a worklet, avoid having to fire up a new process.
*   Matt Davies
    *   Excellent proposal. Would really help us. Slightly different use case. Other side of the equation from the publisher - entity in the middle - but also need this same information.
*   Paul
    *   How would you envision connecting to this flow? Would you be one of the analytics providers that publishers would list?
*   Matt
    *   Don’t know; separate URL. Maybe Bidswitch could indicate that we were involved in this auction on the RTB side. Figuring out at the moment how to do this with network calls and that sort of stuff, but those loopholes will be closed very quickly. Would also be useful to others who are not integral to the component auction. At this stage we seem to be locked out because we can’t run a component auction, and we’re not the top-level seller, so there’s nowhere else to go.
*   Warren
    *   One of the issues as an SSP with implementing Bidswitch’s proposal is that we can’t make multiple calls to the reporting. With this, would be far more straightforward as an SSP to support them directly.
*   Paul
    *   There’s potentially often a lot more losing bids than winning bids - orders of magnitude more. Might get expensive to run analytics scripts for each person who participated.
*   Warren
    *   reportResult might help.
*   Paul
    *   Only for winning bids.
*   Matt
    *   Would be willing to start from winning bids and impressions alone, can revisit lost bids later on.
*   Paul
    *   When you asked about multiple calls to sendReport, do you mean more data to the same origin, or calls for different origins?
*   Warren
    *   Neither would work.
*   Brian May
    *   If you send analytics for every bid
*   Paul
    *   Sounds a lot more expensive to make a lot of calls.
*   Brian May
    *   Didn’t mean to invoke this function for every bid, but rather aggregate that information for the one call.
*   Michael Kleber
    *   Is there some version of this in which information is written into SharedStorage, and then some separate process runs aggregation and sends an aggregate report? On-device coalescing of lots of different losing bids. Avoids the problem of flooding with lots of information from the same browser. And allows for some opt-in for those who want to be notified.
*   Warren
    *   I’ve thought of this, don’t know if from a functional standpoint, if this is very different. This would work, though.
*   Patrick
    *   Agree with Warren that this would work for us too.
*   Paul
    *   The difference seems to be the .well-known file that provides permissions, could be the SharedStorage option.
*   Michael
    *   Publisher .well-known file could tell the browser which entities’ information we should record, and which JavaScript we should run for reporting.
*   Paul
    *   A .well-known file would mean making two fetches for every document, may be a performance issue. But something in the HTML or the auction config that says, go fetch from the .well-known instead of issuing it on lots of sites that have no analytics may be more performant.
*   Patrick
    *   Publisher won’t necessarily know that Bidswitch should be included in their auction config. Maybe it’s a different proposal, but the shared storage write proposal maybe solves the Bidswitch use case and publisher use case, while calling something with a reportResult use case may not solve the Bidswitch use case.
*   Matt
    *   Yes, would need some way of declaring that Bidswitch is involved.
*   Paul
    *   Often more performant to the browser if we can use a declarative form. For example, if we could say, report to aggregation if this event happens.
*   Matt
    *   Something like that would be very useful. Do need to track the impression events. We should contribute to this histogram if something like this happens.
*   Paul
    *   Tricky thing is to know how some entity X wants that, and not somebody else asserts that entity X wants that.
*   Warren
    *   Wondering if you had thoughts of a mechanism for how publishers can indicate which entities should have access to this information in shared storage? Unlikely that a publisher has relationship with Bidswitch.
*   Michael
    *   People who come to the data gathering role not because the publisher brings them along but because they’re involved with some other entity in the auction would have some more limited visibility to the auction. Would presume would have visibility scoped to the entity they have the relationship with. For example, someone who comes in via association into a buyer in the auction would have visibility to that buyer’s bids but not other buyers’.
*   Brian May
    *   Are you suggesting that a subset of data is available to downstream partners? Limited to visibility of partner who brought them to the table, and some general signals available to everyone in the auction.
*   Patrick
    *   Proposed future state, right?
*   Michael
    *   Yes, we don’t have information available to publisher today, that goes through top-level seller.
*   Paul
    *   Makes me think about iframes, how they prevent leaking information out of that frame. Aggregation service to be the medium there.
*   Patrick
    *   Michael’s proposal doesn’t call to aggregation; instead, writes to Shared Storage, and then an entity can call to aggregation.
*   Michael
    *   Two-sided opt-in kind of situation to ensure that nobody’s information is given away.
*   Warren
    *   Any reason we currently can’t do multiple calls to some kind of sendReport option, as a seller?
*   Paul
    *   Not sure we’ve heard a lot of ask for that before. Is it about sending more data, or sending data to more people?
*   Warren
    *   About sending to more people in your supply chain, if you want.
*   Paul
    *   May use more device upload bandwidth, versus using more server-to-server bandwidth, which is cheaper.
*   Warren
    *   That works also.
*   Michael
    *   This sounds like a different conversation; we were talking about aggregated reporting, and now we’re talking about event-level reporting. For buyers, we already support what you’re asking for buyers, who can indicate other domains who can get win type and events-in-the-creative type reports. Don’t have something like that on the sell-side; the solution we have right now is focused on the buy-side.
*   Patrick
    *   So, if buyer is connected to A directly and B indirectly, how would they convey to send to B?
*   Paul
    *   Could include that in their bid directly.
    *   It sounds like the next steps are, think about what an efficient registration mechanism would be for this. .well-known file, HTTP response header, HTML/JavaScript. What has good ergonomics for publishers? We’ve heard HTTP headers can be hard.
    *   And thinking about how to connect this to Shared Storage, so we don’t have to create another new reporting mechanism.
    *   Also, how are we getting permissions right, which would be a security vulnerability.
    *   And we talked about, boiling down the set of desired things into a prioritized list. Pat mentioned that just learning about which sellers were activated was a top thing on his side.
*   Patrick
    *   Could component sellers write to any entity? Does the publisher indicates an analytics party in the well-known?
*   Paul
    *   That’s the question framed above; .well-known file, HTTP response header, HTML/JavaScript. If it’s a runAdAuction argument, don’t know how we would pipe that into an iframe. How does the publisher dictate the analytics providers, and how do auction participants indicate that they’re OK with sharing that information. Might have to talk to security folks to see whether wildcard could be useful.
*   Brian  May
    *   Sounds like we have a general purpose mechanism we want to enable that can give service providers access to data written to shared storage when there is an agreement between the provider and the entity they’re providing services and the entity can indicate what data can be written to storage accessible by their provider. Would need to be a handshake between the principal and the service provider and that could be an opportunity to load the service provider scripts.
*   Paul
    *   Also, want to make sure that the person who’s writing into shared storage has permission to do so.
*   Michael
    *   Yes, need the two-sided opt-in
*   Warren
    *   Would like to push back on having this in runAdAuction. Something the top-level seller would have to add, instead of the publisher.
*   Patrick
    *   No technical requirement that person who calls runAdAuction is the top-level seller. But I agree with the sense of the idea.
*   Brian May
    *   To Paul’s earlier suggestion of capabilities being communicated between participants, it would be helpful in making partnership decisions if there was a standard means by which participants could identify the capabilities they were willing to share with partners.
