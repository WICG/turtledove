# Protected Audience (formerly FLEDGE) WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Oct 25, 2023


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Brian May (dstillery)
3. Roni Gordon (Index Exchange)
4. Youssef Bourouphael (Google Chrome)
5. David Dabbs (Epsilon)
6. Sven May (Google Privacy Sandbox)
7. Caleb Raitto (Google Chrome)
8. Paul Jensen (Google Chrome)
9. Russ Hamilton (Google Chrome)
10. Orr Bernstein (Google Privacy Sandbox)
11. Manny Isu (Google Chrome)
12. Sid Sahoo (Google Chrome)
13. Don Marti (Raptive)
14. Isaac Schechtman (Criteo/BidSwitch) 
15. Joel Meyer (OpenX)
16. Risako Hamano (Yahoo Japan) 
17. Laurentiu Badea (OpenX)
18. Matt Menke (Google Chrome)
19. Brian Schmidt (OpenX)
20. Isaac Foster (Microsoft Ads)
21. Stan Belov (Google Ads)
22. Shivani Sharma (Google Chrome)
23. Alonso Velasquez (Google Chrome)
24. Alex Cone (Google Privacy Sandbox)
25. Marco Lugo (NextRoll)
26. Abishai Gray (Google Chrome)
27. Jeroune Rhodes (Google Privacy Sandbox)
28. Leeron Israel (Google Chrome)


## Note taker: Manny Isu


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


## Suggest agenda items here:



*   [MK] Request for feedback, buyside and sellside buy-in for ad slot size idea: https://github.com/WICG/turtledove/issues/869 
*   Roni Gordon
    *   Additional browser event beacons - https://github.com/WICG/turtledove/issues/826 
    *   K/V lookup timeouts - https://github.com/WICG/turtledove/issues/814
    *   Macro substitutions - https://github.com/WICG/turtledove/issues/817
    *   API versioning - https://github.com/WICG/turtledove/issues/823
    *   Sensitive signals - https://github.com/WICG/turtledove/issues/824
*   Isaac:
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Buyer/Seller Reporting Questions: https://github.com/WICG/turtledove/issues/682#issuecomment-1710965068
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   [Jeroune] Announcement
    *   The Google Privacy Sandbox team will be hosting our next set of webinars on Protected Audience. This set will focus on multi-seller auctions, where we will cover sequential auction setup, different types of auctions and participants involved, examine the overview diagram, and walkthrough a detailed sequence diagram along with code. The first **Americas friendly session** is happening on** Nov. 7th 1-2 pm ET**. A second **EMEA friendly session** is happening **Nov. 8th 7-8 am ET**. To join, please register below: 
    *   AMER: [rsvp.withgoogle.com/events/protected-audience-multisellerauction-amer\_a27cb5](https://rsvp.withgoogle.com/events/protected-audience-multisellerauction-amer_a27cb5)
    *   EMEA: https://rsvp.withgoogle.com/events/protected-audience-multisellerauction-emea\_a27cb5\_339693


# Notes


## [MK] Request for feedback, buyside and sellside buy-in for ad slot size idea: https://github.com/WICG/turtledove/issues/869 



*   Feature request for Ad slot size to be included in the request to the KV server; more directed at Buyer KV server - Is it possible for the KV server to know what slot size the ads is to make retrieving bidding signals more efficient?
*   Generally, the browser sends a single KV request… reusable for all the different PA auctions on a page. In an attempt to square the circle, proposing a new parameter that can be added to the auction config.  When kicking off the auction, what if we added a way for the top-level SSPs to say "here are all the different ad slot sizes on the page". Then the KV server request could include all of them and the KV response will take that into consideration; we can have caching and ad slot size known by the KV server. It seems viable if the buyers and sellers like this idea.
*   [Brian] What percentage of pages know what the ad slot will be?
    *   [Isaac] Might need to run some queries to determine the answer. I think it is significant 
    *   [MK] With the proposal, if you later come across an ad auction for a size that is not on the list, you can still run a PA auction on it, it will just trigger an additional KV request
    *   [Brian] So it seems like we are significantly increasing the number of moving parts in a request? It seems like something we should take into consideration
    *   [MK] Yes, for sure
*   [Roni] **Point #1: **Let's let DSP KV server url opt into this behavior by having some macro in this url - like responsive ads in the render url.  So a DSP that doesn't care about slot size doesn't pay any new cost.
    *   **Point #2: **GPT tag should know all the slot sizes on the page, and it should know how to do this. I think anything we do here would have to do with libraries; I think it should be defined dynamically from javascript to javascript
*   [Joel] This is a lot of lift from GAM specific use case. SSPs won’t benefit because we do not get cache.  How does viewability factor into this?
    *   [MK] The question is to determine whether this would be useful to other buyers — does slot size matter to you in SSP KV? Also, I do not think that viewability should come into this much. It seems like predictive model on viewability to take into account the domain is already possible with the KV server.
    *   [Brian] The call to the KV server cannot be used as an indication that something is viewable?
    *   [MK] I don’t know what logic SSPs use to kick off the auction but certainly possible the call might happen after the load page or at the beginning. Not sure
*   [Isaac] From a DSP perspective, the size of the creative can vary… the creative ID on the trusted bidding signals.. Might help with real time requests for the creatives. On the buyside, this will be relevant to help them return real time signals. Anything that can be done by framework will be cheaper. I would see particular value for a DSP who has that type of creative.  Also could be very useful for filtering, not sending back signals for creatives that cannot bid due to slot size mismatch.


## Additional browser event beacons - https://github.com/WICG/turtledove/issues/826 



*   [Roni] There are two classes of events - At the moment, because JS executed is controlled by buyside, the buyer has to indicate that these reserve event. Without buyer writing, the seller will get none of these events. How do we make it so that frames do not leak things that they aren’t supposed to leak, but having buyers and sellers automatically get the reserved events.
    *   [MK] I would broaden a little and say there are 3 kinds of events
        *   **Render:** Did this url get put in an iFrame? That is something that is automatic and everyone gets to find out and does not require any special opt in. You can observe from outside the frame.
        *   **Viewability:** No automatic way of SSP doing viewability beaconing today. We should invent something like intersection observer, and we should be able to make it happen automatically as configured by SSP, because this is something that can be observed from the outside page.
        *   **Click Event:** Front the browser POV, it happens inside the ad. Like every other cross domain iFrame, somebody outside cannot tell what happens inside. With some opt-in, the iframe will need to say it is okay for somebody outside to see
    *   [Roni] The render event is not automatic because there is no reserved event that gets fired automatically.
        *   MK: the reportWin and reportResult are indirectly indications of render event
        *   [from zoom chat, Matt Menke]: It's fenced frame nav start, not rendering that triggers reportWin reports.
            *   MM: I was just referring to the sendReportTo() and debug reports (which aren't based on navigation initiated by the fenced frame, but rather the navigation to the ad URL in the first place)
    *   [MK] The thing you are asking for is an event for ad click navigation… you want to know that the ads got to send the user to the landing page
    *   [Roni] Even if all we can get is that someone clicked on the bounding box of the rectangle - knowing that the user interacted with the ad container.
    *   [Shivani] Knowing that the click happened is more than what you can get today. There is a change in progress where the opt in has been made a little bit more flexible - as long as API is called, anyone who has registered to receive the beacon will receive it. It’s like an opt-in from the buyer side.
    *   [Joel] Echoing Roni’s request. Publishers are in the dark about what happens on their page. There is a huge need. If Chrome were to standardize a way, this will help move adtech world forward to improve publisher awareness, could end better than today.
        *   [MK] As an SSP, if we found a way to make it possible, would you go so far to take the position to refuse to render any ads that does not opt in to sharing this click-event information?
        *   [Joel] I will turn the decision over to publishers - It’s a monetization decision.
        *   [MK] Roni, do you think that if we did something like that, would that be satisfactory to your issue?
        *   [Roni] Yes, I would like to see how that evolves.
    *   [Shivani] For the non JS opt in, should we continue discussions on the GH issue?
        *   [MK] Yes, we should
    *   [Paul] We do have security concerns because you learn something happening inside the ads, so we have to get opt-in. And opt-in cannot happen just from the IG… the owner of the IG can provide any render url and we can't just let them find out what is happening inside the ad
    *   [Roni] Are we saying that there is no way for the frame to know that the buyer are seller origin is involved?
    *   [Paul] The big issue is that on the web, anyone can create a frame as a parent but there is no way to verify that. We are looking for a way that an ad can say this buyer is okay to share.
    *   [MK] Roni is right that the browser has the ability to know that the ads appeared, but the point is that even if we added an API to help with this, a naive ad does not use any API to check whether the party that caused it to be rendered was someone they expected
    *   From zoom chat
        *   Paul Jensen
            *   HTTP response headers are an efficient way to do this.  /.well-known files are a less efficient but sometimes easier way
        *   Matt Menke 
            *   HTTP response headers also allow us to let the SSP or DSP demand the header be present, or we won't load the ad.
            *   If folks want that ability
    *   [MK] No perfect solutions. We will keep discussing the issue.
