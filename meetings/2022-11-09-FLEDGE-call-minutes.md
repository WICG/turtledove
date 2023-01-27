# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 4pm UTC (during winter).

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Nov 9, 2022


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Michał Kalisz (RTB House)
3. Brian May (dstillery)
4. Charlie Harrison (Google Chrome)
5. Garrett Tanzer (Google Chrome)
6. Matt Menke (Google Chrome)
7. Caleb Raitto (Google Chrome)
8. Bartosz Marcinkowski (RTB House)
9. Russ Hamilton (Google Chrome)
10. Paul Jensen (Google Chrome)
11. David Tam (Relay42)
12. Sven May (Google Chrome)
13. Alex Turner (Google Chrome)
14. David Dabbs (Epsilon)
15. Kevin Lee (Google Chrome)
16. Wendell Baker(Yahoo)
17. Daniel Rojas (Google Chrome)
18. Qingxin Wu (Google Chrome)
19. Marco Lugo (NextRoll)
20. Alex Cone (Coir)
21. Garima Mimani ( Google Chrome)
22. Stan Belov (Google Ads)
23. Zheng Wei (Google Ads)
24. Jonasz Pamuła (RTB House)
25. Andrew Pascoe (NextRoll)
26. Supraja Sekhar (Google Ads)
27. Bartek Łoś (RTB House)


## Note takers:

Bill Rank ([billrank@google.com](mailto:billrank@google.com))


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here]



1. Alex Turner (Google Chrome): Extended Private Aggregation Reporting ([link](https://github.com/WICG/turtledove/blob/main/FLEDGE_extended_PA_reporting.md), [slides](https://docs.google.com/presentation/d/1Dj6t3XrJM78AznQu0dhN1LrwmL-jnoszo3EpqosAXjw/edit?usp=sharing))
2. Garrett Tanzer (Google Chrome):
3.  FLEDGE size-related API changes ([link](https://github.com/WICG/turtledove/issues/312#issuecomment-1307471709))
4. Paul Jensen (Google Chrome): FLEDGE auction performance
5. David Tam (Relay42): Two questions, first regarding permissions policy and second, about removing browsers from interest group via trusted server


## Notes


### Alex Turner (Google Chrome): Extended Private Aggregation Reporting ([link](https://github.com/WICG/turtledove/blob/main/FLEDGE_extended_PA_reporting.md), [slides](https://docs.google.com/presentation/d/1Dj6t3XrJM78AznQu0dhN1LrwmL-jnoszo3EpqosAXjw/edit?usp=sharing))



1. Existing Private Aggregation API
   1. Overview: cross-site data in a noisy way, encrypted and only available to trusted infra: output is aggregate of individual client data with noise added; contributions limited per-origin and per-day
   2. Limits:
      1. No loss reporting
      2. Can only use signals available at call time
      3. Can’t report on signals from other bidders
      4. Difficult or impossible to measure actions contributing to auction latency (info not available to the worklet)
2. Proposed extensions (goal of rectifying issues and add capabilities)
   1. Goals: Enable aggregate measurement…
      1. For _all_ auction participants (winners and losers): bid data, 
      2. For auction winners, data about the auction relative to events which happen with the rendered ad (such as a click on the ad)
      3. For auction sellers, data on auction latency and other statistics
      4. For auction buyers, data on their bidding latency
   2. API outline
      1. Allow calls to be conditional on an event
         1. “reserved.win”
         2. “reserved.loss”
         3. Custom events triggered from a fenced frame
      2. Allow ‘filling in’ fields once auction completes (plus optional scale/offset), e.g.
         1. Winning bid value
         2. Highest scoring other bid
         3. Script run time
         4. Signal fetch time
         5. Bid reject reason
   3. Examples of proposed newly answerable questions
      1. “What’s the click-through rate of my ad (for this user segment)?” [here](https://docs.google.com/presentation/d/1Dj6t3XrJM78AznQu0dhN1LrwmL-jnoszo3EpqosAXjw/edit#slide=id.g1879a475077_2_523)
      2. “How much higher did I need to bid to win the auction?” [here](https://docs.google.com/presentation/d/1Dj6t3XrJM78AznQu0dhN1LrwmL-jnoszo3EpqosAXjw/edit#slide=id.g1879a475077_2_532)

QUESTIONS: 

**Jonasz: Can you have buckets of values for a single event?**

Yes, you can make multiple calls for a single event. (Alex Turner)

**Stan Belov: What latency data is available here? Only available via this private aggregation mechanism or envision (temporary or otherwise) at event level?**

While 3P cookies exist, event-level reporting is still directly available to you. Will NOT be available in the private future. Proposal reviewed is for private aggregation API.

(Michael Kleber and Paul Jensen)

**What will be available for sell-side debugging?**

Slides proposed are for aggregate reporting, not direct.

(Michael Kleber and Paul Jensen)

**Brian May: In thinking through use of the API, there are reports I’d want in as close to real-time as possible, other times I’d use for historical/batch analysis. Separation of real-time vs. delayed/batch processing directories?**

Timing attacks are a consideration, so randomized delay is added by design (10min-1hr), but will take this feedback. Varies by metric. E.g., clicks not needed in real-time. (Alex Turner)

**Jonasz: How does this impact ETA of event-level reporting? Still available for the foreseeable future?**

Mutually exclusive timing. We’ll give ample warning before sunsetting event-level.

**Jonasz: This doesn't seem to support the ability to call setBid multiple times (where only the final one sticks, as a way to handle time-outs)**

Good point, we should look into a similar API here.

 

**Aggregation infrastructure (back-end) availability? Can we test server-side?**

Already in the pipeline ready to be tested. Private Aggregation OT available but without server support. Hope to have ASAP but no specific ETA yet. You can get hands-on testing today.

Charlie: could test the pipes by integrating with the existing server; high-level is all the same

**Brian May: Where do these reports go? Any limit to who they can be sent to?**

Sent to a well-known origin from the script that called them. (Alex Turner)


### Garrett Tanzer (Google Chrome): FLEDGE size-related API changes ([link](https://github.com/WICG/turtledove/issues/312#issuecomment-1307471709))

To make FLEDGE aware of the sizes available, you’ll transition to mandatory declaration of sizes.



*   The k-anonymity check on the container ad’s URL now also includes the container ad’s size.
*   The k-anonymity check on each component ad’s URL now also includes the component ad’s size.

Proposed improvements:



*   Privacy: In the old design, there was wiggle room for the publisher to cause observable differences inside the container ad (or for the container ad to cause them inside its component ads), splitting up k-sized groups based on first-party or third-party data. In the new design, we close this gap by running the k-anonymity check on the totality of information that flows from the embedder into the fenced frame.
*   Flexibility: Due to the privacy improvements above, we no longer need to stipulate a list of allowed ad sizes.
*   Performance: Due to the privacy improvements above (and FLEDGE knowing about ad sizes), we can now support slot size macro replacement in the render url, as requested here, saving a round trip for responsive ads.

**QUESTIONS**

Jonasz: Problem you mentioned is something we’re curious to resolve, too. In our use case, the products will be much more numerous than container ads. Container easier to cross k-anon check, for products it will be harder.

Garrett/Kleber: For a given container, do the ads load at many different sizes across ads? Only relevant if products used in different sizes. Sometimes in different creative, sometimes across different containers. Do you have a sense of metrics here?

Jonasz: Product types won’t be locked to sizes, usually reverse: determine product(s) and then adapt size. Every given product is displayed in a number of many different sizes, typically have popular ad sizes and then have long-tail of many others.

Kleber: Will bring this feedback back. Need to understand if long-tail of sizes is something we can design for or inherently brings privacy risk.

Paul Jensen: Do we use % or pixel count? 

Garrett: We can support pixel count or % of screen dimensions (not container ad dimensions).

Paul: If you define both component and container ad size in terms of screen dimension %, that might help.

Jonasz: Would like to support a single product at any given time in two sizes. Support by your proposed API? Concerned with shuffling. 

Garrett: You can have each component have a different size, load for each size and switch between them when hovering, etc.


    From [feedback](https://github.com/WICG/turtledove/issues/312): Re component ad sizes, the new version of the proposal is more flexible than previously described. (Each component in the same ad can have a different size.) The way you could implement that carousel ad would be something like:


    adComponents: [('ad.com/component1', smallSize), ('ad.com/component1', largeSize), ('ad.com/component2', smallSize), ('http://ad.com/component2', largeSize), …]

Jonasz: This effectively halves the number of allowed component ads.

**Jonasz:** How does this API work with k-anon enforcement? 

**Kleber:** List of components has pre-declared ad sizes, FLEDGE will need to slim down components and sizes so that only available ones are ones that meet k-anon.

**Brian May [comment]:** This will make it more difficult to understand auctions. Won’t have immediately recognizable k-anon checks with an additional parameter. Harder to figure out what’s going on at auction time.

_[little time left for remaining topics, so will review Auction Performance quickly and punt detailed discussion of Auction Performance and David Tam’s questions to next meeting Dec 7]_


### Paul Jensen (Google Chrome): FLEDGE auction performance

Much of FLEDGE’s functionality is heavy on network requests, and most are dependent on previous network requests. Even handshakes are expensive. Even with strong optimization, latency will be tough without parallelization. Question: how much parallelization can we achieve? If much of this is merely dependent on sellers needing to know the _buyers_, we can likely shrink the number of calls down. Today, latency is significant, particularly on mobile.

[BUYERS] Does this sound hugely beneficial?

[SELLERS] Does this sound feasible? Do we know the list of buyers that we want to participate in the auction ahead of time?

COMMENTS

Brian: From experience, client optimization improvements pales in comparison to network call latency, so must work on the latter to improve overall FLEDGE performance.

**Michael Kleber:** We're out of time on this call, we will pick up the parallelization discussion again on the next call in four weeks (skipping Nov 23 due to US Thanksgiving).

**It would be great to have sell-side folks involved in this discussion next time — both Google Ads sell-side, and the OpenX people or anyone else involved in the prebid.js work in this direction.**

Remaining topic (from David Tam) also punted to next session, Dec 7
