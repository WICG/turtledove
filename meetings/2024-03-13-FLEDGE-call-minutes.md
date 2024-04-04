# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 4pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


### NOTE FOR EUROPEAN PARTICIPANTS: US clocks have changed, so this meeting will be 1hr earlier than usual (i.e. 4pm Paris time, not the usual 5pm) for the rest of March.


# Next video-call meeting: Wednesday March 13, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	 



1. Michael Kleber (Google Privacy Sandbox)
2. Sven May (Google Privacy Sandbox)
3. Brian May (Dstillery)
4. Roni Gordon (Index Exchange)
5. Harshad Mane (PubMatic)
6. Shankar Venkataraman (Jivox)
7. Ricardo Bentin (Media.net)
8. Wendell Baker (Yahoo)
9. Laurentiu Badea (OpenX)
10. Brian Schmidt (OpenX)
11. Miguel Morales (IAB TechLab)
12. Alex Peckham (Flashtalking)
13. Arthur Coleman (OnlineMatters)
14. Paul Spadaccini (Flashtalking)
15. Sarah Harris (Flashtalking)
16. Becky Hatley (Flashtalking)
17. Isaac Foster (MSFT Ads)
18. Jason Lydon (Flashtalking)
19. Shafir Uddin (Raptive)
20. Denvinn Magsino (Magnite)
21. Tim Hsieh (Google Ad Manager)
22. Joshua Prismon (Index Exchange)
23. Sathish Manickam (Google Privacy Sandbox)
24. Paul Jensen (Google Privacy Sandbox)
25. Don Marti (Raptive)
26. Alexandre Nderagakura
27. Kenneth Kharma (OpenX)
28. David Dabbs (Epsilon)
29. Courtney Johnson (Google Privacy Sandbox)
30. Vedant Seta (Media.net)
31. Daniel Rojas (Google Chrome)
32. Abhinav Sinha (PubMatic )
33. Andrew Pascoe (NextRoll)
34. Abishai Gray (Google Privacy Sandbox)
35. Sid Sahoo (Google Chrome)
36. Russ Hamilton (Google Privacy Sandbox)
37. Taranjit Singh (Jivox)


## Note taker: Arthur Coleman


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## Suggest agenda items here:



*   kleber notes:
    *   New forum rule: please no job announcements
    *   please check over notes after the fact
*   Fabian Höring:
    *   Short term options & MACRO replacement for VAST/Native workflow
    *   Initial Chrome Demo: https://privacy-sandbox-demos.dev/docs/demos/instream-video-ad-multi-seller/
    *   https://github.com/google/ads-privacy/tree/master/proposals/fledge-formats-rendering
    *   https://github.com/WICG/turtledove/issues/265 \

*   Anthony Yam (For 3/20, need Michael K and Anthony):
    *   Revisit / discuss this issue https://github.com/WICG/turtledove/issues/1028#issuecomment-1942746203 
*   Isaac Foster:
    *   Just added, don’t need to do today obvs: clarify deprecatedReplaceInUrn breadth of support
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Laurentiu Badea
    *   Views on reusing auction configs and signals - encouraged / discouraged / wanted / unwanted ? Follow-ups
        *   need an auction counter signal when component auction is reused
        *   need a way for component auction (or buyer ?) to indicate single use
    *   (for next session) Allow multiple (at least 2) notification urls per worklet via `sendReportTo`, to support win notification from exchange to bidders, or from seller to third parties intermediating the igbid, for example: (https://github.com/google/ads-privacy/tree/master/proposals/fledge-rtb#impression-notifications-from-exchanges-to-bidders) via `igbid[].igbuyer[].igburl` 
*   David Dabbs
    *   [Feature request: include Chrome-facilitated testing label in trusted server requests](https://github.com/WICG/turtledove/issues/946) \
(#946) You have approved sending the labels in the trusted fetches, following Michael Kleber’s rationale, why not then all PAAPI requests?
    *   [Client Hints HTTP Headers in Buyer Trusted Server Calls](https://github.com/WICG/turtledove/issues/1031) (#1031) \
If Chrome permits the test labels, the same should apply to low entropy UACH, yes?
    *   [Reporting and Top-level Execution Timeouts](https://github.com/WICG/turtledove/issues/959)  \
Are you moving ahead as described in Paul’s final comment? \
 \



# Announcements


# Notes

All comments are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/

Announcements



*   [Michael Kleber]
    *   Please make sure you go back and update/correct your notes, especially if you speak at the meeting.
    *   There was push back against job announcements in this forum, so since this has made people uncomfortable I'm proposing that we should stop the practice
*   [Brian May]
    *   Businesses may be reluctant to have employees join the calls if they think they might be be recruited
*   [Wendell Baker]
    *   Anything that indicates that folks are interested in this tech, I think it is ok for others other than Google
    *   [edited W.]Yahoo is looking for expressions of interest in the investment towards Google Privacy Sandbox technologies and other such market-making technologies throughout the industry. Job postings are one avenue for identifying commitment towards the future of this technology.
*   [Michael]
    *   Google's job postings are all on [google.com/careers](google.com/careers), and if anyone gets reassurance from the fact that we're hiring, searching for Privacy Sandbox there may bring you joy.  But this isn’t the right forum, as we don’t want people to worry about being solicited.


## Laurentiu Badea: Views on reusing auction configs and signals 



*   Is reuse across auctions encouraged / discouraged / wanted / unwanted ? 
    *   need an auction counter signal when component auction is reused
    *   need a way for component auction (or buyer ?) to indicate single use
*   Allow multiple (at least 2) notification urls per worklet via `sendReportTo`, to support win notification from exchange to bidders, or from seller to third parties intermediating the igbid, for example: (https://github.com/google/ads-privacy/tree/master/proposals/fledge-rtb#impression-notifications-from-exchanges-to-bidders) via `igbid[].igbuyer[].igburl` 
*   [Laurentiu Badea]
    *   What is the WICG group viewpoint on reusing auction configs and signals - encouraged / discouraged / wanted / unwanted ? Follow-up
        *   Some people don’t want to reuse
        *   Is there a way to know this - to make them only used once

    [Michael Kleber]

*   Looks to me like this is a question that the browser has nothing to do with  
*   Question seems more about how SSPs deal with information that they get from their interaction through DSPs and whether there is some caching of signal reuse there
*   Since this is a subject that is mostly focused on how different adTechs choose to interact with each other, not sure this is the right place to discuss.  Maybe IAB has a better forum?
*   Happy to spend some time on this, but here we are generally browser-focused

    [Laurentiu]

*   For example, for negative bids, the browser does provide a nonce to make sure they are only used once
*   As a buyer or seller for a regular auction, we are using the config blindly.  There is no way for the scoreAd() function know that it has already scored bids in the same auction where it has used these parameters before
*   It is appropriate here because browser is making the functions run in a restricted environment where this can’t be determined

    [Michael]

*   Cases where the browser has some sort of control or understanding what a ‘single time’ means is restricted to cases where the browser has interacted with the server.
*   The use of the nonce works well if someone takes it uses it, and hands it back.  So it lets the SSP be in control of what auction the bid is put into.
*   If the issue is if something coming from the DSP is reused, the browser is not interacting with the DSP server
*   Browser is not in a good position to know if reuse is happening, or to look for a nonce
*   An SSP could change a nonce if they chose to, the browser is not in a good position to defend against that
*   Cases where the browser has an opportunity to ensure that data is passing through some system uncorrupted, are restricted to situations where the browser and a certain server’s domains interact
*   That’s where we focus our attention
*   If additional tools help with that, happy to consider them

    [Brian May]

*   You mentioned that maybe the IAB is the right place to discuss this. Is there a channel with the IAB which can be used to communicate things like this when we discover them?

    [Michael]

*   I have no formal channel. Happy to have other folks bring things to IAB's attention

    [Roni Gordon]

*   Not sure I understand why browser is not involved
*   Worklets can’t keep track of anything, for example
*   Even if an an exchange passes an identifier, how can I identify auction config from two bids in the same auction

    [Michael]

*   Confused on whether SSP or DSP point of view

    [Roni]

*   As a component seller, I return an auction config and it can be used only once

    [Michael]

*   If you are talking about some random party on the publisher page who is doing something unexpected with the configs that come from the SPPs, there is an opportunity for the browser to help with that
*   How do we lock down a communications channel so it can’t be corrupted. directFromSellerSignals was a step in that direction
*   If it is helpful for there to be more direct-from-seller information channel to set up in a way that prevents them from being misused or mutated, that seems like a reasonable feature request
*   A request like that is in line with items we have had in the past
*   Maybe that would help with what Roni is talking about
*   I don’t think the case where SSP received signals from DSP and chooses to use them in multiple auctions is appropriate because there is no way for the browser to ensure the integrity in that case

    [Laurentiu]

*   My bad - I bundled two issues together, this applies to both SSP and DSP, it is the same problem
*   This is handled by the top level seller, who provides a mechanism to populate the component auctions, but no way to reset them once used.
*   Creates a situation that once the auction signals are used on a slot they may end up being used again for repeated auctions which will then have to be deduplicated by both buyer and component sellers

    [Michael]

*   The more we talk about this issue, the more I don’t understand the underlying problem!
*   Is this a situation where an ad slot on a page automatically refreshes once a minute and the auction config is reused over and over again

    [Laurentiu]

*   Yes

    [Roni]

*   Not a malicious use, just a normal configuration.

    [Michael]

*   Not a malicious situation, then maybe we are pushing back in the direction that there should be some kind of IAB standard and some indication by buyer or by component seller if an auction config can be reused

    [Shankar Venkataraman]

*   Some of the uses of auction config which buyer and seller can actually use……

    [Michael]

*   Any kind of auction config is in the hands of whoever calls the API
*   Reuse we are talking about is not inside the bowels of the browser - it is in the hands of someone who is calling the APIs
*   If they use the auction config multiple times, the browser doesn’t know this
*   It seems like the primary issue is to express the intent of the component seller
*   Sounds like adtechs talking to other adTechs about issues they should support

    [Brian]

*   Don’t see that the browser adds any value here, it isn’t in a position of control.

    [Michael]

*   Definition of what we are talking about seems to have changed 2-3 times in discussion
*   If the browser can help, willing to consider
*   Just doesn’t seem like a great use case
*   Anyone else have any ideas?
*   This discussion presents an interesting situation.  It sounds like the discussion is an issue of everyone being early on in adapting to the APIs and getting comfortable with them

    [Shankar]

*   I think Miguel Morales from IAB is on the call if we want to give them feedback.

    [Laurentiu]

*   Is a timestamp available in the sandbox?

    [Michael]

*   It can be passed into the bidding function

    [Laurentiu]

*   That could be used to determine if config is stale or not

    [Michael]

*   The time the browser talks directly to a DSP's server is to the Key Value server
*   You could have a key called "time" that you put in every interest group, and your KV server could send the current time directly to the bidding function.
*   You can also include a timestamp when you build your auction config in your server.
*   By comparing those two timestamps, you could notice if an old auction config is being used more than a minute after it was created, or whatever
*   That is the sort of solution that is available using the tools in the browser today.  Seems like it could be useful in the situation you are talking about

    [Laurentiu]

*   That is an interesting albeit contrived avenue

    [Michael]

*   I agree, feels contrived
*   Adtechs should try to make it clear how they want their configs used

    [Greenwich Conference Room]

*   Paul Jensen and Michael Kleber “talk amongst themselves” - hard to hear

    [Brian]

*   Sounds like we should go back to the ad tech side of things, discuss, and come back with a clear request

    [Michael]

*   Happy to respond if you give us a formal request like that


## David Dabbs [Feature request: include Chrome-facilitated testing label in trusted server requests](https://github.com/WICG/turtledove/issues/946) (#946)

You have approved sending the labels in the trusted fetches, following Michael Kleber’s rationale, why not then all PAAPI requests? \



    [David Dabbs]



*   Mike Taylor gave the go ahead to give the testing labels with new rationale
*   Is it ok for the trustedBiddingSignals(), then why not all the requests?

    [Michael]

*   What is the question?  What other requests are you thinking about?

    [David]

*   Labels are not provided to requests other than trustedBiddingSignals()
*   If it's ok to send rationale in the TBS request, why not provide it for other requests?

    [Michael]

*   The item you are talking about is an opt-in header
*   We don’t want to send to anyone who is not asking for it
*   GDPR sensitive countries expressed concern in the past about handing them information about the user that they don’t need or want

    [David]

*   Russ Hamilton (not Mike Taylor, oops) commented that based on feedback from testers, we are going to send the labels in the header
*   Sounds like they are going ahead and doing that
*   Irrespective of geographic location

    [Michael]

*   What makes that viable is it is only happening for people who opt-in to receive the requests

    [David]

*   That’s what I’m asking - only those in scope of PA

    [Michael]

*   Which ones?

    [David]

*   All of them

    [Russ Hamilton]

*   Creatives are navigations so they’re covered by the existing opt-in. Trusted signals fetches are triggered by the browser so they are not covered by the existing opt-in.

    [David]

*   I’ve opted in as a scoread() - we are over the threshold, I didn’t think of the the creative fetches as being a navigation
*   I don’t know how we would have opted in as a DSP
*   If we are using the same domain, we would have to opt in
*   Leaving that aside of a moment, there is a sImilar question:
    *   Generic client hints on the trusted bedding signals

    [Paul Jensen]

*   You have access to data you put into the browser
*   Can store information at the time you put a person into an interest group


## David Dabbs: [Client Hints HTTP Headers in Buyer Trusted Server Calls](https://github.com/WICG/turtledove/issues/1031) (#1031) \
If Chrome permits the test labels, the same should apply to low entropy UACH, yes?


    [David]



*   Second question: generic client hints
*   Originally supposed to be client hints but they are not
*   In #1031, is there any reason low-entropy client hints can’t be provided
*   I am assuming your response would be instead of a pre-cursor call, you would send it through the auction

    [Michael]

*   If we are talking about things that are not in the call
*   The trusted server calls are different, so that is a good place to have the discussion

    [David]

*   There is a new issue in chromium.  Maybe I should go look at that and make comments there


## David Dabbs: [Reporting and Top-level Execution Timeouts](https://github.com/WICG/turtledove/issues/959) #959

Are you moving ahead as described in Paul’s final comment?


    [David]



*   New topic - execution timeouts
*   Is Paul’s final comment where you decided to move forward
*   Sounded like it was settled
*   Or is there more discussion?

    [Paul]

*   We are still thinking about ways to address this
*   Different than a typical resource fetch

    [David]

*   Comes from the browser not the back end?

    [Paul]

*   Comes from a different process so doesn’t look the same
*   I proposed a couple of different approaches
*   We thought we would move forward with it

    [David]

*   Good to know
*   Takes away from the doubt that Jacob initially posted about it
*   Sounds like we are moving forward with something similar to what we proposed here
*   Didn’t think Google had caught the subtlety

    [Michael]

*   When we propose a possible solution to something that has been asked and there is other feedback that says “this looks helpful” we try to respond
*   We don’t always understand the issue in enough detail that what we propose doesn’t work
*   So please use Github to respond back consistently so we truly understand what the ask is

    [Laurentiu]

*   Wanted to add a piece of data on the reporting timeout issue
*   The timeout - whether we receive the win events or not - is adversely affected by the size of the script and it shouldn't be if timeout measures the function execution
*   Maybe a solution is to tighten up what the timeout really means

    [Russ]

*   The bidding script and reporting script are the same script, so both get parsed here 
*   If you have a bigger script, it takes longer to instantiate the objects and so it takes longer
*   There was also a proposal to separate bidding and reporting scripts which may help since bidding scripts tend to be much larger

    [Michael]

*   That is true

    [Laurentiu]

*   Thought code is frozen in sandbox and gets reused 
*   Is that not the case with reportResult()

    [Russ]

*   It may not be re-fetched and recompiled
*   We make best effort to reuse if we have it downloaded
*   We need to make sure JS environment is fresh and in consistent state
*   Can’t call a function and have same JS environment call another function
*   In general, have to execute top-level JS and then call the function each time

    [Laurentiu]

*   Even for scoring function?  That seems odd

    [Russ]

*   Yes we have to run it again

    [Michael]

*   Frozen context mode is what we are talking about
*   Used once for bidding, once for reporting
*   But that is not the default execution mode for JS
*   Usually there are a set of restrictions on how code can work so you have to restart

    [Russ]

*   Other thing is there are some BA-type internals of JS - like how it handles variables on the stack that are issues
*   We don’t support those execution modes for reporting because we assumed reporting was fast
*   Maybe we need to revisit -we can

    [Laurentiu]

*   Just seems to take longer to compile

    [Michael]

*   We weren’t thinking about having a huge amount of JS loaded, and then having to load the reporting JS
*   May be a good reason to think about separate bidding and reporting JS
*   Allow you to specify a separate JS reporting URL
*   Seems like a reasonable feature request
*   Probably already exists a Github issue on this - Isaac created?
    *   That request was focused on k-anonymity impact of having multiple bidding scripts, but it turns out it would help with this problem also

    [Paul]

*   If you have a lot of data in JS, JS is not super good at handling that
*   WebAssembly is an opportunity here - may allow you to compress the JS
*   If you have a lot of data for bidding and not reporting, then this could be an interesting solution

    [Laurentiu]

*   Sellers can’t use webassembly

    [Michael]

*   Right, that was a buyer-focused comment not a seller-focused comment

    [David]

*   On the process side, is the recommendation to go and find the Github issue with Isaac’s earlier proposal and put some request on it
*   Or do we make a separate Githuib issue
*   How do we let you know what we want here?

    [Michael]

*   I believe the prior feature request only covers a piece of that
*   @David if you can identify the previous issue - create a new issue and refer to the other -that would be great
*   If it turns out sellers want to do something so large that it needs to be handled in WASM- then let us know in a separate feature request

