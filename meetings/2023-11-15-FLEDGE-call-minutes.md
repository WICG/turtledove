# Protected Audience (formerly FLEDGE) WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 4pm UTC (during winter)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Nov 15, 2023


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. David Dabbs (Epsilon)
3. Harshad Mane (PubMatic)
4. Luckey Harpley (Remerge)
5. Sven May (Google Privacy Sandbox)
6. Paul Jensen (Google Privacy Sandbox)
7. Isaac Foster (MSFT Ads)
8. Maciek Zdanowicz (RTB House)
9. Kevin Lee (Google Privacy Sandbox)
10. Orr Bernstein (Google Privacy Sandbox)
11. Roni Gordon (Index Exchange)
12. Leeron Israel (Google Privacy Sandbox)
13. David Tam (Relay42)
14. Ning Hu (MSFT Ads)
15. Fabian Höring (Criteo)
16. Russ Hamilton (Google Privacy Sandbox)
17.  Jeroune Rhodes (Google Privacy Sandbox)
18.  Sid Sahoo (Google Chrome)
19.  Andrew Pascoe (NextRoll)
20. Marco Lugo (NextRoll)
21. Alonso Velasquez (Privacy Sandbox)
22. Abishai Gray (Google Chrome)
23. Caleb Raitto (Google Chrome)


## Note taker: Kevin Lee


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


## Suggest agenda items here:



*   Matt Menke
    *   I’ll be missing today’s meeting, but just wanted to let folks know I’ve uploaded a draft of how ordering and cumulative timeouts tie into auctions. It’s available here: https://github.com/WICG/turtledove/pull/906 
*   Isaac:
    *   Would like to revisit, briefly hopefully, the Dynamic K-Anon Selection feature, in particular w/r/t to the “prioritization reasoning” offered earlier this week [here](https://github.com/WICG/turtledove/issues/729#issuecomment-1807521752)
    *   Buyer/Seller Reporting Questions: https://github.com/WICG/turtledove/issues/682#issuecomment-1710965068
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Roni Gordon
    *   API versioning - https://github.com/WICG/turtledove/issues/823
    *   Sensitive signals - https://github.com/WICG/turtledove/issues/824
*   Luckey
    *   Any plans around loss reporting for buyers?
    *   Note: it is much more important for us to know the market price, rather than our losing bid.
    *   https://github.com/WICG/turtledove/blob/main/FLEDGE.md#54-losing-bidder-reporting
*   Harshad Mane
    *   When an adslot backs various sizes like A, B, and C, the PA only backs one size at a time. It seems that Chrome has the ability to check the available IGs on a device and the supported ad sizes for each IG in this situation. Who chooses the size for a protected audience auction (PAA)—the Publisher, Top Seller, or Chrome internal logic? Can the Privacy Sandbox standardize the supported size consistent in PA to prevent potential data leaks caused by random ad sizes that could identify a user? https://github.com/WICG/turtledove/issues/908  \

*   David Dabbs
    *   `updateURL` ability to update `userBiddingSignals` \
https://github.com/WICG/turtledove/issues/760  \
Intending to land this in M120 [when Mode B testing begins](https://bugs.chromium.org/p/chromium/issues/detail?id=1498491#:~:text=M120%20where%20Mode%20B%20experiments%20will%20begin)?


# Notes


## Matt Menke has written up a documentation on ordering, timeout, and flow control of the auction



*   https://github.com/WICG/turtledove/pull/906 just a pull request so far
*   Please take a look and provide feedback where necessary


## Isaac - Dynamic creative rendering (k-anon)



*   Isaac - Ran an experiment with 24 hours to see how many ads would be blocked by the requirement to only add new ads during the daily update.  
    *   The % of impression traffic is not high.  
    *   Wrote a script for a shorter window, but didn’t help. 
    *   Want more numbers or different numbers.
*   Kleber - If you use a daily update mechanism to get creatives into rotation, and when the creative comes into instance, there is a rollout period of 24 hours that ramps up from 0 to 100% over a course of a day.  Is what you are asking looking into how long it takes for ads to become available in production. 
    *   Result: 0.2% of potential impressions were impacted by the 24-hour delay.  6-figures volume. 
    *   It doesn’ mean that no ad wasn’t shown, but they may have been shown an older ad.  
    *   There may not be a revenue loss, if a bid was made on another IG.  
*   Isaac - the revenue loss can be 0 or some loss. 
*   Kleber - Not the highest priority during the testing period.
*   Kleber - Is the real issue how long it takes a creative to get into production
    *   making the dailyupdateUrl intervals shorter could help?
*   Isaac - reducing it to 4 hours helps but not as much as we want
    *   Continuous vs smaller window
*   Paul - Is the question rate of ads into the browser?
*   Isaac - Impact on customers and systems
    *   Dealing with all the endpoints at a browser scale may be challenging
*   Paul - Want to learn more about what kind of these ads.  You can push an ad instantly to a user with joinAdInterestGroup
*   Isaac - Does not feel that’s correct
*   Paul - Can use joinadinterestgroup and show an ad instantly
    *   Ask: What kind of ads are these? Estimated latency?  Is there a large delay?  What kind of campaign would want an immediate push? Black Friday?  
*   Isaac - In adtech it’s hard to delineate all use cases of the buyers
    *   It’s hard to plan a flash sale a week out.
    *   Would diving into specific cases help? 
    *   Also, the experiment was on create.  Will need to look into IG update. 
    *   The market wants to respond quickly, and doesn’t want to wait 24 hours.
    *   On the publisher page, refresh the creative since it’s based on information that is no longer available on the page. 
    *   It would also make it hard to debug a distributed system
*   Roni - Wants to reiterate on a Black Friday, and other days that advertisers deal with.  
*   David Dabbs - Potential solution to push ads out - if the IG was grouped by origin, would it mess it up? 
    *   If you join on a publisher site, and try to update it on another site. 
*   Paul - If you join an existing one, it can be quicker.
    *   Mask becomes big -> Add the user to a mask IG -> Serve mask ad
*   Kleber - Isaac's issue mentioned the idea of dealing with privacy risks by triggering the update from an environment with limited information, and a natural place to do that would be the k/v server. 
    *   K/V already returns info for each IG.  Used now for priority vector. 
    *   If we add something to the K/V response, a special keyword, saying “please update this interest group.”  
    *   Would that address your needs? 
*   Isaac - Somewhat. Update request rate might be too high.  And it may stress the update endpoint.  Looking for something like append rather than an update. 
*   Kleber - Smallest incremental change that could help would be the k/v server method.  
    *   K/V will eventually be in a TEE.  
    *   While update goes to a regular server. 
*   Isaac - Challenge of scale is real
    *   If we need another endpoint for updates, it’s significant amount of traffic, and it’s a higher operational burden 
        *   Will look further into create vs update


## Luckey Harpley - Loss reporting



*   Luckey - Talking to the Android PA team
    *   Looking for guidance on the Chrome-side
*   Paul - Take a look at the loss reporting
    *   You can take a look at the documentation for `contributeToHistogramOnEvent()` for aggregate loss reporting. 
    *   For event-level loss reporting, take a look at the `sendReportTo()`
    *   Downsampling is a mechanism we are looking at.  
*   Luckey
    *   Will take a look at the docs for aggregate reporting. 
*   Kleber
    *   Update the docs to point to the loss reporting doc
    *   AI: Kevin Lee
    *   Link: https://github.com/WICG/turtledove/blob/main/FLEDGE\_extended\_PA\_reporting.md


## Harshad Mane - Ad slots on the pages support multiple sizes



*   Harshad - Who is deciding the ad size that’s picked 
    *   Is it the publisher through the config, or the top-level seller? 
    *   Or is it Chrome? Looking at the available IG groups, and decide which ad supports it
    *   Who is the decider? 
*   Kleber - When you run a PA auction, the only output is “yes there is an ad” or null. 
    *   PA does not give any info out about how to render the ad
    *   PA does not offer any info about the size of the ad
    *   Therefore, the person who needs to know the ad size, is the person that renders the ad
    *   The party that takes the output and create a tag, and place the tag in the DOM
    *   That party needs to know how much space the ad will take up
    *   Either the publisher is picking a single size, or the party running the script on the publisher page, (most likely the top-level seller), and render it. 
*   Harshad - IG has ads with 300x250
    *   Ad slot 720x90
    *   The IG does not have that ad size available. 
    *   The size selection logic should be looking at the available ad sizes.  
    *   And when it’s not there, who decides what is chosen? 
*   Kleber - The width and height of the outcome are specified in the auction configuration
    *   That is how the auction learns about the size to render
    *   Why can’t size be chosen by looking at all the IGs
        *   Because all the IGs available are secret cross-site info that we are trying to not leak.  
        *   If we allow size to leak, then information can be encoded into the size dimensions
*   Harshad - However, they can’t access those IGs, so they don’t know which sizes can be used.  Chrome is the only one that can look at the sizes.  
    *   Why shouldn’t Chrome pick the size
*   Kleber - This is similar to a scary idea that was suggested in the multi-size ticket
    *   Let the browser determine the slot size, BUT with appropriate noise
    *   https://github.com/WICG/turtledove/issues/825#issuecomment-1764604908 
    *   However, if we do this without a lot of care, then it will be an info leakage vector
    *   The only way the browser can possibly do this is to start a research effort to see if it’s possible for the browser to pick a size that is private, along with involving randomness, and it would also mean the size would not always be the highest bid because that would be a leakage vector. 
    *   Therefore, it sounds like a hard problem for the browser to contribute information to. 
*   Harshad - If random size be a concern, 
    *   Then can PS provide a list of standard sizes to support?
*   Roni - Separate two different problems: (1) who chooses size vs (2) order of operations
    *   Only the iframe creator knows the size of the iframe which would be the top-level seller. 
    *   However, the top-level seller config is created after all component auction configurations.  
    *   And during the contextual auction, the buyers need to be notified of the size.  
    *   The buyers need to return a perbuyersignals for every size.
*   Kleber - How does Prebid know about the ad size that can be filled today?
    *   How do you learn that? 
*   Roni - Ad slots are configured with ad sizes
    *   That’s sent to the contextual auction
    *   Sizes are communicated to the buyers
    *   It’s made available to the contextual auction
*   Kleber - Does it come from the publisher?
*   Roni - The final size is only available at the end of the auction when it’s finalized. 
    *   I know the superset of the sizes, but does not know the final size
    *   Neither does the publisher know
*   Kleber - Taking PA as it is now, as a black box that renders a single size, the question is, is there an info channel that lets the top-level seller know that “this is the size” that the PA auction should look at, and that size can be fed into the contextual auction, and the signals that feed into the PA auction could focus on that size, since that is the only size that’s chosen. 
    *   When is the point of time that size is available on the page
*   Fabian - Having the same PB+GPT integration issue
    *   Get multiple sizes for ad requests for a seller
    *   Want to look for a path that integrates with PB and GAM to allow the final size to be passed into the PA auction
*   Kleber - This really depends on how the top-level seller chooses what the right size is. 
    *   Something like "First item in the size list" would make it easy
    *   Something like "Using a ML model to choose the optimal size" would make it hard
    *   May be somewhere in the middle, I don't know
*   Fabian - Similar to the currency 
    *   In this case, the buyer chooses
    *   Buyer sends back the ad sizes chosen during contextual auction
*   Roni - Currency you can switch back.  
    *   And the buyers pass the size info through perbuyersignals
*   Kleber - Hard to answer this question without any GAM person on the call today, because we don’t know when the size information becomes available. 
*   David - Has PR supporting sizes land? 
*   Paul - Yes, it landed
*   David - There can be different size bundles.  Are the ad sizes passed in a macro? 
*   Kleber - We provide macro substitution if the server needs to find out what the size
*   Roni - Replaced by the size of the iframe, and not the size of the ad
*   Kleber - Is there a GitHub issue on this? 
*   Roni - Harshad just opened up (#[908](https://github.com/WICG/turtledove/issues/908)) and it was mentioned in another issue. 
*   Kleber - Will talk to the GPT folks and see if this question needs to be moved over to their repo, and will investigate into the information flow. 
*   David - GPT supports width and height.  It may be a way to pass a size.

## David Dabbs - updateURL ability to update userBiddingSignals #760
*   Paul - Implementation is close.  It would be M121.  
    *   Kleber - Merging into an old branch is for bug fixes, and new features would go on a new branch. 


## No Meeting Next Week (Nov 22)



*   Kleber - Next week is the day before thanksgiving. 
    *   Usually low attendance
    *   My instinct is to cancel.  
    *   Vote for canceling. 
*   Isaac - Would love it for just to be us, but would also like to include the industry
*   Kleber - Nov 29th will be the next time we meet. 
