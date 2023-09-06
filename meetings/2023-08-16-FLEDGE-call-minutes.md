# Protected Audience (formerly FLEDGE) WICG Calls: Agenda & Notes

Calls take place on ~~some~~ Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Aug 16, 2023


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Nick Llerandi (Triplelift)
3. Andrew Aikens (TripleLift)
4. Bosko Milekic (Optable)
5. Xavier Capaldi (Optable)
6. Antoine Niek (Optable)
7. Brian May (dstillery)
8. Sid Sahoo (Google Privacy Sandbox)
9. Paul Jensen (Google Chrome)
10. Orr Bernstein (Google Chrome)
11. Don Marti (Raptive)
12. Priyanka Chatterjee (Google Privacy Sandbox)
13. Luckey Harpley (Remerge)
14. Alex Cone (Google Privacy Sandbox)
15. Russ Hamilton (Google Chrome)
16. David Dabbs (Epsilon)
17. Harshad Mane (PubMatic)
18. Risako Hamano(Yahoo Japan) 
19. Matt Menke (Google Chrome)
20. Isaac Foster (MSFT Ads)
21. Sergey Fedorenko (MSFT Ads)
22. David Tam (Relay42)
23. Tamara Yaeger (BidSwitch)
24. Alexandru Daicu (eyeo)
25. Manny Isu (Google Chrome)
26. Guy Teller (eyeo)
27. Caleb Raitto (Google Chrome)
28. Joel Meyer (OpenX)
29. Stan Belov (Google Ads)
30. Tristram Southey (Google Privacy Sandbox)
31. Martin Thomson (Mozilla)


## Note taker: Manny Isu 


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here — no agenda no meeting!]



*   Isaac (MSFT) Issues (don’t all have to be mine, so grouping so we can easily filter)
    *   <span style="text-decoration:underline;">BA: Jan 1st 1% vs Feb Scaled Testing https://github.com/privacysandbox/fledge-docs/issues/55</span>
    *   <span style="text-decoration:underline;">User IG view/delete interaction w/r/t BA payload optimization https://github.com/privacysandbox/fledge-docs/issues/56</span>
    *   <span style="text-decoration:underline;">Multiple Bids Per IG (checkin, was discussed a while back) https://github.com/WICG/turtledove/issues/595</span>
    *   <span style="text-decoration:underline;">PA Deals Phase: https://github.com/WICG/turtledove/issues/686</span>
    *   <span style="text-decoration:underline;">Cross Device https://github.com/WICG/turtledove/issues/607</span>
    *   <span style="text-decoration:underline;">Production Operations: </span>

        <span style="text-decoration:underline;">https://github.com/WICG/turtledove/issues/728 \
https://github.com/WICG/turtledove/issues/620</span>

*   Nick (Triplelift)
    *   desirabilityScore: is there a range? What’s to stop a component seller from scoring ads with a tremendously high desirability score?
    *   Clarifying how the attestations file lookup/check works (Seller perspective); is the “Seller” origin checked prior to every auction?
*   Bosko (Optable)
    *   Clarifying question on suggested IG scope/granularity
    *   GAM throttling


# Notes


## desirabilityScore: is there a range? What’s to stop a component seller from scoring ads with a tremendously high desirability score?



*   [Nick] How will PA API distinguish between which one to choose?
    *   [MK] Every IG produces bids and the bid comes with an amount of money associated with it - It has to be comparable across different bidders. On the other hand, the desirability score is what the party running the auction assigned to each bid for how good they think it is. The desirability score is purely internal and it's never seen by anybody except that seller. The browser has no idea of what this means except larger is better; and less than or equal to zero means this ads should never win. The winner of the component auction is passed along to the top level auction for the top level seller to evaluate and assign their own desirability score, doesn't matter what the component seller thought of it. The desirability score is put in the reporting that goes back to the seller but does not go to any other parties.
    *   [Brian] So the seller is taking a bunch of different attributes and scoring them… wondering what we can do to understand what goes into each of those scores.
    *   [MK] Browser has no idea how buyers are coming up with the desirability scores. GAds in one of their articles may have talked about their calculation for their desirability scores, see here: https://github.com/google/ads-privacy/tree/master/proposals/fledge-multiple-seller-testing#technical-details 
    *   [Isaac] In my discussions with Sergey, and discussing the ranking function… the SSP has the opportunity to pass in things from the auction context. Can we talk about the canonical way to pass in values into the function?
    *   [MK] Every one of the IG on the device has an opportunity to make a bid… seller sees bid from IG one at a time; the js code assigns the desirability score for each bid and the highest one is the ad that is chosen. https://github.com/WICG/turtledove/blob/main/FLEDGE.md#23-scoring-bids
    *   Any information that is coming from the bidder; there is stuff that comes from the publisher page - info that publisher makes available like first party information, stuff that the seller puts into the decision making process - directFromSellerSignals; trusted scoring signals - info retrieved from the sellers KV server; renderURL is looked up in the SSPs KV server.
    *   Also, the SSP is probably enforcing some kind of rules that they got from the publisher on what kind of ads that they may not want on their site.
    *   [Sergey] Will that particular renderURL be used to render the Ad?
    *   [MK] Yes, that is a guarantee that the browser makes.
    *   [Sergey] So every creative will have a renderable URL on the SSP side
    *   [MK] Yes, that is correct.
*   [Sergey] So every DSP will have to register their creatives with every single SSP that they participate in?
    *   [MK] Not sure if that is required. If the SSP needs to do an analysis of the image, then they need to know about the candidate ads.  But they do not need to know about it in advance, all depends on how they manage their relationship with the DSP, which can provide metadata for the ad in the bid.
    *   [Sergey] Most SSPs do not blindly propagate ads to the public which means that it must be registered somehow - it brings up the question on how that is going to happen. Given that there is limited logging in TEE… it moves away from where DSPs and SSPs are going… we are moving towards 1.) Trusted Bidders: Allow all buyers to bid as long as they declare the brand, type of creatives etc. in the bid request so we can apply at quality. For other buyers, we do allow dynamic registration
    *   [MK] I think it could still work in this case… For ads that did not win the auction, you do not get the event level information into what happened, but you do have a way to find out what happened from an aggregate standpoint. You can learn all the creatives you have bid with but haven't yet registered with the corresponding SSP using the aggregate feature.
    *   [MK] It seems like the crux of the question is: How can a pull model work? How can one discover a url that hasn’t been registered yet?
        *   One model might be to change to a push model: DSP can use aggregated reports to learn about URLs that are winning auctions, and can tell SSPs to scan those URLs
        *   Either SSPs or DSP could use aggregate measurement to learn some hash(render URLs).  But DSPs are in a much better position to convert that back to the actual URL itself; this is hard for SSPs who would need to discover novel URL strings they had never seen before.  One option: DSPs could run a hash-to-render URL lookup service and the SSPs could use that.
*   [Harshad] If there are 5 SSPs with an IG… do the renderURL need to be send to the SSP KV server all in the same URL?
    *   [Paul] They are GETs right now given the privacy model. The browser groups together SSP requests and reduces the number.
    *   [MK] I think that is the essence of the problem here, if the KV server URL contains a bunch of ad render URLs, could get much too long
    *   [Harshad] There are other parameters too that need to be parsed
    *   [MK] I think Harshad makes a good point, we should consider whether they should be POST
    *   [Martin Thomson] should be the HTTP QUERY method, if you want it idempotent but with a body \

*   Matt Menke (call chat comments)

     \
In practice, Chrome will currently coalesce all DSP requests from a single auction, but may issue multiple SSP signals requests


     \
Since bids trickle in, and we don't want to delay scoring until we've received all bids.


     \
PerBuyerSignals are coalesced (If there are two auctions at once with the same buyer, one buyer may get results from two separate requests, though)


    Also, if multiple SSPs share a buyer in components from the same auction, there may not be a single request, but somewhat weirdly sharded requests.


    There will never be more buyer signals requests than times the buyer appears in (component) auctionConfigs.


    So best not to rely on all signals for a single auction coming from a single buyer or seller signals request. \
Since that would not be accurate.

*   [Joel] What is the relationship of the renderURL and the top level ads
    *   [MK] In the ad components, the top level ads have 1 renderURL and…
*   [Harshad] _<Participant suggested a different flow, with browser fetching per-buyer signals directly from their server, and SSP response just containing a set of URLs instead of the signals themselves>_
    *   [MK] It seems like it will delay the processing of the bidding operation by a bunch due to the round trips involved in your suggested flow. The tradeoff for performance does not seem optimal.


## Clarifying question on suggested IG scope/granularity

[Bosko] For IG granularity, IG per advertiser site… what is the canonical example and recommended way to do this?  One IG per interest?  One IG for all of a person's interests on one site?

https://github.com/WICG/turtledove/issues/207#issuecomment-1573785298



*   Our goal is to make sure that both of the options you mentioned are doable. As we have had more discussions, we have heard folks say that the best way is to minimize the number of IGs that the user is in to having 1 IG for all the activities for a user. This means that even if there are several different things that we did on a site, we could still have 1 IG for a user that did a collection of things.


## [Isaac] Lots left on the agenda.  Can we have more meetings?

[MK] Yes, let's increase frequency to every week, for now. 

**Next call on Wednesday Aug 23**
