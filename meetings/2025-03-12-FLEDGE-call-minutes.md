# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California.  Note: that's 4pm Paris time for the rest of March!

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday March 12, 2025

Note that the US has now changed our clocks, so March meetings are an hour earlier in Europe+UK

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!



1. Michael Kleber (Google Privacy Sandbox)
2. Clément Charnay (Criteo)
3. Nicolas Urien (Criteo)
4. Andrew Kwok (Criteo)
5. Brian May (unaffiliated)
6. Matt Kendall (Index Exchange)
7. David Dabbs (Epsilon)
8. Alex Peckham (Flashtalking)
9. Alex Turner (Google Privacy Sandbox)
10. Sid Sahoo (Google Privacy Sandbox)
11. Rushil Walia (Google Privacy Sandbox)
12. Orr Bernstein (Google Privacy Sandbox)
13. Jeremy Bao (Google Privacy Sandbox)
14. Paul Jensen (Google Privacy Sandbox)
15. Shafir Uddin (Raptive)
16. Fabian Höring (Criteo)
17. Trenton Starkey (Google Privacy Sandbox)
18. Alexander Tretyakov (Google Privacy Sandbox)
19. Sven May (Google Privacy Sandbox)
20. Alonso Velasquez (Google privacy Sandbox)
21. Bharat Rathi (Google privacy Sandbox)
22. Eyal Segal (Taboola)
23. Laurentiu Badea (OpenX)
24. Miguel Morales (TechLab)
25. Kenneth Kharma (OpenX)
26. Owen Ridolfi (Flashtalking)
27. Andrew Pascoe (NextRoll)
28. Sathish Manickam (Google Privacy Sandbox)
29. Brian Schneider (Google Privacy Sandbox)
30. Ardian Poernomo (Google Ads)
31. Victor Pena (Google Privacy Sandbox)
32. Abishai Gray (Google Privacy Sandbox)


## Note taker: Orr Bernstein


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which  can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here (meetings with no items may be canceled):



*   Clément Charnay: Discuss Private Aggregation special metrics bucketization feature proposal: https://github.com/WICG/turtledove/issues/1084 
*   Jeremy Bao: Discuss the proposal of join IG through HTTP response headers


# Announcements

Privacy Sandbox folks are holding every-second-Wednesday meetings in the hour after this meeting to discuss the trusted server elements of Protected Audience work. For more information see https://github.com/WICG/protected-auction-services-discussion/issues/27.

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!


# Notes 


## Clément Charnay: Discuss Private Aggregation special metrics bucketization feature proposal: https://github.com/WICG/turtledove/issues/1084



*   Clément Charnay
    *   We would like to post-process browser signals.
    *   One post-processing possible with k-anon offset, bucketized by intervals of five milliseconds.
    *   Proposed a callback to post-process, take the browser signals, could define this function as they want.
    *   Alex proposed to be able to provide bucketization scheme as processing for signals, i.e. each bucket threshold.
    *   Would be happy with this, addresses use case.
    *   Alex said this proposal would be considered. Want to discuss this further.
*   Alex Turner
    *   Threshold proposal still makes a lot of sense, especially as starting point that gives utility we need, can add more to it as needed.
    *   Don't have a timeline to share just yet, hopefully will be able to get back to you with updates pretty soon.
    *   Need to resolve before building this: need some limit on the number of thresholds. What is a reasonable limit for your use cases? 20? 100? 1000?
*   Clément Charnay
    *   20 buckets is probably plenty for our use cases
*   Nicolas Urien
    *   Maybe as many as 60 for bid shading. Don't expect more than that.
*   Alex Turner
    *   Even with 60, I wouldn't expect that to take crazy amounts of memory on the browser.
*   Michael Kleber
    *   Happy that we can use static declaration instead of callbacks, which meets our goals of not needing to spin up new JavaScript contexts that may take a lot of resources
*   Paul Jensen
    *   Agree with what Alex and Michael were saying to avoid another worklet to do post processing as originally proposed.
    *   Could be part of something else - Private Model Training - https://github.com/WICG/turtledove/blob/main/PA_private_model_training.md 
    *   We also had a need for post-processing. There might be things you don't want to do on every call to `generateBid()`, especially with Private Model Training.
    *   Broke `reportWin()` into two parts - `reportWin()` and `reportAggregateWin()`, which has access to more data, but can only call sendEncryptedModelingSignals, but can also access the Private Aggregation API. Can use the `aggregateWinSignals` from generateBid(), or reportWinInputs from a prior call to reportWin(). Because `reportAggregateWin()` only has access to these two functions that only emit data in aggregated form, `reportAggregateWin()` can have access to more data.
*   Michael Kleber
    *   Key difference - they want to be able to use this in the losing bid situation as well, and reportAggregateWin() is only for the winning bid.
*   Clément Charnay
    *   This is correct, need this for both winning bids and losing bids.
*   Fabian Horing
    *   To clarify, `reportAggregateWin()` doesn't get access to `generateBid()` runtime, is that correct?
*   Paul Jensen
    *   That's correct. But any post-auction signals it can have access to.
*   Michael Kleber
    *   If there are additional signals that are useful in `reportAggregateWin()`, we're very open to adding those. This is the maximally privacy-safe environment because the only way to get information out is through the privacy-safe aggregation mechanisms.


## Jeremy Bao: Discuss the proposal of join IG through HTTP response headers



*   [David Dabbs] Prior discussions:
    *   https://github.com/WICG/turtledove/issues/1191 
    *   https://github.com/WICG/turtledove/issues/82 
    *   Jeremay Bao: Also https://github.com/WICG/turtledove/issues/1228 
*   Jeremy Bao
    *   I'm a Product Manager working on PA
    *   There's been an ask to join interest groups through HTTP response headers
    *   We know that, today, cookies can be added through JavaScript or through HTTP response headers. In PA, we want IGs to have similar feature parity to how cookies can be added.
    *   Different reasons from different parties on why people need this. Concerns with 3p JavaScript; performance (reducing the volume of data on the critical path of website loads); addresses as issue with sandboxed iframes.
    *   The other part of the problem is around IG update capabilities. Two ways to update an IG after creation - after auctions (rate limited to at most once a day to conserve resources), and separately using `updateIfOlderThanMs`, an optional field in the trusted bidding signal response. We've heard a request that we want a faster update mechanism.
    *   Structured headers can't contain arbitrarily complex data, limits on how many layers of hierarchy can be used; creating a lightweight skeleton IG and then using updateAfterCreation to fill it in.
    *   Proposed solution: Create three APIs through HTTP response headers — `Ad-Interest-Group-Join`, `Ad-Interest-Group-Leave`, and `Ad-Interest-Group-Clear-Origin-Joined-Groups`
    *   For Ad-Interest-Group-Join, there would be a limited set of fields that could be specified. Supports both regular and negative IGs - `additionalBidKey` can be only be used for negative interest groups; `updateURL` can only be used for regular interest groups.
    *   New boolean field, `updateAfterCreation`.
    *   For Ad-Interest-Group-Leave, just the name of the IG to leave.
    *   For `Ad-Interest-Group-Clear-Origin-Joined-Groups`, can specify both the owner whose IGs to leave, and can also include `groupNamesToKeep`, which specifies which IGs not to leave.
*   David Dabbs
    *   A lot of this looks like a recapitulation of what we talked about. Michael at that point said it looked reasonable and we could take a look eventually, so it's good that we're doing so now.
    *   Looking at the mini-spec of the join, I remember asking about accommodating negative interest groups. Additional bids need the biddingLogicURL, right?
*   Orr Bernstein
    *   Yes, negative interest groups can only have and must have name, owner, lifetime, and additionalBidKey.
*   David Dabbs
    *   Are you saying that the updateAfterCreation attribute isn't available at the same time? Would be great if they could be available at the same time.
*   Jeremy Bao
    *   We're planning to ship both of these - header APIs and the updateAfterCreation - at the same time.
*   David Dabbs
    *   Implicit in this example, the Leave header must be coming from the owner's origin?
*   Michael Kleber
    *   That's correct, and matches the JavaScript API. You don't want someone else leaving an IG on your behalf. Joins can be delegated with the established delegation mechanism.
    *   We'll put the design up as soon as possible. We didn't want to wait to talk about this in this forum until after that was published.
    *   We'll have that venue for feedback available soon.
*   David Dabbs
    *   If you have questions about how we'll use this, let us know. This seems to walk and quack like the duck we were asking for. This is great to see.
*   Jeremy
    *   Almost at the end of the doc, and then I have some questions for you.
    *   A few details. Error handling - no error handling with headers. API callers have to make their own way to detect errors.
    *   Regarding access control, today we have the Permissions-Policy cover these APIs. We want to have that also cover the this API. There's also a delegation mechanism, which we want to keep as well to maintain feature parity with the JS API.
    *   For the IG updateAfterCreation, if that's set to be true, we'll trigger the update right after creation. We're also considering adding the same to the JS API, so you could specify this on the JS API and have the same result.
    *   Questions about the `updateAfterCreation` - how would this impact performance? Curious, what benefit does this delay bring us?
*   David Dabbs
    *   Can answer that, but first a clarifying question. One advantage was the opportunity to set the lightweight shell, and be able to have some control to say, if I'm registering this interest group, if it already exists on this device, don't override it. That way, it's easy to plant a shell, but if you've already done that, you're not going to keep overriding that.
*   Michael Kleber
    *   You're vision is, writing a shell IG would be a conditional. But the update that would be triggered shortly thereafter, that would have to happen either way, because otherwise it would reveal whether or not the IG was already on the user's device.
    *   Is this a feature that you would hope to exist as part of the JS API also, or only have the `createOnlyIfItDoesNotExist` feature on both?
*   David Dabbs
    *   Once header-based exists, I can't imagine going back to the JS API. Eventually, I suspect the JS API is likely to wither.
*   Jeremy Bao
    *   Would like to better understand this use case. If I may be viewing this website several times in a week, the first time I create a skeleton and then update to get a full IG. The next day, I don't overwrite the IG when the website tries to join.
*   Brian May
    *   I can imagine that I create an IG today and have it updated at a specific time. The update server changes day ovre day.
*   David Dabbs
    *   Using the shell pattern, you have all your creatives, but when you join it again, all of that gets replaced.
*   Michael Kleber
    *   Let's dive into this a little more. IGs have a way that update works, where the way that update works lets you update fields without overwriting existing information in the IG in other places. Maybe what you're asking is that those be preserved if there's an interest group there already. Maybe what you want is update semantics.
*   David Dabbs
    *   I assume that all of the tooling, visibility integration would be there, e.g. in the Application tab ->Interest Groups, you can see joins for debugging.
    *   Also, error reporting, maybe the reporting API, if someone's using that header and they're not allowed to use the Permissions-Policy, maybe the reporting API can let the party that returned the header know.
*   Fabian Horing
    *   Very interested in this feature. Already what we're doing today in a degraded mode. After creation, we're updating.
*   Michael Kleber
    *   What you're saying is that creating the interest group shell and getting it to update soon?
*   Fabian Horing
    *   Yes.
*   Jeremy Bao
    *   Using updateIfOlderThanMs?
*   Fabian Horing
    *   Yes.
*   Ardian Poernomo
    *   [Thumbs up emoji]
*   David Dabbs
    *   Background sort of fetch, marking and financial services firms, it's in the rendering path of the site, whereas we can take the heavy rendering of the IG and it'll happen in the background in the updateURL.
*   Fabian
    *   Not our main motivation to do this, but if it can improve latency, it would be nice too. Usually we do this because we don't have all of the information in the right triggers. So we only add the basic interest group.
*   Brian May
    *   It also helps you focus your update resources in one place rather than having them spread out across multiple websites.
*   David Dabbs
    *   With createAfterUpdate, will also be guaranteed that by the time you leave the page, it'll be ready to go. Won't have to wait for the first hit auction.
*   Paul Jensen
    *   Jeremy, you mentioned that we don't have a great error reporting mechanism, and David you mentioned it would be helpful know if e.g. your Permission-Policy was set correctly. Some of the way that the browser communicates that today is through request headers, to convey whether a particular request is eligible, e.g. for ARA, we convey whether that request is eligible.
    *   Probably don't want to add that eligible header to every request from the browser. What requests do people need the eligibility header attached to? I can imagine that subresource requests, a frame or script that does the tagging leaderboards
    *   
    *   calls.
    *   We already have a fetch flag that we use for several things in PA - `adAuctionHeaders` - we can use something like that, but it's limited to just fetches, not for loading JavaScript.
*   Brian May
    *   You're suggesting a signal that can be used to communicate what the capabilities are.
*   Paul Jensen
    *   It's possible, but heavy weight. We wouldn't want to add this to every single request.
*   Brian May
    *   Interesting capability, but would also want to have a reactive thing for when errors happen.
*   David Dabbs
    *   The questions you were posing - those might be good things to put into the doc as well.
*   Jeremy Bao
    *   Question to Brian - reporting, could this be a follow-up, or would this whole feature not work without reporting?
*   Brian May
    *   Would work without reporting, but cases where it's failing and we don't have visibility as to why it's failing. Aggregate feedback would still be useful.
*   Jeremy Bao
    *   Back to earlier question, there was some request on the request (#1191) to have a delay. Why?
*   Ardian Poernomo
    *   The main idea is to have the joinIG call wait.
*   Michael Kleber
    *   The goal is to debounce, if you get three joins, only do the request once.
*   Brian May
    *   I can imagine people dropping a whole bunch of IGs on the page, all at once, and not wanting the browser to hit the server all at once.
*   David Dabbs (after the call)
    *   @jeremybaoty would appreciate calling out situations where the headers are disallowed and probably silently ignored, such as, for example `updateURL` responses, &c.
