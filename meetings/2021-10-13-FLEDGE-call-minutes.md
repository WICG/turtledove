# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC.

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Oct 13 2021


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Caleb Raitto (Google Chrome)
3. Paul Jensen (Google Chrome)
4. Chris Evans (NextRoll)
5. Angelina Eng (IAB & IAB Tech Lab)
6. Bartek Łoś (RTB House)
7. Fred Bastello (Index Exchange)
8. Jeff Kaufman (Google Ads)
9. Brendan Riordan-Butterworth (eyeo GmbH)
10. Andrew Pascoe (NextRoll)
11. Stan Belov (Google Ads)
12. Erik Anderson (Microsoft)
13. Russell Stringham (Adobe)
14. Russ Hamilton (Google Chrome)
15. Jonasz Pamuła (RTB House)
16. Brad Rodriguez (Magnite)
17. Wendell Baker (Yahoo)
18. Matt Wilson (NextRoll)
19. Valentino Volonghi (NextRoll)
20. Bartosz Marcinkowski (RTB House)
21. Lukasz Wlodarczyk (RTB House)
22. Irlinta Mavromati Terzopoulou (Google Chrome)
23. Viraj Awati (Amazon)
24. Alex Cone (IAB Tech Lab)
25. Ryan Arnold (Procter & Gamble)
26. Shivani Sharma (Google Chrome)
27. Tamara Yaeger (IPONWEB)
28. Paul Farrow (Xandr)
29. Anthony Molinaro (OpenX)
30. Daniel Rojas (Google Chrome)
31. Sergey Tumakha (Microsoft)
32. Sven May (Google Chrome)
33. Martin Gruau (Captify)
34. Helene Maestripieri (Google Chrome)
35. Matt Menke (Google Chrome)
36. Bill Landers (Xandr)
37. Sam White (LiveRamp)
38. Jeff Wieland(Magnite)
39. Don Marti (CafeMedia)
40. Aditya Desai (Google)
41. Christa Dickson (Meredith)
42. David Tam (Relay42)
43. Julien Delhommeau (Xandr)
44. Garima Dhingra (Google Ads)
45. 


## Note taker: Angelina Eng


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

 		


## [Suggest agenda items here]



*   For 13.10.2021 call: https://github.com/WICG/turtledove/issues/215  (Bartek Łoś)
*   For 13.10.2021 call:  https://docs.google.com/document/d/1rB0ixgJyXA5q3DkShncAhcy42rGD8NrJjHXg5EGVHhU/edit Issues/Questions can be raised on Fenced Frame’s github repository [here](https://github.com/shivanigithub/fenced-frame). (Shivani)


## Notes



*   https://github.com/WICG/turtledove/issues/215  - Bartek Łoś (BŁ)
    *   BŁ: Follow up about bidding worklet performance limitations.
    *   BŁ: Benchmarks with webassembly show that performance could be improved 10x.
    *   BŁ: Other benefits: reducing time of script initialization, reducing time of model weights parsing, obfuscation, easier migration of the current code, SIMD operations availability.
    *   BŁ: In the mentioned benchmark, we assumed that wasm binary would be hardcoded in buyer's js, compiled and instantiated it generateBid. It could be improved even more but with some other API extensions. eg. resources / wasm binary preloading, compiled wasm modules caching.
    *   Paul Jensen (PJ) - We've tried turning on jit, made substantial improvement but it didn’t get all the way to Node.js.
    *   PJ: When jit was off, it was for a different model. Some of the security folks recommended to switch sandboxes to no-executable-pages
    *   PJ: Fair amount of time parsing javascript. They are thinking of ways to improve it.
    *   PJ: Chrome does code caching already for normal renderer JS, not worklet JS. Improve parsing if they can keep in a code cache.
    *   PJ: Precompiled binaries - a way to improve the parsing.
    *   PJ: Does benchmark use SIMD operations? There may be more privacy considerations when using SIMD.  
    *   BŁ: No, it doesn’t use any SIMD operations. For benchmark purposes, we rewrote the code we had in the first benchmark from JS to C++, compiled it and hardcoded it in JS (as a wasm binary) - and noticed improvement just from it.
    *   BŁ: To sum up, my question could be divided into 2 parts:
        *   Is it an option to provide a bidding worklet implementation with support for web assembly? (we can observe some boost from it, without preloading WASM binaries and without caching compiled WASM modules).
        *   If so, is it an option to provide some API extensions to achieve these improvements (resource preloading, compiled code caching) and some additional performance boost?
    *   BŁ: Side note: there is no option to turn on web assembly without turning on jit (but in our benchmark we do not take advantage of it - because there is no warm-up or snapshots, we just run generateBid in similar way as it could be done in a bidding worklet)
    *   Stan Belov: Custom snapshots allow recreate V8 context, can help with reinitializing state 
    *   Is there a way to initialize the bidding function before the auction starts? Any caching can increase memory. If an auction uses a similar interest group script for many bids to an auction, there is potential for reuse.  
    *   If you start affecting future runs with previous runs, you risk passing of information. Auction to auction can be tricky.
    *   Kleber - Possibly initialize the worklets idea - that could be considered? - preinitialize at auctions - opportunity for code on page that on device at auction is going to run in future, and do some “set up work”, but risk is that the contextual auction and not to run on device auction could be thrown away.  So might end up doing the work for nothing.  Hinting to browser that an on device auction does seem like a valuable thing, even if it is to help Chrome in setting up.  Will consider
    *   Valentino - Number of auctions that are going to run - compounded by buyers that want to participate in on device auctions along with the contextual data, they are going to need to react server side, so that the volume of contextual is fed to the on device bidding.
        *   There is no way to reduce the worklets or bandwidth usage to deal with all the contextual requests.  Becomes a bigger response with the contextual signals. Need to consider the length of time to execute as well.
    *   Kleber - contextual signals - are they often reused in auctions? Or freshly generated for each bid on page? Is there possibility for SSP to cache layer? Vary by url or shared by different pages for a site? Signals being used with the first party cookie, then they would be harder to reuse.  But at site level would be easier.
    *   Valentino - would be at the site level, at page level.  Minimum would be at the page level url.  The more detail then the more likely the frequency.  But it could include other campaign specific related information that changes.  Could use ML to as example,  sync to tune in to budget left over.
    *   Andrew Pascoe - they do stuff more granular.  Maybe some caching.  Granularity is usually at the ad slot detailed level. eg Some slots ATF, BTF.  Updated machine learning parameters, there hasn’t been a complete audit with the contextual signals and user information split, especially down to the interest level. Things that can invalid caching is time of day.  They do update their model frequently.  Could deemphasize certain ad slots, and update the model. But when caching up to the minute is not an issue.
    *   Kleber - Things that change at url and ad slot level? And if ad slots are different across the pages? 
    *   Andrew - they look at ad slot at each page.  They take the combination of both and a feature in the model.
    *   Valentino - the flow of information is incentive to do more auctions, and uses bandwidth of information, and becomes a change of events.  Possibility it to separate contextual fetching.
    *   Andrew - they expect SSP and publishers send more detailed info in the publisher signal object, that would change their response, and change by users.  Regarding cross features, they consider all of it with factoring machine, and average it out with some sort of vector.  Whatever signals gets crossed with other signals with their models - and is dynamic when things alter.
    *   Kleber - might need to have simpler models for on device bidding vs. server side. The reference to trusted servers might be a very good thing to consider. The fact that things are served thru TS are immune to the problem Valentino points out, that need to respond to each auction, even the ones you are not involved in. Very inline with expectations of trusted key value server and privacy models. Need to spend more time considering
    *   Andrew - simpler models - if they receive contextual request and do server side, they would be as detailed as possible. Contextual and caching might be really tricky.
*   Fenced frames from a developers perspective - Shivani Sharma
    *   She is trying to highlight web visible changes that are happening for FF.
    *   https://docs.google.com/document/d/1rB0ixgJyXA5q3DkShncAhcy42rGD8NrJjHXg5EGVHhU/edit
    *   From developers perspective from publisher or ad itself, they will need to be kept in mind:
        *   Opt in header
        *   CSP, fetch metadata fetched
    *   Some web visible difference with fenced frames document with link to design docs.
    *   Please provide feedback and comments while they are designing things.
    *   Issues/Questions can be raised on Fenced Frame’s github repository [here](https://github.com/shivanigithub/fenced-frame).
    *   Most of these will apply to Origin Trial when Fledge is integrated with FF.
*   TPAC meeting happening online.
    *   https://www.w3.org/2021/10/TPAC/
    *   Next FLEDGE meeting will be on Nov 10, the one that would beOct 27 cancelled in deference to TPAC.
    *   In particular, check out the breakout session list for next week linked to from https://www.w3.org/2021/10/TPAC/format.html#oct18-22. 


# No meeting Oct 27!  See you Nov 10
