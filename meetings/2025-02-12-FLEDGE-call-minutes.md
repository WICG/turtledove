# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday February 12, 2025

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!



1. Michael Kleber (Google Privacy Sandbox)
2. Sven May (Google Privacy Sandbox)
3. Brian May (unaffiliated)
4. Luckey Harpley (Remerge)
5. Alexander Tretyakov (Google Privacy Sandbox)
6. Phil Acker (Amazon)
7. Roni Gordon (Index Exchange)
8. Fabian Höring (Criteo)
9. Matt Davies (Bidswitch | Criteo) 
10. Paul Jensen (Google Privacy Sandbox)
11. Kevin Lee (Google Privacy Sandbox)
12. Orr Bernstein (Google Privacy Sandbox)
13. Pooja Muhuri (Google Privacy sandbox)
14. Sathish Manickam (Google Privacy Sandbox)
15. Donato Borrello (Google Ad Manager Video)
16. Laurentiu Badea (OpenX)
17. Patrick McCann (Raptive)
18. Xavier Plantaz (Google Privacy Sandbox)
19. Lydon, Jason (FT)
20. Trenton Starkey (Google Privacy Sandbox)
21. Kenneth Kharma (OpenX)
22. Sid Sahoo (Google Privacy Sandbox)
23. Abishai Gray (Google Privacy Sandbox)
24. Victor Pena (Google Privacy Sandbox)
25. Shafir Uddin (Raptive)
26. Koji Ota (CyberAgent)
27. David Dabbs (Epsilon)
28. Alonso Velasquez (Google Privacy Sandbox)
29. Andrew Pascoe (NextRoll)
30. Maksim Orlovich (Google Privacy Sandbox)


## Note taker: Matt Davies


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which  can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here (meetings with no items may be canceled)



*   Fabrice Gaignier/Fabian Höring
    *   Discuss  https://github.com/WICG/turtledove/issues/1367#issuecomment-2545927798
    *   Discuss https://github.com/WICG/turtledove/issues/579#issuecomment-2563046393
*   Phil Acker
    *   Discuss https://github.com/WICG/turtledove/issues/1151#issuecomment-2631433357
    *   Related to https://github.com/WICG/turtledove/issues/1367#issuecomment-2642893405 


# Announcements

Privacy Sandbox folks are holding every-second-Wednesday meetings in the hour after this meeting to discuss the trusted server elements of Protected Audience work. For more information see https://github.com/WICG/protected-auction-services-discussion/issues/27.

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!


# Notes 


## Fabian Höring: https://github.com/WICG/turtledove/issues/1367#issuecomment-2545927798

Fabian: 

Discuss the ticket and the comment 

Fabian is doing checks on the integration 

They lose participations for the most valuable bid requests 

When they have a lot of interest groups then they see drop a lot of participations

These are the most valuable users and want to discuss best ways to get around this. 

Groups are batched with the KV calls 

Played with some parameters 



*   Expensive calls 
*   Multiple calls then some will get through and improve the participation rate, batching into groups for potentially better participation
*   This did not make any difference to participation rates and is not doing anything 

Do not know what is happening and would like to know how to monitor and experiment with this. 

Want to know how best to improve this 

Want to monitor how to add these to the aggregation api 

For some reason their bidding script does not get activated and want to know how best to interact with the aggregation api 

Want to know the bidding and interest group and be able to define the bidding groups outside of the api and then how best to work with aggregation api 

Paul Jensen: Note that there is another closely-related issue, responded recently, see https://github.com/WICG/turtledove/issues/1347#issuecomment-2650727321 



*   Participation rate is an interesting metric 
*   It is affected by bidder 
    *   How often this is executed vs how often you get a PA request 
    *   The seller of the auction fulfilling the promises and allows the auction to continue 
*   The seller may decide to cancel the PA auction after the contextual bid
*   Then you get the trusted bidding and yet not generate bid 

Fabian:

This is exactly what they want to monitor.

Not able to ask the information from the seller but does not have all the information that they all have 

Nothing comes out of the auction and should be measured by private aggregation api

Number of interest groups per auction (Agg) and not based on the bidding scripts 

Need to define in gen bid and you need events not attached to the JS context

RTM has some events that can provide feedback 

You get a RT Monitoring report when you have an IG 

Do you send RTM if the seller cancels the auction - it is not something that we have currently specced yet 

How does this parameter have no effect 

Have 40 IG get all with one batched call - don't want to get 1 they want 2 calls 

Would expect more IG get through to the auction and this is not the case

Is this where the Auction is cancelled with the Seller - the IG will not participate as the auction was cancelled?

Should also know top level how many IG participated, but cant get this data as it is not coming out of the auction 

What do they want to observe - TLS cancelling auctions - need to be some correlation between how many ig on user device and the chance that the seller chooses to cancel the PA auction and serve a contextually targeted act 

What is the business logic between running contextual or PA auction

Related to the value of the user? 

Is this correlated between the times when there is PA vs a valuable contextual auction 

Not unreasonable that what is being see is that TLS are cancelling PA 

Michael Kleber: agrees that chrome should have the ability to send this data. 

Brian May: There are a couple of batches and this should in theory double the chance of participation rate but this is not the case

Fabian: Not a win rate issue it is a participation rate and this is 

Maksim Orlovich - if your controlling batching KV by the limit it should deliver them at the same time for the requests - will not be spreading them around with corresponding bids but will try and run at the same time 

If the responses are basically the same the latency will be the same irrespective of the volume of batches 

Fabian: If he has two JS scripts they should be 

Michael: If you can measure the cancellation of PA auctions would be very helpful as this gives much more data that can be tracked and you can then get the data to know if the participation rate is related to the cancellation of PA auctions or something different 

Should be a task for chrome team to take this task and work out the best way to let you measure this 

There is no way for a publisher side report for auction cancellations to include information on the buyers present in the browser, and there can not be a way to have that correlation with participation rate -  data from the publisher will not help there - agree that we need some way of providing this to the DSP 

David Dabbs: what are the signals on aborting the auctions 

There is an emerging group of pubs working on getting things started earlier and having the mechanism to measure if you're bidding or participating in this. 

Publishers should also need to know if the runner of the auction and the parties should know what is happening when auctions or users of auctions are removed from the auction 

Should be aggregated and allowed to be shared in a privately aggregated way. 

Roni Gordon: Index had lots of issues with GAM on all of this during CMA testing ramp-up  - participation rate and would be useful for sellers as well to understand this information 

It is against the principle of aborting the auction as this is designed to speed up the infrastructure and auction - but should be a design constraint that needs to be considered 


## Phil Acker: Discuss https://github.com/WICG/turtledove/issues/1151#issuecomment-2631433357

Also https://github.com/WICG/turtledove/issues/1367#issuecomment-2642893405 

Issues are both interrelated. What they are trying to do is monitor and measure how often scripts are timing out using the `percent-scripts-timeout` metric. This metric is currently rolled out to 1% stable.

**Question**: What is the timing for the `percent-scripts-timeout` to ramp up from its current status of deployed to 1% stable and be available on all browsers?

Is in the process of rolling out, What is the timeline for when this metric is ramped from 1-100% 

Paul:

Rollout timelines 

Brief overview: 

Chrome has early release channels 

Canary daily > dev weekly > beta weekly / monthly and then stable 

Generally follow the practice of 2 weeks on each channel.

Two weeks allows for ramp up and get good measurement on the effects on the experiments 

If the metrics are measured to be negatively affected and then 

Current 1% - could be rolled out to 100% on monday and is keen to roll out but dependent on no negative metrics 

keep watching issue #1151 to follow the roll out 

Even after the metric is deployed to 100% stable, it is not instantaneously available. The update requires the browsers to be restarted. Allow for another week or so to hit a large proportion of the population.  

It has some discoverability? - described in #1367 there is a way of checking if a device has this capability and should allow you to monitor. 

Brian: Various google chrome devports - can this be made available on a dashboard to monitor 

Alonso Velasquez: A dashboard that would show how close they are to rollout and how it is rolling out?

Chart with a line that shows the rollout 



*   If there was a dashboard that shows the rollout it may not show what an individual user would be able to

Paul: For every feature there is a chrome status with explainer and linked to spec 

Has the feature on when things are being rolled out 

Has a blink use counter which shows the various features and usage - but updated once a month and the github issues is more accurate

Issue: if you don't know the github issue number you may miss this and a dashboard can cover the updates easier 

Paul: Monitoring the blink mailing list as a way of checking and monitoring to get the details 

Caution on what you can get from the use of the blink counter as the use of the apis that are monitoring the usage and is reflected on the % of pages where a particular api is called as there are various factors including adtech’s using a particular feature and this affects the use counters 

Brian: Chrome has some way of monitoring level of development and there is a way of monitoring when things go from 1 - say 5% if there is visibility to the usage of this could be useful to adtechs to know if it is worth using 

Michael: When a particular feature is less than 1% stable, try and wait till it is 1% stable then this is a good time to start to develop against it, but once it is 100% stable this is a good indicator that it will be available globally across the board 

1% is a way to see if something is breaking and if so then shortly move to 100% 

BM: Possible to get the updates on how it is going from 1% stable to full population 

MK: The full population is usually 2 weeks after 1% but not hard and fast rule as dependent on no negative consequences - but a good rule of thumb around the timelines 

BM: Get an update if things get stalled from the 2 week timeline?

AV: There could be many engineering teams working on this that could delay things 

MK: if something has not rolled out to 100% within say a month - feel free to ask the question 

PJ: Multiple reasons - lots of launch processes behind this can take a while to get approval / holiday releases may slow things down etc … 


## Fabien: https://github.com/WICG/turtledove/issues/579#issuecomment-2563046393

FH: Planning to add global click feature on chrome ?

Is this a complicated feature to add as this would be useful for dsps

Adding boolean to previous wins have a user click on them? 

Do not have the suitable people in the room right now so 

MK: Thought that there may be a feature where adtechs can do this on their own however in this instance this is likely to be something that adtechs cant do on their own so we should explore this further. 

DD: If an issue is posted more than 24 hours in advance and needs a particular person from chrome to answer, would this be useful if there is an item on the agenda and need a particular person in the room for the discussion, would be easier to have this in the doc in advance - however chrome team would try and do this anyway 

Also could be posted in the ticket and asking them to have them discussed in the meeting - should be a good way to 

MD: is the plan to release the Chrome version of Protected Audience before the Android version?

MK:  Chrome version is fully released.

David Dabbs: Sellers control, to some degree, how much they use Protected Audience

MK: Right, buyers and sellers can choose to participate.  Nothing on Chrome side that is an impediment to using PA as widely as possible.

MD: When is the Android version coming online?

MK: Android folks not in the room to answer.
