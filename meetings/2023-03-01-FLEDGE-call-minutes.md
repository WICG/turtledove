# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 4pm UTC (during winter).

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday March 1, 2023


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Matt Menke (Google Chrome)
3. Russ Hamilton (Google Chrome)
4. Orr Bernstein (Google Chrome)
5. Tim Hsieh (Google Ads)
6. Kevin Lee (Google Chrome)
7. Brian May (dstillery)
8. Martin Pal (Google Chrome)
9. Sven May (Google Chrome)
10. Paul Jensen (Google Chrome)
11. Andrew Pascoe (NextRoll)
12. Elias Selman (Criteo)
13. David Dabbs (Epsilon)
14. Maciek Zdanowicz (RTB House)
15. Rotem Dar (eyeo)
16. Jourdain Casale (Index Exchange)
17. Alex Cone (Coir)
18. Joel Meyer (OpenX)
19. Akshay Pundle (Google Privacy Sandbox)
20. Jonasz Pamula (RTB House)
21. Supraja Sekhar (Google Ads)
22. Wendell Baker (Yahoo)
23. Caleb Raitto (Google Chrome)
24. Stan Belov (Gogogle Ads)
25. Ahmed Elsayed (FoxPush)


## Note taker: Martin Pal


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here]

Held over from previous call:



*   FLEDGE: custom reporting dimensions https://github.com/WICG/turtledove/issues/165, Tim Hsieh, Stan Belov
*   iframes' usage in FLEDGE going forward; communication from container to nested iframes; resizing (ctx: https://developer.chrome.com/docs/privacy-sandbox/fledge-api/feature-status/), Jonasz Pamuła
    *   Issue: https://github.com/WICG/turtledove/issues/469

New items:



*   


# Notes

Michael: 2 leftover items from last time. First topic: issue 165, custom reporting dimensions.



###   FLEDGE: custom reporting dimensions https://github.com/WICG/turtledove/issues/165, Tim Hsieh, Stan Belov

TIm Hsieh: GAM supports feature called buyer reporting ID. Bidder in bid response can pass a bidder reporting ID which is an arbitrary string. GAM will allow reporting broken down by this buyer reporting ID. We would like to support this in the FLEDGE world. Currently FLEDGE doesn’t allow buyer to pass arbitrary info to logging. How can we support?

Michael: The event level reporting offers to report on the outcome of the auction. Lets report on arbitrary data contextual and first party. Only limited amount of data from the interest group: k-anon renderUrl. Discussion about allowing some additional signals for ML training. That is possible with event level.

Aggregate reporting: much more freedom. You can use arbitrary information from IG, pub page etc. Info only in aggregate, but much richer slicing.

For custom reporting dimensions, if you only need aggregate, use agg reporting. If that doesn’t fit your needs, let’s talk about why not. If you want additional info from IG into the event level report, I don;t see how to do that without leaking privacy.

Tim: in a world with aggregate reporting, this can be supported. In the event level world, we can slice arbitrarily by contextual signals. 

In our use case, buyer might want to slice things by campaign. In this case we need IG info.

Michael: if slice by campaign, event level report has renderUrl. If you can tell campaign from renderUrl, you can do this in offline logs processing. If same ad can belong to multiple campaigns: you could make unique renderUrl for each campaign (hits k-anonymity). 

Brian May: If all we want is distinguish a specific creative being shown, can we append campaign id to the URL

Michael: we don’t want the browser manipulate the URL unless there’s no better way. Seems like buyers can do this on their own, with no custom logic in the browser.

Paul: In addition to the renderUrl, the event level report will also contain the IG name, if it is k-anonymous jointly with the renderUrl. Perhaps you can use the IG name field – but still you’re subject to k-anonymity.

Brian: Can we have an arbitrary field that will be added to the event report?

Paul: If you mean aggregate reporting, you don’t need to worry about k-anonymity. 

Michael: Some ppl think a smaller number of IGs per user may be a good way to use fledge; we want this approach to work. In this setup, the IG name may not be meaningful (e.g. if IG == user). In that case, perhaps allow some other string in lieu of IG name (still subject to k-anonymity). 

Matt Menke: Sounds like this will cause a lot more k-anon checking

Michael: We can avoid that.  Today we do two k-anon checks: for renderUrl alone and for (renderUrl, igName). 

Now suppose each ad has metadata associated with it. We can have another piece of per-ad metadata (“ad reporting id”). One ID per ad.

If an ad doesn't specify a reporting name, then fall back to IG name.  So still two k-anon checks, and falls back to today's behavior if the new optional ad reporting id is omitted.

If a renderUrl is used by multiple campaigns, we can use campaignId as ad reporting ID.

David Dabbs: How do house ads work in terms of fledge? We have a single ad that never gets trafficked by the buyer. We fall back to this single house creative in A/B experiments. We call it a pathological case, but can happen as fallback.

Michael: Natural way would be to talk the fallback creative, and append a url param (campaignId) ignored for rendering purposes, but used for log processing. If you are doing an A/B experiment, both your control and treatment are subject to k-anonymity, so perhaps this is an OK way to do this.

Tim: the renderUrl will be needed to pass info between buyer and seller (some url param). 

Stan: if there was additional info to encode in the renderUrl



*   Seller needs to do an ad quality check for each variant of the url, or be aware that several url variants represent the same creative
*   Would buyers and sellers need to agree on some protocol for which params don’t impact rendering

Michael: Alternative is to have a piece of metadata separate from renderUrl that is available in event reporting. If buyers and sellers want this, we can make it happen.

Stan: this is available to reportWin, but not reportResult

Michael: should this be available to reportResult?

Sellers should be able to check renderUrl. 

Should the seller see the campaignId? CampaignId seems like buyer private info, that should not be visible to the seller. 

Can we have a single thing that is appropriate for what buyers need, and can it be also shared with the seller. Or is there a thing that should be kept private to buyers.

Stan: perhaps buyers can choose if this will be visible to seller.

Michael: buyers and sellers may fight whether it’s required to share

Paul: we could have one buyer-seller shared ID, one buyer-only ID

Michael: I’d be happier with a single answer, but we can make this configurable if that’s not possible

Any time we add a capability, different people will come up with different use for that capability, so we could have two different options for seller-visibility if that is needed, but I would like to simplify.

Sounds to me like people are generally happy with a reportingID metadata for each ad, that does get shared with the seller, and for an ad with no reportingID we fall back to the IG name which would not get shared with seller just like today.

Jonasz: Decoupling what we report from IG name sounds like a good capability. We thought of extending IG name with more info.  \
Question for Stan: is the reporting ID something that has a prescribed meaning?

Stan: we don’t assign meaning. Buyer defines the ID, and it is passed to seller.

Jonasz: we want to report something more granular than campaingId.

Tim: the ID may be anything the buyer wants. 

Stan: In some cases semantics may be assigned by who is the buyer, seller, the relationship etc. As a general principle we should not assume semantics.

Michael: we still want k-anonymity, and we don’t want unlimited calls to k-ano server

Every time you get reportWin, if you get the more granular name, you know it has passed the threshold. Also, the k-anon server can be queried by anyone.

Brian: it would be nice to know how far I am from being k-anonymous

Michael: the k-anon server only tells you over or under the threshold. If an ad has not yet won

We run an auction. If the winning renderUrl passes k-anon, it wins. If not, we tell the k-anon server to bump up the count, drop the winner and rerun auction. If an ad is in that state of warming up, there’s no further info on how far it is from the k-anon threshold.

The best way would be to use aggregate reporting of how many times an ad tried to show and failed.

David: what you described about agg reporting: where do I generate the data for aggregate reporting?

Paul: The extension to the private agg api support this. You learn the reason why the ad was not shown. There may be a pending pull request describing this. Will get back to you on this.

Brian: would be great to get info about campaigns early in the fight to be able to fix errors.

Michael: the agg reporting about reason for not showing, which Paul mentioned, ought to give you insight into the health of your campaign. 

Brian: will we have to waterfall to the reason

Martin: End-to-end expectations of aggregate reporting system timeline?  What is iteration length if you use this process for iterating on debugging your ad campaign?  We should come back to this another time.

Michael: should we take up topic #2?



###   iframes' usage in FLEDGE going forward; communication from container to nested iframes; resizing (ctx: https://developer.chrome.com/docs/privacy-sandbox/fledge-api/feature-status/), Jonasz Pamuła, Issue: https://github.com/WICG/turtledove/issues/469

Jonasz: let’s go for it. Recently it was announced that fenced frames won’t be required until 2026. For us, iframes rendering is more convenient, especially for ads composed from multiple pieces.



*   Coordinate styles across elements of the creative
*   Resizing (parent iframe can control size of nested frames, align them in a grid)

FLEDGE says fenced frames won’t allow this.

We think the use cases are not harmful to goals of fledge. Are we on the right paths? We wouldn't want to invest resources into this kind of approach only to find out that some of these capabilities may be blocked in the meantime. (While still in transitory period, before 2026.)

Michael: having more time to understand what capabilities fenced frames need to have to make them work for use cases is good. 

One way to get privacy is k-anonymity. Allowing frames to share information may be a way to get past k-anonymity. We need to find techniques to let you accomplish your goals without allowing malicious actors to abuse such mechanisms. 

Jonasz: discussion of what to enable in fenced frames is a somewhat different topic. We intend to use iframe communication for non-malicious things. If Chrome says at some point "other malicious buyers are using that for wrong things, let's block this for everyone", we'd be in a tight spot.

Paul: we should get fenced frames folks to chime in. We need to put more thought into how fenced frames can communicate while limiting leakage. 

Jonasz: if you think our use case is legit and can promise not to break it, that would be awesome.

Michael: We're fully aware that FLEDGE is not bulletproof now. We are choosing not to clamp down on certain capabilities that are important for use cases. If it turns out a capability is being abused, we will not react by being shocked that abuse is possible, we already know that abuse is possible.  We have chosen to err on the side of making it useful enough rather than clamp down in short/medium timeframe. We will react in ways that avoid hurting people using FLEDGE in non-abusive ways.

Shivani: +1 to Michael. From a fenced frames’ solution perspective, we are thinking whether/how we can support limited comm channels main ad and component ads and how to support related functionality e.g. native ads. It would be good to have a github issue posted for your issue.

Jonasz: https://github.com/WICG/turtledove/issues/469
