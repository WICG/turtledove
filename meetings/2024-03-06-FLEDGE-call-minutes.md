# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 4pm UTC (during winter)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


### NOTE FOR EUROPEAN PARTICIPANTS: The US changes our clocks on March 10, so for three weeks beginning March 13, this meeting will be 1hr earlier than usual (i.e. 4pm Paris time, not the usual 5pm)


# Next video-call meeting: Wednesday March 6, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	 



1. Paul Jensen (Google Privacy Sandbox)
2. Brian May (Dstillery)
3. Roni Gordon (Index Exchange)
4. Russ Hamilton (Google Privacy Sandbox)
5. Sven May (Google Privacy Sandbox)
6. Joshua Prismon (Index Exchange)
7. Kevin Lee (Google Privacy Sandbox)
8. Tim Hsieh (Google Ad Manager)
9. Garrett McGrath (Magnite)
10. Youssef Bourouphael (Google Privacy Sandbox)
11. Matt Davies (Criteo | Bidswitch)
12. Arthur Coleman (OnlineMatters/ThinkMedium)
13. Miguel Morales (IAB TechLab)
14. Ricardo Bentin (Media.net)
15. Alexandre Nderagakura (independant)
16. David Dabbs (Epsilon)
17. Paul Spadaccini (Flashtalking)
18. Chris Nachmias (Flashtalking)
19. Tim Taylor (Flashtalking)
20. Isaac Foster (MSFT Ads)
21. Wendell Baker (Yahoo)
22. Rickey Davis (Flashtalking)
23. Fabian Höring (Criteo)
24. Pawel Ruchaj (Audigent)
25. Matt Menke (Google Chrome)
26. Abishai Gray (Google Privacy Sandbox)
27. Orr Bernstein (Google Privacy Sandbox)
28. Qingxin Wu (Google Privacy Sandbox)
29. Alex Peckham (Flashtalking)
30. Courtney Johnson (Google Privacy Sandbox)
31. Anthony Yam (Mediaocean)
32. Andrew Pascoe (NextRoll)
33. Owen Ridolfi (Flashtalking)
34. Becky Hatley (Flashtalking)
35. (Jivox)
36. Taranjit Singh (Jivox)
37. Jon Foust (Google Privacy Sandbox)
38. Zutao Zhu (Google)
39. Shivani Sharma (Google Privacy Sandbox)
40. Patrick McCann (Raptive)
41. Caleb Raitto (Google Chrome)
42. Sathish Manickam (Google Privacy Sandbox)
43. JASON LYDON (yelling from FT)
44. Harshad Mane (PubMatic)
45. Garima Mimani(chrome)
46. Alonso Velasquez (Google Privacy Sandbox)
47. Laura Morinigo (Samsung)


## Note taker: Arthur Coleman


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## Suggest agenda items here:



*   Kevin Lee 
    *   Quick announcement for Privacy Sandbox DevRel team hiring
*   Roni Gordon
    *   reportWin / reportResult execute as part of runAdAuction(), whereas the forDebuggingOnly endpoints only fire when the opaque URN is rendered by the IFRAME
        1. “If the bid being generated or scored loses the auction, the URL will be fetched. These worklets may also call a `forDebuggingOnly.reportAdAuctionWin()` API which operates similarly to `forDebuggingOnly.reportAdAuctionLoss()` API but only fetches the URL after a winning bid or score”
        2. https://github.com/WICG/turtledove/blob/main/FLEDGE.md#7-debugging-extensions
        3. https://github.com/WICG/turtledove/blob/main/Proposed\_First\_FLEDGE\_OT\_Details.md#reporting
            1. “The URL is fetched after the reporting worklet completes.”
        4. _“The sendReportTo() function can be called at most once during a worklet function's execution. The URL is fetched when the frame displaying the ad begins navigating to the ad.”_
        5. TLDR – I’ll likely open a github issue to clarify the timing of these network calls in the explainer
*   David Dabbs
    *   Request to kindly merge clarifying PRs from me and Stan Belov regarding `perBuyerSignals` (I believe mine encompasses his)
        *   DDabbs: [Small clarifications / consistency for arbitrary metadata values](https://github.com/WICG/turtledove/pull/1054)  
        *   Stan: [Clarification for what JSON-serializable values can be](https://github.com/WICG/turtledove/pull/1053)
*   Fabian Höring:
    *   Short term options & MACRO replacement for VAST/Native workflow
    *   Initial Chrome Demo: https://privacy-sandbox-demos.dev/docs/demos/instream-video-ad-multi-seller/
    *   https://github.com/google/ads-privacy/tree/master/proposals/fledge-formats-rendering
    *   https://github.com/WICG/turtledove/issues/265
*   Isaac Foster:
    *   Revisit possible solutions for CDN based static file hosting https://github.com/WICG/turtledove/issues/813
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Anthony Yam (next week, need Michael K):
    *   Revisit / discuss this issue https://github.com/WICG/turtledove/issues/1028#issuecomment-1942746203 


# Announcements


# Notes

As usual, WICG and W3C rules on intellectual property apply

Note daylight savings time change this weekend

[Kevin Lee] 

**Announcement**



*   Hiring for talent in dev rel engineering and tech writers
*   Technical background preferred but not required
*   Familiarity with ad ecosystem very helpful
*   Tech writer job down but will be reposted
*   [kevinkiklee@google.com](mailto:kevinkiklee@google.com) to get started

**Roni Gordon**



*   reportWin / reportResult execute as part of runAdAuction(), whereas the forDebuggingOnly endpoints only fire when the opaque URN is rendered by the IFRAME

[Roni]



*   Not entirely clear from documents when these things happen
*   sendReport() clear but others are not
*   Will open a Github issue to have explainer updated
*   David making similar issues and comments
*   The forDebugging endpoints are not being executed when auctionwinner is determined
    *   If the win fires then why would the forDebugging not fire at the same time?
*   If you use -----, you will never see any call backs
*   

[Paul Jensen]



*   Feel free to file an issue

[Isaac Foster]



*   Very good to clarify that
*   Would be helpful: to understand a bit better what the planned timeline for support for the win and the loss side of the reporting (for reportAdAuctionWin and reportAdAuctionLoss in forDebugging

[Paul]



*   Most of reporting sent out when add begins to render/network fetch
*   Desire to keep reporting tied to that
*   We do run the reporting worklets asap after the auction so when it is time to send reports they are ready to go

[Isaac]



*   Two questions
    *   1. When in the auction flow should forDebugging fire?
    *   2. When will forDebugging go away?

[Paul]



*   https://groups.google.com/a/chromium.org/g/blink-dev/c/3RUQk0GCC9Q/m/EF\_wChk\_AQAJ
*   Post I made 
*   Back history
    *   Added forDebugOnly API even before the origin trial
    *   A way to debug what’s going on inside the auction
    *   Left it in for awhile
    *   We originally intended to remove these APIs at third-party cookie deprecation (3PCD) time, but received feedback that they were essential for doing root cause analysis in escalation situations (i.e. critical crash).  Instead of removing debug reports entirely, we now plan to heavily downsample them at 3PCD
    *   Represents information from contextual and advertiser side
    *   We don’t want it to be in the final version
    *   Plans for getting rid of it: now plan to move to downsample along with 3PCD.  We will turn on downsampling at that time
    *   Instead of sent out all the time, will be sent out rarely

[Isaac]



*   It represents a huge privacy issue so we should get rid of it
*   Unclear if that means that the browser that has moved to deprecation will move to downsampling
*   Is the 3PCD timeline 2025? 2026?  Not clear on timeline

[Paul]



*   Not sure but ----- I believe in Mode A and B testing, forDebugOnly is not downsampled.
*   When 3PCD will happen - we’ve discussed a few times
*   Don’t have an exact answer right now - will be clear later this year

[Isaac]



*   Even saying “we maintain the flexibility to remove this at any time” would be help[ful
*   I would rather not use this for production purposes if it is not reliably going to be around
*   But if it is, I will get questions as to why we are not using it
*   By “it” I am talking about moving to downsampling

[Paul]



*   Downsampling after cookie deprecation time
*   Will be announcements later this year

[ Roni]



*   Issue is what you define as 3PCD “time”?
*   Understanding what you are saying is that “you are not making this change at all until 100% deprecation occurs”

[Alonso Valazquez]



*   Precise definition: hard to give you the exact timeline
*   There are other APIs that we are dependent on so hard to give more precise timing
*   ACKNOWLEDGE: that more precise guidance is needed.  Working on getting this not only for forDebugging only but other features
*   Need is acknowledged

[Roni]



*   Nothing changed before Q4?

[Alonso]



*   Have to get back to you on this
*   Thanks for reminding us we need to do this 

[Isaac]



*   Would it be reasonable to say “the APIs that exist today are treated as a live API and will not be removed without an actual definition of what the real date is with sufficient notification”?
*   And how much in advance will notification occur?
*   Will you treat it as a breaking change for example?  If an alpha feature that can go away at any time, that is different.

[Alonso]



*   Still TBD
*   Paul?  

[Paul]



*   Any visible change we make will go through regular release process 
*   We will do that with 3PCD and we will give more guidance as to other items that go along with that
*   But will have to get back to you with clear guidance

[Roni]



*   Today not downsampled for anyone with any cohort at all?
*   And it will stay as it is today until 3PCD?

[Paul]



*   No to first question
*   And yes to second

[Patrick]



*   Question on TEE and KVS and timeline for Aggregation API

[Alonso]



*   Anything else that has the same dependency would be on the same timeline
*   Right now the Protected Audiences team is ----- this one API going to (down?)sampling

[Isaac]



*   Understanding if moving to downsampling to any cohort would be treated as a breaking change and then what the publicized process is for that breaking change - know you can’t give a date, but if you can say “we are treating as a breaking change” that would be great

[Alonso]



*   Fair feedback

[Brian May]



*   Let's not coincide this change to removing the forDebugging API with 3PCD 
*   Allowing people to see into the back end of what the change to 3PCD will be and be able to analyze still very helpful

[Paul]



*   May be reasons in opposite direction for now

[Brian]



*   I thought we were removing it because of information flow/privacy concerns
*   But argument can be made on the other side that keeping it for some additional period of time would be useful
*   Cutting everything off simultaneously that will throw us into chaos

[Alonso]



*   So keeping forDebugging - you mean just see how sampling will work or be able to understand broader functionality?

[Brian]



*   Broader - ability to look behind the curtain at anything

[Alonso]



*   For this API that’s why we have put a label in that says ”this has been sampled’

[Brian]



*   That is useful and helpful, still  think value in keeping the API around to see things post cookie information channel loss.

[Alonso]



*   We hear you
*   Labels there to provide that

[Paul



*   We will get back to you with a more concrete timeline

**<span style="text-decoration:underline;">David Dabbs</span>**

Request to kindly merge clarifying PRs from me and Stan Belov regarding `perBuyerSignals` (I believe mine encompasses his)



*   Discussion of textual consistency changes to FLEDGE.md contained in Issues #1053 and #1054.

[David]



*   (having mic issues) 
*   In chat  \
Paul I saw your response (and also Caleb’s) on the PR and have posted a response.  \
Please proceed to other agenda topics.  

**Isaac Foster:**

Revisit possible solutions for CDN based static file hosting https://github.com/WICG/turtledove/issues/813 



*   This question relates to the issue of same-origin constraints on _biddingLogicUrl, biddingWasmHelperUrl, dailyUpdateUrl _and_ trustedBiddingSignalsUrl_.  This constraint has been applied to both the interest group owner and SSPs. There is a discussion in https://github.com/WICG/turtledove/issues/421 about relaxing the constraint for interest group owners.  813 is asking for a similar relaxation of the constraint for others in the ad tech supply chain.  The challenge in doing so is that it violates some core security constraints imposed in the browser that cannot be changed.
*   [Isaac]
    *   This has been coming up in internal discussions
    *   Having CDN hosting and still relatively high volume stuff
    *   Having to shove those into the same load balancers - this will become a bigger issue for us at scale
    *   Can try some code fixes
    *   Curious if you have updates and to discuss redirects which gives you some control on the server
*   [Paul]
    *   Need to spend more time looking at this
    *   From conversation with other teams, arrived at a solution that would allow for bidding scripts, bidding signals, etc coming from different origins
    *   Started writing up a response, wanted to get feedback on accuracy first and no major security problems
    *   Can run through it quickly
        6. Two things we are trying to protect
            2. Making sure the origin being fetched from allows their response to be shared with the origin that will receive it.
            3. Making sure the origin receiving the fetched response understands which origin it originates from, thus giving them the ability to only allow responses from certain origins.

        I think we can support this extension given a few additional restrictions:

        7. Use CORS for fetching trusted signals, specifying an `Origin` header with the origin of the bidding/scoring script that will receive the signals.  This helps to address concern #1.
        8. When the trusted scoring signals origin doesn't match the scoring script origin, require the scoring script fetch to return a response header indicating allowed trusted scoring signals origins.  This helps to address concern #2.
        9. When trusted signals origin doesn't match bidding/scoring script origin, how the signals are presented to the script so they are not confused for a same-origin response, for example add a new parameter to generateBid()/scoreAd() that is a map from the origin returning the data to the data returned.  This helps to address concern #2.
        10. To avoid unnecessary CORS preflight checks we should consider whether we should safelist any existing headers.
    *   [Issac]
        11. Buyer gets to say who the scoring signals are coming from?
    *   [Paul]
        12. That helps us determine who that is going to 
        13. If the origin not the same as the script origin, in those cases we should probably change how those signals are returned
        14. Now for allowing to be a different origin, especially true for scoring script, you don’t know if auction signals have been modified
        15. We should probably move that to a different parameter that gets passed to the script
        16. Example: third argument of the trustedBiddingSignal
            4. For not the same origin, we pass an extra parameter that maps to the origin of the scoring signals themselves
        17. When we say we are going to start using CORS for this, the default fetching mode, possibility to use no-cors - so should we use CORS anyway
            5. CORS provides a check you might not expect that would break fetching
            6. Want to make sure that it isn’t impacting latency if we use CORS
            7. CORS has safe-listed ways of doing things that should work for our API
            8. Also a set of headers that are safe-listed, and we probably should update that list to include headers we are using for Protected Audiences
            9. CORS is a normal request if all headers safe-listed, all calls are safe-listed
        18. END RESULT: Think we can support requested changes to Issue #813 by using CORS tests
    *   [David Dabbs]
        19. Martin Thompson suggested use of query method - does a move to that open avenues or relax this weird case?
    *   [Paul]
        20. Not expert on query method
        21. Not the safest of methods that I saw but not as familiar with it
    *   [David]
        22. Context for the comment: might be more appropriate for caching semantics, and here we are talking about the safety of the request
        23. But conceptually it is a query
    *   [Paul]
        24. Did come up a couple of times when thinking about this
        25. Today fetches are GET requests - they are CORS safe-listed, which means they can be cached.  An important part of Protected Audiences
        26. We thought about this awhile ago - we posted Key Value Server 2.0 API.  Does not require switching to a POST request
        27. So we have been thinking about how we can be caching those requests and the CORS requirements
        28. V2.0 of this API should work for this (CORS safe listing as well)
        29. 
    *   [Brian May]
        30. Fairly dense topic - lots of moving parts
        31. Can you do a presentation on your proposal so we all get a clear picture
    *   [Paul]
        32. Sure - haven’t posted yet.
    *   [Brian]
        33. Most service providers in adTech will want to do cross-origin stuff - easier, more flexible, cheaper
    *   [Roni]
        34. 100% agree
    *   [Isaac]
        35. 100% agree.  And need it documented sufficiently in advance before you implement CORS so we can update our infrastructure in advance
        36. I would love a diagram of this
        37. On caching 100% agree on scoring script
        38. TrustedBiddingSIgnals a little different- not sure we will do a lot of caching
    *   [Paul]
        39. On caching: the getURL request - not really cacheable across publishers, and maybe you don’t want them to be
        40. One place where caching is important to them is where there are multiple slots on one page with multiple auctions - very useful to cache briefly - much more efficient
    *   [Isaac]
        41. Agree and take point.  Of course, if you had implemented the request for multiple auctions in single…
    *   [Brian]
        42. Seems like good idea for browser team to draw picture of what this would look like so adTech folks can give feedback on where browser view does not work with adTech view
    *   [Paul]
        43. Probably something useful to include in docs
    *   [Isaac]
        44. Thank you for progress on this.  Despite comments, will make many people happy
    *   [Brian]
        45. Kind of a critical part of getting things ready for production
    *   [Paul]
        46. Very tricky and easy to get security issues wrong with disastrous results. Which is why we are being so thoughtful.
*   

**Paul Jensen**

Reporting and top-level worklet timeouts #959



*   This goes back to a discussion of [Issue #959](https://github.com/WICG/turtledove/issues/959) from Jacob Goldman on [2024-02-07 meeting](https://github.com/WICG/turtledove/blob/main/meetings/2024-02-07-FLEDGE-call-minutes.md) about differential timeouts that the seller can specify not working when extended to reporting.  There is a fixed 50ms timeout in the reporting worklet that is causing reports to be dropped 10-20% of the time.

[Paul]:



*   Timeouts for reporting scripts was discussed a few meetings ago - the 50ms timeout
*   Trying to think about ways to fix that
*   Ideas are thinking about raised some questions I’d like feedback on:
    *   How customizable do we want that timeout to be?
        *   Different for different reports and different providers - make it complex but very customizable?
        *   Or make it very simple that applies to all reports and all auctions?
*   Do any SSPs on call - how specific do you want this

[Isaac]



*   Are we trying to make sure that reporting function timeout per buyer - is that it?

[Paul]



*   Yes that’s the question
*   We want to give enough time for reporting to complete

[Isaac]



*   Don’t really want to do per buyer - the page experience can get messed up in a differential way as a result

[Patrick]



*   The ---- API has a property so that it -----

[David]



*   I thought these requests outlived the frame host

[Brian]



*   Maybe consolidate how we get these signals to the people who want them

[Roni]



*   Seems odd to define it as a seller
*   But to be clear, this isn’t on a hot path - its running in the background
*   It’s more about ensuring that they don’t get prematurely terminated before report can be generated than the timeout per se

[Paul]



*   Yes (to Isaac’s point) we do need to worry about degrading the user experience
*   I will think more about that

[David]



*   Something simple that removes the doubt

[Brian]

ARA has a function that it reports when it can if it can’t report right away. Maybe do something similar to that.
