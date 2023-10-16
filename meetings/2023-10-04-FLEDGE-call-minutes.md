# Protected Audience (formerly FLEDGE) WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Oct 4, 2023


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Leo Bierent (Criteo)
3. Matt Menke (Google Chrome)
4. Luckey Harpley (Remerge)
5. Fabian Höring (Criteo)
6. Sven May (Google Privacy Sandbox)
7. Paul Jensen (Google Privacy Sandbox)
8. Laurentiu Badea (OpenX)
9. Joshua Prismon (Index Exchange)
10. Alonso Velasquez (Google Privacy Sandbox)
11. Orr Bernstein (Google Privacy Sandbox)
12. Russ Hamilton (Google Chrome)
13. Isaac Foster (MSFT Ads)
14. Sid Sahoo (Google Chrome)
15. Marco Lugo (NextRoll)
16. Abishai Gray (Google Chrome)
17.  Caleb Raitto (Google Chrome)
18.  Kevin Lee (Google Privacy Sandbox Google Chrome)
19.  Jonasz Pamuła (RTB House)
20.  Andrew Pascoe (NextRoll)
21. Joel Meyer (OpenX)
22. Manny Isu (Google Chrome)
23. David Dabbs (Epsilon)


## Note taker: Isaac Foster


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


## [Suggest agenda items here — no agenda no meeting!]

_Note: old items were cleared out when we used this doc for the [in-person meeting at TPAC](https://github.com/WICG/turtledove/blob/main/meetings/2023-09-11-Protected-Audience-TPAC-notes.md).  Please re-add if there was something held over that you still want to discuss_



*   [Alonso Velasquez] For debugging only proposal: https://github.com/WICG/turtledove/issues/632 
*   Roni Gordon [held over, was unable to attend Sept 27 meeting]
    *   same origin constraints - https://github.com/WICG/turtledove/issues/813
    *   K/V lookup timeouts - https://github.com/WICG/turtledove/issues/814
    *   Macro substitutions - https://github.com/WICG/turtledove/issues/817
    *   API versioning - https://github.com/WICG/turtledove/issues/823
    *   Sensitive signals - https://github.com/WICG/turtledove/issues/824
    *   Multi-size banners - https://github.com/WICG/turtledove/issues/825 +1000
    *   Additional browser event beacons - https://github.com/WICG/turtledove/issues/826 
*   Isaac:
    *   Buyer/Seller Reporting Questions: https://github.com/WICG/turtledove/issues/682#issuecomment-1710965068
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
    *   Any updates on TEE requirements https://github.com/WICG/turtledove/issues/731
*   Leo  : Discussion about Increasing the number of ad components entering the bid from 20 to 40 https://github.com/WICG/turtledove/issues/810 
    *   


# Notes

Michael Kleber: normal bookkeeping, please sign in, etc. Isaac=notetaker, Not Isaac’s job to be perfect, please go look at notes doc, correct mistakes made by Isaac, which he will. Join WICG, under W3C, for IP guarantee reasons.

MK: first agenda, Alonzo


## [Alonso Velasquez] For debugging only proposal: https://github.com/WICG/turtledove/issues/632 

Alonzo: timeboxing, PM working for MK, update on[ status of 632](https://github.com/WICG/turtledove/issues/632), want to get feedback, forDebuggingOnly() functions is to facilitate debugging, available in scoreAd and generateBid (buyer and seller use cases), help understand what’s going inside the processes, plan of record is that these functions become unavailable at 3PCD, still have functions that are only for win events through the other report\* functions, after listening to ad techs have concluded three basic things:



*   Net new onboarding and development continues after 3PCD, benefit to getting more verbose data
*   Once integration is live, we all have monitoring/alerting in place, need to know if a thing is happening with a bug and figure out what that is, so we know you want to know how to figure those things out, first step is detection, followed by root cause analysis dig around for what’s fishy part of that data universe is what happened in auction see things like samples of data payloads to see what looks right/wrong.

Alonzo: …we have heard that, understand criticality, given that feedback: workable path to support these use cases with level of fidelity that supports the needs but is heavily downsampled, want do downsample to make it untenable to do things that we don’t want them to do with the APIs, going to summarize here now



*   Guardrails to how often ad tech gets the data
    *   Sampling rate enforced by chrome, 1/1000 times it fetches the url, else NO. 
    *   When the answer is NO, 999 times out of 1000: Cool down period: 1 year, for this particular user device and for this adtech, chrome will not evaluate this device again for another year, this prevents malicious scripting and fun actions for the actor to try to understand who this person is and try to spam them.
    *   When the answer is YES, 1 time out of 1000: report written (i.e. reporting URL gets fetched), now there’s a 3 year lockdown, this device will not write any report to any ad tech.

Alonzo:...: We did some math, we believe that a given ad tech gets 4.7K to 5.4K reports per day.  That’s the proposal, helps utility of debugging while keeping the narrative

MK: nothing to add, want to hear feedback, as ever not final want to hear feedback and discussion. This is a very challenging area, can see in the discussion in #632, but the current mode can't stay after 3p cookies are gone, it is way too privacy leaking, something must be done.  We get that the most drastic of eliminating all debug logging entirely might be tooooooo drastic, so looking for middle ground that still prevents widespread tracking, trying to walk across that narrow bridge. There will be disadvantages to competing principles, please help us shape these tradeoffs.

Jonasz Pamula: thank you! Few questions:



*   Buyers and sellers compete with each other for ability to send report. Multiple buyers want to submit debugging reports, what happens if one has 1000 IGs and one has 1 IG, does that mean one of them has a way better chance.

MK: magic of cool down period is, it doesn’t matter whether the ad tech calls the debugging api 1 or 1000 times, first time you might get a report or not report based on odds ^, after that all times you call are irrelevant, all the other times you’re in the lockout period where you won’t get a report no matter what.

JP: not asking about number of IGs, asking about who goes first and whether they have an advantage

MK: who goes first gets a very tiny higher chance b/c of how sequencing works, but this is only relevant if both ad techs would otherwise have received reports, so odds of relatively ordering having an impact is 1 in 1 million

Alonzo: remember that by rolling the dice they have hurt their chances in doing so in the future, so they are incented to be smart about how they do that.

JP: to clarify, simple example, 2 buyers in the world, 1 IG on each device, first one is always 1 MS to generateBid, 1 is 50 MS to generateBid, each calls the reporting api at the end of the bid, wouldn’t that mean the second buyer never sees anything?

MK: No. first buyer gets to send 1 time in 1000, other 999 times they go into cooldown period that only impacts that same ad tech, the 1 in 1000 is scoped to browser (999/1000 cooldown is scoped to ad tech who lost, 1/1000 winner is 3 year lockout of the entire browser API across all ad tech).

Fabian Hoering: confirm target is 5K events per ad tech, will you adjust if that is not reached, that is minimum per day?

MK: no. Not trying to guarantee number of reports, estimate from Alonzo ^ is based on assumptions about how the players use the APIs. We considered but rejected a massive central system to keep track of the number of reports each tech has gotten to do smart rate limiting, that seemed way difficult.  With this plan, each browser is acting independently of all the others, we wanted to avoid coordination between the browsers to govern rates or some such.

FH: second question, I can override this setting locally to get reports all the time for dev work.

MK: yes for local copies of chrome for development purposes.

FH: not visible to network tab, these requests.

Paul Jensen: work in progress

Isaac Foster: Ad tech that calls the API is taking a risk — can you tell that you're locked out?

MK: No, but there is no harm in calling when you're already locked out (no further lock-out as a result).

Alonso: If in generateBid or scoreAd you somehow know that this particular auction is more interested to report than other ones — for example something just happened that seems like a bug — then you can decide to call the API more often, to increase the chance you get a report from that kind of auction.

Josh Prismon: projection on steady state numbers assume that all requests are enrolled?

A: yes

Josh P: general telemetry, we don’t know if we have an issue and want to observe to see if something weird is happening, second is we know there’s an issue and we’re trying to find it. So, if we leave these on for everything ( the first case) am I poisoning the well for the second case (targeted troubleshooting). Is that intended, or is shold those use cases be handled in different ways.

A: if you use for debugging only that could happen, but the thing about detection is you have other tools available, you have event-level reprotWin and reportResult functions, those are around for longer till at least 2026, can use that for alerting for things that are NRT, for things that you don’t need immediate feedback and can wait longer, maybe discuss with private agg team, but private agg solution is there for loss and win data, and you’re getting conditions for those cases, you can pull private agg info, but it’s not for immediate alarms, but you’d get it a bit later.

Josh P: trick is finding the right balance between general telemetry and specific ad hoc debugging. 

JP: is that logic enforced for 1% in mode b?

MK: too soon to know, still in design phase. Likely no, after 3PCD, want debugging to be easy while 3PC still in place. Right now looking for feedback on design.

JP: less restrictions for 1% during mode b?

A: proposal is independent of A vs B modes, plan is for this functionality to be available once 3pcd starts in earnest, don’t want to take away rug from under feet during testing period. Client only implementation lets you make choices and tradeoffs. Make 3pcd testing for CMA as painless as possible. Don't want to disrupt cma testing.

MK: JP asks good question: does 1% mode B traffic count as 3pc removal for that 1% of users, and if so should we expect to not see forDebuggingOnly for the 1% of users? Answer is we don’t know right now, need to figure that out, don’t have the API in hand right now, need to flesh this out in particular JPs question.

A: ah, true.

MK: Want to move on from this topic to next agenda item soon

David Dabbs: i bow out gracefully

MK: is Ron here?

Josh P: Roni is off this week.

MK: do you want to go through the Roni questions?

Josh P: no hold till Roni is back. Want broader visibility.

MK: hmmm yess, &lt;strokes beard>. Hold those till next week till Roni back.

Isaac: no please do others, would love to hear on 825 but wait for roni

MK: I did comment 1 minute before meeting, learned from Isaac

Isaac: excellent


## Leo  : Discussion about Increasing the number of ad components entering the bid from 20 to 40 https://github.com/WICG/turtledove/issues/810  

Leo Bierent: ^

MK: Paul, thoughts?

PJ: haven’t thought about the ad components one. Would like to know about what those ads look like, not sure I understand fully, talked with JP before and talked about a product carousel type of creative, 20 seemed reasonable, but let’s hear about other formats. What is the privacy impact of having more component ads: tricky with component ads, top ad can’t see component ad contents for privacy, has knock on effects, FF reporting you want to know which one the person clicked on, FF reporting was augmented to support that so now more component ads means more leaking from the IG due to which-ad-was-clicked precision.

LB: pagination in dynamic ads is part of criteo offering, reduces a lot of %.

PJ: spanning across time that the different ads are showing? 

MK: when you say 20 is too restrictive, are you showing a single ad that shows > 20 products, or is it a single user seeing more than 20 with a different product, but they’re different ads.

LB: same ad, exploratory, 20 will impact size of banner, less creative offering and exploration of catalogs for which we have millions and want to pass K-anon easier.

PJ: you can have more than 20 component ads inside an IG, just limited per impression.

LB: yes

PJ: do they rotate through ads

LB: they will rotate in a single ad

PJ: so show 10, 10 seconds later show another 5?

LB: yes

PJ: can be accomplished in other ways. Run another auction to refresh ad slot. Maybe if category of ads you could divide into 1000 into 5 categories, rotate between them.

FH: need to repay if you’re rotating

PJ: don’t have to use component ads for this, could use groups of ads, button to go to next set, 

FH: something

PJ: say headphones, 1000 models, group by gaming, high quality audio etc, pick which set to show, inside of one ad rotate through ones in that category. How specific does each headphone/product need to be?

LB: want to understand why 40 vs 20 is a privacy threat.

PJ: not a cutoff, more of a sliding scale, more means more potential leak. Let’s talk in issue, see an actual ad to make sure we’re communicating well.

LB: thanks!

MK: if you have stats to understand the need, we don’t love having more ads go into the ad but don’t actually show to the user and only appear if there is substantial interaction, lets you circumvent k-anon, so it would help to understand this better, understand rates at which these things are seen.

LB: got it, any help with k-anon that results in tradeoffs we’d like to talk about

MK: right, but if you have products that aren’t being shown, then not clear what the k-need is.

MK: only a few minutes left, no snack sized issues to talk about, so just wrap? Desperate final words from anyone? David Dabbs want to say what you were gonna before.

DD: since you offered! I will, since Alonzo is around, will say that I’d like some more clarity, unrelated to fledge, to test labeling, you’ve given some info but still need more about format of labeling beyond exampleLableA, perhaps it’s saying they are super trivial, labeling 100% is good to avoid weird issues, since it’s starting in Q4 the sooner we get this info the better.

A: thanks, we’ve heard feedback on labeling 100%, help me understand the case we’re trying to solve

DD: critero should explain, look the issues

Isaac: lets you distinguish between “not sent” vs “not in bucket”

MK: thanks, done for hour, next week on the 11th we’ll meet again.
