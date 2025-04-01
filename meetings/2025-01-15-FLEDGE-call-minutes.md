# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday January 15, 2025

Happy New Year.  Jan 8 call was cancelled because no agenda items

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!



1. Michael Kleber (Google Privacy Sandbox)
2. Brian May (unaffiliated)
3. Pooja Muhuri (Google Privacy Sandbox)
4. Victor Pena (Google Privacy Sandbox)
5. Courtney Johnson (Google Privacy Sandbox)
6. Matt Kendall (Index Exchange)
7. Paul Jensen (Google Privacy Sandbox)
8. Wojciech Rybak (RTB House)
9. Charlie Harrison (Google Privacy Sandbox)
10. Matt Davies (Bidswitch | Criteo)
11. Ivan Staritskii (BidSwitch | Criteo)
12. David Dabbs (Epsilon)
13. Kevin Lee (Google Privacy Sandbox)
14. Orr Bernstein (Google Privacy Sandbox)
15. Abishai Gray (Google Privacy Sandbox)
16. Laurentiu Badea (OpenX)
17. Lydon, Jason (Flashtalking)
18. Owen Ridolfi (Flashtalking)
19. Becky Hatley (Flashtalking)
20. Tim Taylor (Flashtalking)
21. Sven May (Google Privacy Sandbox)
22. Eyal Segal (Taboola)
23. Denvinn Magsino (Magnite)
24. Patrick McCann (Raptive)
25. Nour Nabil (Google Privacy Sandbox)
26. Harshad Mane (PubMatic)
27. Sarah Harris (Flashtalking)
28. Shafir Uddin (Raptive)	
29. Jeremy Bao (Google Privacy Sandbox)
30. Hillary Slattery (IAB Tech Lab)
31. Aymeric Le Corre (Lucead)
32. Felipe Gutierrez (Microsoft)
33. Alex Peckham (Flashtalking)
34. Rushil Walia (Google Privacy Sandbox)
35. Donato Borrello (Google Ad Manager)
36. Andrew Pascoe (NextRoll)
37. Bharat Rathi (Google Privacy Sandbox)
38. Koji Ota (CyberAgent)


## Note taker: Hillary Slattery


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here (meetings with no items may be canceled)



*   seller-independent Real-Time Reporting API opt-in mechanism (Wojciech Rybak, RTB House)
    *   https://github.com/WICG/turtledove/issues/1277#issuecomment-2590747593 

*   Third Party reporting (Ivan Staritskii, BidSwitch)
    *   https://github.com/WICG/turtledove/issues/1220 
 
*   Creative Scanning proposal
    *   https://github.com/WICG/turtledove/issues/792 



# Announcements

Privacy Sandbox folks are holding every-second-Wednesday meetings in the hour after this meeting to discuss the trusted server elements of Protected Audience work. For more information see https://github.com/WICG/protected-auction-services-discussion/issues/27.

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!


# Notes 


## seller-independent Real-Time Reporting API opt-in mechanism (Wojciech Rybak, RTB House), https://github.com/WICG/turtledove/issues/1277#issuecomment-2590747593

Problem: The opt-in Mechanism is difficult to use for buyers 



*   (minor) Relies on seller turning on API, buyers are dependent on sellers turning API ON/OFF. 
*   (major) Request for buyer to be able to turn API OFF due to above
*   (minor) no way for buyer to confirm which seller had actually turned the API on/off after request was sent  
*   Potential path
    *   Ad config inside perBuyerSignals for sellers to give info to buyer 
        *   Dependant on the request - no way to turn off a seller that’s spamming a buyer
    *   Expose endpoint containing config file that the browser could fetch and turn API on/off based on what the endpoint says
    *   Seller could get pbs, look for auction config and put it into real time auction monitoring config
    *   Buyers assign sellers an ID in the real time monitoring report that could populate inside generateBid to give buyers info of which seller is sending reporting (or not)
        *   Hits privacy budget
        *   Would only know in aggregate 
*   Paul Jensen: [Explainer doc outlining constraints by design](https://github.com/WICG/turtledove/blob/main/PA_real_time_monitoring.md#opt-in-mechanism-consideration)
    *   At auction time, browser only has auction config to go off of 
    *   Leakage risk using trusted bidding signals 
    *   Unsure of forward path if bidding script load fails
*   Scoped to only script load failure reports w/in real time reporting API 
*   Is there an existing comms channel that buyers could leverage that would impact the auction 
    *   Use PreBid config
        *   Avoid browser ping requirement, use pub’s prebid config 
        *   Parallel to pbs for ‘perBuyerConfig’ that would be exposed directly to buyers 
            *   New object for container to hold more static stuff that buyers expect sellers to pass through unchanged 
    *   Update to [ORTB Ext](https://github.com/InteractiveAdvertisingBureau/openrtb/blob/main/extensions/community_extensions/Protected%20Audience%20Support.md) 
        *   Requires buyer to provide ORTB “contextual” response, but could work
        *   Feeds into browser real time monitoring config
        *   Mechanism similar to how currency (`cur`) attribute works in ortb extension
        *   One option: Add new attribute similar to pbs and cur but allows buyers to turn on monitoring in real time
            *   The things we do for you
        *   A different option: Bundling this into pbs 
            *   Transparent to sellers 
            *   No additional work required by seller
            *   Browser would need to look inside perBuyerSignals and understand a special field inside
        *   Adding handshake w/browser requires some coms between ORTB and PAA whenever new attributes are supported 
            *   SSPs building an auction config would look at ORTB object that’s a sister field to pbs 
    *   Trusted Bidding Signals 
        *   Authenticated from buyers
*   Foundational change in current design of what is contained within perBuyerSignals since it would require browser to look in an object for a purpose it doesn’t currently hold.  Right now perBuyerSignals can be whatever buyer wants and the browser does not interpret it at all.
*   MKleber: I think Chrome should go back and assess this involvement by the browser. If warranted, and per Charlie we find other needs, we’ll investigate a new mechanism.
*   Laurentiu: Sellers are already shoveling and assembling form buyers’ "contextual response (ORTB)” into the PA auction Config. Sellers just need to do a small additional bit of config assembly.  Let's keep doing the thing we are doing now, sellers just do a little bit more, it already works.
