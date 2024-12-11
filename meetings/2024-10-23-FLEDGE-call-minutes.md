# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Oct 23, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!	 



1. Michael Kleber (Google Privacy Sandbox)
2. Roni Gordon (Index Exchange)
3. Brian May (Unaffiliated)
4. Alex Peckham (Flashtalking)
5. Becky Hatley (Flashtalking)
6. Sven May (Google Privacy Sandbox)
7. Paul Jensen (Google Privacy Sandbox)
8. Anthony Yam (Flashtalking)
9. Luckey Harpley (Remerge.io)
10. Laurentiu Badea (OpenX)
11. Matt Kendall (Index Exchange)
12. Garrett McGrath (Magnite)
13. Brian Schmidt (OpenX)
14. Patrick McCann (Raptive)
15. Harshad Mane (PubMatic)
16. Sid Sahoo (Google Privacy Sandbox)
17. Arthur Coleman (ThinkMedium)
18. David Dabbs (Epsilon)
19. Owen Ridolfi (Flashtalking)
20. Tim Taylor (Flashtalking)
21. Matt Davies (Bidswitch | Criteo)
22. Jeremy Bao (Google Privacy Sandbox)
23. Alonso Velasquez (Google Privacy Sandbox)
24. Rickey Davis (Flashtalking)
25. Aymeric Le Corre (Lucead)
26. Pooja Muhuri (Google Privacy Sandbox)
27. Andrew Pascoe (NextRoll)
28. Kenneth Kharma (OpenX)
29. Victor Pena (Google Privacy Sandbox)
30. Orr Bernstein (Google Privacy Sandbox)
31. Isaac Foster (MSFT Ads)
32. Koji Ota (CyberAgent)
33. Peiwen Hu (Google PRivacy Sandbox)
34. David Tam (paapi.ai)
35. Abishai Gray (Google Privacy Sandbox)
36. Hari Krishna Bikmal (Google Ads)
37. Trenton Starkey (Google Privacy Sandbox)
38. Shivani Sharma (Google Privacy Sandbox)
39. Evan Fosmark (MSFT Ads)


## Note taker: Arthur Coleman 


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here (meetings with no items may be canceled)



*   [10/23] Anthony Yam (Flashtalking)
    *   Continued discussion of / update on this issue: https://github.com/WICG/turtledove/issues/1028 
    *   [kleber: Maybe continue with topic in the future — there was still more discussion to have]
*   Pat McCann: https://github.com/prebid/Prebid.js/issues/11730 and solution at https://github.com/prebid/Prebid.js/pull/12205 [also resolves https://github.com/WICG/turtledove/issues/1093 and https://github.com/WICG/turtledove/issues/851] and raises https://github.com/WICG/turtledove/issues/1298
*   [Charlie Harrison]: Discuss https://github.com/WICG/turtledove/blob/main/PA_private_model_training.md 


# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!


# Notes 


## Pat McCann: https://github.com/prebid/Prebid.js/issues/11730 and solution at https://github.com/prebid/Prebid.js/pull/12205


### [also resolves https://github.com/WICG/turtledove/issues/1093 and https://github.com/WICG/turtledove/issues/851] and raises https://github.com/WICG/turtledove/issues/1298

Michael: New topic.  Let’s start here.  About parallelization of auctions..

[Pat McCann]: We spend time talking about implementing the PA APIs where there are long delays and rendering times.  851 proposed a solution, where auctions are handled simultaneously.  Issue with the proposed submission of header bidding and runAdAuction() simultaneously - resolved in sequence.  In that case, a promise is required for the buyer signals and auction signals and various other things.  We have now implemented that in prebid - issue 12205.  That covers 11730 - makes a proposal to follow some of the Chrome team's suggestions and the Group WP suggestions in Poland.  Also in issue 851.  Also opened issue 1093 and has a workaround of some of our concerns around interest group buyers not being something that can be made a promise.  Then issue 1098, that indicates that the FLEDGE explainer and the actual list of what can be a promise have gotten out of sync.  There also seems to be an additional opportunity to add additional things - some folks on the Chromium team - on the Chrome source code, where you can clearly see what is and is not a promise.  That suggests one or two other things can be a promise in the comments.  If this is what we want to do, then basically we would be in a situation that anything that could be a promise potentially would be a promise.  Appreciate the Chrome team’s entire position in the comments but anything that delays the downloading of decisionlogicJS cannot be a promise - can be a bit problematic.

Wanted to note the implementation is moving forward and bring this to the attention of the forum.

[Michael Kleber]: Thank you for the bunch of time put into this already.  Great improvement to the architecture and performance and latency in the browser.  Has been strange on the Chrome side because, from a latency point of view, if you do the simplistic thing and measure the latency from the time the PA API gets invoked until the ad gets rendered, adding the parallelization capability causes the time to extend.  That's because parallelization involves people calling the API earlier than they would have otherwise.  If you are interested in trying to measure the impact of this parallelization, think carefully about how to use a consistent notion of the starting time to begin from.  It should not depend on whether you choose the early start or the late start time when all the information is available to you.

[Pat]:  One of the things we intended to measure was the time between when you call the ad server and when the server receives its impression beacon.  Both of those events are in the ad server logs- uses a sequential framework for header bidding. Then you do the server side contextual auction, then you call runAdAuction() on that response.  But now you run runAdAuction() at the same time that GAM display is called.  But if you use this alternative prebid product that calls runAdAuction() at the same time or very close to the same time you are making request calls in prebid, the amount of latency added to the total time is quite small.  In order for the Google Ad Manager to support this, you share an API call that the publisher can make.  “Hey ad server, I have incomplete auction configs but I am not planning on submitting any more.”

[Michael]: That is great - progressing as it should be.

[Roni Gordon]: A few things floating around.  Been following the developments on the prebid side.  When we originally discussed this with the Chrome team - there were a number of unknowns as to how to null out the script runners because you have to guess what to do with the interest group buyers before you had the results of the contextual auction.  Strange stuff like having to set the timeouts to zero. Wondering if all that has been sorted?  Changed?  Any guidance on how to solve the static list at contextual time versus the revised list in this new model.

[Paul Jensen]:  The mechanism you mentioned about zeroing out the timeouts is the preferred method.  Interested to hear that there are weird messages. 

 

[Pat] In the last discussion you suggested another preferred method - setting in the seller signals a list of interest group buyers where the desirability would be set to zero and then the seller decision logic would look at seller signals and then zero out the desirability of anyone on that list….

[Paul] Also a way for buyers to set up auctions.  I think that way is a bit less efficient if you are still generating all those bids..  With the timeout it should be more efficient now.

[Michael]  Seems problematic if you are making the buyer submit all those bids and then just throw them away.  Probably not something we would want to suggest you do.

[Roni] Seems like they will all race to get there.  I will bring that back to a dedicated issue.  Seems like the explainer version of that needs updates.  Other issue is related - trying to get a better understanding of the Github issue - where is a lot of back-and-forth - at least on the seller script runners the distinction between pulling the decision logic so you can fetch that and start that versus the trusted scoring signals (TSS) versus the trusted buyer signals which are not used until the auction start.  Assume that is just a superposition of time over this.  What does that mean - does that make sense?  Does it add  a lot of unnecessary complexity? \
 \
https://github.com/WICG/turtledove/blob/main/PA_implementation_overview.md#starting-script-executors \
> Same script origin, same JavaScript URL, WASM URL, trusted signals URL, same frame

[Paul]:  Probably the best way for the implementation to go would be for us to fetch as many reasonable number of decision logic scripts ahead of time that we easily can fetch.  We should do that initially when runAdAuction() is called.  

[Roni] I agree with what Paul just said but the implementation doesn’t match that.  The script runner for the sellers includes the trusted scoring signals URL until they are used after.  And that’s where the complexity comes in.

[Pat] The solution is that the trusted scoring URLS could be a promise.

[Roni]  I’ll drop a link to the explainer for this.  If you look at that it describes how the executors are grabbed and what tuples are required to reuse one. If you look at the seller one it contains all the decision logic you expect plus the trusted scoring signals URL.  

[Paul] I don’t think we allow the trusted scoring signals to be a promise.

[Roni] That's the issue.  It isn’t used at the beginning.  You don’t do anything with those until the bits come back because you don’t have any renderURLs to submit to the TSS.  Is that implementation fine or is there something privacy wise that prevents you from doing something later?

[Paul] Not a privacy thing - a performance thing.  With JavaScript there is a lot we can do when getting the URLs ahead of time.  You can fetch and return to the network.  Then you have to download it.  Then parse and compile the JavaScript, then get a process ready to run JavaScript in the event we do score things later.  Providing the URL to the browser as soon as possible is the best way.  

[Roni] You don’t do work ahead of time with TSS – it’s not used until scoreAd time

[Paul] We may need to pre-check those sorts of things. Not sure we do now.  But - there is going to be some form of benefit by providing URLs sooner than later.  Do you see a reason to provide it later versus sooner?

[Michael]  Can you explain what you want to do with the seller signals URL that makes it beneficial to not say what it is at the beginning?

[Roni] It’s what is required to generate a URL in this version.  It means you have to bring all that code into the adapter awareness before the contextual call starts.

[Michael] So it's about what’s built into prebid.js and shipped that way versus what comes back as part of the response from the buyer and seller servers later on?

[Roni] Right.  The origin has to be attested, it isn’t going anywhere  The decision logic URL is a relatively static CDN asset.  That’s the thing you want to prefetch and cache.  But the KV is very much dependent on the bids you get and their properties.  Putting a non-CDN asset hard-coded into a JavaScript library which is built by publishers and shipped in a way that you can never control and get back creates a huge amount of complexity which makes it difficult to take advantage of the new approach. Very different from today.  Today, I have a list of buyers I can update periodically.  There are ways to cancel out mid-running. Once it is in scoreAd() - I can do whatever I want.  I don’t need the KV results until scoreAd() time.  But because it isn’t a promise I have to figure out how to generate in advance.  So trying to figure out what we can and can't do.

[Michael]  Thank you, that makes sense.  We should look into this as it doesn’t make sense to provide to the TSS early.  But the argument to give it later seems compelling.

I think we will take this back and look at it and see if it can be provided using a promise.  This depends a lot on how things are implemented, I'm not sure what the resolution will be. 

[Roni] Do you want it in the current Github issue or a new one?

[Michael] Not sure.  Paul?

[Paul] New issue is probably better.

[Michael] Thank you  

[Isaac Foster] Question about that was that - the reason to not make something a promise - is that let’s keep things as simple as possible - or is there some bigger concern?

[Michael] Two reasons for not making a promise

1. If information is not available early, then we can’t use it and so it is irrelevant to make it a promise.  Because until that promise is resolved, we can’t start the auction.  Making a promise would not make new processing possible

2. If it provides some substantial implementation barrier for what the browser has to do.  Somewhat of a question of how much benefit there would be versus how much work is involved to change the overall implementation.  On general principals, no objection to turning things into promises and to have them provided as late as reasonable.

[Isaac] I don’t buy #2.  The first one sounds like a footgun concern.  Why would you ever make it that way?  In theory, we should assume ad techs are capable of handling those kinds of decisions.

[Paul]  I would suggest that the performance issues are quite significant.  Now that we allow cross-origin trusted signals, we can’t pre-connect it unless we know what it is.  And pre-connecting is a lot of the network latency.

[Isaac]  I would say keep it simple unless there is a reason not to, especially when we are trying to make the spec broader.  Think that it is an issue when we assume adTechs can’t be trusted to use these tools well.

[Michael] I don’t think it is a distrust of ad tech to use the tools well.  I just think it is nonsensical to make a call to this particular API with a promise, if the API doesn’t do anything at all until that promise resolves. Making something that can be fulfilled with a promise implicitly tells the caller of the API it is ok to call the API before we have a particular value.  But if we don’t resolve the promise until later, then the browser is sitting there doing nothing.  Feels like we have misled the developer.

[David Dabbs]  Let’s go back.  Pat: Is there anything that you’ve said out there that we need from Chrome?  Other than a little filing of rough edges on the spec?

[Pat] We need to get the explainer and the list of what can be a promise to get back into sync. And Chrome team should figure out exactly how we will implement

[David D] Second thing.  There is a request there - to create an issue or PR or is the Chrome team going to work on it separately?

[Pat]  Tied together.  Created issue and then have Chrome prioritize it.

[David D]  A lot of work has gone into this.  This is going to be a heavily relied-upon pattern of use. I don’t know that I can formally make a request - but if there is a WPP platform test or test scenario in your stable it would be great if you could make it available to us. 

Last thing is your initial response about cautioning about how we measure this - if there is an opportunity for some affordance - like standard performance measurements API in the browser?  If there is something that doesn’t add complexity that allows us to capture accurate measurements on the client side, that might be something to investigate.  

[Michael] Good question.  Can’t think of anything along the lines of what you are saying.  There are three points in time.  The last one is easy - when the ad renders.  But there are two earlier points in time - the point at which the way it works today and then the alternate to make the call with parallelization.  The browser can’t magically know both of those.  The browser doesn't know what the hypothetical starting time would be.  We don’t have any good way to measure the right beginning to ending time.  The developer is in a better position to do this.  We love measuring performance stuff in the browser, but in this case we are handicapped.  We need to depend on you as the developer.

This is likely to be the dominant way the API will be implemented in the future.  We are happy on the Chrome side to do more work on this if there are things that still need fixing - so talk to us.  We want this to work well.


## Topic: Continued discussion of / update on this issue: https://github.com/WICG/turtledove/issues/1028 

[Alonso Velasquez]  Moving to next agenda item.  

[Anthony]  I did post a response to the Github issue this morning.  (Anthony shows issue 1028 comment on screen).

[Michael] Did read the thing you posted.  Sounds more like the ads composed of multiple pieces the way we have talked in this group in the past.  

Issue around naming confusion around components. So this notion of the outer HTML that comes from the DSP and inner HTML that comes from the CAS is the approach you described here.  My immediate reaction is that it doesn't grapple with the k-anonymity questions.  It is important that during the auction the browser needs to know which URLs are or are not k-anonymous.  Because it goes to whether the URL wins the auction or not.  So we can’t implicitly do what you suggest in this proposal where we defer the execution of the CAS work until after the auction has ended and rendering has started. Whether the CAS returns a k-anonymous or non-k anonymous URL has backward propagating implications for whether that URL wins the auction or not.  So I don’t think it works in that case.

[Anthony] My response is you are really fixated on the ad URL and that that is the exact creative URL.  That is not how it works today.  Your team has already instructed us to continue to function with shared storage to do this or other options.  The thing you have proposed you don't know what the final creative URL is going to be at the end.  I don’t see how this is any different than that but easier.

[Michael]  I think what I just said is exactly my response to that question.  The fundamental thing we need to do to make an API that has the appropriate privacy properties is to make sure that the URL is prevented from being used on this particular page in a way that it might be a unique identifier for the person. That can’t work.

[Anthony] If we created an ad server interest group and registered the ads on the advertiser site ahead of time, and those pass k-anonymity, why is that an issue?

[Michael] Not sure what you mean by ‘pass k-anonymity’.  K-anonymity means the same ad has to be shown to a number of different people.  When you create the interest group, you don’t know if anyone sees that.

[Anthony] When we call something like selectAdCreative()or whatever this gets called - before that exits - on the first impression that won’t be the case.  Some number of  impressions later, these URLs will have been requested enough times to pass k-anonymity.  And if they don’t, then we are happy to have a default ad that will show, like in shared storage selectURL().

[Alonso] Internally we have been talking about the k-anonymity sequentiality. I think Anthony is talking about the sequence.  But I think some of the sequence he puts here aligns with some of the things we are thinking of.  I think we should ask questions and then come back with further ideas.  I think everyone understands that the k-anonymity check has to happen and that nothing will be displayed if it doesn’t pass the k-anonymity check.  I don’t think that the writeup says a k-anonymity check has to happen at time xyz.  The issue is how does k-anonymity get to evaluate the last URL.  I think we are closer to something that works for conversion.  

[Michael]  I think that is exactly right.  Seems like we should be able to come up with something that works.  But doing the juggling to know that the decision for the appropriate creative  is early enough for the auction to know what the decision is, we need to think about.  But it should be feasible.

[Alonso]  So maybe some question I have - Anthony on the suggestion on the fourth bullet - I think I understand it.  SelectAdCreative() function actually being involved at render time in an iFrame.  Right? (Anthony replies “Yes”).  So one of the things I want to discuss publicly is: in this last step if we can see what the current spec says about impression reporting - fenced frames reporting as we called it although it doesn’t mean you have to have fenced frames to work on it  The party that is the owner of this iFrame is the party ultimately triggering the calls to the servers that have been defined earlier.  So far in the current implementation of PA - it’s based on current DSP tech.  Right now DSPs are the ones aware of having to use that ‘last mile’ delivery.  So I wanted everyone to be aware - let’s say this is the final answer of what we do - it means for the ad server it means you have additional tasks to do to ensure that the SSPs and DSPs can do reporting with sendReport() function.  I want folks to understand that on the DSP side that is different from what happens today.  Making sure people are aware of that.  I am going to put an explainer of what I am saying in the chat.

https://github.com/WICG/turtledove/blob/main/Fenced_Frames_Ads_Reporting.md 

[Anthony] We already have this problem today if we are going to use the proposed approach with shared storage because we would have created an iFrame as the creative ad server and then we would become the owners of that frame.  There has already been discussion about delegating between the ad server and the DSP to track clicks.  Seems it would be nicer if there were multiple parties involved here to make them both be able to run code within the iFrame and do their regular tracking behavior in an iFrame.  Where we can take these steps on our own.  Collaboration is hard - don’t want to have to collaborate with every DSP.  It’s possible but not easy.

[Alonso]  Another thing I want to bring up.  Around Oct/Nov last year we made an enhancement of this particular infrastructure that does allow - at least on the buy side - that the DSP can declare up to 10 additional domains (third parties) that would be allowed to receive this third-party reporting - configured as is right now with reportWin() and reportResult().  I don’t think Shivani Sharma is on call - oh she is - when we think about SSAR and what is out there in the spec now, it has the ability to delegate  what we did to allow buy side macros and multiple destinations.  If we have the interest group owned by the ad server - which is the current proposal - they would also have to be able to say “see up to 10 additional destinations” that could get the reports.  Just like the current design where the DSPs can do it.  I don’t want to oversell or misrepresent what we did - but are we close to that with the current plan?

[Shivani Sharma] Two points:



1. Technically not possible - right now it can only be possible for 10 additional parties - it can only register it and report windows out.  So whichever new functionality we introduce for the new creative selection worklet, we would have to be able support that same functionality in the corresponding reporting for the creative.
2. What we touched upon earlier - the collaboration part.  If it is allowed for the creative selection worklet to mention these 10 parties that can receive something, is that enough for the ecosystem because that would require some up-front collaboration between DSPs.  There is an ecosystem decision here.

[Anthony] The browser knows that. I don’t know how you engineered it - theoretically the browser could know the DSP one even at render time that we called this function.  Now the browser knows there are two parties involved here.  I don’t have the answer, but there must be a way for us both to independently report.

[Shivani]  Right now, there are certain ways the browser allows for different workers to register to get reports - automatic beacons for example. The worklets of the buyer/seller can register with automatic beacons and even if the code inside the frame doesn’t explicitly say I want to send it to the seller or buyer but if it has a header that says allow automatic beacons to be sent, then anyone who registered for it can get reporting through automatic beacon.  At some point, the iframe has to say we can send the information to the seller and buyer.  But the code inside the frame has to do some steps in certain supporting scenarios.  Is the automatic beacons enough or do we need something more?

[Alonso]  It's about making this actionable in the ecosystem.  The other parties that are getting this information - all the additional third parties need to be able to get these reports.  Looking for ideas/feedback.

When the DSP says “I want this specific set of things” - why would automatic beacons not be enough? What use case would not be covered by automatic beacons?  And are any of these things that only the DSP would be aware of?  Example is an ad verifier which is configured by the DSP, but we are thinking about other companies beyond the DSP.  Some parties may be defined on the buy side versus sell side.  I am thinking about more use cases.  Right now we have a solution for ad verifiers.  So for this use case, we need to extract from the DSPs - we need to add 10 destinations for both buy side and sell side. Struggling to understand what change is needed to allow this - not sure we can expect the ecosystem to coordinate in advance

[Shivani] There is a lot of coordination needed.  Even if these 10 entities can receive reports, right now it has to be sent by someone.  It should have been registered earlier and it has to be sent by the code.  Since this is third-party server code, do they have enough knowledge of that code to send the third-party verifier - that is the question.

[Alonso] I don’t expect the ad server to know which third-parties the report should go to - not the DSPs party.

[Anthony]  We’ve already talked about how an ad selection worklet that logic could have access to information provided by the DSP about the impression.  Seems like there is some coordination possible for when the DSP wins the bid, this is code I want executed inside the frame and these are the parties I want the report to go out to.  

[David D]  You are hot on the topic.

[Alonso] This is what DSPs are doing now with these identifiers.  We are going to extract the destination for the interest group.  That is the workflow we have supported today.  Makes sense how the agencies inside the DSPs configure the creative.  I don’t know if they are doing that on the web servers.  Are we aware of agencies adding tags in their requests?

[David] Yes, agencies add a lot of trackers

[Alonso] I think I have another idea for the ad server.  Maybe I can continue with Shivani and Michael offline and then report back.

https://github.com/WICG/turtledove/blob/main/Fenced_Frames_Ads_Reporting.md

[David] If you have something specific - DSPs - please repeat it in the issue 1028.  
