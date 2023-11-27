# Protected Audience (formerly FLEDGE) WICG Calls: Agenda & Notes

Calls take place on ~~some~~ Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Sept 6, 2023


## Attendees: please sign yourself in!	



1. Paul Jensen (Google Chrome)
2. Xavier Capaldi (Optable)
3. Roni Gordon (Index Exchange)
4. David Dabbs (Epsilon)
5. Kevin Lee (Google Privacy Sandbox)
6. Sid Sahoo (Google Privacy Sandbox)
7. Sven May (Google Privacy Sandbox)
8. Michael Kleber (Google Privacy Sandbox)
9. Isaac Foster (MSFT Ads)
10. Abishai Gray (Google Privacy Sandbox)
11. Itay Sharfi (Google Privacy Sandbox)
12. Fabian Höring (Criteo)
13. Manny Isu (Google Chrome)
14. Orr Bernstein (Google Chrome)
15. Priyanka Chatterjee (Google Privacy Sandbox)
16. Marco Lugo (NextRoll)
17. Russ Hamilton (Google Chrome)
18. Caleb Raitto (Google Chrome)


## Note taker: Manny Isu


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here — no agenda no meeting!]



*   Isaac (MSFT) Issues (don’t all have to be mine, so grouping so we can easily filter)
    *   <span style="text-decoration:underline;">User IG view/delete interaction w/r/t BA payload optimization https://github.com/privacysandbox/fledge-docs/issues/56</span>
    *   <span style="text-decoration:underline;">Dynamic but K Anon Creatives https://github.com/WICG/turtledove/issues/729</span>
    *   <span style="text-decoration:underline;">Multiple Bids Per IG (checkin, was discussed a while back) https://github.com/WICG/turtledove/issues/595</span>
    *   <span style="text-decoration:underline;">PA Deals Phase: https://github.com/WICG/turtledove/issues/686</span>
    *   <span style="text-decoration:underline;">Cross Device https://github.com/WICG/turtledove/issues/607</span>
*   Nick (Triplelift)
    *   scoreAd can output a modified bid value in component auctions. Is it this modified bid that makes it to GAM for pub reporting?
        *   Non-component auctions: there is no modifiedBid output. Wouldn’t that still need to be required for a seller to implement a margin?
*   Xavier (Optable)
    *   reportResult and reportWin currently support an event-level sendReportTo which can be used to report bid information for billing purposes. However it will be replaced with the Private Aggregation Reporting which mentions it can be used for pricing information: https://github.com/patcg-individual-drafts/private-aggregation-api#protected-audience-reporting The resulting aggregatable reports are processed with the aggregation service which explicitly states all bucket values will be noised: https://github.com/WICG/attribution-reporting-api/blob/main/AGGREGATION\_SERVICE\_TEE.md#added-noise-to-summary-reports - this seems incompatible with reporting for billing purposes. Is this the intended future mechanism?


# Notes

This meeting will not happen next week (skipping Sept 13) because it's W3C TPAC.  See you there, if you plan to attend!


## **Dynamic Creative Selection** - <span style="text-decoration:underline;">Dynamic but K Anon Creatives https://github.com/WICG/turtledove/issues/729</span>



*   [Isaac] What is the extent that we will see dynamic creative selection as part of the IG? In order for a creative to render, the url has to be declared in the ads element of the IG. Syncing our data to IGs is a challenge - On many platforms, creatives can be updated regularly and then be out of sync. It will help to keep those campaigns in sync
    *   [Paul] There are a variety of different reasons why we have the IG have a list of the ads today. The two that are most relevant are:
        *   FLEDGE descended from the turtledove idea where you could have a lot of separation from the different pieces of the auction - anything pulled in real time from random url has a potential to leak. Ads url could be identifying.
        *   If we get these ads url at the time of the auction, even if they are k anon, we do not know where they came from - by declaring ahead of time, then we know that they are not randomly pulled out of a hat at auction time. These are privacy protections. We also discuss the things we can offer the users - transparency and controls. If these ads show up randomly, we would not be able to offer this transparency to our users.
    *   [Isaac] The user interaction piece is understandable. Can you help me understand the concern not related to k-anon?
        *   [Paul] If one of these millions of ads show up at auction time, we will not know if this is fingerprinting. By declaring it ahead of time, we will know that it is not
    *   [Isaac] Couldn’t the identity joining happen at IG declaration time?
        *   Perhaps, but any activity that happens after then, you would not be able to join them.
    *   [MK] The user transparency story is a great answer to your question. 
    *   [Isaac] It will be helpful to understand the way you are weighing… My concern is that they may be challenges to us telling that story.
*   [David] Assuming that the new negatively targeted ads feature is released, you can do anything you want with ads that are negatively added - it can be slipstreamed into the auction to the degree I understand it.


## reportResult and reportWin currently support an event-level sendReportTo which can be used to report bid information for billing purposes. 



*   [Xavier] However it will be replaced with the Private AggregationProtected Audience Reporting which mentions it can be used for pricing information: https://github.com/patcg-individual-drafts/private-aggregation-api#protected-audience-reporting The resulting aggregatable reports are processed with the aggregation service which explicitly states all bucket values will be noised: https://github.com/WICG/attribution-reporting-api/blob/main/AGGREGATION\_SERVICE\_TEE.md#added-noise-to-summary-reports - this seems incompatible with reporting for billing purposes. Is this the intended future mechanism?
    *   [Paul] There are different ways to look at it. The Private Aggregation API for Protected Audience is related to billing. While each bid price might be noised, the sum of positive or negative noise, the noise to 1 bit might be significant but overall might be less. We are thinking more and more about it - extending the private api to meet the needs here. It is a complicated area and this is why we are going to allow event level win reporting until at least 2026, which reports bid value without any noising. We will continue to think about more ways to satisfy the need of billing - we are asking folks to experiment with it. Can you elaborate more on the areas you are most worried about?
    *   [Xavier] Pacing… [inaudible]
    *   [Paul] reportWin is meant for assembling the event level report. In terms of retrying the fetch, I am not 100% positive that we retry it. We could certainly think about it if its a significant issue.
*   [Roni] I am curious about the noise requirements for the impression count. Fundamentally, it seems like the private aggregation api will not provide the 1000 number
    *   [MK] This is the fundamental question of DP. Adding a randomized amount of noise to aggregate is the best definition that the privacy community has come up with in the past 50 years of what it means to make something private. A thousand without any noise attached to it… The larger a space you’re aggregating over, the smaller the noise will be. There is a noise lab tool that lets you play around with the different parameters: https://developer.chrome.com/docs/privacy-sandbox/summary-reports/design-decisions/

        https://goo.gle/noise-lab


        Also, event level win report that carries the amount of money that change hands will not go away until at least 2026. And it will not go away until we find an alternative way to handle this use case. We are keenly aware of this use case and will not break it.


## Negative Targeting



*   [David] Put together a bunch of questions on it
*   [Orr] Will take a look at the questions

## <span style="text-decoration:underline;">User IG view/delete interaction w/r/t BA payload optimization https://github.com/privacysandbox/fledge-docs/issues/56</span>



*   [Isaac] In the move to B&A services, one of the features is that… are we going to require that the adtechs will be able to delete those when the IG is removed?
    *   [Paul] The ultimate optimization is sending one identifier. In today’s world, cookies are fairly small and represent users across different sites. The mental model is really just a random number, unique enough to identify a person. If you remove that number and have no way of getting it back, the data on the server becomes meaningless.
    *   [Isaac] But it still exists on any previous server side or persistent state and can still be used.
 
