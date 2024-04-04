# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 4pm UTC (during winter)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Feb 21, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	 



1. Michael Kleber (Google Privacy Sandbox)
2. Shankar Venkataraman(Jivox)
3. Jacob Goldman (Google Ad Manager)
4. Ricardo Bentin (Media.net)
5. Ruturaj Vartak (media.net)
6. Laurentiu Badea (OpenX)
7. Alex Peckham (Flashtalking)
8. Becky Hatley (Flashtalking)
9. Harshad Mane (PubMatic)
10. Miguel Morales (IAB Tech Lab)
11. Anthony Yam (Flashtalking)
12. Matt Wilson (NextRoll)
13. Youssef Bourouphael (Google Privacy Sandbox)
14. Laura Morinigo (Samsung)
15. Abishai Gray (Privacy Sandbox)
16. Orr Bernstein (Google Privacy Sandbox)
17. Antoine Niek (Optable)
18. Paul Spadaccini (Flashtalking)
19. Arthur Coleman (OnlineMatters)
20. Amitava Ray Chaudhuri 
21. Russ Hamilton (Google Privacy Sandbox)
22. Pawel Ruchaj (Audigent)
23. Alexandre Nderagakura (Not affiliated)
24. Kevin Lee (Google Privacy Sandbox)
25. Wendell Baker (Yahoo)
26. Zach Edwards (Victory Medium)
27. Tim Taylor (Flashtalking)
28. Denvinn Magsino (Magnite)
29. Jason Lydon (MO/FT)
30. Matt Davies (Criteo | Bidswitch)
31. Alonso Velasquez (Google Privacy Sandbox)
32. Laszlo Szoboszlai (Audigent)
33. Garrett McGrath (Magnite)
34. Guillaume Polaert (Pubstack)
35. Jeroune Rhodes (Google Privacy Sandbox) 
36. Sathish Manickam (Google Privacy Sandbox)
37. David Dabbs (Epsilon)
38. Andrew Pascoe (NextRoll)
39. Ilham Elkatani (Zeta Global)
40. Brian May (dstillery)


## Note taker: Arthur Coleman


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## Suggest agenda items here:



*   Isaac Foster:
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   David Dabbs
    *   Size on bids and macros 
https://github.com/WICG/turtledove/issues/312#issuecomment-1942991181 

*   David Dabbs
    *   Returning Multiple Bids 
https://github.com/WICG/turtledove/issues/595 \
https://issues.chromium.org/issues/323856489
*   Zach Edwards
    *   There was a recent question from the IAB Tech Lab in AdExchanger, "Finally, open source the Google Chrome code used in execution of the Protected Audience APIs. Companies need to feel confident building their businesses on that code, especially when there’s no contract, commercial guarantee or accountability from Chrome."
    *   Are they asking for the existing, "fledge-key-value-service" available @[ https://github.com/privacysandbox/fledge-key-value-service](https://github.com/privacysandbox/fledge-key-value-service) 

        or is there another service for this?

*   David Dabbs
    *   FLEDGE: deprecatedRenderURLReplacements (fka `deprecatedReplaceInURN?)` 
https://issues.chromium.org/issues/325990067 
https://github.com/WICG/turtledove/issues/286


# Announcements


# Notes

W3C rules apply for IP.  Please do not discuss any IP items that you do not want contributed to the public



*   Wendell Baker: There was a trust and safety proposal by Orr Bernstein.  Was that discussed?
    *   Michael Kleber: There was an hour long discussion in this meeting three weeks ago, reviewing Orrs work.  Notes at https://github.com/WICG/turtledove/blob/main/meetings/2024-01-31-FLEDGE-call-minutes.md, PDF of Orr's slides at https://github.com/WICG/turtledove/blob/main/meetings/2024-01-31-FLEDGE-call-slides-creative-scanning.pdf Please take a look

None of people requesting agenda items were in attendance so none of these!


## Laurentiu Badea: Has Chrome started to use the fixed float format for bid values in events because we are seeing discrepancies when we are trying to do reporting?


*   Michael Kleber: For the clearing price values that come out of the auction, yes.  
    *   Russ: We are rounding it to 8 bits of precision - so you get 8 bits in the exponent. It is stochastically calculated, so it should come out ok in the averages
    *   Michael Kleber: Stochastic rounding means that the value that comes out of the auction that is reported at winning time will have 8 bits of precision.  If the bid value you make has 8 bits of precision, the two match.  If you make a bid that is half way between two 8 bit values, it will be rounded up or rounded down half of the time.  If it is closer to one of the two values by say ⅓ to ⅔, it will round up ⅓ of the time and down ⅔ of the time to get a stochastic value.  The point is you should expect that the average of the rounded values will be the same as the average of the original unrounded values.
*   Laurentiu Badea - when will this get finalized?
    *   Russ Hamilton: I don’t know - as long as I’ve been working on it it is 8 bits - we did play for 1 month with 16 bits, but we ended up taking it down to 8.
    *   Laurentiu Badea; Assuming the stochastic averaging - is the same value sent to us, the seller, as  what is sent to the buyer?  That is not what we are finding.
    *   Michael Kleber: That is surprising.  We would expect the rounding to happen only once and be the same for both
    *   Russ Hamilton: It should happen only once and you should get the same values to the buyer and seller. I don’t know why they would be different.
    *   Michael Kleber: Is it possible you have currency conversion going on?  If so, you might have rounding in one currency and then rounding going on in a different currency and so they get out of alignment.
    *   Laurentiu Badea: Have not deployed currency conversion yet.  It looks like a &lt;0.1% variation.  Looks like it is a rounding issue.
    *   Michael Kleber: Sounds like it.  Possibly related to the currency conversion even if everything is in the same currency.  If someone is thinking about working in micro-dollars and another person working in dollars, you might have different rounding

        But this does surprise me.  You should file a bug report and we should investigate on the chrome side.  Give a demo page showing how it happens - will make it easier for us to try and reproduce.  If it is only something happening in aggregate, then anything you can give us to help us resolve it would be appreciated..


        @Russ - how do we recommend people file bugs against the Chrome implementation?  GitHub or crbug.com directly?

    *   Russ Hamilton: I think we should do it as a Github issue.
    *   Michael Kleber: Do it as a Github issue if that is easiest for you
    *   Laurentiu Badea - Thank you for clarifying the expectation and I will go back and see what we can discover
    *   Also the bid values in the for DebuggingOnly reports are full precision, correct?
    *   Michael Kleber: I believe so, yes.  So if you report the exact value with forDebuggingOnly, you should be able to check that the reported values are the result of rounding to 8 bits of mantissa.
    *   David Dabbs::  Is there a Web Platform Test (WPT) for that?
    *   Russ Hamilton: I think there is a web platform test for that
    *   Michael Kleber: Context: The idea of web platform tests is it is a bunch of tests that test different parts of the API and the tests run automatically on the most popular web browser every night - including chrome, safari and edge.  And when it is implemented on different browsers, this is how we make sure the functionality is interoperable between browsers.  Right now Chrome only implements this, so interoperability is not part of the concern yet.  But it's still good to let people see how we are thinking about it and how it should work.

Michael Kleber:  David you are now here! You have agenda items.


## David Dabbs: Size on bids and macros 
https://github.com/WICG/turtledove/issues/312#issuecomment-1942991181



*   Summary of history of topic:
        https://github.com/WICG/turtledove/issues/312#issuecomment-1942991181 is a discussion of how, where, and when to specify ad sizes in auctions, component auctions, and bids.  The discussion was begun in June, 2022 and has since had many of its recommended changes incorporated into the specifications and pushed into chromium.  
        However there was an issue raised in #312 by Eron Castro where inspecting the fenced-frame configuration yielded null results.  Garrett Tanzer replied that Inspecting the width and height fields on the returned FencedFrameConfig is currently nonfunctional in FOT1.  
        This conversation is related to that last discussion.

*   Following up on a comment by Eron  - has that been implemented and is it working now?  Has been put into the explainer.
*   According to the explainer, if one declares the size metadata with your bid and you win, that chrome should replace the ad width and ad height macro in your creative.  I am using an iFrame and Fenced frames, but I am assuming it is all frames.  But I am not seeing the replacement.  Noted in the comment that there are contradicting ways to handle it in the explainer.
*   Alonso Velasquez: We do need Garrett or the engineers for this one.  We’ll have to take it offline.
*   Michael Kleber: We probably just need some demo code and with that it will show better how it will work.
*   Alonso Velasquez: I will have Garrett take a look at this.
*   David Dabbs: Another question.  I was looking ahead to this requirement that sometime in the future that the stated purpose of the metadata declaration is so that a particular renderURL and interest group in Chrome may be rendered into that size so that the magic k-anonymity machine can pre-fetch those k-anonymity calcs of that renderURL and size.  Is that the correct understanding?
    *   Michael Kleber: Something like that.  Certainly the pre-fetching is nice, but the underlying question is why do we care about size being part of the k-anonymity calculation in the first place?  It goes to how much information should it be possible to share with the top level frame versus the inside of the rendered ad frame, which is supposed to know about k-anonymous information coming from advertiser site
    *   David Dabbs: When I went to declare size on the ad plus doing so on the metadata on the renderURLs and the interest group, the size metadata did not come in
    *   Michael Kleber: Are you talking about metadata
    *   David Dabbs: Yes, that info is not making into the worklet right now
    *   David Dabbs: Is this how it will work in the future?
    *   Russ Hamilton: No, we do plan to pipe it in, it just hasn’t been done yet.
    *   David Dabbs: That is good because if we are required to use PA’s metadata system, then it would be good to have that for determining the suitability of renderURLs and not have to figure it out on our own. So good to know.
    *   Russ Hamilton: It will be specified a little differently in the ads than in the interest groups.  That has to do with k-anonymity enforcement.  In the interest group itself there is an abbreviated way to share a list of sizes across multiple ads, but since some ads might be k-anon at some size and others not, we will need to offer a different format.
    *   David Dabbs: That would be good to know as far in advance as possible, because then we can plan ahead.
    *   Russ Hamilton: It is on our roadmap to get done.  We understand the issue
    *   Michael Kleber:  Appreciate that.  We are more focused on more urgent items that are on the cookie removal timeline.  We do plan to get them in plenty of time to make sure people are not surprised in the future.  We will take a look at it again.  Kevin Lee says he will have a demo of how to use size for declarations.
*   David Dabbs: I do have another small question if no one else had one queued up.
*   Michael: Let’s go to Jacob Goldman


## Jacob Godman: Two weeks ago we spoke about timeouts in the reporting functions (https://github.com/WICG/turtledove/issues/959).  I haven’t seen any movement on extending the default 50ms timeouts. 



*   We talked about allowing having settable values between some low and high value in the conversation a couple of meetings back.
    *   We would also be open to a command-line switch to turn off timeouts so for testing we could work with this without worrying about timeouts
*   ?: Don’t think there was any discussion of command line
*   Michael Kleber: Working on timeouts issue.  @Jacob - are you surprised that your Javascript is running for more than 50ms?
    *   Jacob: Yes but we have reproduced that on multiple machines.  I think a possible next step would be investing more in the profile. 
    *   Michael Kleber: Not sure if because you are trying to build a URL that is overly long so it would take a lot of time to process.
    *   Jacob Goldman: We use JSprotocol buffers in some places, but there is an input part as well that we are parsing.  Maybe there is a technical library we can use that is not so resource constrained. 
*   Ilham Elkatani: Want to know how metadata works for ads that are expandable and ads that have a container
    *   Ilham:  We would essentially be leveraging the render width… want to confirm that case. \

    *   Michael Kleber: Please clarify about the kind of ad you are talking about.  Top line answer is that fenced frames are for ads that are a single size, not like iFrames.  Not a match for situations where you have an event that causes the ad to expand or contract.  Reason is because that requires message passing and cooperation between the ad frame and the contents of the surrounding frame and fenced frames are about NOT allowing that communication channel.
    *   Ilham Elkatani: That answers it.  Can the same be said for other types of ads that are companion banners? Or some audio placement but the advertiser is including a companion banner.
    *   Michael Kleber: Again, not sure I understand the question.  I don’t see an inherent problem with that.
    *   Ilhan: How do we render the audio aspect and the length of that audio?
    *   Michael Kleber: That sounds like a step in the direction of the conversation about VAST style video ads that we were talking about last week (NOT briefly).  How does that part of the ad get communicated to the….
        *   Notes from last week are here: https://github.com/WICG/turtledove/blob/main/meetings/2024-02-14-FLEDGE-call-minutes.md#tim-hsieh-updated-proposal-for-video-and-native-support-httpsgithubcomwicgturtledoveissues265issuecomment-1914779917 
    *   Ilhan: I’ll look at those notes.
    *   Michael Kleber: Question you are asking is reasonable.  Generally probably that requires new background thinking is better handled as a github issue so you can include information on how it works today to clarify the question.
    *   Ilhan: Gotcha


## Fenced Frames and rendering and network and ad servers



*   Anthony Yam: Another question on FF (Fenced Frames) for the future.  Right now if we look in shared storage and select the URL, there are limitations like 8 maximum.  If you look at the FF page, there will also be CVMs that are logged and audited, so maybe there is no call to a server at all or is there a worklet in the browser that decides what to render.  Is that the vision for the future - the ad server is never actually called except for logic that defines what goes into shared storage.  If so can we get more access to data
    *   Michael Kleber: This area is somewhat in flux now.  There are multiple different proposals in the last month for ways in which we can make rendering more private but provide more flexibility than what we have proposed before.

        We talked last week about pre-defining URLs.  The answer is that iFrame rendering is still available with regular network access and we need something better. But the details about the long-term rendering things are in flux and we are looking for lots of suggestions that balances all these concerns.  But it is still not resolved.

    *   Anthony: Makes sense.
    *   Michael Kleber: Great question, certainly. The answer is the more information that can be brought to bear on what gets rendered inside the frame the more you have to worry about that information being leaked when rendering happens and so the more you have to have the rendering operation to be more locked down.  Currently the mechanics we have for preventing this is a first step.and none of our approaches is locked down at this point.  But bringing more information to bear at rendering time makes the locking down harder and we need to do more work.

##   David Dabbs: Returning Multiple Bids 
https://github.com/WICG/turtledove/issues/595 
https://issues.chromium.org/issues/323856489
*   Is what you all are implementing - do you have a writeup or is what you are shooting for relates to the last comment you made in that issue? \
It has been an oft-requested feature and seems to be in progress.
    *   Michael Kleber: Yes in progress, Maks is working on it but not on this call.  We heard from multiple bidders to serve ads all at once instead of bid once and then going through the whole process again is preferred.  We are in favor of making things easier, so a reasonable thing to want to do depends on how you implement your bidding function.  If you have already done the work of selecting the bid the first time, we shouldn’t make it necessary to do it again.

        It turns out it is hard to do that in some cases, it is at odds when ads are composed of multiple pieces.  The problem is the number of ads is an exponential of the number of ad components.  Having people do those calculations all at once is not a good thing.


        The mechanic proposed to do all at once will work for some use cases but not others.


        And with other work we have done, people are coming up with other ways to use Protected Audience and when they try to do that and it is reasonable, we try to find a way to support that use case.

    *   Laurentiu Badea:  Want to ask about the repeated call to generateBid() with regard to forDebuggingOnly events.  Do we get events from the first call and from the second call ? Also Which timeout limits are applied given that generateBid() runs twice.  May not be able to complete because of the timeouts?
    *   Michael Kleber: For debugging reports, the system does not distinguish between the two invocations. The debugging report will be treated exactly the same in the first and second rounds.  If you send a reporting event, each reporting event has the ability to do reporting.  @Russ do you know how timeouts work in this case?
    *   Russ Hamilton: For the cumulative timeout, get one timeout for both. In the individual timeout, each reporting call times out individually.
    *   Michael Kleber: We immediately run generateBid() with k-anonymous calculation available to it.
    *   Russ Hamilton: We don’t even rerun the top level script.  We just call the function again. (However, don’t rely on modifications to the script environment, we don’t make any promises not to change that.)
    *   Laurentiu Badea: I have observed that generateBid() may be called with an empty ads array the second time, if there are no k-anon ads left.
    *   Russ Hamilton: In the future we will fix that - it is certainly a bug
    *   Michael Kleber: To clarify, if your interest group is not carrying around any ads, then we already won't ask you to bid.  But if you have ads and none of them are k-anon, then we will ask you to bid a second time, but no ads are available to you.  That second call with empty ads is the bug we will fix.


## Zach Edwards: There was a recent question from the IAB Tech Lab in AdExchanger, "Finally, open source the Google Chrome code used in execution of the Protected Audience APIs. Companies need to feel confident building their businesses on that code, especially when there’s no contract, commercial guarantee or accountability from Chrome."



*   Zach: Are they asking for the existing, "fledge-key-value-service" available @[ https://github.com/privacysandbox/fledge-key-value-service](https://github.com/privacysandbox/fledge-key-value-service) ?  Or is there another service for this? Will PAAPI code be open sourced power articles for the last couple of days?  Will FLEDGE key value service - is that the PAAPI approved code?
*   the article here:[ https://www.adexchanger.com/data-driven-thinking/six-critical-business-challenges-the-privacy-sandbox-must-address/](https://www.adexchanger.com/data-driven-thinking/six-critical-business-challenges-the-privacy-sandbox-must-address/)
*   Michael Kleber: Don’t know what IAB Tech lab is asking about, but the code is part of the Chromium code base and is available to be viewed by anyone.  All code that implements the API is indeed open source.
    *   The state of the KVS right now is that some people are using BYOS to respond to KV requests.  There is an entirely open source implementation of a key value services made for running within a TTE which we have posted 
    *   regarding the k/v service open source blog post:[ https://developers.google.com/privacy-sandbox/blog/open-sourcing-fledge-key-value-service](https://developers.google.com/privacy-sandbox/blog/open-sourcing-fledge-key-value-service)
    *   repo:[ https://github.com/privacysandbox/fledge-key-value-service](https://github.com/privacysandbox/fledge-key-value-service)
*   Shankar: Anyone from IAB to help clarify this?
*   Miguel: From the groups perspective, chromium is open source but the final build may not reflect what is in the open-source code.
*   Michael Kleber: Maybe the suite of tests known as Web Page Tests, which we discussed earlier, is relevant here?  That runs nightly against actual copy of chrome.  
*   Miguel: Yes we do understand the limitation there.
*   Michael Kleber: There is no public versus private version of chrome where PAAPI is implemented differently.  WYSIWYG
*   Miguel: Ideally, a hash that you can check against the executable to make sure we are all on the same release/binary.
*   Michael Kleber: Chrome browser is one big binary and it does have some open source parts and some that are not, so not a verifiable build against that path that I know of.
*   Miguel: I will bring up the test suite with IAB Tech lab and have us implement something around it.
