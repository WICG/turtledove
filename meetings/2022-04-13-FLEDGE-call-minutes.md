# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Apr 13, 2022


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Brian May (dstillery)
3. Jeff Kaufman (Google Ads)
4. Andrew Pascoe (NextRoll)
5. Sven May (Google Web Platforms Team)
6. Martin Gruau (Captify)
7. Chris Evans (NextRoll)
8. Benjamin Dick (IAB Tech Lab)
9. Don Marti (CafeMedia)
10. Matt Menke (Google Chrome)
11. Paul Jensen (Google Chrome)
12. Jonasz Pamuła (RTB House)
13. Bartosz Marcinkowski (RTB House)
14. Michael Coward (Arcspire)
15. Tamara Yaeger (IPONWEB)
16. Joel Meyer (OpenX)
17. Peiwen Hu (Google Chrome)
18. Wendell Baker (Yahoo)
19. Stan Belov (Google Ads)
20. Ryan Arnold (P&G)
21. Rotem Dar (eyeo)
22. Supraja Sekhar (Google Ads)
23. Daniel Rojas (Google Chrome)
24. Philippe de Lurand Pierre-Paul (Google Chrome)
25. Aditya Desai (Amazon)
26. John Mooring (Microsoft)
27. Marco Lugo (NextRoll)
28. Caleb Raitto (Google Chrome)
29. David Tam (Relay42)


## Note taker: Joel Meyer


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


## [Suggest agenda items here]



*   Priority and perBuyerGroupLimits [jefftk]
    *   https://github.com/WICG/turtledove/pull/276  
    *   Interaction with multi-seller
        *   IG-level limits?  Timeout?
*   Provide Bid Filtering Reason to Buyers [Supraja]
    *   https://github.com/WICG/turtledove/issues/274
*   Access to data from FLEDGE OT for folks who are step removed. (bmay)
*   Jonasz Pamuła (RTB House): Browser release/update characteristics - For functionalities that are implemented and merged during the Origin Trial, when will they become available on users' browsers? In the subsequent Chrome milestone? Or earlier? For example:
    *   https://chromium-review.googlesource.com/c/chromium/src/+/3570005 
    *   https://chromium-review.googlesource.com/c/chromium/src/+/3553503
    *   https://chromium-review.googlesource.com/c/chromium/src/+/3536593
*   Andrew Pascoe (NextRoll)
    *   [Issue 283](https://github.com/WICG/turtledove/issues/283), Use of Cross-Site Data in Browser
    *   [Issue 269](https://github.com/WICG/turtledove/issues/269), Mechanism for Retroactive Segmentation
    *   [Issue 270](https://github.com/WICG/turtledove/issues/270), Scalable Account Targeting Mechanism


## Notes

Michael Kleber: When you speak, please check the notes to confirm that the scribe got it right and make any corrections necessary. Please sign in if you haven’t already. The agenda is quite long and growing. Before we get into individual issues, Jonasz has some questions about how browser updates and releases work now that we’re in the OT phase. Paul, would you like to start off by addressing that to give folks a good understanding of what is happening there?



*   Jonasz Pamuła (RTB House): Browser release/update characteristics - For functionalities that are implemented and merged during the Origin Trial, when will they become available on users' browsers? In the subsequent Chrome milestone? Or earlier? For example:
    *   https://chromium-review.googlesource.com/c/chromium/src/+/3570005 
    *   https://chromium-review.googlesource.com/c/chromium/src/+/3553503
    *   https://chromium-review.googlesource.com/c/chromium/src/+/3536593

Paul Jensen: Sure, the way Chrome works is that we have a source tree and we commit there and there are a bunch of different release branches. We cut a canary release each night, that’s the most frequent one and the best way to get recent code. It’s also the least stable. The next more stable is the Dev branch, which is also cut from the tip of the tree, similar to the canary release. Every four weeks we cut a branch for the Beta release. When a beta branch is cut, that becomes the stable release branch after a month. There is a very nice tool called Omaha Proxy, https://omahaproxy.appspot.com/, that shows which versions of Chrome that are currently out there and which features are in which releases. Helps to identify which release a CL went into.

Jonasz Pamuła: Thanks for the summary, I think what really matters for us is what version of the code are people running. The release cycle that you described means that once beta is released and people start using it, if a fix or feature for Fledge gets merged, it will only be available on user’s browsers after the next release of the Beta. Is that right? Or is there a way to get a new release to users' browsers faster?

Paul Jensen: At any time we can technically merge CLs into any older release and get it out there. For Beta that can happen for often, for a main release, that is much less frequent.

Jonasz Pamuła: So it doesn’t happen that often to push new code to an already cut branch?

Paul Jensen: Not quite, for beta it happens quite often, for main releases less often.

Jonasz Pamuła: What happens if you merge something for a previous beta release? Do users automatically get it or do they have to take some action? We are wondering if it is necessary to support multiple versions of browsers within the Origin Trial.

Paul Jensen: All the updates for all the browsers on all the channels happen automatically, so users don’t need to take action - they will be prompted to restart. We don’t want people running old versions of browsers for years. If you look at Omaha Proxy you can see how often changes happen. We can’t instantly push out releases, particularly for the stable channel, given the number of users and the bandwidth required. We are also constantly monitoring crash statistics, etc, to determine the quality of releases.

Michael Kleber: To answer Jonasz’s timing questions, if there is a change to the tip of the source tree (like the ones you linked to in your question), it will show up very soon for someone running the dev version. If it’s important enough that we merge it back into the previously cut beta branch, then it will show up in the beta users in about a week (+time to restart), and for stable users that change will usually only become relevant in the next Chrome milestone - eg 1-2 months later, depending on where we are in the release cycle.

Jonasz Pamuła: Thanks, that answers my question.

Michael Kleber: Brian…

Brian May: Yes, we’re talking about the OTs that will combine Fledge and Attribution Reporting, I’m wondering if there is discussion about making data available for those who don’t work closely with the users/browsers.

Michael Kleber: Probably a separate topic, would be good to know what data you need. Let’s defer.

Michael Kleber: Let’s address the agenda in order, starting with topics held over from last time. If the topic is no longer relevant please point that out. First item is JeffTK asking about priority and per buyer group limits.



*   Priority and perBuyerGroupLimits [jefftk]
    *   https://github.com/WICG/turtledove/pull/276  
    *   Interaction with multi-seller
        *   IG-level limits?  Timeout?

Jeff Kaufman: Sure, I think the main thing I wanted to do was flag the issue for the group since there’s a lot of interest in how this works. Great to see the work along these lines. I think it’s important to address the potential performance issues and see if people have any thoughts on it.

John Mooring (JM): Sure, so there are per-buyer limits - is that the number of IG per buyer that could bid? I’m assuming that’s the case and not a time out. In which case, SSPs will be attempting to tune the auction for the amount of time it takes.

Michael Kleber: Is Russ here? No.  Paul?

Paul Jensen: I think right now we did just add a way to limit the number of IGs. This is our first take at this limit. We can certainly adjust it to be timeout based.

Michael Kleber: I believe there is a timeout based limit? There already was a maximum number of milliseconds that an auction could be run, set by the seller in the Auction Config. This is an additional way to limit the # of IGs that participate, going hand-in-hand with some kind of priority system so a buyer could determine which IGs participate based on priority when they encounter a limit.

John Mooring: My understanding for the per-buyer timeout is that it is specific to the execution time for the buyer’s script, which isn’t an effective way to tune the total time they take in an auction. It seems like the missing piece is the ability to tune the total time taken by a buyer in an auction. Don’t want the pub to cancel the auction.

Jeff Kaufman: The pub can’t cancel the auction right now.

John Mooring: Good point, just meant that there was a timeout that the Pub could set.

Jeff Kaufman: The pub can do that, they have a promise that they can race against their own promise, but it’s not a cancellable promise. Fledge can use a huge amount of CPU and there’s no way to tell the browser not to do that.

Michael Kleber: There are a bunch of things we’re talking about,, let’s pull them apart and think about them separately. Jeff is right that Fledge kicks off a  bundle of computation and there is currently no way for the publisher to cancel it all. The publisher can give up and move on, but not stop processing or ask for the best IG ad so far. That seems like a reasonable feature request but we don’t have a way to do it today. The controls we do have are 1st, a timeout in milliseconds that can be set on a global or per-buyer level and stop the bidding script if it goes too long. And we have a way to limit the # of bidding scripts that run per buyer, which means you can limit the total amount of compute time the auction takes. John is correct in pointing out that we don’t have any way of tracking the time taken for a network request. That’s the way it happens for most http requests today, so as soon as you include a script/resource in your page, you’re committed to waiting for it. Browsers generally timeout after 30 seconds of inactivity on a connection. I believe the k/v calls have a similar timeout. We could change that to treat k/v requests separately, but we haven’t done that yet. As we monitor behavior we can determine if we need to do that. The fact that one party's k/v server could slow down rendering of the ad that comes from the Fledge auction is a compelling reason to do that. Generally the idea that "if you’re slow you harm yourself" doesn't apply here (and may not be true in general either). Sorry for the monologue - getting to the raised hands…

Brian May: I’m thinking it would be pretty important to know how often a pub gives up on the Fledge auction and does something else so we have a sense of how usable FLEDGE is. We should have some means of determining that an auction was terminated because the publisher gave up.

Michael Kleber: If we add the ability to terminate the auction we should add the ability to record on that as one of the outcomes of the auction.

Brian May: If we don’t know how often pubs are falling back to the contextual ad we don’t know how successful Fledge is, i.e., whether poor results are because FLEDGE is working poorly or pubs are giving up on it.

Michael Kleber: Right now the way to understand that is to see how often a Fledge auction has a winner that is not rendered. Seems like a situation that ad-tech is probably familiar with because similar things happen today. Eg - pub lazy renders an ad below the fold and the ad is never displayed because the user never scrolls down. So this is, in some sense, a new reason for why that might happen and I agree we can do a better job of reporting it than just using view pings and render pings and leaving a mystery as to the rest.

Brian May: I’m concerned that FLEDGE is creating a larger gap and people will misattribute the reason to FLEDGE.

Jeff Kaufman: Good to pull things apart. I think there is a thing John is trying to get at, that if I only want to render my high priority IGs. I think that there is a sensible approach of using a Timeout. We care much more about how much browser resources we are using and wall clock time we are consuming than the # of IGs.

Michael Kleber: That is a good point. There is a need to be able to timeout after a certain amount of processing time which would incentivize people to make faster IG bidding scripts.

John Mooring: Wanted to note that for some IGs they may short-circuit for the pub or user.  For example, some IG that only bids on a small number of publisher sites.  Doesn't make as much sense if there is a limit on the number of IGs that get to bid.

Supraja Sekhar: Does that include network calls and network call time?

Michael Kleber: Yes, I believe that is what Jeff is getting at. There is a conversation on the pull request on 276 Itself, https://github.com/WICG/turtledove/pull/276, where we can have the discussion about whether some other kind of limit is better than an IG count limit. I will point out that according to undergrad CS classes, the idea of something that allocates and schedules jobs on a finite set of resources is an Operating System, so we are getting dangerously close to having FLEDGE be an OS. We should have Russ weigh in. It might be that the answer is to have more discussion on GitHub.

Andrew Pascoe: A lot of these reasons are why I suggested that a Bandit algorithm might work to address some of these issues. I’ve received some feedback on how to add clearer controls but haven’t had time to iterate. I also don’t think it’s a good idea to implement an OS within the browser. Also wanted to throw out there a new issue #287 Auction Performance Testing, https://github.com/WICG/turtledove/issues/287, which used the API in Chrome to test what would happen if there were a bunch of slow bidders and what the outcomes were in scenarios like that. It might be a useful reference for this discussion.

Michael Kleber: Great, haven’t looked - thanks for pointing it out.

Paul Jensen: I think it sounds good to say we could specify a time for all of one buyer's IGs, but it may get more and more complicated as we progress toward an OS. FLEDGE in Chrome is going to be doing a bunch of concurrent running/execution, and similarly network requests will happen concurrently, so wall clock time isn’t a good indicator. Also, some requests may be shared, so it will be more and more complicated to try and figure this out. The two sets of limits we have now might be a good place to experiment with before we get closer to the OS approach.

Michael Kleber: I agree, starting with something that is the simplest thing that might work and then understanding what deficiencies it has is a natural direction to go and then we can have conversations in the future if the simplest thing doesn’t live up.

Marco Lugo (ML): I do like the idea of compute limits more than the group limits. But in testing FLEDGE I did notice that the CPU you have matters substantially. I think we should take into account that as we consider limits.

Michael Kleber: I agree that compute matters, so compute limits are set by the party running the auction. If that party wants to set different limits for different devices, they can do that. But a ms is a ms, so that should help with not degrading user experience too much.

Andrew Pascoe: How is the pub supposed to the set the compute limits based on device with the plans to restrict the UA information? How can they understand what is even available on the device?

Michael Kleber: Not sure what constraint you’re talking about that would make it harder for a pub to understand how fast this device is. But I expect that there are lots of ways to do it today and there will be in the future.

(Structured User Agent, for any readers.)

Andrew Pascoe: I believe someone on the Chrome team talked about stripping down the UA to only identify some major features.

Michael Kleber: The reduction in UA granularity that is part of the ambient work happening in Chrome and other browsers is primarily about decreasing the amount of potentially device identifying fingerprint surface, sent in the HTTP header. This fingerprinting surface has historically been indiscriminately sprayed all over the entire world. So the work is focused on changing how the device looks to the server.  APIs provided in the JS are different and still exist. I expect that the ability to tell between a modern fast, medium fast, and old slow device will still be possible.

Brian May: My computer and my browser are going to act very differently depending on how many tabs I have open. So it’s not only what machine I have, but also the state of my machine.

Michael Kleber: Thanks for admitting you are a person with so many tabs open, that is courageous of you in a room filled with browser implementers. I still think that moving the conversation to the #276 issue is the right thing to do. Any last words before we move to the next topic on the agenda?



*   Provide Bid Filtering Reason to Buyers [Supraja]
    *   https://github.com/WICG/turtledove/issues/274

Michael Kleber: The next held-over item is Provide Bid Filtering reasons to buyers, #274.

Supraja Sekhar: We spoke about this a couple weeks ago, how will a seller pass the reason why a bid was filtered from the auction, as a part of the auction API. There are external ways for a seller to share this reason with a buyer, but we think it will be most beneficial in the context of Fledge. The GH issue lists a couple reasons we think would fit into the enum a seller could provide to a buyer. It would be good to hear from others on the call if there are other reasons that should go into the enum, and also from the Chrome team.

Michael Kleber: Thanks for opening the issue and starting the list. I think we’ve discussed several times the need for an enum to provide an understanding of what happened. The list we talked about before included many things like various timeouts at the infra layer, script unavail, script failed to load, etc. Then gets into auction dynamics reasons, why a bid might be made and then rejected by the seller. I agree that we should develop a list at greater length. Paul, any thoughts on how we should work to develop this list?

Paul Jensen: The list seems reasonable so far, interested in knowing if people think anything is missing. Let me put it into the chat.

[list here] https://github.com/WICG/turtledove/issues/274



*   Invalid Bid
*   Bid was Below Auction Floor
*   Creative Filtered - Pending or Disapproved by Exchange
*   Creative Filtered - Blocked by Publisher
*   Creative Filtered - Language Exclusions
*   Creative Filtered - Category Exclusions

Andrew Pascoe: Maybe that the pub canceled the promise?

Paul Jensen: As MK was saying these reasons are coming from the seller, but we may have outside reasons why the bid got filtered.

Supraja Sekhar: These are the reasons why the bid got filtered by the Seller in the runAdAuction call. In this case it’s a single bid that would otherwise have been successful but got filtered due to one of these reasons.

Andrew Pascoe: Want to make sure I’m clear, this is for the lowest level auction?

Stan Belov: It’s not necessarily specific to the multi-level auction, but I imagine we’ll start with the simplest level and then look at multi-level auction. If we consider ML auctions, I don’t think the Fledge ever considered moving the info around from the top-seller buyer to the lower level auctions, that seems like it would open a can of worms. How much communication is allowed between a top level seller and a low-level buyer who may not have a business relationship.

Michael Kleber: I think there’s a world where you as a seller in a Component Auction were not chosen in the final auction, but the fact that this occurred seems like an easy thing to include in this enum list. I suppose it’s possible that we don’t need to make an enum list and we could just have the system produce arbitrary strings - anything within 20 chars, say. Is that preferable to us putting together a full list of possible outcomes?

Stan Belov: I think it’s more a question of the privacy characteristics. The potential motivation of having a list is two fold: 1) standardization, and 2) help provide privacy.

Michael Kleber: Yup.

Brian May: Wondering about how this sort of thing might evolve into a fully realized FLEDGE implementation since you’re potentially giving a lot of information to a lot of people.

Supraja Sekhar: If it’s an enum, then it’s a limited set of info.

Brian May: If everyone in the auction gets all the info, that’s a lot of info going around.

Stan Belov: I think this would be under the control of the seller. A seller can choose to provide this to the buyer in its auction. 

Michael Kleber: I do think we need to consider the privacy implications, since the seller will have access to data from multiple different sites and there is a need to consider privacy leakage.

Brian May: Everything that is limited for privacy reasons becomes a confounding aspect for people attempting to determine the behavior of the market they work in.

Joel Meyer: Wanted to agree with Stan: I like an enum for standardization, and it would be scoped to the auction in which a bidder participated.  As a bidder in the component auction, you should be told why we, the owner of a sub-auction, didn't win.  Can we propagate that to our buyers?

Michael Kleber: What reasons do you have in mind?

Joel Meyer: We have losing ads in our component auction, and we can tell buyers why they didn't win, e.g. "brand safety issue".  Now our winner goes into the higher-level auction.  Do we, the runner of the component auction, get that same sort of info about what happens?  Can we pass that along to our buyers?  Today we might not get information about that, but we can hope for something better than today.

Supraja Sekhar: Could be wild west for multi-level auctions

Stan Belov: There certainly are cases where the top-level seller reports its rejection reason to its downstream sellers that are sellers themselves.

Joel Meyer: True, sometimes we can do that today.  Is there a communication channel like that in FLEDGE?

Stan Belov: Right now, doesn't seem like there is any communication channel from seller to buyer for losses.  Do we want to look at it from the perspective of seller-to-buyer and then look at multi-level, or look at the whole thing as a single issue?

Supraja Sekhar: I still have gaps in my understanding of multi-level auctions.  Still need to think about this.

Joel Meyer: As long as that enum is used in top-level and lower-level auctions, seems like a good starting point.  Still an outstanding question of the privacy implications of the channel, which I'd love to hear from Chrome

Brian May: How is this information eventually going to be used?  Eventually we will want to know about a specific situation, whereas to be privacy compliant we'll only be able to tell about classes of situations.

Supraja Sekhar: Can you give an example?

Brian May: You'll say to a buyer "you're losing a lot because your ads aren't being accepted by appropriateness rules", but won't know which publisher without leaking a lot more.

Supraja Sekhar: Sounds like an aggregated reporting API problem.  In the initial FLEDGE trials, you can still get event-level information.  So for now, the buyer function can learn the reason, still fair game while event-level reporting is available.

Brian May: True, but we want to think about the long-term status as well.  We don't want to tie ourselves to a temporary solution.

Andrew Pascoe: On the buyer side, even this kind of aggregated information is still useful.  Buyer who sees their creatives being disapproved a lot — go back to the client, work with them on getting their campaigns changed.  Might want to learn which publishers, of course, but even aggregated stuff is helpful.

Brian May: I agree with that, I’m thinking about the shifting of time and the availability of information. If a campaign runs for 2 weeks and it takes a week to realize that your creatives are being blocked across pubs, that is significant.

Andrew Pascoe: Yea, but that’s kind of the world we’re going into with campaign performance. At least for our business, more info is better. And this is more info, even at an aggregate level.

Brian May: If FLEDGE is going to survive, it’s got to work well enough that people have confidence in it. Otherwise they’ll just say “let’s move everything to contextual.”

Michael Kleber: Thanks for the conversation, I will note that we’re at time. We have pointed out that there is a need for these types of reporting information. Must stop for today. Thanks for joining.
