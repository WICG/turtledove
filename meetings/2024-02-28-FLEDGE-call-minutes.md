# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 4pm UTC (during winter)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Feb 28, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	 



1. Michael Kleber (Google Chrome)
2. Charlie Harrison (Google Chrome)
3. Brian May (dstillery)
4. Roni Gordon (Index Exchange)
5. Rushil Walia (Google Chrome)
6. David Dabbs (Epsilon)
7. Pawel Ruchaj (Audigent)
8. Arthur Coleman (OnlineMatters)
9. Sathish Manickam (Google Privacy Sandbox)
10. Harshad Mane (PubMatic)
11. Matt Menke (Google Chrome)
12. Kevin Lee (Google Privacy Sandbox)
13. Paul Spadaccini (Flashtalking)
14. Sven May (Google Privacy Sandbox)
15. Paul Jensen (Google Privacy Sandbox)
16. Alonso Velasquez (Google Chrome)
17. Orr Bernstien (Google Privacy Sandbox)
18. Isaac Foster (MSFT Ads)
19. Laurentiu Badea (OpenX)
20. Joel Meyer (OpenX)
21. Wendell Baker (Yahoo)
22. Rickey Davis (Flashtalking)
23. Ricardo Bentin (Media.net)
24. McLeod Sims (Media.net) 
25. Shankar Venkataraman (Jivox)
26. Laszlo Szoboszlai(Audigent)
27. Russ Hamilton (Google Privacy Sandbox)
28. Alexandre Nderagakura (Not affiliated)
29. Abishai Gray (Google Privacy Sandbox)
30. Courtney Johnson (Google Privacy Sandbox)
31. Kenneth Kharma (OpenX)
32. Qingxin Wu (Google Chrome)
33. Manny Isu (Google Chrome)
34. Vedant Seta (Media.net)
35. Owen Ridolfi (Flashtalking)
36. Fabian Höring (Criteo)
37. Renan Feldman (Google Privacy Sanbdox)
38. Becky Hatley (Flashtalking)
39. Anthony Yam (Flashtalking)
40. JASON LYDON (“)
41. Jonasz Pamuła (RTB House)
42. Jessica Cobbe (Nexxen)
43. Garrett McGrath (Magnite)
44. Alex Peckham (Flashtalking)
45. Tim Taylor (Flashtalking)
46. Stan Belov (Google Ad Manager)
47. Nikunj Agrawal (Google Privacy Sandbox)
48. Przemyslaw Iwanczak (RTB House) 
49. Miguel Morales (IAB TechLab)
50. David Tam (Relay42)
51. Taranjit Singh (Jivox)
52. Andrew Pascoe (NextRoll)


## Note takers: Manny Isu; Arthur Coleman


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## Suggest agenda items here:



*   Rushil Walia - Google (Privacy Sandbox)
    *   Request for requirements relating to real time monitoring of PA auctions
    *   https://github.com/WICG/turtledove/issues/430 
*   Charlie Harrison:
    *   Improved mechanisms for `modelingSignals` and TEE-based model training
    *   https://github.com/WICG/turtledove/issues/1017
*   Isaac Foster:
    *   Revisit possible solutions for CDN based static file hosting https://github.com/WICG/turtledove/issues/813
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   [Roni Gordon (will be joining late)](https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736)
    *   [reportWin / reportResult seem to fire when runAdAuction completes, whereas the forDebuggingOnly endpoints only fire when the opaque URN is rendered by the IFRAME](https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736)
        *   [ I don’t believe this is expected… otherwise it would create discrepancies; have yet to file a github issue, WIP](https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736)
*   David Dabbs
    *   Follow up to Michael Kleber’s comment on https://github.com/WICG/turtledove/issues/1062#issuecomment-1969153894 \
 \
_But also, if you're talking about frequency capping across different IGs joined on the same site, then this seems reasonable — but perhaps we can improve the Protected Audience previous wins data so that you don't need the KV server for it._
*   Fabian Höring:
    *   Short term options & MACRO replacement for VAST/Native workflow
    *   Initial Chrome Demo: https://privacy-sandbox-demos.dev/docs/demos/instream-video-ad-multi-seller/
    *   https://github.com/google/ads-privacy/tree/master/proposals/fledge-formats-rendering
    *   https://github.com/WICG/turtledove/issues/265


# Announcements


# Notes


## Rushil Walia - Google (Privacy Sandbox): Request for requirements relating to real time monitoring of PA auctions https://github.com/WICG/turtledove/issues/430 


*   Slides: https://github.com/WICG/turtledove/blob/main/meetings/2024-02-28-FLEDGE-call-slides-Real-Time-Monitoring-feedback-request.pdf 
    *   This thread was started to propose extensions of FLEDGE whose purpose is to support effective monitoring and error reporting of client side auctions The notion of these extensions was as an error reporting API intended to quickly detect the most severe types of outages, where in the worst-case 100% of remarketing queries would crash or return errors. 

    	


        Extensive discussions ensued in the thread and in the last post the Google Sandbox team asked the group to provide better guidance on requirements.

*   [Rushil] Want to obtain requirements from all your team and make sure everyone in the room is represented. Use Case: You want to be able to catch events pretty quickly as it happens. Also, on the seller side, you also want to be able to catch these events pretty quickly on the ad logic side. This is intended to catch events that happen in GenerateBid or ScoreAd. Proposing building a new API that has a much tighter SLA. This is for trend detection and anomalies.
    *   [Brian May] There will be an interest in getting stuff quickly but also getting feedback on small amounts of interactions, e.g. when starting something small before ramping up
    *   [Rushil] Looking to get more feedback so it will be helpful to engage further. We are still shaping it.
*   We are doing this as a local DP mechanism as a histogram of noise from generatebid or score ad metrics. Any massive deviation from the histogram trendline will trigger an alert
*   If we do not have a priority then it will be randomized.
*   If you are a buyer, you will have a trendline of errors that happen. But if you introduce a new logic, and there is a spike, your bucket could be to send a histogram of errors. You may also want to know which seller is impacted or the environment, you’ll have the ability to define the bucket so when there is a contribution being made, we know what histogram to add it to.
*   On the seller side, you may want to know a histogram of the scoread worklet set
    *   [David Dabbs] It will be wonderful to see agencies for the publishers who are hosting these auctions at the bottom of the food chain… so they don’t have to depend on their partners
    *   We can engage in the future by understanding the requirements. For now, we are engaging based on scoreads and generatebid
    *   [Kleber] David, can you elaborate?
    *   [David] Configuration and control - it will be great to build an api that can provide information to publishers directly
    *   [Brian] Diff categories of conditions that can be treated uniquely. Bid logic crashing is different from bid logic losing consistently. Have we considered these - crashing vs ratio decline?
    *   [Rushil] Not codifying the error types. It will be adtech defined. Definitely diff error types but this may not be the solution for that
    *   [Alonso] This is feedback about items with urgency. The question is how “urgency” is defined. 
    *   [Brian] There are errors that adtech is responsible for because they have done something wrong vs errors that systemic in nature
    *   [Isaac] Example is publisher may have set a new campaign and misconfigured it the wrong way
    *   [Alonso] Precisely, that is in scope here
    *   [Kleber] The priority algorithm may address that so let’s hold off for now
    *   [Rushil] **Call to action:** Comment on the GH issue as well because these feedbacks are good.
*   [Rushil] Feedback: How quickly would you like to detect these errors? What metrics are important? How would you define the buckets? Also, what generateBid and ScoreAd volumes are you seeing today? This will help us to understand the noise to apply
    *   [Isaac] Metrics will be streamed out of various systems in real time, once a minute or more frequently; there will be alerts on the dashboard and whoever is on pager would get an alert when it crosses over a threshold. The threshold will differ from system to system. There are two kinds of metrics: 1.) Known error condition 2.) Fail safe and we don’t know why; someone would need to look at the error log.
    *   [Rushil] Very validating to hear. Thinking along the same lines
    *   [Isaac] Not just push out metrics for failures but also for successes. You can expect to have a minimum number of successes, so it will be great to have some kind of detection
    *   [Brian] Are you suggesting that we have baseline numbers?
    *   [Isaac] Any observability is good
    *   [Brian] Need some priority system to get information quicker to stop the bleeding before we spend out budget out
    *   [Paul] Reminder that eventlevel win reporting is still a thing - you can get real time reports using it
    *   [Fabian] Can you explain why you think PAgg API is not a good fit?
    *   [Rushil] It is familiar in that we are building histograms. The difference lies in the mechanism - PAgg runs through the agg service and has some delays. Sometimes the delay can be costly because of the SLA impact.
    *   [Fabian] But is it really a delay?
    *   [Rushil] Yes, there is a delay and processing time
    *   [Charlie] There are also batch sizes for the privacy budget service.
    *   [Fabian] The future of PAgg is to split use cases - I think it is mandatory to improve PAgg
    *   [Charlie] Yes, there is a path to make it better for this use case but there would be scaling issues given the real time need
    *   [Isaac] What is the theoretical availability of this in the long run vs short run?
    *   [Rushil] This specific mechanism is to arm you with a differentially private monitoring solution - long term solution and shape might transform over time
    *   [Brian] We need to look at carrier signal, that the system is functioning properly to know that we are not missing errors. We need reassurance that all is going well with the system
    *   [Rushil] Can you not achieve it with baseline trends?
    *   [Brian] I think we can but have not thought deeply
    *   [Paul] I think event level win reporting should be able to do that for you
    *   [Rushil] Will connect with the team offline to think more about it but for now, the EL win reporting is the first thing that comes to mind


## Charlie Harrison: Improved mechanisms for `modelingSignals` and TEE-based model training https://github.com/WICG/turtledove/issues/1017



*   https://github.com/WICG/turtledove/issues/1017

This is a new issue that aims to help improve the support for bid optimization in Protected Audiences without impacting the privacy stance of the API. This use-case typically involves:

*   Predicting an outcome associated with serving a certain ad (e.g. a predicted click through rate or conversion rate)
*   Varying bid price to optimize this outcome
*   [Charlie] - The issue is all about the field we added to protected audiences called modelingsignals()
    *   An egress channel
    *   Limited to the amount of bits you can store to 12 bits
    *   Subject to noise 1% of the time
    *   The reason invented was to aid in training bidding models - hence the name
    *   While generatedate() gets all info, there is not good feedback to teach a model.machine to properly fit
    *   Hearing that modelingsignals() is not the best vehicle for this?
        *   Not enough bits to train in large bidding models
        *   The randomizer does not scale well when channel capacity increases
        *   Not consistent with high-dimensional output
    *   We are in the research phase to figure out a better solution to model with sensitive information
        *   How can we design a better mechanism to allow you to train better models?  That is the motivation
    *   Two solutions we are exploring
        *   Can we design a local mechanism that is better than this mechanism
            *   DO something local with local noise on a fancy model and get output on reportwin() like you do today
        *   More complicated
            *   More an aggregation style mechanism
            *   Rather than a noisy version of model signals, instead we encrypt the model signals and allow it to be processed in some TEE system
            *   Maybe that TEE system has been tuned to solve the private model training problem
            *   Implement to run private model training algorithms that are more efficient than local training algorithms
            *   Needs something like the private aggregation API to gather the data and then run the model and spit out the result
        *   We think that central solution would provide more utility than the local solution, so that is where we are directionally aiming
        *   We wanted feedback on that thinking and pivoting model
    *   [Brian] Can we pursue both paths?
    *   [Charlie]: Worry that if we do both we spread ourselves too thin.  Both are promising and we are thinking about how to sequence them.  We felt in looking at the approaches that a local solution might have advantages for accuracy, but we felt that utility of centralized approach causes us to think to do it first
        *   There is a huge gap between something using a local mechanic vs. a central mechanic
        *   In local, you can train in your own environment and operates outside of sandbox
        *   In central training system, there will be complicated interaction between your model training system and the privacy sandbox
            *   Makes model training a lot more complicated - eg. feature transforms, hyperparameter tuning, model eval, etc.
        *   Our ask is:
            *   is this vision compelling?
            *   What should the high-level requirements/features be
        *   We will look at those features, prioritize and figure how to implement
    *   [Brian]: 
        *   What I expect we want to do is to evaluate what works well locally and what works better centrally.
    *   [Charlie]
        *   Think that is in the realm of possibility
        *   This is analogous to what Rashil was talking about earlier about real-time aggregation/reporting on error messages
    *   [Michael]
        *   Takes a little bit of privacy leakage to have each auction choose which kind of modeling signals to spit out
    *   [Charlie]
        *   I think we can solve that problem
        *   Admittedly, this is my ‘high-level schpiel” and am happy to have offline conversation about it
    *   [David]
        *   Who actually sets those modeling parameters?
    *   [Charlie]
        *   Set in generatebid() by the buyer and then flow out to them
    *   [Michael]: 
        *   If people who are doing modeling training with the 12-bit model - would be great for you to speak up and talk about how it is working for you and issues you are seeing
    *   [Fabian]
        *   We are using extensively - key to our bidding
        *   We are very interested in having this capability added as a feature
        *   I don’t see how local noisy approach can work
        *   You release the modeling signals and then can generate the model from that  which seems better for feature engineering.  You can then use some generic ML algorithms
        *   Today we have two APIs, Private Aggregation API and Attributed reporting API,, the question is how to turn this capability into one unified API and release modeling signals + labels
    *   [Charlie]
        *   You are referring to the issue that we have a separate attribution reporting API and could that be used to model features that come from Protected Audiences API?
    *   [Fabian]
        *   Yes that is the issue
    *   [Charlie];
        *   We not only want to be talking about conversions, but also the conversion label
        *   Could look at also encrypting the label and have them all processed in the same TEE.  
    *   [Brian]
        *   Are you considering the capability of providing more robust signals around conversion?
    *   [Charlie]
        *   TBD.  My initial sense is we should be shooting for a simple MVP first, and then add in those more subtle/complex features
        *   If we could train data from ARA in a TEE we could probably allow you to provide more fine-grained information
    *   [Fabian]
        *   This is different than error reporting because you noise the feature
    *   [Charlie]
        *   This is why we are not enthusiastic about the local noise approach
        *   There is research about local noise in a high-dimensional regime.  There are mechanisms that work a lot better
        *   But there is not a lot of evidence that this works when applied to features directly
        *   A lot of unknowns how to deal with categorical features in that sense
        *   A lot of open thorny questions about how to do this properly
        *   Happy to share ideas we have seen..
        *   Don’t think they by themselves are silver bullets
        *   Most used in federated learning - where you add local noise directly to that gradient, which is not this exact use case
    *   [Brian]
        *   Have you thought about the possibility for allowing for multiple different strategies for local noise?
    *   [Charlie]
        *   Yes, we want the solution to be extensible
        *   Choosing between a 12-bit approach vs an encrypted approach, we want to be able to add more “cool stuff” as the familiarity with use increases
    *   [Jonasz]
        *   I can see training in TEE work conceptually.
        *   At the end of the day the devil is in the details: how easy it is to create models, train them, debug, troubleshoot.
        *   Have you any prior art with TFX or any practical experiment you can take a look at?
    *   [Charlie]
        *   Not yet, still early
        *   Hope with time to share more about ongoing research that is happening.  
        *   We are trying to put together what a design/framework might look like
    *   [Jonasz]
        *   It's important to also keep the current aggregate reporting mechanisms in mind, as one approach to ML optimization.
        *   We are using them right now, and we're planning to continue using them long term for ML optimization.
        *   The federated learning paper from 2017 shows the basic approach.
    *   [Charlie]
        *   Actually creating a gradient in reportwin() and then reporting on that
    *   [Brian]
        *   Will it go under the ARA reporting API group or a new group?
    *   [Charlie]
        *   Plan to do it within existing groups
