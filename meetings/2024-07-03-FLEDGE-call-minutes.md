# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday July 3, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!	 



1. Michael Kleber (Google Privacy Sandbox)
2. Brian May (Dstillery)
3. Arthur Coleman (IDPrivacy)
4. Patrick McCann (Raptive)
5. Don Marti (Raptive)
6. Aymeric Le Corre (Lucead)
7. Jacob Goldman (Google Ad Manager)
8. Sven May (Google Privacy Sandbox)
9. Orr Bernstein (Google Privacy Sandbox)
10. Gianni Campion (GoogleAds)
11. Sarah Harris (Flashtalking)
12. Owen Ridolfi (Flashtalking)
13. Jeremy Bao (Google Privacy Sandbox)
14. Isaac Foster (MSFT Ads)
15. Tim Taylor (Flashtalking)
16. Luckey Harpley (Remerge)
17. Laura Morinigo (Samsung)
18. David Tam (Relay42)
19. Abishai Gray (Google Privacy Sandbox)
20. Alex Peckham (Flashtalking)
21. Stan Belov (Google Ad Manager)
22. Shafir Uddin (Raptive)
23. Matthew Atkinson (Samsung)
24. Ken Gordon (Azure)
25. David Dabbs (Epsilon)
26. Koji Ota(CyberAgent)
27. Matt Davies (Bidswitch | Criteo)
28. Kenneth Kharma (OpenX) 
29. Paul Spadaccini (Flashtalking)
30. Rahul Shelat (Google Privacy Sandbox)
31. Andrew Pascoe (NextRoll)
32. Maybelline Boon (Google Privacy Sandbox)


## Note taker: Arthur Coleman	


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here:

*   Isaac Foster:

    *    Trusted Scoring Signals for Financials: Via [Comment](https://github.com/WICG/turtledove/issues/824#issuecomment-2198376904) on [Roni Original](https://github.com/WICG/turtledove/issues/824) 


    *    Brief revisit the “coarse information sharing” thing, we had talked about setting up time but never did, all got too busy…can even answer here


    *    Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846


    *    Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736 \


*   Gianni Campion (briefly discuss 0 ads API for joinIG  https://github.com/WICG/turtledove/issues/1191)


*   Yao Xiao

    *   discuss header api for joinIG https://github.com/WICG/turtledove/issues/82

*   Jeremy Bao (Google Privacy Sandbox)
    *   Feedback on negative interest group targeting proposal and usage budget https://github.com/WICG/turtledove/issues/896#issuecomment-2174598427


# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for their own comments - so go back later and make sure the notes are accurate

You can even fix them on Github later!! :) 


# Notes 


## Isaac Foster: Trusted Scoring Signals for Financials, via [Comment](https://github.com/WICG/turtledove/issues/824#issuecomment-2198376904) on [Roni's issue 824](https://github.com/WICG/turtledove/issues/824)

[Isaac] - Thinks we talked about this before around Roni’s issue - there is a challenge that we have right now with pricing trends - more than one, actually. 

Ideally, folks would rather not put sensitive pricing terms into the web page.  Also a bit of a challenge with linking up with final pricing via your scoring.  What we at MSFT do today - when we receive a bid from a DSP, while we are scoring we are also applying various financial terms to the bid to determine what the final price would be.  All of which is not just a straightforward function of the bid. All of that magic happens, scoring happens, and then - you want to report out the figures you scored on.  And those figures need to be consistent - bid $1 and you pay 90 cents - preferably there is a consistency between scoring and reporting time.  Right now there is some challenge around that.  Also, the object graph of what goes into pricing can be pretty complicated.  A lot to jam into the auction config.

So those are the problems.  My idea was two fold.



1. Chrome piece of support
2. Something the industry would have to do to enable this

Simple version:

The framework that Protected Audience Framework uses would inject into the scoring function of the seller the signals that had been sent back by the trusted scoring signals - the seller’s KV call - and those could be used along with things like creative audit to settle pricing.

Then you know you are getting back the same signals back from the scoring function and reporting function.  Then you wouldn’t need to put your entire graph into the scoring config.  Then the industry would have to say there is enough information in the config of the renderURL to do the scoring/reporting.  

[Michael Kleber] - Let me repeat back what you are saying.  There are two different times in the course of the Protected Audience auction where somebody gets to look up information from a trusted key value server - today your own, later the real, trusted one.  One of those times is during bidding, some data made available to the bidder in generateBid().  In that case the keys are very unrestricted.  You can put whatever you want in the keys of the interest group, then look up information from that from the trusted bidding signals server.  And that information would not be safe to make available at event level logging time because keys have no good privacy properties and so values returned from server won’t have good privacy properties either.

But the other time is when a party doing the sell side and scoring - when they get to look up signals from their trusted scoring signals server- the lookup key is the render URL of any ad that is of a bid being considered or scored in the near future.  And the winning ad has this k-anonymity property associated with it.  The thrust of the ask here - since we are already letting other reporting contain the winning ad renderURL itself, then can’t we let the winning event-level report contain other information - some values looked up from the sellers from the KV server.  The contention is that there is no new privacy risk here because it is a key and if you know the key you should be able to know the value associated with that key.  More convenient to make the value used in scoring available at reporting time because it prevents items from unexpectedly changing in the meantime, which could happen if you update the server at the wrong moment.

Do I have that correct?

[Isaac] Yes.

[Stan Belov].  I understand the proposal from the API standpoint.  I need to understand better how we would use it.  In this case, are you thinking about some sort of pricing decisions that are specific to individual ad creatives?  There’s another question, but explain this first.

[Isaac]  It would require both a feature on the Chrome side and then the buyer/seller have to agree on the protocol.  Let’s stick to the idea it is fully in the renderURL for now.  Today, in our exchange, we may apply different contract terms, fees, bid ups vs. bid downs - things like that - depending on advertiser and publisher and deal specifics.  If that information is encoded in the renderURL - the IDs that the buyer and seller have agreed upon- then when that went to get looked up in the trusted scoring signals call.  The seller could have indexed whatever from the combination of parameters - it is bid up, rev share, private  marketplace etc.  That then goes back into the scoreAd and the report itself since those two things can actually share code.  You can have a common function and send common parameters into that common function.  That should be roughly guaranteed to be consistent data between the two.  You don’t have to put your pricing terms through the private auction config which is a little bit less secure,  It seems a little better to me

[Brian May] When you say data and consistent logic, you mean between the buyer and seller?

[Isaac] I meant between score time vs. report result time.  Score time vs recording of financials from the transaction time.  So like today when we do our stuff around the auction, we log all that immediately.  That process that is scored says ‘here are the financials’.  Hypothetically, if we put that in a report result, we don’t have a way to put all that complex information in.  Or the data may have changed.  We scored on V1.01 and we reported on V1.02 - or contracts changed

[Stan] Sounds like you are proposing to encode financial information that may affect pricing in the renderURL.  And one of the major use cases here could be to deal with some sort of agreement between a buyer and a publisher - correct?  I was wondering if you have looked at some other threads on the same topic of deal support.  I just want to make sure by looking at this use case to make sure you know pricing terms that need to be accessed in the reporting and need to be accessed in scoreAd.  But they themselves are not reflecting the actual creative.  Can these concepts be split apart in some way?  From an exchange perspective, there is a need to duplicate a renderURL, for each combination of potential deals, attributes related to classification,etc.

[Isaac] Have read #873 before, so I do think we should maintain the decoupling of renderURL from important reporting information. So I did put something in the comment like the initial proposal - the renderURL - would we factor in some of the buyer and seller reporting ID things that still have the k-anonymity properties that will help us maintain the healthy decoupling.  There is a second cut of this we need to specify better.

The additional piece for Chrome would be in addition to the renderURL going through on the trusted scoring signals call, it would also be the buyer and seller reporting ID, which should be ok if we can agree on the k-anonymity restriction.  Which we have already agreed in other contexts is needed. So I think we can maintain that decoupling and still get something useful.

[Stan]  I think the use of the buyer and seller reporting IDs and trusted scoring signals needs to be clear. 

I think the idea of propagating scoring signals to the reporting function is also quite valuable, especially if you want to ensure that the promises made to the publishers are fulfilled.  Let’s say the creatives that don’t satisfy the requirements were indeed filtered.  Maybe a publisher excludes certain categories, for example.  That way synchronization between score time and reporting time are the same is very valuable. 

[Michael] Not surprised that this also turns into a request for the buyer and seller reporting (deal) ID to become a part of what is sent to the the seller’s trusted server - I think that is already there in your comment Isaac.  I anticipated that would be part of the request.

Again, I agree that this does not seem like a dramatic change to the privacy stance, because all this information you want included in the event level report that you can look up after receiving the event-level report - except for the synchronization issue.  That seems ok even if a design change that addresses an edge case.  Not too burdensome.

From a privacy point of view, the seller's trusted scoring signals server is receiving multiple bids at the same time. As long as we are in BYOS, then that is obviously a new privacy leakage because it is possible the values returned by the TSS server could be based not just on the renderURL but all the other renderURLS as well which might include a unique user ID.  If you are running your own server, you could use this to include a user ID in the event logs.  

If you are using BYOS and you do decide to cheat, then you can just log the user ID in your log server side.  It is not like putting them in the event level report is  a dramatic change in terms of level of harm, but let’s be clear we are giving away a mechanic to put user ID in the event level report.  That’s not something we want to do.  Requires migrating to a secure server running inside a TEE that have limits on how computation works.

[Isaac] Want to make sure I am clear.  It's not that there is a new privacy leak, it's that it exposes the leak in an additional place.

[Michael] I mostly agree.  There is already a privacy leak if your server wants to cheat in logging stuff you could find a way to log user ID in some way based on all bids sent to your server.

[Isaac] Then you would have something you produced from all renderURLs in the same place as your auction config.  

[Michael]  It is definitely an opportunity for the event level report to include a new privacy risk that it does not include today

[Isaac] Yes, I agree, there is marginally more information.

[Michael] I will have to talk to some other folks on the team to see how this marginal privacy risk stacks up with other things.  Only makes me slightly uncomfortable - only because it is related to BYOS for now.  If your goal were to cheat by violating what a trusted server is supposed to do, then doing it by returning data back through the browser and into the win report  is a much more visible and discoverable way of cheating.  Versus doing it server side which is an invisible and non-discoverable way of cheating.  I think this is a tolerable privacy risk as a result.

[Stan] From the privacy perspective, in today's environment the trusted scoring signals server can see a combination of information about all the ads and bids plus the top-level domain.  Where here the potential delta would be the combination of all contextual information that is available in the report.  This could include some proxy for a user ID, correct?

[Isaac] What I was thinking, I think you could do it such that you would do it in such a way that you get an ID in a renderURL that may not pass k-anonymity, but that doesn’t matter, and put this in your process on your BYOS key value server.  Does make it a bit easier, but not fundamentally different

[Stan] I would argue something different.  I think that the argument is that the incremental risk is the combination of the contextual information that is available in the report

[Isaac] Yes you can do that in the BYOS scenario

[Stan] A good question if the BYOS trusted scoring signals are required to be k-anonymous or something else.

[Isaac] I’d have to check.  The renderURL is only judged after the key value call?  Michael?

[Michael] - I think that is right.  We have to talk about it with some folks not on the call. But it's not so big that I think it should not keep us from doing this.  Both Isaac and Stan - it sounds like you think there is value.  Any other points of view?  Good discussion and we understand the goals.  Tied to request to send buyer and seller reporting ID to Key Value Server and the whole deal support work going on.


## Gianni Campion (briefly discuss 0 ads API for joinIG), https://github.com/WICG/turtledove/issues/1191

[Gianni] - I think there was a Github discussion about what we would like to do regarding having a lightweight joinAdInterestGroup().  The idea is that the first daily update is going to update most of the stuff anyway.  Our plan is to join the lightweight joinAdInterestGroup() across the response so you have a skeleton of information to send the device for how you set up an ID.  Then we can rely on the first daily update to fill in the blanks needed for the interest group to perform its duties.  Proposal is to have a field that is triggered on the first update with a certain delay.  Most of the discussion seems to revolve around Brian May’s comment about the failure of the first update.  So I didn’t look too much in detail, but if you think about it, let’s say you have an API that says _trigger this daily update in 10 seconds_ and the network fails, we would still have what exists now - an update that fixes it after the first auction.  It becomes a quality issue related to how much do you lose between the first joinAdInterestGroup() and the first update.

[Michael] I think this is reasonable.  We talked about once before in the context of supporting the lightweight interest group joining from the HTTP response header.  So not have to have a Javascript element on the page to do this but instead use a JavaScript response header.  This seems like a good idea to me - lightweight join and call soon afterwards with JavaScript response header.  Seems like we should do this - no promises about priorities - a fine future element to do.

[Gianni] So what are the next steps?

[Michael]  Us to get around to doing it.

[David Dabbs] Question for Gianni.  Is your pattern of use - Isaac brought this up - he wanted to reduce the number of updates in a series of page-to-page-to page - this doesn’t get rid of that - which is fine. It is up to the caller to do that.

[Gianni] What is the pattern you are mentioning?

[David] Where you are going to drop and join interest groups.  And instead of rendering the full interest group, in your approach you render a minimum and then 10 seconds later you still have to go back to an updateURL()  to fill out the interest group later.  

If you want to reduce the number of those update calls, that’s up to the caller to do that.  This is just getting it off the main line of rendering on the page - make it lighter.

[Gianni] Yes.  And you probably delete that after the update.  If the same interest group gets called multiple times - maybe that interest group should be updated only once. So for this group, fire only one update for them.

[David] Netting it out: _We do it after the user has left the site over time_ - might be more than we can expect from Chrome. It can probably guarantee doing something when the page/frame ceases to exist (kind of like _fetchLater_) but probably not beyond that. 

[Michael] A good thing to be concerned about here.  Gianni - I would like the interest group to update itself soon, not once a day, for example.  There has to be a minimal amount of time between updates - now it is 10 minutes - so we don’t overrun the browser.  If you try to do this join and get an update immediately, and you try to do it for each page, then it seems like it is good to include in this request what behavior you hope for.  I assume you are asking for an update when the user is on the same page where they were adding to the interest group.

[Gianni] Not necessarily while on the same page, would be ok to happen a little later

[Michael] But you are not asking for Chrome to deliberately wait until after the user has left this page, right?  I don’t want you to be disappointed when a user goes to six different pages in a row, you don’t get an update on every page.

[Brian] I suggest we set a flag that when someone calls joinAdInterestGroup().  Let’s say I want to add this interest group, the call is made but if the flag is set a certain way, then don't have me add the same information over and over.

[David] In other words, it should silently fail.

[Brian] Yes.  If I do the same things 5 in a row, only do it once and ignore the other times.

[Michael] If you do a minimal join for an interest group already there on the browser, then you probably don’t want to overwrite all the information on every page.  So there is some complexity.

[Gianni] My desire, let’s take the example of example.com - we set my IG join to delay one minute - in that one minute I visit 6 pages that have an interest group, call it Interest Group 10.  I queue up six daily updates as a result.  Not a good idea to do all six - so only do the last one?  We need to agree on a mechanism here.

So I keep adding this lightweight thing, and the heavy lifting occurs with the one daily update.  So the last update should count.  I think that my preferred behavior is to use the last one, since if we were doing all the updates the last one would be the final state.

[Brian] We should be very careful about how we are thinking about the application of this.  If we produce this capability, then what people will do is they will create 100s of lightweight interest groups, wait for them to update, and then try to monetize them in the future.  

[Gianni]  How is that different?  It is already doable.  The API only allows you to not miss the first daily update

[Michael] I don’t think there is any new marginal interest group creation capability here that people can’t do today.  That risk exists today.  This new capability would make it just as easy to create a bunch of interest groups except you would double the load on your server because you’d have to add a second call a few minutes later.

[Brian] Let me push back on that.  If I can create an interest group by popping minimal information into a header and then blasting a bunch of those headers down into the browser, then I will do a lot more interest group creation and then later update - so more likely to have it happen.  It takes a lot more effort to create a number of interest groups today.

[David] No it doesn't take much more effort.  You just have to wait and lose a couple of auctions on the page. By the time the user goes and leaves the page, you have a couple of double or triple taps on auctions where Chrome hasn’t loaded you up.  Now the update occurs and you have a fully operational interest group.   And then you have a similar problem.

[Brian] If the pattern is I create shell interest groups vs. fully-functional interest groups that can be immediately used, I will create many more shells because they are easy to create.

[Michael] The point is that to create a shell and turn into a real interest group - the space between those two operations is a few seconds, so you are not reducing the work really because in a few seconds you have to do the full update anyway.  The difference in workload in the two scenarios does not seem enough to me to change people’s likely behavior.

[Brian] If I can create light groups and coordinate quickly only with myself now, I’ll do that more likely.

[Gianni] You wouldn’t be able to do it now, for example by returning an interest group that only has a daily update - no keys and no ads.   WIthin that automatic, it is literally something you can do now if you have implemented the update function.  If your update function updates the keys… (interrupted) \


[Brian]  Not suggesting we shouldn’t do this, but we need to be aware of how people might use it.

[Michael] The only marginal change Gianni is asking for is to update this newly created interest group in a few seconds rather than waiting for the 10 minutes that is the current minimum update functionality. It would get updated in the first auction.  So I don’t bother waiting for this interest group to be invited to be in an auction.

Yao - this  seems related to what you asked about in your agenda item (shown below)


    **Yao Xiao**


        ** discuss header api for joinIG https://github.com/WICG/turtledove/issues/82**

[Yao] Thank you.  Basically what happens today - the way tagging works -  on the advertiser side we inject the iFrame and the server side returns a second response.   Inside this second response we have the joinAdInterestGroup API getting invoked. In this way we can join the interest group and store it in the browser.   But there are performance issues around iFrames and, equally, we have to make sure the tag supports the joinAdInterestGroup() API, which is a JavaScript API.  But there are companies/users that don’t want to support a JavaScript API, they want a header-based solution instead.  We have already done something like this for attribution reporting API and shared storage API.   If we are going to move to the header-based approach above, we want to provide header-based support for all three endpoints - joinAdInterestGroup, leaveAdInterestGroup(), and ClearOriginJoinedAdInterestGroups(). 

[Michael] This is exactly the kind of support that would make this Gianni ask into a useful thing.  Aligned with prior discussion.

[David] So to the original request, you (Michael/Chrome) agree this ask is reasonable and useful, but can’t provide commitment/timing. Revising my earlier request for Chrome to provide a recognised means to ‘crowdsource’ your stakeholders’ interest to guide your dev priorities.

[Michael] Github is the way to do that right now.  Our PMs try to take in info from the various sources and trying to prioritize based on what they are hearing from the market. 

[Arthur Coleman] David, you asked this a while ago.  My company has built a community site for Protected Audience, I just did the roadmap for it.  Let's talk offline.  Happy to pull something together, with Alex and other Google folks.  I agree that it's needed, we should have a way for the community to thumbs up/down and help prioritization.

[David] WICG repos now have the _Discussions_ feature available.  That's a possible option here, where we already review issues. 

[Brian] Is there value in people placing a “thumbs-up” on issues?

[Michael] People writing something in a github issue is better than thumbs up.  Much more informative

[David] Yao, are you looking for a way to express an _entire_ interest group in a structured header or just a portion?  

[Michael] Let me point out there are limits to the lengths of HTTP headers. 

[Notetakers note for reader’s reference: Each individual header (name and value combined) has a maximum length of approximately 4096 bytes. This limit ensures compatibility with various network protocols and prevents excessively large headers from causing issues.  The total size of all request and response headers combined has a limit of approximately 250 KB.]

[Michael] The only way we could do a good job of supporting that without putting a limit on interest groups is to do the lightweight approach.  A flag that says I would like to update this interest group as soon as possible and some limit in the HTTP header for what parameters you could set.

[David] Yes, in the issue we outlined a specific attribute set for this “shell” pattern.

[Brian] You may want to create multiple interest groups for one owner at one time.

[Isaac] Ability to create interest groups via header - doing the light shell with refresh would be highly valued.  Publishers are always hesitant to add JS to their page.

[Michael] 100% our motivation for doing the header approach.


## [Jeremy Bao] - Solution for negative interest groups

We didn’t get to cover that today.  Several requests around that and we are creating a proposal around that.  I see several folks on the call related to that proposal - please take a look at it.  Github Issue [#896](https://github.com/WICG/turtledove/issues/896).
