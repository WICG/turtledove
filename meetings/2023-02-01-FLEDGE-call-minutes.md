# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 4pm UTC (during winter).

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Feb 1, 2023


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Sven May (Google Chrome)
3. Brian May (dstillery)
4. Russ Hamilton (Google Chrome)
5. Paul Jensen (Google Chrome)
6. Tamara Yaeger (Criteo)
7. Manny Isu (Google Chrome)
8. Andrew Aikens (TripleLift)	
9. Ali Nouri (Google Ads)
10. Ryan Mathews (Google Ads)
11. Megan Graybill (Google Ads)
12. Tim Hsieh (Google Ads)
13. Don Marti  (CafeMedia)
14. Peiwen Hu (Google Chrome)
15. Orr Bernstein (Google Chrome)
16. Supraja Sekhar (Google Ads)
17. Alonso Velasquez (Google Chrome)
18. Risako Hamano (Yahoo Japan)
19. Marco Lugo (NextRoll)
20. Martin Pal (Google Chrome)
21. Caleb Raitto (Google Chrome)
22. Jonasz Pamuła (RTB House)
23. Fabian Höring (Criteo)
24. Elias Selman (Criteo)
25. Dinah Shender (Google Ads)
26. Zheng Wei (Google Ads)
27. Joel Meyer (OpenX)
28. Matt Menke (Google Chrome)
29. Daniel Rojas (Google Chrome)
30. David Dabbs (Epsilon)
31. Connor Doherty (Amazon)
32. Alexander Tretyakov (Google Chrome)
33. Andrew Pascoe (NextRoll)


## Note taker: mannyisu


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here]



*   [Event level signals for generateBid and reportWin https://github.com/WICG/turtledove/issues/435](https://github.com/WICG/turtledove/issues/435) 
    1. Requestor: Ryan Mathews - Google Ads
*   Daily update url, Fabian Höring
    2. https://github.com/WICG/turtledove/issues/424 
    3. https://github.com/WICG/turtledove/issues/361  
*   Interest group size https://github.com/WICG/turtledove/issues/402 , Julien Aimonino
*   Currency of the bid  https://github.com/WICG/turtledove/issues/166, Tim Hsieh - Google Ads


# Notes


## [Event level signals for generateBid and reportWin https://github.com/WICG/turtledove/issues/435](https://github.com/WICG/turtledove/issues/435) 



*   [Ryan] Some signals will be beneficial for Event-Level Reporting - Recency, Join Count - proposing have 2 or 3 buckets bits for us to pass information into different functions
    *   [Paul] Reviewed the proposal - the signals exposed in reportwin has been thought about and we are trying to keep this minimal. The more we expose will allow for potential identity join. We really do want to think carefully about these. It is important to consider future use cases we want to support/explore sooner rather than later. We can do model training inside of a TEE; explore more private ways of doing these trainings. There is a bunch of things to consider when we think of adding more signals to reportwin.
    *   [MK] It seems like the browser provided signals are easier to fit into the picture than the adtech provided signals. The adtech provided signals seem harder to explain and subject to potential abuse than the browser provided signals.
    *   [Alonso] Would love to hear from other representatives of the ecosystem. What are your thoughts on these 4 signals?
    *   GH issue already has comments from RTBHouse (Jonasz) and Criteo (Fabian)
*   [Jonasz - RTBHouse] Aggregate reporting - about adding information to reportWin, a lot depends on the details. Agree that we have to be careful. It should not be a replacement for aggregate reporting. In the end, we should move towards the long term goal = aggregate reporting. We will definitely try the signals, experiment and provide feedback. By long term, what do we mean?
    *   [MK] We will clarify that timeline soon.  Event-level win reports not about to go away, but the goal is aggregate reporting, definitely not going to be replaced.
*   [Fabian - Criteo] We welcome the proposal. Looking at aggregate reporting, it seems complicated. The only objection is that we worry about homogeneity in what companies can offer, so we prefer each adtech to provide its own signals. Not sure adding 2 signals and Chrome adding a handful more is a good long term approach.
*   [Brian May] Curious about the interaction model - what kind of information is provided to folks who are using the API that has the potential of inferring things about the user?
    *   [MK] The prop is in addition to the way FLEDGE reporting works right now. Every IG has the opportunity to say this is the report I want to send. Reports on the winning ads, and this is a prop to allow a bit more information. 
*   [Fabian] Additionally, ML training on advertiser data for Event-Level which is good. We need to have the information in the Trusted Server because we do not want to expose it to the javascript. By design it is impossible to leak if it runs in the TEE.
    *   [MK] If it is stored in the IG at the time a user is added to IG, then it can also be stored as a key which will make it sent to the Trusted KV server.
*   [Paul] Along the same lines, one of the browser provided signal is Join Count which is something already being provided to GenerateBid


## Daily update url, Fabian Höring



*   1: https://github.com/WICG/turtledove/issues/424 

[Fabian] https://github.com/WICG/turtledove/issues/424 It is executed when a user goes to the publisher website. We see a lot of users that do not come back everyday on First Party data



*   [Paul] Are you saying that the update happening on the publisher page is less frequent and less useful?
*   [Fabian] I want the new campaign to be taken into account before the auction executes. I am suggesting to do the updates daily if the user does not come back within 24 hours.
*   [MK] What pattern of usage are you trying to optimize for?
*   [Fabian] For frequent users, I get to do the updates myself. But it happens often that the same user is not seen for some time.  First-party cookie seen less often than third-party cookie because user doesn't return to same site.  A good design is for users not seen for 24 hours, and should be updated daily.
    *   [MK] It seems like the update mechanism is not working as well as we expected it to be so I think we need to debug why.  But this shouldn't need any return to the same site: once a browser is in an interest group, that IG should get to update itself regularly if its owner gets to be in an auction on any site, not just the site where you added it.
*   [Matt Menke] Background updates reveal information about the users’ IP address, so we opted out of background updates.
*   [David Dabbs] In my experiment, after I win the auction, no call out to my daily url. If one registers an IG with no ads (because none are currently running, say a new advertiser), we have the expectation that daily update will actually run daily so that new ads can join the IG without the user needing to return to the advertiser site.
    *   [Paul] After each auction, we take all the IGs that participated and start queuing up daily updates for them. Of course, we do some rate limiting. If you are not seeing that, please file a bug and we can take a look.
    *   [MK] Our expectation: Once a person is in an IG, and IG participates in auction, then if that IG has not had an update in a while, then it should issue a request for an update and it should be visible in the export at chrome://net-export/. If this is not the case, then we have a bug we need to investigate.
    *   [David Dabbs] Suggestion: Update the OT documents so we can validate what we are looking for…
    *   [Paul] This section covers exactly that: https://github.com/WICG/turtledove/blob/main/Proposed\_First\_FLEDGE\_OT\_Details.md#interest-group-updating


## Daily update url, Fabian Höring



*   2: https://github.com/WICG/turtledove/issues/361  

[Fabian] https://github.com/WICG/turtledove/issues/361 Started looking into segmented audience - we might need to create lots of IGs



*   [Paul] Thinking about it - some choices about IG updates have broader effects eg. K anonymity updates. There are knock on effects that could slow down auctions. Drafted up a response for this issue - hope to have more discussions about it. Will post on GH shortly - But it is a complicated subject and has some privacy implications.

## Interest group size https://github.com/WICG/turtledove/issues/402 , Julien Aimonino

**\*\*\* Julien is not here today, we will skip and come back to this issue in a later call \*\*\***


## Currency of the bid  https://github.com/WICG/turtledove/issues/166, Tim Hsieh - Google Ads

[Tim] The issue is that FLEDGE does not have a way to receive bids in different currencies. We are tracking a proposal that I have not posted yet



*   [Brian] Will there be a need for exchange rate tracking?
*   [MK] Browser definitely does not want to be in the business of providing a currency exchange rate. There are some practical concerns we need to consider.  Without limits, this is an issue from a tracking vector point of view, the same ad could bid in many different currencies to identify many different people. There are possible ways to fix this but we need to consider several different things before nailing a solution. From a practica ppov, the US dollar is the only way GAM is treating auctions in the FLEDGE experiment.
*   [Tim] Will post our proposal in GitHub
