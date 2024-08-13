# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday May 22, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	 



1. Paul Jensen (Google Privacy Sandbox)
2. Michael Kleber (Google Privacy Sandbox)
3. Brian May (Dstillery)
4. Wojciech Biały (Wirtualna Polska Media)
5. Harshad Mane (PubMatic)
6. Garrett McGrath (Magnite)
7. Miguel Morales (TechLab)
8. Sven May (Google Privacy Sandbox)
9. Matt Davies (Bidswitch | Criteo)
10. Matt Menke (Google Chrome)
11. Laura Morinigo (Samsung)
12. Matthew Atkinson (Samsung)
13. Alexandre Nderagakura (Not affiliated / consulting)
14. Laurentiu Badea (OpenX)
15. Brian Schmidt (OpenX)
16. Isaac Foster (Msft ads)
17. Konstantin Stepanov (Microsoft Ads)
18. Pat McCann (Raptive)
19. Sarah Harris (Flashtalking) 
20. Jacob Goldman (Google Ad Manager)
21. David Dabbs (Epsilon)
22. Fabian Höring (Criteo)
23. Arthur Coleman (IDPrivacy/ThinkMedium)
24. Paul Spadaccini (Flashtalking)
25. Yanay Zimran(Start.io)
26. Pawel Ruchaj (Audigent)
27. Aymeric Le Corre (Lucead)
28. Victor Pena (Google Privacy Sandbox)
29. Courtney Johnson (Privacy Sandbox)
30. Shafir Uddin (Raptive)
31. Elmostapha BEL JEBBAR (Lucead)
32. Alonso Velasquez (Google Privacy Sandbox)
33. Rickey Davis (Flashtalking)
34. Abishai Gray (Google Privacy Sandbox)
35. Alex Peckham (Flashtalking)
36. Andrew Pascoe (NExtRoll)
37. Stan Belov (Google Ad Manager)
38. Taranjit Singh (Jivox)
39. Sid Sahoo (Google Chrome)


## Note taker: Victor Pena


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here:



*   Patrick McCann
    *   InterestGroupBuyers as promise: https://github.com/WICG/turtledove/issues/1093  \

*   Isaac Foster:
    *   Patch’y Updates When on Joining Origin: https://github.com/WICG/turtledove/issues/1162
    *   Attestation Process  https://github.com/privacysandbox/attestation/issues/53 
    *   Brief revisit the “coarse information sharing” thing, we had talked about setting up time but never did, all got too busy…can even answer here
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736


# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.


# Notes


## Patrick McCann, InterestGroupBuyers as promise: https://github.com/WICG/turtledove/issues/1093 



*   Patrick McCann raises issue 1093. Prebid SSPs want to have access to contextual signals. 
    *   Additionally, asking for a requirement to list all entities in auction for component sellers to block a buyer prior to auction. As a solution to hardcode logic as an SSP.
    *   Wojciech B. asks if this is a single-level auction, Patrick confirms his view is only for component auctions. 
    *   Patrick says Component auctions promises or auction requirements are unchanged. 
    *   Wojciech says can a solution stagger the component auctions to different points of time to parallelize the auction. 
    *   Patrick says entities have hard coded interest groups (IGs) then you have to figure out who are the IG buyers which would be challenging
    *   Patrick says his view is that component auctions would run and then another top level auction would run. 
    *   M Kleber says Privacy Sandbox reviewed a similar solution previously as an auction blob that could be fed in as a component of another auction. It is not the architecture PS uses.
    *   Michael recaps issue 1093 and provides a potential solution design as a response to 1093’s problem statement
        *   kleber: I think to achieve this goal you could:
            *   start the auction, with each seller listing a maximal set of all the buyers it works with.  This is compatible with how prebid works.
            *   each component seller could at some later time decide which buyers it wants to accept bids from, and pass that information into the auction via a Promise in its own sellerSignals
            *   Then in that buyer's scoreAd(), they could quickly reject any bids from buyers that they decided to exclude in this particular auction
        *   This would give the right _auction winner_ and is something that could be done with the API as-is.  However, Pat is quite right that this would be somewhat inefficient because it might result in buyers running bidding logic to produce doomed bids.  So from the latency point of view, there is room for improvement here, even if the outcome is possible to achieve today.
    *   Isaac Foster says they want to improve efficiency during auction like loading buyer IGs earlier. 
    *   Michael says maybe this is not parallelism, instead it is about starting to load earlier. 
    *   Isaac adds detail to how SSPs view the auction for example want to increase buyer counts and invite them but can skip buyers lacking proper signals. 
    *   Patrick says maybe don't need a change in the auction config…change the decision logic like avoid downstream bidding functions, like kleber's suggestion
    *   Michael says component seller kicks off auction with maximum set of buyers. Component seller identifies or restricts to a smaller list of buyers. If the browser understands that restriction, then we have the opportunity to optimize the later set of buyers: might be able to skip the bidding step for bids that won't ever have a chance to win, and might be able to skip the scoring step if the seller has already told us they plan to reject the bid based on the buyer, so end up scoring fewer bids. 
        *   The seller can pass signals to its own scoring logic and also there is an opportunity to share this signal in a way the browser understands which makes those additional latency optimizations possible.
    *   Paul says maybe it is a solution to set buyer timeouts to zero? This avoids wasting time for a buyer who the seller decided they don’t want to be part of the auction.  This might do a good job of conserving those resources already today, even without us shipping any new feature in the browser.
*   Michael says we have a limited 30 minute session. We are going to other hands up. 
*   Yanush Piskevich - Another architecture would be to run whole auctions in parallel, and have the output of one auction be able to feed into another later auction.  Would that do a better job with parallelization? 
    *   Michael says we don't plan to stitch together multiple on browser auctions, we spent time talking about this design decision a couple of years ago. If that turns into a desirable feature then we might consider 
*   Brian May - There is a concern with leakage if buyer timeouts are set to zero and if a better solution would be a single standard that all entities can rely on. Question whether an exclusion or inclusion list would be wanted, assume the latter, but maybe we allow it to be defined with the list. As with any feature that allows modification of the auction dynamics, we’d want to have reporting for this feature so we could assess the impact it is having in cases where the bidding performance isn’t what we expect.
*   Michael says we could do it with either an allow or deny list in syntax, and we don’t need a full solution design today
*   David Dabbs - The mechanism should not interfere with a seller’s ability to randomize buyer order for fairness, as recommended by the [Implementation Overview](https://github.com/WICG/turtledove/blob/main/PA_implementation_overview.md). The buyer set changing mid-auction has interplay with buyer eligibility and updateURL fetching. Buyer(s) ‘speculatively’ added at the start then omitted would never have been eligible.  Michael says this is a good point that we will need to consider

Meeting ended promptly after 30 minutes
