# Protected Audience (formerly FLEDGE) WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Sept 20, 2023


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Roni Gordon (Index Exchange)
3. Luckey Harpley (remerge.io)
4. Matt Menke (Google Chrome)
5. Paul Jensen (Google Chrome)
6. Xavier Capaldi (Optable)
7. Youssef Bourouphael (Google Chrome)
8. Matt Wilson (NextRoll)
9. David Dabbs (Epsilon)
10. Russ Hamilton (Google Chrome)
11. Przemyslaw Iwanczak (RTB House)
12. Itay Sharfi (Google Privacy Sandbox)
13. Laurentiu Badea (OpenX)
14. Akshay Pundle (Google Privacy Sandbox)
15. Joel Meyer (OpenX)
16. Priyanka Chatterjee (Google Privacy Sandbox)
17. Xing Gao (Google Privacy Sandbox)
18. Orr Bernstein (Google Chrome)
19. Marco Lugo (NextRoll)
20. Brian Schmidt (OpenX)
21. Jeroune Rhodes (Privacy Sandbox) 
22. Mihir Gandhi (Google Privacy Sandbox)
23. Roopal Nahar (Google Privacy Sandbox)
24. Fabian Höring (Criteo)
25. Andrew Pascoe (NextRoll)
26. Isaac Foster (MSFT Ads)
27. Michael Mihn-Jong Lee (Google Privacy Sandbox)


## Note taker: Paul Jensen


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


## [Suggest agenda items here — no agenda no meeting!]

_Sorry, old items were cleared out when we used this doc for the [in-person meeting at TPAC](https://github.com/WICG/turtledove/blob/main/meetings/2023-09-11-Protected-Audience-TPAC-notes.md).  Please re-add if there was something held over that you still want to discuss_



*   [Itay Sharf] Bidding and Auction Monitoring and debugging Plans 
*   Isaac Foster: creative URL pre-registration (https://github.com/WICG/turtledove/issues/729, Isaac (and Maybe Sergey) there at 11:30)
    *   Roni Gordon: https://github.com/WICG/turtledove/issues/792 is related
*   Roni Gordon
    *   same origin constraints - https://github.com/WICG/turtledove/issues/813
    *   K/V lookup timeouts - https://github.com/WICG/turtledove/issues/814
    *   Macro substitutions - https://github.com/WICG/turtledove/issues/817
*   David Dabbs
    *   Potential for advertising application of the promising Fenced Frames output gate proposal Shivani Sharma presented at TPAC. https://github.com/WICG/fenced-frame/issues/15#issuecomment-1721834156
*   [Jeroune Rhodes]- Protected Audience Webinar
    *   The Google Privacy Sandbox team will be hosting our next set of webinars on Protected Audience. The first session will go over Protected Audience technical fundamentals where we will cover key concepts such as an ad interest group, setting up an auction config, initiating an auction, generating a bid, and scoring an ad.  We will also walk through the code and the demo.
    *   The first Americas friendly session is happening on Sept. 26th 10-11 am ET. A second EMEA friendly session is happening Sept. 27th 7-8 am ET. To join, please register below: 
    *   AMER: https://rsvp.withgoogle.com/events/protected-audience-fundamentals-amer  
    *   EMEA: https://rsvp.withgoogle.com/events/protected-audience-fundamentals-emea  


# Notes


## Itay B&A Monitoring and debugging plans

**Slides that Itay presented are at https://github.com/WICG/turtledove/blob/main/meetings/2023-09-20-FLEDGE-call-slides.pdf**

Itay: I’m a PM from Privacy Sandbox focusing on trusted servers.  Akshay is joining me.  We’ll be talking about monitoring and debugging.  TEE has different level of access controls.  We’ll talk about how to debug and monitor given those access controls.

Summary:

<span style="text-decoration:underline;">Recapping B&A</span>. Enhancement to Protected Audiences allowing running auction in the cloud.  2 services for SSP and 2 for DSP, both run in TEEs. AWS and GCP in alpha phase.  Server-side supports all features of on-device.  All bidding scoring and reporting logic can run on high speed servers.  TEE B&A servers can run with BYOS key-value server.  Allows buyers and sellers to scale.

Optimized for performance.  Auction config controls whether it’s on device or on server.

Bidding and scoring logic hidden.

<span style="text-decoration:underline;">B&A privacy and security recap: </span>Open source code.  Deployed into TEEs in public cloud.  Coordinators manage crypto keys.  Adtechs do not have direct access to keys.  Browser encrypts.  B&A services get private keys to decrypt the data.

<span style="text-decoration:underline;">Roadmap:</span> How do debugging and monitoring happen if these machines are in TEEs and data is only decrypted inside TEEs?

* _Jul '23 alpha:_ No prod traffic, Debug and local mode
* _Nov '23 beta:_ <1% prod traffic, DP metrics + Developer consented debugging
* _Jun '24 scaled testing:_ Prod ready, DP noise cannot be turned off

Akshay:

#### Monitoring overview

Building system to collect system and business metrics.  Classified as privacy safe and not.  The unsafe ones will be DP noised.  Separate from aggregation service.

Aggregated on-server.  DP noise added, and then exits TEE.

Uses OpenTelemetry.  Can forward to dashboards and alerting systems.

Servers pre-define metrics.  Classified as privacy impacting or not.

Privacy non-impacting still aggregated.

Privacy impacting are aggregated and noised and then forwarded to monitoring systems.

Once in cloud monitoring, it’s DP private.  Can be analyzed at that point.

#### Privacy Safe Telemetry

DP noise added to privacy impacting metrics.

Overall privacy budget defined for collecting all privacy impacting metrics.

Budget affects noise.

Controls: adtechs can pick subsets of instrumented metrics to collect per service.  How many metrics affects how much noise added per metrics.  More metrics = budget spread thinner = more noise per metric.

Metrics aggregated in TEE first.  Period of aggregation is configurable.  Data exported at end of period.  Longer periods mean noise is proportionately less.

#### Debugging

Debugging is different in TEE due to access controls.

Adtech consented debugging - adtechs gain access to specifically specified requests.  Can see extra data on their servers.  When consented, it’s unencrypted.

Local, debug mode  - Can be used to replay consented debugging, including debugging.

Aggregate error reporting - 

#### Consented debugging

Let’s adtech debug their servers.  The server is flowing through a bunch of servers.

Developer sets Chrome to consent debugging of their B&A requests.  Developer inputs debug token into Chrome.  Token controls which servers will be able to debug; i.e. not other adtech servers.  Only in servers with the matching token is it debuggable.

B&A request is sent like normal but has the debug token encoded in it.  Publishes unnoised metrics and unencrypted messages and logs, when request is a consented request.  These reports are put into cloud monitoring systems.

#### How consented debugging works

Chrome has UI to enable consented debugging, and to input the debug token.

Chrome encrypts B&A request like normal.  B&A request goes through the prod pathways as other B&A requests.  Other adtech’s B&A servers will pass the request through like normal.

The B&A server with a matching debug token will log the request.  Request can be taken from log and replayed.

#### Local debugging

B&A servers can be run locally.

#### Debug mode

Tee servers can be run in debug mode.  Logs can be observed in those systems.

#### Aggregate reporting

Error counts collected and aggregated.  You can monitor error counts in an aggregated and noised way.  Can be used to monitor status/health of server.  Documented in explainers, looking for feedback, esp on use cases and whether they’re covered or not by current design, and what other monitoring integrations you use.

Itay: We are aware it’s complicated and different in TEEs.  Top priority is making sure it covers use cases.

Roni Gordon: Whether or not we’ll be able to see in debug mode the stuff we can see now like stack traces?

Akshay: In debugging cases, you get extra info from prod system and the unencrypted request to replay.  Request can be replayed on local instance with lots of debugging available, e.g. stack traces and logs, unnoised metrics, and unencrypted request and response, and standard debugging tools.

Roni: Will we see everything from interface?

Akshay: You see unencrypted response but not other pieces.

Roni: That’s half the problem.  Seller mostly forwards requests.  Seller needs to see responses to debug scoring.

Akshay: You can look at error rates per buyer.

Roni: Today we can debug the whole flow.  Not sure how to debug end to end with B&A.  How to coordinate with buyers and sellers.

Michael: Trying to understand interactions between production and debug servers.  From DSP POV, if you send consented debug token request, it goes through prod system, gets to DSP TEE now knows the request unencrypted that came through SSP.  DSP can then replay locally to debug further.  What happens when you’re an SSP?  If SSP sends consented debug request, request gets to SSP’s TEE server.  Outside TEE, can SSP server replay a request, including calling DSPs’ TEE servers, and then see the responses?

Akshay: No.  Debug mode servers aren’t in prod system.

Arun: If you do consented debugging, SSP gets unencrypted request.  SSP and DSP can coordinate to run both servers in debug mode.  Requires coordination.

Mihir: SSP and DSP can share consented debug tokens, so both can see the logs including unencrypted request and response.

Roni: This is incomplete compared to today where request and response are visible.  This works well for DSPs for less so for SSPs.

Michael: Debug at scale?  This is about debugging traffic coming from test version of Chrome with consented debug token.

Roni: At scale was a misnomer.  At scale this cannot be done with a single developer.

Akshay: SSP would log things with consented debugging; would like to know what things would be helpful to debug this.

Roni: I have not set this up.  It could get us to 90%, but don’t know until try to set it up.

Arun: Coordination part is a speedbump.   We need to make sure the consent makes sense and is meaningful.  For sellside, when debugging, can we look at all inputs including response?

Akshay: Would like to know what is missing from logs.

Arun: Roni, is that sufficient, to see all the sell-side things logged?  Can log response?

Roni: can we log DSP response?

Aksay: SSP can see inputs to scoreAd.

Roni: that matches on-device.

Arun: Sounds like it is sufficient.

Roni: Would like to try this out.  We need to see what we can on-device when in TEE for B&A.


## [Jeroune Rhodes]- Protected Audience Webinar

David Dabbs: Highlight PSA.

Jeroune: Ecosystem program manager for Privacy Sandbox.  Webinars:

The Google Privacy Sandbox team will be hosting our next set of webinars on Protected Audience. The first session will go over Protected Audience technical fundamentals where we will cover key concepts such as an ad interest group, setting up an auction config, initiating an auction, generating a bid, and scoring an ad.  We will also walk through the code and the demo.

The first Americas friendly session is happening on Sept. 26th 10-11 am ET. A second EMEA friendly session is happening Sept. 27th 7-8 am ET. To join, please register below: 

AMER: https://rsvp.withgoogle.com/events/protected-audience-fundamentals-amer

EMEA: https://rsvp.withgoogle.com/events/protected-audience-fundamentals-emea 
