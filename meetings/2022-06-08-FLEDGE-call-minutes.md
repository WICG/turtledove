# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday June 8, 2022


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Brian May (dstillery)
3. Peiwen Hu (Google Chrome)
4. Paul Jensen (Google Chrome)
5. Sergey Tumakha (Microsoft Ads)
6. Wendell Baker (Yahoo)
7. Marco Lugo (NextRoll)
8. Alex Cone (IAB Tech Lab)
9. Angelina Eng (IAB / IAB Tech Lab)
10. Andrew Pascoe (NextRoll)
11. Joey Trotz (Google Chrome)
12. Matt Menke (Google Chrome)
13. Zheng Wei(Google Ads)
14. Russ Hamilton (Google Chrome)
15. Bartosz Marcinkowski (RTB House)
16. Grant Nelson (TripleLift)
17. Christina Brauer (Capital C)
18. Brian Schneider (Google Chrome)
19. Aditya Desai (Amazon)
20. Caleb Raitto (Google Chrome)
21. Jonasz Pamuła (RTB House)
22. Tamara Yaeger (Iponweb)
23. Sid Sahoo (Google Chrome)
24. Aleksei Gorbushin (Walmart)
25. Stan Belov (Google Ads)
26. Rotem Dar (eyeo)
27. Brad Rodriguez (Magnite)
28. Fred Bastello (Index Exchange)
29. Martin Pal (Google Chrome)
30. Joel Meyer (OpenX)
31. Anthony Molinaro (OpenX)
32. Saurabh Shah (Walmart)


## Note taker: Angelina Eng


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here]



*   Request for general update, how is the OT going? — Paul?
*   (Andrew Pascoe) [Issue 270](https://github.com/WICG/turtledove/issues/270), Scalable Account Targeting Mechanism


## Notes



*   **Request for general update, how is the OT going? — Paul?**

Paul Jensen:



*   Different performance improvements:  look at most recent items in the github tracker.  Currently looking at those.
    *   Bid filtering: when to and not to run bids so that js doesn’t need to be activated if it doesn’t need to.
    *   Reusing the javascript execution environment.  When js is used for FLEDGE, a new js environment needs to be created.
        *   Desktop you can see fractions of millisecond vs. on android is a few # of milliseconds.
    *   Giving more metrics for sellers regarding the bid.
*   Origin Trial
    *   Careful on metrics Chrome is sharing.  There’s been a lot of auctions going on.
    *   Ramped up to 50% of beta channels
        *   Each channel is growing in size.
        *   50% of users in beta channel can have FLEDGE API accessible.
            *   Can’t share user population size.
    *   Not too many hiccups during the OT. Haven’t found any crashes.  It’s been steady and seeing continuous growth.
    *   If you filter out a good portion of the bids, auction will act faster.
    *   FLEDGE Origin Trial is a third party origin trial which is not typically for publishers to opt-in.  It’s more likely being used by ad tech companies that are on a variety of different sites.
        *   Aleksei Gorbushin: Walmart Ads (as an advertiser) is having a hard time finding partners to test with.
        *   Chrome doesn’t disclose companies who are testing via OT.  Ad Tech companies are welcome to identify themselves.
        *   Angelina Eng: IAB Tech Lab's Browser OS Task Force is looking to find companies that are testing or interested in testing, to develop a framework — can reach out directly to Angelina, [angelina@iab.com](mailto:angelina@iab.com).  There's a Monday "Addressability & Measurement" meeting that will have some more details.
        *   Jonasz, RTB House: We have some OT results: Seen organic clicks, but priority would be to see scale and traffic in experiment.  There are 2 immediate areas to work on
            *   Performance - optimizing resource consumption on browser
            *   Event level reporting - minor technical issues, WIP
            *   Agg reporting - we are waiting for incorporating of agg reporting to FLEDGE, this becomes more and more pressing as we are approaching the "Transition Period: Stage 1" (https://privacysandbox.com/intl/en\_us/open-web/#the-privacy-sandbox-timeline)
            *   They were able to run an auction and see click to advertiser’s page, but not in position to see metrics and assess since scale is too low at the moment.
                *   If any specific questions you can reach out to Jonasz Pamuła.
            *   Michael Kleber: Yes, beta portion to see if it is working is the right thing.  Scale will be likely to see as it moves to the next OT phase.
        *   Joel Meyer: OpenX is close to OT testing.  Working with the Prebid community to figure out how to work with the GAM published plans.  Looking to find publishers and DSPs to test with.  Currently want to test the pipes, not the scaling side.
*   Issue #270
    *   B2B style ads Interest in advertising to certain companies, not individuals.
    *   Identify users work at a particular “account”, and there hasn’t been a lot of talk in W3C groups.
    *   FLEDGE has a bit of scalability problem with account-based marketing. A FLEDGE is on advertiser site and registers, and see domain, then can advertiser to them, but they are not going to be able to reach alot of people, and advertisers want to advertise to them in like a data co-op way.
        *   Is this acceptable to use for FLEDGE?
            *   Instead of adding them to the interest group, but another advertiser wants to be able to reach those users from that account.
            *   The ability to cycle thru different advertisers to this “interest group”.
            *   Or is there a way that FLEDGE can explicitly support Account Based Marketing (ABM)?
            *   FLEDGE designed to be more generic to add to a generic interest group across different sites. When a browser is observed to add to a particular interest group as long as the user is permissioned and delegated to do so.
        *   Kleber: Yes the mechanism is just how FLEDGE should work, and ability to allow multiple advertisers to advertise to that browser in that interest group, is feasible. The decision of which advertisers that will “compete” for that audience for that browser, that the company needs to make decision who to pick.  If you wanted, you could call a second auction to do so, there are many options. Can be different randomly, based on geography - any other ways to use FLEDGE. If there is a design about FLEDGE making it difficult to use, then feel free to open issue or discuss ergonomics.
            *   The API should be able to support these use cases.
            *   How do we do this without stuffing too many advertisers into an interest group?  - Worth considering: getting reporting back out, and if changing ads with interest group, then they can be able to control budgets, and get a return to be able to see that to control for it with the Trusted Server.
            *   Model explained seems like there’s a resource issue because there may be high competitions.  How do we go about figuring out especially for high value auctions.  How do you get past the threshold but still be able to perform?
                *   Ad tech company might be able to reduce the number of advertisers that need to compete in the browser, maybe look at the data and decide which “10” would most likely to win.
        *   Maybe campaigns can be more dynamically updated.  If campaign gets started/stopped frequently, there might be budget constraints and such.
            *   Part of privacy model - the ads that show up are ads that are already stored before the user goes to that site and is part of the privacy model from the beginning.
            *   Ads are chosen ahead of time and cached in the browser to show up immediately. They want to ensure k-anonymity for the ad that shows.
            *   The daily mechanism is limiting the data to be made available of the user.
            *   Where the ad is shown is in the key value place. Still discussion on what other info will be made to key value info. There may be more signals to the key value server that is trusted.
            *   Anything that changes the ad targeting capabilities based on larger collection of information is changing the privacy properties. 
        *   Angelina: Is the actual creative asset being stored in the browser?  Coming from ad ops, there are times where the ad changes over time, dynamically, same campaign but different messages.
            *   Kleber: Originally thinking the actual creative stored in browser, and stored in fenced frame without touching the server at all.
                *   Ideally, there should be a way to package up ads without touching the server.
                *   Currently the ad rendered url will be stored in the browser, the url stored in the fenced frame will be able to refer to a network. Eg. timed ads, streaming video, there are resource consumptions as a trade off.
                *   Otherwise have to worry about timing attacks.Rendering ads over the network and we need more private ways to accomplish the task.
            *   Angelina: This is definitely not how creatives work today.. Most clients want to swap out creative as needed.  Many don’t deliver creative ahead of time, and when new creative is developed it needs to be swapped out “immediately”.  
        *   David Dabbs: This is a new system, Chrome is going to have info on market dynamics.  Are there any terms and conditions about how the data will be used or shared?
            *   Angelina: IAB terms and conditions 3.0 have some rules, but they haven't been updated since 2009.  Will be updated starting in 2023, but it won't be quick.  SKAdNetwork and Privacy Sandbox will make this interesting and see how to apply these to new standards.
            *   Google commitments to UK competition and market authorities.  Chrome is not going to be collecting market dynamics information or make it available to Google Ads that aren’t available to everyone else.  All participants get the same type of information for marketplace dynamics.
