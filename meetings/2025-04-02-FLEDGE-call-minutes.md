# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California and 5pm Paris time.

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday April 2, 2025

March 19 and 26 were both cancelled because no topics

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!



1. Michael Kleber (Google Privacy Sandbox)
2. Brian May (unaffiliated)
3. Rushil Walia (Google Privacy Sandbox)
4. Orr Bernstein (Google Privacy Sandbox)
5. Sven May (Google Privacy Sandbox)
6. Pooja Muhuri (Google Privacy Sandbox)
7. Matt davies (Bidswitch | Criteo)
8. Bharat Rathi (Google Privacy Sandbox)
9. Paul Jensen (Google Privacy Sandbox)
10. Alex Peckham (Flashtalking)
11. Garrett McGrath (Magnite)
12. Shafir Uddin (Raptive)
13. Sathish Manickam (Google Privacy Sandbox)
14. Kevin Lee (Google Privacy Sandbox)
15. Fabian Höring (Criteo)
16. Patrick McCann (Raptive)
17. Nicolas Urien (Criteo)
18. Xavier Plantaz (Google Privacy Sandbox)
19. Laurentiu Badea (OpenX)
20. Denvinn Magsino (Magnite)
21. Abishai Gray (Google Privacy Sandbox)
22. Alonso Velasquez (Google Privacy Sandbox)
23. Miguel Morales (IAB TechLab)
24. Victor Pena (Google Privacy Sandbox)
25. Matt Kendall (Index Exchange). 
26. Roni Gordon (Index Exchange)
27. Omri Ariav (Taboola)
28. Chris Fredrickson (Google Privacy Sandbox)
29. Elmostapha BEL JEBBAR (Lucead)
30. Fatma Moalla (Criteo)
31. Johann Hofmann (Google Privacy Sandbox)


## Note taker: Sathish Manickam


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which  can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here (meetings with no items may be canceled):



*   Proposal: New trigger for highestLosingBuyerBid in aggregate loss reporting https://github.com/WICG/turtledove/issues/930 (added by [Rushil Walia](http://who/rushilwalia), cc [n.urien@criteo.com](mailto:n.urien@criteo.com))
*   Discuss context of https://github.com/prebid/Prebid.js/issues/12729#issuecomment-2772860753 (Matt Kendall) 	


# Announcements

Privacy Sandbox folks are holding every-second-Wednesday meetings in the hour after this meeting to discuss the trusted server elements of Protected Audience work. For more information see https://github.com/WICG/protected-auction-services-discussion/issues/27.

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!


# Notes 


## Rushil Walia: https://github.com/WICG/turtledove/issues/930



*   Bid-shading as the new trigger for highestLosingBuyerBid. Bid-shading is the buyside use case for first-price auction, where you adjust the final bid price based on models. Both win and loss reports are needed as modeling signals. 
    *   Ad techs could egress every bid, but Priv Agg API limits this, and it's probably more data than you need. 
    *   We are considering adding a reserved.highestLosingBuyerBid trigger to the Protected Audience reporting via Private Aggregation. 
    *   More details are available in the github issue linked. We are seeking feedback. 
*   Michael: Welcoming the feedback here or in the github issue. 
*   Patrick McCann: Are you interested in the highest desirable bid, or are you interested in what was your highest bid? Because Sellers can modify the desirability. 
*   Rushil: We are mainly looking at the highest bid from the buyer, not the desirability. 
*   Nicolas Urien: As buyers we are interested in the highest bid, because that is the value we have. There is a subsequent request in the GitHub that discusses an equivalent top level trigger. In the end, the buyer can’t access the desirability score, nor extract it via Private aggregation API.
*   Patrick: Ok.. but if the loss reports include other winning bid’s price or data, it may be an additional leak.. 
*   Michael: There is no new information shared, and there are no leaks in the proposal. It is merely filtering down the information that lets the buyer know the highest bid value. 
*   Partick: Ok.. but I assumed desirability will be something useful for the buyers. Are you also considering the winning bid data, which is what other exchanges do? 
*   Rushil: Some information of winning bid and highest scoring other bid is already available. 
*   Michael: more details on the current reporting bid capabilities are available here: https://github.com/WICG/turtledove/blob/main/FLEDGE_extended_PA_reporting.md
*   Elmostapha Bel Jebbar: who will get access to the highestScoringOtherBid exactly? 
*   Rushil: Any buyer who is eligible to receive reports based on auction participation criteria. E.g. for the trigger reserved.highestLosingBuyerBid, buyers will be able to register a histogram contribution for highestScoringOtherBid from that auction, if they participated with a bid that received a seller desirability score > 0
*   Matt Davies (on Chat): If you know the value of your bid and it is equal to the highest scoring other bid - would you then not know that it was yours?
*   Rushil: Your bid value will be based on the bucket range, but granular details will not be available. 
*   Michael: There is a separate report, which can be limited to win reports. It can include ‘I have won, and other details’ for the winner. That is available.
*   Rushil: But that won’t be available in the loss reporting. 
*   Patrick (on chat): can the highestScoringOtherBid be the contextual winner
*   Michael: In the early days of PA, it was not possible. But things have changed now. The highest scoring bid that ends up not winning will get reported. Having the contextual bids come through as additional bids is how it is built now, and all buyers are welcome to use it. 
*   Patrick: Contextual bids may come through other routes. And knowing what was the winning bid, in case of loss will be useful information. Contextual bidding has evolved and added a bunch of tooling around it. And if information is not available it would impact these. 
*   Michael: That loss information is not available from the browser right now: PA does not have information on what the winning bid was, if that bid came from a purely contextual flow that never touched PA. But sellers are fully able to decide which bids to accept and reject. The browser can't make sellers give you that information if they don't want to.
*   Patrick. The only loss reporting that is useful is in the contextual bids.. And not clear how this change can help. 
*   Roni (on Chat): are we talking about an aggregate signal that is the equivalent of highestScoringOtherBid in fDO callbacks?
*   Rushil: No we are talking about a new trigger for Private aggregation report for loss. 
*   Paul Jensen: Just a clarification: If there are two component sellers, and lose the bid, do they both get the highestScoringOtherBid? 
    *   Rushil: yes.. It would be per buyer.


## Michael on the context for Matt Kendall’s post:  https://github.com/prebid/Prebid.js/issues/12729#issuecomment-2772860753 



*   Michael: The storage access header API is to inform if the 3p cookies are turned off in a browser. We did this primarily, because people may run PA auctions in browsers only where the cookies are turned off
*   Question in this issue: Is there a possibility that any script (such as prebid),  determines that while 3p cookies are not available to me, but is it generically available? 
*   Matt Kendall: I didn’t create the main issue, I think DV3 might have. Context is related to inserting an 3p iframe in the 1p context and get that similar information? 
*   Patrick (on Chat): this was proposed by dv3 here https://github.com/prebid/Prebid.js/issues/12729 . dv3 proposed prebid discover 3rd party cookie availability by this method https://github.com/WICG/turtledove/issues/1345#issuecomment-2494657919 
*   Patrick: Proposal is to insert a frame and postmessage instead of a cookie, is that it? 
*   Johann Hofmann: There is a security aspect to revealing a 3p if a 1p cookie is available. We don’t do that, since it may lead to some security issue. However, if the constraint is to get, instead of any specific 3p cookie, generally understand if it is blocked? 
*   Patrick: If I happened to be a 3p, do they get access to the storage access header? 
*   Johann: That’s a good question, the user consents are double keyed
*   Brian: Seems like there are two pieces of information we could get: 3rd party cookies are disabled for the browser (there may be exceptions, but the setting is turned off) and 3rd-party cookies are turned off for the specific 3rd-party which is about to be called. First let’s us know if we want to use cookies we need to do a round trip to see if they’re available. Second lets us know we won’t be able to set cookies, so don’t do anything that depends on them.
*   Fabian: 3rd party cookie access flag should be consistent across several supply channels for one Chrome browser and work for the current Chrome facilitated mode B traffic 
*   Johann: Chrome will take into account feedback and design something
