# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday May 1, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	 



1. Michael Kleber (Google Privacy Sandbox)
2. Brian May (Dstrillery)
3. Matthew Atkinson (Samsung Electronics)
4. Arthur Coleman (IDPrivacy/ThinkMedium)
5. Paul Jensen (Google Privacy Sandbox)
6. David Dabbs (Epsilon)
7. Roni Gordon (Index Exchange)
8. Manny Isu (Google Privacy Sandbox)
9. Ricardo Bentin (Media.net)
10. Harshad Mane (PubMatic)
11. Laurentiu Badea (OpenX)
12. Brian Schmidt (OpenX)
13. Jacob Goldman (Google Ad Manager)
14. Shafir Uddin (Raptive)
15. Paul Spadaccini (Flashtalking)
16. JASON LYDON (Flashtalking)
17. Tamara Yaeger (BidSwitch)
18. Warren Fernandes (Media.net)
19. Sarah Harris (Flashtalking) 
20. Youssef Bourouphael (Google Privacy Sandbox)
21. Aymeric Le Corre (Lucead)
22. Michael Ivancic (Direct Digital Holdings / Colossus SSP)
23. Wendell Baker (Yahoo)
24. Alonso Velasquez (Google Privacy Sandbox) 
25. Orr Bernstein (Google Privacy Sandbox)
26. Benny Lin (Bombora)
27. Bram Woolcott (TripleLift)
28. Victor Pena (Google Privacy Sandbox)
29. Antoine Niek (Optable)
30. Elmostapha BEL JEBBAR (Lucead)
31. Kenneth Kharma (OpenX)
32. Sid Sahoo (Google Chrome)
33. Sathish Manickam (Google Privacy Sandbox)
34. David Tam (Relay42)
35. Abishai Gray (Google Privacy Sandbox)
36. Andrew Pascoe (NextRoll)
37. Pawel Ruchaj (Audigent)
38. Peiwen Hu (Google Privacy Sandbox)
39. Maybelline Boon (Google Chrome)


## Note taker: Tamara Yaeger


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here:



*   Guillaume Polaert (can be done async on github)
    *   [Addition of an analytics/reporting entity to enable centralised reporting.](https://github.com/WICG/turtledove/issues/1115)Things we could explore:
        *   Pinning down the exact issue.
        *   Where does Privacy Sandbox stand on the PAAPI vision and its constraints?
        *   Brainstorm a couple of potential solutions.
        *   Implementation proposal: https://docs.google.com/document/d/1dmtOXo1WAWmPXOoi0B1CR8RfqcAek0nU3kgcFyyDT8Y/edit
*   Isaac Foster:
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Alonso Velasquez (Google Chrome): Given the request from the SSP community to [enable in-bid Creative Scanning within Protected Audience](https://github.com/WICG/turtledove/issues/792), would like to hear from buyside representatives some illustrative examples that show from your perspective what are the challenges with always pre-registering creatives with SSPs,to round out our research. 


# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.


# Notes

Michael  Kleber (Chrome): This weekly meeting hosted by Chrome team about PAAPI now has a couple of different every-second-week companion meetings that you might also be interested in. (1) Tomorrow MSFT Edge team holding fortnightly meeting about ad selection API work; deets are linked in announcements section ^^ (2) Next week, implementation of Protected Servers for use of PSB APIs.  \


On the agenda today we have Github issue from Guillaume. Should we wait for Guillaume?


## Addition of an analytics/reporting entity to enable centralised reporting #1115 https://github.com/WICG/turtledove/issues/1115 

Warren Fernandes (Media Net): He’s ok w us heading it up. onboarding for Media dot net for SSPs from sales side, a fundamental issues is that current spec is designed w buyer and seller, but there are other entities in the ecosystem. This proposal looks at concept of pub being able to independently verify analytics provided by their sales side partners. Proposal draws comparisons to prebid analytics being the analytics adapter, and how enabling that enables pub to use independent entity, not related to seller, to verity parameters of auction. Proposal auction also highlights some important events that analytics provides for pubs that effectively consume and log data of ongoing auctions, and proposes an alternate path to capture data and make available to pubs. New elements are concept of adding analytics entity (not sell or buy side), suggestions around permissioning. Going by the comments on the issue, it’s important that pub has way of determining which analytics entity are authorized, and both buyers and seller need to be informed of presence of analytics entity. Sample structure suggests how entity may be able to consume data and report it back. Two new functions for analytics (1) real-time reporting (2) aggregate reporting, with the idea that we may want to do segregation on what is available at the entity level. See attached Google doc for more details.

Michael: I have questions – you mentioned some amount of getting similar functionality as what "Prebid Analytics" offers today. That is a case where in status quo, some amount of ad decision making is inside the browser and is made visible by prebid. That’s in stark contrast to ad selection decision making that happens on SSP servers and server-to-server RTB comms. Is this design something that has any analog to think about in the RTB S2S context? Or new and only existing on browser part of ad selection? \
 \
Warren: This ties more closely to what’s available on client side. Don’t think there is a significant analog to server side. Even if auctions happen on server side they get to client side at some point. It all boils down to client being able to do logging.

Harshad Mane (Pubmatic): Even in case of Prebid Server, analytics is available

Michael: So because Prebid Server as a piece of infrastructure implements monitoring API, this is available sometimes. Second big question: as you mentioned, you proposed something that has two diff pieces; one piece is real time reporting, other is aggregate reporting. I would prefer to think of those two pieces individually and rename; (1) aggregate, (2) event-level reporting instead of real-time. The diff is in specificity in data, because from the PS pov, the more user-specific the data is the more it incurs risk of cross-site tracking.

Warren: Timeliness is important, there are 2 things that pubs can do w analytics; (1) reporting to cross-verify, (2) checking if config is correct, needing some ability to detect if some seller is being left out.

Michael: Why is real time use case a use case for a 2nd independent entity, rather than a use case where the pub already has someone whose job is real time reporting? Having 2 entities that are doing real time reporting to double-check may have some added value, but seems like a different level of importance compared to the non-real time version. Doesn’t have that tight latency constraint.

Warren: Yes 2 diff use cases, may be best to separate them.

Brain May (Distillery): We could have streaming aggregates that give immediate feedback

Michael: Figuring out how to aggregate + preserve privacy is what we’ve been developing in PSB; sometimes possible to get aggregation and low latency at the same time, but usually a tradeoff w something else, what properties are most important.

Brian: We should look at data and treat different classes, there are pieces of info browser can emit about info between buyer / seller that we can provide relatively quickly. Other data has user relationships. It shouldn’t be lumped in one set of data, we should check for potential to reveal info about users.

Michael: From a practical POV, people who do not work on privacy may be too willing to act as if some pieces of info doesn’t have privacy impact, but it does and makes cross-site tracking easy.

Brian: Can we define a set of data points that are privacy-respecting that are standard data provided to analytics providers?

Warren: Doc has specific suggestions; that we have association of an auction and whether we have some impression delivered at event-level is already available to sellers. 

Michael: Just to set the framing, I would prefer not to anchor on what is available to DSPs / SSPs in event level reporting today as the right place to start. From the PSB pov, the event level reporting that is going to buyers / sellers today is a necessary crutch at this time, but is not sufficiently private for the long-term. We’ll have to continue working on PAAPI to make things more private as time goes by. We would prefer to design something that meets the long-term privacy bar, instead of having to replace it later. For 2 diff use cases, event-level realtime versus aggregate, I would be happy to land an aggregate-based solution because that is the direction we’d like to go in to serve all use cases possible to serve that way. If there are additional use cases not met by an aggregate solution we can look into that, but our best effort now is to figure out how far we can push aggregation as a tool.

David Tam (Relay42): What buy / sell side signals would still be available in Private Attribute API? We need enough info to be able to support multi-touch attribution from advertiser perspective. 

Michael: The aggregation paradigm at a high level – in a context where aggregation is the way reporting happens, we can make more info available to the processing side as long as the processing is followed by aggregation. We usually need to restrict access to keep report from going to untrusted server. Think of this as a multi-stage process: Get access to detailed data in a private environment for processing, and then have aggregation be how it comes out of the private environment. For multi-touch attribution, it is well served by a model that records a variety of diff events including detailed info in a private store of info that does not get sent to a server. Then we can run business logic to process those individual events and assign a level of credit using any algorithm, then send aggregated reports and how much credit should go to multi diff parties. So individual highly detailed reports do not get sent off. We should look at this here and see if we can use it to address Warren’s use case. In PSB the way to use this approach, available in the browser today, is a combination of two APIs: (1) Shared Storage and (2) Private Aggregation API sitting atop Shared Storage. Basic flow is when things happen in browser that are of interest, you don’t send info to server, you write info to on-device storage owned by one particular domain, then some time after Shared Storage there is a processing stage where script can be run to read all Shared Storage (on device analysis, comparison, etc), and create an encrypted report that gets sent to server and feeds into Aggregation Service.

Brian: My understanding is you can provide rich set of info on large # of events and higher bandwidth of info w/out ability to tie to specific event. You can do the inverse; less info on more discreet events. We tend to look at end-state big model that will provide info and satisfy use cases. We should consider a model in which a report that is information rich is provided encrypted to an intermediary server which decides how much info it can emitted to a final party. Small amounts of information is emitted at event-level and the more information that is being emitted, the more aggregated the outputs would need to be. Generate everything at event-level and emit at appropriate level.

Michael: There’s a big gap between Shared Storage server and what Brian described, which I think might be a multi-year research effort with the privacy community to figure out other metrics instead of differential privacy. I’m hesitant to say we should kick off a new research effort to meet a use case unless we know we can’t meet it with what the research community has already spent time on. It sounds very hard, and maybe similar to what’s been proposed that has turned out to not be private, so I definitely don't want to count on any solution like that.

Ricardo Bentin (Media Net): Prebid JS can be put in debug mode, that would write events that are happening to Prebid JS to console. Can that be used for browser?

Michael: There is already an active discussion about how an extension in the browser can get a stream of info on different auction events. That sounds more similar to what you’re talking about. Useful for a dev to deeply understand what is happening on their own machine. We can permit the user to do that because there isn’t a deep privacy risk. What Warren is asking about is different, it's intended to let pub / ad tech to get a broad view over lots of ppl, not only those waiving privacy protection. We're always interested in providing new / better APIs for the extension use case also.  See ongoing work in https://github.com/WICG/turtledove/issues/937.

Warren: Regarding sunsetting realtime event level reporting, would it be possible to moving \_\_\_\_\_\_\_…….

Michael: There are fundamental things that have to keep working for advertising; there are some things happening at event level now that don’t need to happen on that level. If we can figure out ML training model in trusted environ, before we get rid of event-level we will need to support a lot of use cases. Event-level reports are enough info for a pub to learn every protected audiences ad. We’d like a world where ads APIs are private enough, that the pub doesn’t build a profile based on ads we are showed. Aggregate reporting is great because of privacy, my goal is to figure out what we can support w it. 

Brian: What you described is how we started w Turtledove, taking auctions from server and move to browser for privacy. What we evolved to is that we want a server bidding / auction infra that would relieve pressure from client. I’m suggested is equivalent is bidder model fo that, only allow data to be emitted from protected environment where it seemed privacy-preserving. So I give you all records from my campaign, once it reaches threshold, an aggregate report is released.

Michael: The paradigm I described is processing on events to create reports to send to aggregation service. Part of that is trusted execution environment. The other part, taking detailed data and doing processing, that’s a thing similar to on-devise vs auction bidding question depending on how much processing you want ot do. It could be that using trusted execution rather than processing on device has some value. In the ML stage, it could be too big to run in browser, so how do we bring in server to trusted execution environment? It’s about where computation happens. You’re right Brian that PSB has pushed things from on device to server side. This processing in Shared Storage much be another one where we have use case. 

Brian: We talked about aggregation as batching; has the PSB team considered developing model like that but real time? Constant flow of aggregate values instead of in batches.

Michael: Interesting area to explore. Not sure on status for thinking about, has come up in several contexts. Generally there are some tricky privacy tradeoffs, the more you’re getting info from server the more opp for leakage. Batching has some useful privacy properties. Maybe shorter batches could work. Tradeoff is that shorter a window, less data, the noisier aggregates need to be to preserve privacy. It depends on understanding use cases plus quantity of data. If you’re trying to get smaller amt of data, it’s possible to do that and get less noise. It’s when you want more high fidelity that you need more noise. 

Warren: How do we follow up?

Michael: The Goog doc helped w understanding. Github is a fine place for discussion, will tried to add something from the docs. 

David T: Is the trusted execution environment representative of private aggregate API?

Michael: Trusted exec environments is how private aggregation API receives reports, processes, adds noise, and sends back out. Report is generated in browser w signal, then those reports sent ot ad tech that created them, ad tech collects encrypted reports, they go to aggregation w reports to aggregate and get back a histogram w appropriate noises added. Aggregation service runs inside trusted execution environment.

David T: At the moment running in AWS and GCP?

Michael: Yes.

Michael: Next agenda item: Alonso would you like to bring up your issue?


## Alonso Velasquez (Google Chrome) on https://github.com/WICG/turtledove/issues/792 

Alonso (Google Chrome): We have a way of creative scanning in bidstream. Asking the DSP community, from your perspective, why is it so important? We heard from SSP pre-registration for creative is available, but sometimes not the DSP can afford to do. What makes pre-registration that a DSP may not be the only way to register, what would you prefer? What challenges do you see for pre-registration?

David D (Epsilon): Creative pre-registration is in the dustbin, very few SSPs require. We’re happy to not do it and generally we don’t. I think there is only one SSP that has it but they want to get rid of it.

Brian: From my DSP days, this was overhead that didn’t have value. When there were opps to get rid of it, we did. 

David D: We don’t have to presubmit, but there are still overheads for creative approval. SSPs that don’t allow creative to win w/out review, their APIs read out from the process so we know. Doesn’t require pre-submission.

Alonso: Pre registration had this overhead, required DSP to do integration plus operation flow. It’s in the bidstream, I’m internalizing that it’s an opportunity to get better / more automated. Pre-registration is going back.

David D: For everyone it could be a significant negative externality.
