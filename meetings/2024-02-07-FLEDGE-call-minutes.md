# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 4pm UTC (during winter)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Feb 7, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	



1. Paul Jensen (Google Privacy Sandbox)
2. Brian May (dstillery)
3. Matt Menke (Google Chrome)
4. Roni Gordon (Index Exchange)
5. David Dabbs (Epsilon)
6. Shankar Venkataraman (Jivox)
7. Wendell Bake (Yahoo)
8. Sven May (Google Privacy Sandbox)
9. Orr Bernstein (Google Privacy Sandbox)
10. Tim Hsieh (Google Ad Manager)
11. Laura Morinigo (Samsung)
12. Ricardo Bentin (Media.net)
13. Jacob Goldman (Google Ad Manager)
14. Miguel Morales (IAB Tech Lab)
15. DavidTam (Relay42)
16. Laurentiu Badea (OpenX)
17. Arthur Coleman (OnlineMatters)
18. Luckey Harpley (Remerge)
19. Pawel Ruchaj (Audigent)
20. Fabian Höring (Criteo)
21. Sathish Manickam (Google Chrome)
22. Rickey Davis (Flashtalking)
23. Tim Taylor (Flashtalking)
24. Becky Hatley (Flashtalking)
25. Tamara Yaeger (BidSwitch)
26. Isaac Foster (MSFT Ads)
27. Kenneth Kharma (OpenX)
28. Warren Fernandes (Media.net)
29.  Vedant Seta (Media.net)
30. Gowtham Kommineni (LiveRamp)
31. Amit Gupta (Jivox)
32. Ruturaj Vartak (media.net)
33. Daniel Rojas (Google Chrome)
34. Andrew Pascoe (NextRoll)
35. Taranjit Singh (Jivox)
36. Siddharth VP (Jivox)
37. Laszlo Szoboszlai (Audigent)
38. Garima Mimani (Google Privacy Sandbox)
39. Anthony Yam (Flashtalking)
40. Courtney Johnson (Google Chrome)
41. Kevin Lee (Google Privacy Sandbox)
42. Drew Schoentrup (Big Crunch)
43. Russ Hamilton (Google Chrome)
44. Owen Ridolfi (Flashtalking)
45. Alex Peckham (Flashtalking)
46. Steven Valdez (Google Chrome)
47. Matt Davies (Criteo | Bidswitch)


## Note taker: Arthur Coleman


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


## Suggest agenda items here:



*   Isaac Foster:
    *   [Revisit Persistent Opt Outs question w/r/t PST](https://github.com/WICG/turtledove/issues/915#issuecomment-1892962819) (see PST specific question [here](https://github.com/WICG/trust-token-api/issues/288))
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Tim Hsieh
    *   Updated proposal for Video and Native support https://github.com/WICG/turtledove/issues/265#issuecomment-1914779917 
*   Jacob Goldman
    *   Reporting and top-level worklet timeouts https://github.com/WICG/turtledove/issues/959 
*   David Dabbs
    *   Is `updateAdInterestGroups()` temporary? It is in the spec but unmentioned in the main explainer. Search [here](https://github.com/search?q=repo%3AWICG%2Fturtledove%20updateAdInterestGroups&type=code)


# Reminder of Upcoming Events:



*   [Announcement] Chrome Facilitated Testing Webinar
    *   The Google Chrome team will be hosting our next external webinar sessions where we will focus on the testing and preparing for the current 1% restriction of third-party cookies in Chrome along with use of Mode A and B available as part of Chrome-facilitated testing.

        The first **Americas friendly session** is happening on**  Feb. 1st 12-1 pm ET**. A second **Japanese language session** is happening **Feb. 6th 3:30-5:30 pm JST**. A third **EMEA friendly session** takes place on** Feb. 8th 12-1 pm GMT**. 

    *   To join, please register below: 
        *   AMER-friendly: [Register Here](https://rsvp.withgoogle.com/events/chrome-facilitated-testing-webinar-amer)
        *   EMEA-friendly: [Register Here](https://rsvp.withgoogle.com/events/chrome-facilitated-testing-webinar-emea)
        *   Japanese language: [Register Here](https://rsvp.withgoogle.com/events/testing-and-preparing-for-third-party-cookie-restrictions-oh)


# Notes

Note intellectual property rules apply for W3C

Announcements:



*   See above


## About [Revisit Persistent Opt Outs question w/r/t PST](https://github.com/WICG/turtledove/issues/915#issuecomment-1892962819)

Isaac Foster



*   Last time we chatted about this a bigger issue around privacy rights
*   Issue around being able to honor opt-outs
*   Seems like private state tokens are the initial motivator for the ”am I human thing” but it seems like some of the docs seem to say that it is a way to communicate web safety across domains
*   Persistent opt-outs can fall into that.
*   Seems like private state tokens might be directionally handled to handle coarse grained information
*   Haven’t heard back from PST folks on long-term thinking - aware there are limits right now
    *   Only two private state tokens by partition
*   Hoping to get someone official to give their opinion of whether
    *   A. Pursuing this type of use case for PSTs is one we are open to
    *   B. If so, then would that potentially align with PST direction?

Paul Jensen



*   PSTs is about users going to an adtech’s website and opting out and being persistent across all sites

Isaac Foster



*   Let me restate for Xandr and other MFST properties
*   We have a platform level opt-out (privacy.xandr.com) that basically says “do not show me ads from your platform”
*   Different from GDPR, CCPA etc.  Different than when we get “per request” per those standards - we get a giant string on request
*   In a persistent platform opt out at privacy.xandr.com what we do is we can store this value in your browser and no matter what site we are integrated with we know you say “don’t show me ads”
*   Without cookie mechanic, that system goes away.  There are few fun tricks you can do, but there is no reliable way to do this.

David Dobbs



*   Is your request to have that specifically for PAAPI or to replace that specific compliance use case under the GPS?

Isaac Foster



*   Not intended to refer to a browser-based API.It is not unrelated.  But if someone comes to Xandr - 
*   Ideally there were be “something” (Privacy sandbox an overused term) - is there an API we can use for this broader use case and PST seems a good way to do that

Brian May



*   Sounds @Isaac Foster sounds like you want a per domain, able to see across partitions solution
*   Sounds like private state tokens - talked to --- this morning about this issue

Steven Valdez



*   In terms of information in terms of PSTs, there are a lot of limitations around that.  Might make sense to have a different API to do this for that specific use case for that limited amount of data.  Given this user case, we could have a slightly different privacy model.  PSTs seem like a good place to do this, but it isn’t set up for it exactly right now

Isaac Foster



*   Would be good to see where you see PSTs going - seems like it originated with anti-fraud but it seems like some intentionality naming it PSTs that it was meant for the broader use case
*   Clarity here would be helpful
*   If there is a similar but different web API that could be restricted to this use case - reliable for that domain - I think that would be fantastic

Steven Valdez



*   This could be incubated separately from PSTs
*   But this is a good use case to start with to think about how to approach

Isaac Foster



*   If you envision this as a new thing, how do we go about starting discussions about that (good question!)
*   Should we have some rough conversation with people in industry - maybe not immediately - but how would you see this?

Steven Valdez



*   Could have a side meeting and then start on a side proposal to PSTs.
*   We can get an email thread set up and see who is interested in chatting about this?
*   ACTION ITEM: Set up an email group to discuss

Brian May



*   Why are you starting with PSTs vs. starting with the requirements and then look at the right tech?

Isaac Foster



*   My intention is not to say PSTs are the right place to start
*   Digging around PSTs there are a lot of properties there that do seem to be potentially valuable.
*   The way it could handle coarse-grained information across sites seems like it could be applicable
*   To the extent that it makes sense to have it “shaped like” PSTs that’s fine - 
*   But I would like to be able to implement this use case for coarse-grained information - that’s how I would frame it

Brian May



*   Is there an analogy between “this web site wants to track you” and “do you want to allow tracking for behavioral targeting”?
*   Is that approach insufficient

Isaac Foster



*   I’d prefer that we don’t have to have a browser-controlled popup - my instinct would be to have a separate API to handle in a different context.

Paul Jensen



*   Look for a compromise on some level that is useful for ???
*   If we surface this in some kind of isolated environment, it has a potential to be more private 
*   Wonder if there is a way to have PAAPI keep track of this using some kind of cross-site identifier

Issac Foster



*   This would have to occur outside of the private auction - because then we wouldn’t want to show them adds from even our contextual auction
*   So doing it within the auction would not work for us
*   One reason I thought PSTs were interesting that framework currently has some ideas about how you limit bit-leakage that would make it work across 32 different adtechs
*   If you could only read it in the context of your domain script, that would work for now although not ideal.

Shankar Venkataraman



*   Question: What about using shared storage?

Isaac Foster



*   Wev’e talked about using shared-storage and exfiltrate that via the 12-bit limit per day,  
*   When I said reliable earlier there are a few things we could do with shared storage to start with that would give us pretty good coverage.
*   I don’t think that would be a good primitive here
*   But if you ask me about workarounds for this, but from a more web primitive for this is I don't think shared storage is the way to go

Isaac Foster



*   Request that goes out to any domain could use a special header

Brian May



*   If I go out to an FSP and it doesn’t hit Xandr directly, how does Xandr know about it

Isaac Foster



*   If we get a request form an external SSP and we don’t have that mapped then we couldn’t tell
*   But if mapped to an identity I believe we look it up on the back end.,

Brian May



*   Problem is these two rarely meet

Isaac Foster



*   That is certainly true

Paul Jensen



*   I think adding this to a discussion of PSTs is useful, but it may be a more general use use case where PAAPI isn’t the right place to do the implementation

Isaac Foster



*   I see this as a Web API not under PAAPI
*   I just don’t know what forum to get this thread started

Brian May



*   Have you considered trying to raise this in PATCG?

Isaac Foster



*   Happy to do that if that is the right place

Paul Jensen



*   Feel free to reach out to Steven
*   Eric Troutman is the other person to contact


## TOPIC 2: Reporting and top-level worklet timeouts https://github.com/WICG/turtledove/issues/959 

Jacob Goldman:



*   There are different timeout controls that the seller can specify (in the auction config)
*   Those timeout controls do not seem to work when  extended to reporting.
*   We were wondering if we could discuss extending new controls to modify that timeout.
*   This is coming from seeing a significant uptick in flakiness across our regression testing
*   Seems like 50 milliseconds is not enough time to generate a complete event.
*   We started a little bit of profiling but were not able to really hone in on it
*   We don‘t have a way to measure
*   The frequency of this is occuring in production

David Dabbs



*   When we use the report to track our spend - 
*   What kind of signal loss are we looking at?

Jacob Goldman



*   We don’t know in production
*   In our regression testing we experience up to 10-20 percent flakiness .
*   We would like our control to … 
*   We think this is a larger problem and we are looking for guidance on how to handle

Laurentiu Badea



*   Could some of this be due to the fact that generateBid() is not allowed to pass data to reportWin, so reportWin has to redo part of the same calculations to re-generate data for the report?

Paul Jensen



*   

Matt Menke



*   Currently when the frame running the auction is destroyed, any running or pending reporting worklets for auctions run in the frame are aborted, due to their hooks into the frame. This does not affect network requests for sending the reports, which can outlive the frame.
*   Fixing this is an item on our to do list.
*   Paul later pointed out  this is probably not a common corner case, since we start running reporting scripts immediately after an auction completes (even before the ad is actually used), but it may appear in synthetic testing.
*   Increasing timeouts is reasonable, but too high a timeout can potentially slow down in the browser after a user has navigated away.

Paul Jensen



*   When we were designing timeouts in the beginning, we had a more simplistic vision
*   Initially, just a 50ms timeout by default
*   Other work with scoreAd()
*   We had 50 ms here so why not there
    *   We want the auction to take a reasonable amount of time because good for everything
    *   We need to give people reasonable amount of time to calculate
    *   Another reason: exfiltration of signals by covert channels
    *   Example: you could make a signal out of the protected worklets
    *   Hard to to do to protect against, especially Javascript to Javascript
