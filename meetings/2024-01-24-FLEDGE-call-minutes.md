# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 4pm UTC (during winter)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Jan 24, 2024 


## Note: 1/24 call will be only half an hour

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Harshad Mane (PubMatic)
3. Shankar Venkataraman (Jivox)
4. Roni Gordon (Index Exchange)
5. Brian May (dstillery)
6. Sven May (Google Privacy Sandbox)
7. Paul Jensen (Google Privacy Sandbox)
8. Alex Peckham (Flashtalking)
9. Leeron Israel (Google Privacy Sandbox)
10. Jacob Goldman (Google Ad Manager)
11. Zach Edwards (Victory Medium)
12. Laurentiu Badea (OpenX)
13. Brian Schmidt (OpenX)
14. Youssef Bourouphael (Chrome)
15. Amit Gupta (Jivox)
16. Taranjit Singh (Jivox)
17. David Dabbs (Epsilon)
18. Alex Cone (Google Privacy Sandbox)
19. Tamara Yaeger (BidSwitch)
20. Becky Hatley (Flashtalking)
21. Siddharth Muthukumaran (Flashtalking)
22. Wendell Baker (Yahoo)
23. Anthony Yam (Flashtalking)
24. David Tam (Relay42)
25. Caleb Raitto (Google Chrome)
26. Xavier Capaldi (Optable)
27. Manny Isu (Google Privacy Sandbox)
28. Orr Bernstein (Google Privacy Sandbox)
29. Alexandre Nderagakura (Not affiliated)
30. Abishai Gray (Google Privacy Sandbox)
31. Gowtham Komminneni (LiveRamp)
32. Sathish Manickam (Google Chrome)
33. Jeroune Rhodes (Google Privacy Sandbox) 
34. Kenneth Kharma (OpenX)
35. Vedant Seta (Media.net)
36. Chris Nachmias (Flashtalking)
37. Fabian Höring (Criteo)
38. Matt Davies (Criteo | Bidswitch)
39. Alonso Velasquez (Google Privacy Sandbox)
40. Arthur Coleman (OnlineMatters)
41. Laszlo Szoboszlai (Audigent)
42. Pawel Ruchaj (Audigent)
43. Russ Hamilton (Google Chrome)
44. Owen Ridolfi (Flashtalking)
45. Jason Lydon (Flashtalking)
46. Andrew Pascoe (NextRoll)
47. Ricardo Bentin (Media.net)
48. Warren Fernandes (Media.net)


## Note taker: Manny Isu


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


## Suggest agenda items here:



*   Orr's Creative Scanning proposal: https://github.com/WICG/turtledove/issues/792 
*   Isaac:
    *   [Revisit Persistent Opt Outs question w/r/t PST](https://github.com/WICG/turtledove/issues/915#issuecomment-1892962819) (see PST specific question [here](https://github.com/WICG/trust-token-api/issues/288))
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Shankar Venkataraman
    *   Continue discussion on https://github.com/WICG/turtledove/issues/924
    *   https://github.com/GoogleChromeLabs/privacy-sandbox-dev-support/issues/201
*   David Dabbs
    *   https://github.com/WICG/turtledove/issues/1002 \
Isaac’s inquiry about `lifetimeMs` handling in updateURL processing \

*   [Announcement] Chrome Facilitated Testing Webinar
    *   The Google Chrome team will be hosting our next external webinar sessions where we will focus on the testing and preparing for the current 1% restriction of third-party cookies in Chrome along with use of Mode A and B available as part of Chrome-facilitated testing.

        The first **Americas friendly session** is happening on**  Feb. 1st 12-1 pm ET**. A second **Japanese language session** is happening **Feb. 6th 3:30-5:30 pm JST**. A third **EMEA friendly session** takes place on** Feb. 8th 12-1 pm GMT**. 

    *   To join, please register below: 
        *   AMER-friendly: [Register Here](https://rsvp.withgoogle.com/events/chrome-facilitated-testing-webinar-amer)
        *   EMEA-friendly: [Register Here](https://rsvp.withgoogle.com/events/chrome-facilitated-testing-webinar-emea)
        *   Japanese language: [Register Here](https://rsvp.withgoogle.com/events/testing-and-preparing-for-third-party-cookie-restrictions-oh)


# Notes

[Jeroune Rhodes:] Chrome team facilitates testing next week + another one the following week. If interested in Mode A / B testing, please join.



*   To join, please register below: 
    *   AMER-friendly: [Register Here](https://rsvp.withgoogle.com/events/chrome-facilitated-testing-webinar-amer)
    *   EMEA-friendly: [Register Here](https://rsvp.withgoogle.com/events/chrome-facilitated-testing-webinar-emea)
    *   Japanese language: [Register Here](https://rsvp.withgoogle.com/events/testing-and-preparing-for-third-party-cookie-restrictions-oh)

[David D] Creative Submission scanning - Can we set aside a session to deep dive into it? It will be nice to take it beyond comments on documents



*   [Michael] That is correct - there is a document that comprehensively states how to do creative scanning. This is a key use case we are trying to figure out a good long term privacy centric solution: https://github.com/WICG/turtledove/issues/792
*   [Orr] Open to having discussion live in a subsequent session or a combination of Github and live discussion.
    *   [Michael] Okay, let’s add it to the agenda for next week **[Action Item]**
*   [Michael] Buyers telling sellers about their creatives in advance is the easy way to do this. We are seeing what we can do in the browser to better facilitate a smoother and more automated way for SSPs to learn about the creatives. 

**Continue discussion on https://github.com/WICG/turtledove/issues/924**



*   [Shankar] We need to figure out how we can use some signals by the ad server
*   [Tanjit] When we get a call as an ad server, we do not have any context with respect to the IG name. Even from other signals, it is not clear what information can be leveraged. It will be great if there is a spec or document that can guide us to use the user bidding signals
*   [Michael] One of the active topics of discussion is parties like the ad servers being able to contribute with signals for ad selection. The thing for us to collectively explore is to what extent is possible to let you exercise your decision making capability in a way that does not involve mixes of signals from different parties being exfiltrated from secure environments like the PA auction.  
*   [Flashtalking] DV360 told us it will be difficult to pass in the IG
*   [Shankar] The whole idea of a DSP like DV360 passing in the IG is a non starter because of the privacy concerns
*   [Flashtalking] Why won't we be able to given k-anon checks?
*   [Shankar] At the point of registering the IG, you actually register the render URL and that is what is checked for k-anon plus the IG name.
*   [Michael] How much of the utility you are looking for would you get if at the time of rendering, you can learn the IG name subject to the same k anonymity constraint of that IG? It was not clear to me whether that information gets you exactly what you are looking for or not. It seems to me that we are leaning towards the solution being at the time of the creatives being in the IG
*   [Shankar] Probably, But we have to go experiment what else we’ll need
*   [Flashtalking] it is worth exploring. But there are other things that change after IG is created… inventory changes etc. Maybe we can make this work for now, it will be a step forward but we need DV360 to make that work.
*   [Flashtalking] Will the ads still be able to look in SS and choose it?
*   [Michael] At the time the ad is selected there is a SS mechanism called select URL. Makes it possible for your renderer… it is certainly possible it could be used for creative rotation.
*   [David Tam] Is it possible to update the renderURL without the user being on the advertiser website?
    *   [Michael] Yes, the IG gets to update themselves right now. 
*   **Chatspace exchange:**
    *   Michael Kleber
    *   11:03 AM
    *   Please sign in:[ https://docs.google.com/document/d/1Kr0hpfQ\_Q1LX1aN00D5k\_09yV\_a7WE9RSn69nS3nZho/edit](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit)
    *   Laszlo Szoboszlai
    *   11:03 AM
    *   sorry I stopped it, it just starts automatically :D
    *   Brian May
    *   11:03 AM
    *   Thanks Lazlo.
    *   Michael Kleber
    *   11:04 AM
    *   Please sign in:[ https://docs.google.com/document/d/1Kr0hpfQ\_Q1LX1aN00D5k\_09yV\_a7WE9RSn69nS3nZho/edit](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit)
    *   Laszlo Szoboszlai
    *   11:05 AM
    *   yes captions are available but I couldn't copy them  (for note taking purposes)
    *   Tamara Yaeger
    *   11:05 AM
    *   I'll take next week then, Manny?
    *   You
    *   11:06 AM
    *   Sounds good Tamara. Thank you!
    *   +1 to check notes after me. It can be tough to get everything accurately.
    *   Matt Davies
    *   11:08 AM
    *   Does anyone have the registration link to reporting part 2?
    *   Jeroune Rhodes
    *   11:08 AM
    *   AMER:[ https://rsvp.withgoogle.com/events/chrome-facilitated-testing-webinar-amer](https://rsvp.withgoogle.com/events/chrome-facilitated-testing-webinar-amer)
    *   Brian May
    *   11:08 AM
    *   Thanks
    *   Jeroune Rhodes
    *   11:08 AM
    *   EMEA:[ https://rsvp.withgoogle.com/events/chrome-facilitated-testing-webinar-emea](https://rsvp.withgoogle.com/events/chrome-facilitated-testing-webinar-emea)
    *   David Dabbs
    *   11:09 AM
    *   https://github.com/WICG/turtledove/issues/792
    *   David Dabbs
    *   11:13 AM
    *   That's a single-vendor solution.
    *   David Dabbs
    *   11:22 AM
    *   So a k-anon gated ${IG\_NAME} macro?
    *   Fabian Höring
    *   11:27 AM
    *   using shared storage during creative rendering requires a round trip though, so it increases latency
    *   Warren Fernandes
    *   11:29 AM
    *   Is there a doc detailing the role of SSPs in the protected audiences ecosystem?
    
