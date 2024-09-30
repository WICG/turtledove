# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Sept 18, 2024 

(Sept 11 meeting was canceled due no suggested agenda items) 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!	 



1. Paul Jensen (Google Privacy Sandbox)
2. Michael Kleber (Google Privacy Sandbox)
3. Roni Gordon (Index Exchange)
4. David Dabbs (Epsilon)
5. Sathish Manickam (Google Privacy Sandbox)
6. Alex Peckham (Flashtalking)
7. Kevin Lee (Google Privacy Sandbox)
8. Garrett McGrath (Magnite)
9. Laura Morinigo (Samsung)
10. Lydon, Jason (FT)
11. Fabian Höring (Criteo)
12. Omri Ariav (Taboola)
13. Eyal Segal (Taboola)
14. Matt Kendall (Index Exchange)
15. Alonso Velasquez (Google Privacy Sandbox)
16. Manny Isu (Google Privacy Sandbox)
17. Raz Kliger (Taboola)
18. Shivani Sharma (Google Privacy Sandbox)
19. Guillaume Polaert (Pubstack)
20. Jeremy Bao (Google Privacy Sandbox)
21. Sid Sahoo (Google Privacy Sandbox)
22. Warren Fernandes(Media.net)
23. Luckey Harpley (Remerge.io)
24. Joshua prismon (Index Exchange)
25. Arthur Coleman (ThinkMedium)
26. Bharat Rathi (Google Privacy Sandbox)
27. Diana Torres (MLB)
28. Koji Ota (CyberAgent)


## Note taker: Alonso Velasquez


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here (meetings with no items may be canceled)



*   Fabian Höring Protected Audience TPAC Agenda https://github.com/WICG/admin/issues/194#issuecomment-2356201005
*   Aditya Agarwal: Update on issue https://github.com/WICG/turtledove/issues/1115 
*   Omri Ariav - Taboola: Update on native advertising feature requests
    *   Suppression for native follow up - https://github.com/WICG/turtledove/issues/896#issuecomment-2272778495 
    *   Ad coordination
    *   Taboola comment to the ‘Creative pre-registration strategies’ doc (#[792](https://github.com/WICG/turtledove/issues/792)) -  supporting the native assets with more native ads on page
    *   Native look and feel follow up (reference to the OpenRTB native object - https://www.iab.com/wp-content/uploads/2018/03/OpenRTB-Native-Ads-Specification-Final-1.2.pdf)  

*   Alexandra Peckham (need to push this to 10/2, due to attendance conflicts on our end)
    *   Discussion of / update on this issue: https://github.com/WICG/turtledove/issues/1028 


# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.



*   **_Note: The Microsoft meeting has come out of dormancy and is indeed happening tomorrow!_**
    *   [Fabian] What will we jointly talk about at TPAC? Will there be an opportunity for Criteo to present their findings and benchmarks of the Edge Ad Selection API? 
    *   [Michael K] The most efficient way to do this is to coordinate between the Edge and Chrome WICG teams, so the best way to get this organized will be to bring up questions in tomorrow’s Edge call (Sept 19th)

**No WICG meeting on Sept 25th: conflict with TPAC**

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!! :) 


# Notes 

New blog post: https://privacysandbox.com/intl/en_us/news/upcoming-privacy-sandbox-developments/



##   Updated public developer documentation:
*   Deals guide: https://developers.google.com/privacy-sandbox/private-advertising/protected-audience-api/use-case/deals
*   Reporting ID usage guide: https://developers.google.com/privacy-sandbox/private-advertising/protected-audience-api/guide/reporting-id

##   Fabian Höring Protected Audience TPAC Agenda https://github.com/WICG/admin/issues/194#issuecomment-2356201005
*   [Fabian] Will there be an opportunity for Criteo to present their findings and benchmarks of the Edge Ad Selection API? 
*   [Michael K] The most efficient way to do this is to coordinate between the Edge and Chrome WICG teams, so the best way to get this organized will be to discuss in tomorrow’s WICG Edge call of Sept 19th 

##   Aditya Agarwal: Update on issue https://github.com/WICG/turtledove/issues/1115 
*   [Aditya] Ability requested for publishers to allow analytics by 3rd partier
*   [Paul] we are doing internal drafts as a response to the proposal. The main open question is how might the publisher declare the permissions, is it going to be via response header or via fetch? Working thru the pros and cons
*   [Warren] Yes, we are agreeing on needing to answer this. We want to make sure this information is sent to the auction before it begins. 
*   [Paul] the network fetch is likely easier for adoption but worse from a network/bandwidth synchronization standpoint, may introduce intra locks and sync considerations 
*   [Alonso] are we trying to solve a publisher-only problem or a more generic problem?  Could match other needs from other roles.  Encourage folks to speak up but want to make sure we aren’t missing other needs.

##   Omri Ariav - Taboola: Update on native advertising feature requests
*   Overall (account mgmt):
    *   [Alonso] Which account manager are you working with?
    *   [Omri] Ray Brusca on the Android side. Waiting for someone we can engage on the Chrome side. 
    *   [Alonso] Good to get priority between items. Let me reach out to Ray for any news on the Chrome-focused requests here.
    *   [Omri] Looking forward to feedback sprints.
    *   [Alonso] I’ll follow up on this.
    *   [Omri] Would like to discuss open questions on GitHub issues.
*   Suppression for native follow up - https://github.com/WICG/turtledove/issues/896#issuecomment-2272778495 
    *   [Omri] Looking for a status update 
    *   [Michael K]: we have tried to enable a lot of what you are asking for that is possible in the contextual call within the protected audience environment in a way that continues to respect our privacy goal. Things we enable is allow the browser to hold a number of ad candidates and then allow for the suppression logic to happen in a secure environment. We have been looking at this for a while and we don’t think it’s possible. 
*   Ad coordination:
    *   supporting rendering endless ads side-by-side that have variety and do not repeat ([example](https://apnews.com/article/chelsea-liverpool-score-league-cup-final-a941133d22cdb8c5909b10ed30292e7e), scroll to the end) (ref - [1199](https://github.com/WICG/turtledove/issues/1199), [1097](https://github.com/WICG/turtledove/issues/1097), [1074](https://github.com/WICG/turtledove/issues/1074))
    *   [Omri]: looking for an update on progress. The 3 FR above some up what we’re looking for Chrome to solve under “ad coordination”. The current solution makes it hard for us to engage correctly, we need to wait for the rendering processes to end, not ideal. 
    *   [Michael K]: as a reminder, we prefer declarative ways for the ad selection process to happen. This is preferable than to allow the logic of using the browsing history as a filtering criteria, because as it’s known to most here, the browsing history represents multiple sites that the browser has visited, and that is antithesis to our privacy goals. 
    *   [Eyal]: If you see the proposal, our idea is to have a set of constraints during the auction and let the browser determine what needs to be filtered, inclusive of the seller’s rules
    *   [Michael K]: Yes, question for you, in the proposal do you see anything concerning from a privacy perspective based on what I went over? 
    *   [Michael K]: also recall that there are multiple parties that can have script on page so there is no way for the browser to know which party added the constraints, it could be Taboola or some other entity that is not intended to participate, so that is a worrying thing for us.
    *   [Eyal]: We don’t see the same worry, we think we are proposing a way to send this information from contextual ads to AuctionConfig 
    *   [Michael K]: I think I see what you are proposing, let the contextual response/reply from the server take some of those signals to do that filtering then enter them into the auction configuration. 
    *   [Eyal]: one thing to solve is that the information can come from different zones 
    *   [Fabian]: from a buyer perspective we think this feature request is useful, we need a way to solve it, we see it being somewhat similar to the negative interest group approach (https://github.com/WICG/turtledove/issues/896). Not in favor of yet another API but rather auctionConfig. We care less about duplicate ads (as it already happens today via multiple channels, 100% duplication on same seller is still an issue however), but we care more about losing the opportunity; if an ad is filtered out, we would like the opportunity to find another one we can still bid with. 
    *   [Michael K]: we agree everything you say is reasonable to try to solve. 
    *   [Eyal]: For duplication, we don’t want to see the same buyer with the same campaign and the same creative. We can bump into this diversity issue. 
    *   [Michael K]: Suppose Taboola buys via 2 different SSPs, are you asking that if you are bidding via SSP 1 that SSP 2 for the same impression opportunity should be suppressed?
    *   [Eyal]: as a buyer, I don’t care 
    *   [Michael K]: Would request for each role to self-represent their requirements/constraints
    *   [Eyal]: okay from a seller perspective I have our own rules, we can’t allow the same advertiser to submit the same creative/ad across multiple SSPs. 
    *   [Michael K]: We would like to hear from others in the buyside to directly give input on this requirement as well
*   Taboola comment to the ‘Creative pre-registration strategies’ doc (#[792](https://github.com/WICG/turtledove/issues/792)) -  supporting the native assets with more native ads on page
    *   [Orr]: Looking at the table of proposals you gave us, we are exploring solutions precisely like the trusted server idea, the first row of your table. We will have more on this subject soon. Also, the feedback you have already given with your table of ideas is extremely valuable. 
    *   [Roni] Index Exchange is also very interested in this discussion, apologies we haven’t been able to follow up as fast as we wanted
*   Native look and feel follow up (reference to the OpenRTB native object - https://www.iab.com/wp-content/uploads/2018/03/OpenRTB-Native-Ads-Specification-Final-1.2.pdf) 
    *   [Michael K]: as a reminder we have done material research and development already on how to enable formats like video rendering. We believe that a lot of what we have done for video will apply to the native use case, so we will be not starting from zero when we start looking at what is needed specifically for native. 
