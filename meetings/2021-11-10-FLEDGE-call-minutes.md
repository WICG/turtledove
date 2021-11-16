# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 4pm UTC (during winter).

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Nov 10 2021

(No meeting Wed Oct 27 due to W3C's TPAC)


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Paul Jensen (Google Chrome)
3. Angelina Eng (IAB / IAB Tech Lab)
4. Brian May (dstillery)
5. Brendan Riordan-Butterworth (eyeo GmbH)
6. Michael Coward (Arcspire)
7. Wendell Baker (Yahoo)
8. Christa Dickson (Meredith)
9. Jeff Kaufman (Google Ads)
10. Fred Bastello (Index Exchange)
11. Bartek Łoś (RTB House)
12. Bartosz Marcinkowski (RTB House)
13. Richard Anton (Amazon)
14. Irlinta Mavromati Terzopoulou (Google Chrome) 
15. Russ Hamilton (Google Chrome)
16. Sergey Tumakha (Microsoft Ads)
17. Jonasz Pamuła (RTB House)
18. Przemyslaw Iwanczak (RTB House)
19. Joel Meyer (OpenX)
20. Aleksei Gorbushin (Walmart)
21. Andrew Pascoe (NextRoll)
22. Matt Menke (Google Chrome)
23. Matt Wilson (NextRoll)
24. Don Marti (CafeMedia)
25. Aloïs Bissuel (Criteo)
26. Aditya Desai (Google)
27. Isaac Schechtman (IPONWEB) 
28. David Dabbs (Epsilon)
29. Stan Belov (Google)
30. Brad Rodriguez (Magnite)


## Note taker: Angelina Eng


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

 		


## [Suggest agenda items here]



*   (Paul Jensen) A couple FLEDGE worklet performance topics:
    1. Discuss FLEDGE auction interest group selection mechanisms
    2. Discuss FLEDGE worklet language needs
*   (Jonasz Pamuła) https://github.com/WICG/turtledove/issues/233, caching of bidding logic
*   (Jonasz Pamuła) Detecting generateBid timeouts?
    3. an API for finding out "how much time is left"?


## Notes



*   IAB Browser/OS Ads Testing Task Force run by Angelina Eng helping companies understand PS. and next meeting will start to identify use cases and document needed requirements / capabilities.
    *   Group has asked if they have to pay to participate.
    *   Contributions need to be subjected to the IPR restrictions.
    *   No fees to community group, but fees for business group. https://www.w3.org/community/about/fees/
    *   Brian May - lack of understanding from business community in IAB group. Encouraging them to participate. Companies looking to hear from biz group and what they want to to do, then join the Task Force.
    *   To join task force: email [angelina@iab.com](mailto:angelina@iab.com) 
    *   Preferred way to kick-off discussions in W3C.
    *   Feedback from task force will feed into the W3C groups.
*   Various FLEDGE worklet performance topics (Paul Jensen)
    *   Performance of the auctions results in performance of worklets.
    *   How can it be faster - with cacheing methods, WASM or javascript
    *   Other explorations:
        *   Way to limit # of interest groups in an auction.  Based on # of folks running bidding scripts.  If too large then causes issues.
        *   How can we pick which interest groups would participate. Priority value to interest groups? High and low values, buyers can declare priorities.
        *   Another way, characteristics such as contextual ad call is related to certain topics, and some interests groups don’t want to participate. Some sort of flag the seller may indicate. Might be characteristics they don’t want to participate.
        *   Wendell Baker: learned from Right Media in open market with similar constraints. Could be exponential. Anytime sentence use “we”, we are the market operators, and in control of supply and demand to clear the trade. 
            *   Learned a “pay to play”. The market operator can move the bid based on fees.
            *   Also found that they could take advantage of the “law of large numbers”. They can drop queries. Flip a coin, they could pick a subset.
        *   Kleber - the ideas that Paul presented were about what tools the browser could use to make available to limit the number of interest groups in an auction - a technical requirement. The person to use the tool is probably the party running the auction. Market operator would not be the browser.  
        *   Wendell - Chrome is running an auction and running the trade. WE can work that out later. The technical aspect is that it’s running the code. 
        *   Brian May: rich area for discussion.  They’ve dealt with similar things.  With possibilities which is going to be priority, and what are the preconditions would be in the marketplace.  We are talking about a different model. RM currently owns the auction.  With this, it not only a community asset building in the browser, but an asset with a different lifecycle.  We want to be able to set up ahead of time.  Interest groups and bundles results in a lot of resources for the browser.  We’ll need to set up rules way in advance. We need some sort of auditing, we need assurance rule was properly administered. The browser needs to have some way to identify it did happen and run right and fair.
        *   Jonasz: two problems we are trying to solve: prioritizing interest group order within a single buyer; and splitting computational resources across buyers.  The first one is something we (RTB H) are doing in the current server-side system.  We (quickly) calculate a preliminary score per ad, and this should be doable in FLEDGE as well.  Question is how to do it across multiple buyers.  Seems could be some mechanism to gamify the resources.  Goal to prioritize interests groups that are likely to bid more.  For context, there is an issue there #79.
        *   Kleber - something like ordering interests group from a bid provider, is sort of a tool that browser could have.  Limit the # of interests groups for each buyer, the browser would not be able to predict who would win.
        *   Joel: if I was publisher, I would want as many buyers as much as possible. How do we give publishers control while giving users control too.
        *   Kleber - Chrome believes party who runs the auction is representing the publisher. A different consideration if we have to separate those parties.
        *   Maverick Grey: he idea when assigning interest group vs. when auction happens on publisher side. There is no way to modify the priority of the group, after the group is assigned by the browser.  Changing priorities over time will be difficult.  
        *   Jonasz: The more flexibility we have in computing the preliminary score, the better. If we were able to calculate that given IG signals and contextual signals in real time (somehow keeping the computational cost low at the same time), that would be optimal. But a single static number per IG is already much better than a random / arbitrary ordering.
        *   Angelina Eng: FLEDGE is all about remarketing and retargeting.  Advertisers don't frequently change the priorities of their KPIs, just may vary from campaign to campaign.  Not sure where the reference to priority changes over time comes from; not my experience.  Perhaps would create separate IG if we wanted to target in another way.
        *   Brian: Makes me think about the server-side-components discussion again, to move things closer to real-time.  I would really want to know when auctions are running in browsers I haven't touched recently (even if aggregate)
        *   Angelina: I retract that — advertisers _do_ change priority over time, based on optimization of campaigns. Take my statement back.
        *   Alois Bissuel: concerned about the interaction between interest groups which FLEDGE wants to separate. If we want to do that locally in a secured javascript environment, concerned about reporting. More difficult to calibrate results. 
        *   Russ Hamilton: could realtime bidding be useful interest group prioritization.
        *   kleber: Sounds like that might help 
        *   Angelina: has there been limits to # of interests groups that advertisers can create? - has not been discussed.
        *   Paul: We have some large limits, but nothing tight
        *   Angelina: expect this would be different for differing types of advertisers — many more IGs for retail, for example.  Advertisers do abuse the number of segments.  (kleber: is that per-user, or across all users?  Angelina: The very large numbers are across all users; an individual person might be in several, but not enormous numbers.)
        *   Brian: Limit implied by the conversion measurement API, which translates to a limit on #groups an advertiser can create.  We will probably need some limit on per-person IGs from one entity.
        *   kleber: don't want to encourage shenanigans to circumvent this limit, like an IG owner dividing themselves into 10 owners.
        *   Angelina: Conversion API is challenging because it's a KPI, but there are other metrics that people care about tracking a lot also.
        *   Jonasz: sellers / publishers might want to allocate resources dynamically during a single auction.  If a buyer bids high for a single IG, it makes sense to allocate more resources to that buyer. If a buyer bids low for a number of IGs, the chance they will win the auction if given more resources is low. (Given the fact that the buyer would order the IGs by the preliminary score.)
        *   Maverick: is there any way to add interest groups, if there is any more space?
        *   Kleber: Current no feedback when you try to add to a group.  If failed, perhaps could be a problematic fingerprint signal.  
        *   Andrew Pascoe: Could treat this like a multi-armed bandit problem. maybe there's some stuff in the literature to solve, maybe Chrome could keep a cache of contextual responses, and on a per publisher set, evaluate dummy auctions in the background to generate offline data to update the multi-armed bandit selection. Evaluate prices out of the interest groups or seller selections stuff. The data generated is based on real auction data.
        *   Kleber: some interest groups might not be able to bid in real world, and Chrome can evaluate what it could look like, and readjust the priority of the interest group.
        *   Michael Coward: No way for Bidder to signal ‘not interested’ for a set period of time to self-select out of the next X auctions per publisher or globally, this could be for daily/hourly frequency capping and longer for content or topics not relevant for that interest group; any cached response would not require javascript execution.
        *   Kleber: Browser would be able to see history of the ad.
        *   Don Marti: Chrome already has a [site engagement score](https://www.chromium.org/developers/design-documents/site-engagement), if concerned about budgeting resources between different FLEDGE bidders, perhaps higher engagement score sites can receive more resources for FLEDGE auctions. (It’s not in the user’s interest to burn a lot of cycles running an auction on a site that they’re about to bounce from, but it could be a better investment on a more engaged site)
        *   Aditiya Desai:  when a buyer chooses to add user to interest group, they can establish priority.  Question - how many can be they be in from a storage perspective.  The buyer can evaluate every interest group that user is part of with late decision making. There are different performance considerations for each. 
        *   Wendell: when doing server side, there’s a central planner figures out what to do, and controller where the system should be set.  Chrome is taking the roles individually.  It becomes an economic problem to a cacheing problem. With a bit of logging you could figure out what happened.
*   Kleber: Quick show of hands, should we meet in two weeks, on Nov 24 = day before US Thanksgiving?  Hands were 11 in favor of meeting, 5 in favor of skipping.  See you in two weeks.


# Next meeting Nov 24
