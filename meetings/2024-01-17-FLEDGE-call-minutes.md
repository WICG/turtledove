
# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 4pm UTC (during winter)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Jan 17, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Brian May (dstillery)
3. Harshad Mane (PubMatic)
4. Shankar Venkataraman (Jivox)
5. Ricardo Bentin (Media.net)
6. Sven May (Google Privacy Sandbox)
7. Youssef Bourouphael (Google Chrome)
8. Matt Menke (Google Chrome)
9. Paul Jensen (Google Chrome)
10. Arthur Coleman (OnlineMatters)
11. Russ Hamilton (Google Chrome)
12. Matt Davies (Bidswitch | Criteo)
13. Kevin Lee (Google Privacy Sandbox)
14. Jacob Goldman (Google Ad Manager)
15. Fabian Höring (Criteo)
16. Julien Aimonino (Criteo)
17. Isaac Foster (MSFT Ads)
18. Laurentiu Badea (OpenX)
19. Joel Meyer (OpenX)
20. Sid Sahoo (Google Chrome)
21. Orr Bernstein (Google Chrome)
22. Drew Schoentrup (Big Crunch)
23. Amit Gupta (Jivox)
24. Taranjit Singh(Jivox)
25. Laszlo Szoboszlai (audigent)
26. Amitava Ray Chaudhuri (Adobe Adcloud)
27. Wendell Baker (Yahoo)4
28. Rickey Davis (Flashtalking)
29. Becky Hatley (Flashtalking)
30. Kenneth Kharma (OpenX)
31. Patrick McCann (Raptive)
32. Don Marti (Raptive)
33. Pawel Ruchaj (audigent)
34. David Dabbs (Epsilon)
35. Alonso Velasquez (Google Chrome)
36. Andrew Pascoe (NextRoll)
37. Chris Nachmias (Flashtalking)
38. Alex Peckham (Flashtalking)
39. Zach Edwards (Victory Medium)
40. Anthony Yam (Flashtalking)
41. Owen Ridolfi (Flashtalking)
42. Abishai Gray (Google Chrome)
43. Roni Gordon (Index Exchange)
44. Joshua Prismon (Index Exchange)
45. Benny Lin (Bombora)


## Note taker: Orr Bernstein


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


## Suggest agenda items here:



*   Isaac:
    *   [Revisit Persistent Opt Outs question w/r/t PST](https://github.com/WICG/turtledove/issues/915#issuecomment-1892962819) (see PST specific question [here](https://github.com/WICG/trust-token-api/issues/288))
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Roni Gordon
    *   Sensitive signals - https://github.com/WICG/turtledove/issues/824
*   Fabian Höring
    *   forDebuggingOnly availability - https://github.com/WICG/turtledove/issues/632
*   Shankar Venkataraman
    *   Continue discussion on https://github.com/WICG/turtledove/issues/924
    *   https://github.com/GoogleChromeLabs/privacy-sandbox-dev-support/issues/201


# Notes

**Buyer/Seller Reporting Questions: https://github.com/WICG/turtledove/issues/682#issuecomment-1710965068**



*   Isaac
    *   Paul answered that we can jam anything we want to into that string.
    *   It’s still possible for a creative to render because the way it’s used for k-anonymity is different from how it’s used for reporting (how it’s passed into report\*). Is that correct?
*   Michael
    *   The way that k-anonymity interacts with reporting - the way that PA API works right now - is that there are two possibilities for what happens at reporting time
    *   The event-level report includes the renderURL (and all the first-party information)
    *   The event-level report includes the renderURL and additional information that was included for slicing / analytics. It used to be just the name of the IG group, but now we’ve moved to the buyer-reporting ID and buyer-seller reporting ID that might be used in reporting.
    *   The IG name/ buyer-reporting ID / buyer-seller-reporting ID only gets to be reported if the ad + metadata reaches the threshold of k-anonymity at the time of reporting.
    *   If the ad by itself hasn’t reached k-anonymity, it can’t win the auction.
    *   The ad + label must also reach k-anonymity to be reported in the reporting APIs.
    *   The workaround is to put everything the buyer needs on the URL, so the URL can’t win the auction until everything needed in reporting has reached the k-anonymity threshold.
    *   You can do aggregate reporting even with no k-anonymity attached to it.
    *   If you’re talking about deals and it affects how much you’re going to bid, there are no k-anonymity bars associated with the data available at bidding time or the information passed to scoring. On the other hand, stuff available in event-level reporting, it is limited by k-anonymity. Where possible, you should consider aggregate reporting.
*   Isaac
    *   I assume that the reason event-level reporting is around until 2026 is because the systems currently require event-level resolution (Isaac Note: for billing and audit purposes in particular, our current GRC rules require it, changing how we promise to handle compliance will take some time
    *   In the case of a deal which might affect pricing terms, I don’t think aggregate will work.
    *   I’m pretty sure it’s correct that we can include everything on the URL.

**forDebuggingOnly availability - https://github.com/WICG/turtledove/issues/632**



*   Fabian Höring
    *   Can you start with context on what’s been put forth by Chrome?
*   Michael
    *   When people began using the PA API, we added a debugging mechanism called forDebuggingOnly, which would be available in the middle of the JavaScript that did all of the important functions of the PA API - at bid time, scoring time, reporting time. All of the inner JavaScript that doesn’t generally get to touch the network - the API keeps these lock down to protect privacy.
    *   We created this debugging mechanism so that people could experiment with the API and see what’s going on inside the auction while it was happening to help you debug.
    *   That’s something that obviously cannot exist in the future, because it’s an escape hatch, unlimited ability to exfiltrate information.
    *   The idea was, once 3PC go away, we’ll shut down the ability entirely.
    *   A bunch of people very reasonably pointed out that having no ability to do debugging was a bad way to run a piece of code in the wild.
    *   This group of people came up with a heavily downsampled forDebuggingOnly. Everytime you ask to use it, the browser flips a coin, and 1/1000 it allows, but the other 999 times, it makes you wait for as short as two weeks or as long as a year before you can ask again. When you do send a report, then no more reports can be sent from that browser for another three years. It wouldn’t be useful to track a user across the web.
    *   Compromise position that aims to make debugging possible while protecting privacy
*   Fabian
    *   For us, since the very beginning, it’s a critical piece on how we debug our code. We use this to compare what we have in generateBid with data we have offline. And we use it to report errors. For example, we learned that there was an error where we didn’t get perBuyerSignals, and we wouldn’t have found this otherwise.
    *   The sampling compromise seems fine.
    *   It’s written in the ticket that it would be 5000 events, but it’s very, very few. Also, the lockout periods will strongly bias towards new Chrome browsers. We sometimes need errors from real users and not errors from new Chrome browsers, which are often bots, which clear their data, and so appear again and again in the population of forDebuggingOnly.
*   Michael
    *   There’s a Google Spreadsheet calculator linked as a comment in this issue - https://github.com/WICG/turtledove/issues/632#issuecomment-1786093608 - that lets you put in assumptions to calculate how many debug reports you’d receive per day. But it’s hard to know because of the interaction between different ad techs. Once a report gets sent, the browser can send no more reports to any ad tech. So it depends on how many other ad techs are using this API.
    *   Do you use this API all the time, or only when something unexpected or surprising happens? Or do you use it somewhere in between? Probabilistically choose to send a report, and choose that probability based on how “surprising” this event is.
    *   The calculator has one field - from a pessimistic POV - how many ad techs do you think are using this API as much as possible? Whenever a user is not in the cooldown period. However many ad techs are using the API, the less likely you are to be able to use this API.
    *   The cooldown mechanism is there to protect your ability to debug from other ad techs.
    *   The answer to how good a job it will do for your needs depends on what you’re trying to do with it.
        *   To use it for error reporting, it should do a good job. After getting a dozen or two dozen reports of this error, you should be able to reproduce, get this error occurring on your local machines, and fix the error.
        *   To get generic background reporting and then recreate things in the laboratory, it could be useful there if you select a lower probability - ask to send reports 1 time in 100 or something like that - keep these users available if that buggy thing later happens in this user’s browser.
    *   The number depends on both how they use the API and how everybody else uses this API.
    *   The goal is to have reasonable behavior when another ad tech is doing the spammy thing, and also when you’re doing the reasonable thing, getting more reports when something interesting happens.
    *   Aggregate reporting can be useful here too
*   Brian May
    *   This is going to disproportionately impact small campaigns.
    *   This is a long ticket, I haven’t read through the whole thing. What’s the ID used to determine lockout?
*   Michael
    *   The browser. But this is only when the report is actually sent.
    *   But in the 999/1000 when one ad tech asks to send a report and the browser says no, then it’s locked out only for that ad tech, not for everyone else.
*   Fabian
    *   Having one browser send one event every eight years is not reasonable.
    *   If you increase the probability and put everybody in a lockout state, that makes sense. But the lockdown and cooldown states are far too long.
*   Michael
    *   Making a cooldown shorter is probably not what you want, because the less cooldown, the more the ability of one ad tech to hurt another ad tech. The cooldown is about protecting you from somebody else misusing this API.
    *   Lockout is all about protecting this user from tracking. They don’t care which ad tech found out something about them. A report from a particular user being sent too often is something we want to avoid. A lockout period of three years when you sent a report is a relatively reassuring story about what reporting happens on their browser. We can play with parameters and see if changing this has a significant impact on the number of reports you would receive. But having e.g. more than one every year, doesn’t sound right to me.
*   Roni Gordon
    *   We use forDebuggingOnly for more than just debugging. Let’s assume that that’s a solved problem, that using this for its off-label use can be done another way.
    *   But the idea that something someone else does affects me, then that’s a non-starter. I think we have it upside-down.
    *   I think when there’s a new Chrome version, these things should reset. When there’s a new Privacy Sandbox experiment, these things should reset.
    *   I want to understand what’s happening in the auction, at a frequency in which auctions happen. If we get there, we’ll be in a better state.
*   Michael
    *   What you’re describing is exactly what led to the parameters we have now. We had a lot of conversations about the sorts of things the API should be used for, and picked parameters that fit those needs. I’m happy to talk more about problems that aren’t solved with the new parameters.
    *   But structural changes - i.e. Chrome releases a new version every month - are not a reasonable privacy story.
*   Roni
    *   If anyone can build a profile based on one report every 30 days, that’s a really good ML model.
*   Michael
    *   If you tried to do debugging last month, 1/1000 you succeeded in getting some information from that user. Otherwise, you had to wait 2 weeks to try again. The highest probability thing that happens is that you get to try again one month later. The intent of the API is that you have a never-ending supply of new users to provide debugging reports.
*   Brian
    *   Two ways we envision people using the API
    *   One is that they’re asking for reports continuously to try to detect issues
    *   Or one is to report when there’s an issue that needs debugging
    *   The reservoir will run dry.
*   Michael
    *   That’s the conversation we had when we chose these parameters.
    *   Let me pull up the calculator.
*   Brian	
    *   It’s difficult to run our business when it depends on everybody else doing the right thing.
*   Michael
    *   Yes, which is why we built the API as we did. If you’re an ad tech that calls the API every chance you get, then the number of reports you get is about 13,000 reports per day. That’s our estimate for the number from someone who calls it all the time.
    *   If you’re an ad tech and you only call it when you know of a particular issue, if the bug happens on 10,000 browsers, then you’ll get 6 or 7 reports per day of the exact error so you’ll have a case in hand with all the information you need to debug it.
    *   Exactly those two conditions are what we built the API for.
*   Brian
    *   6 or 7 reports on a campaign that has a couple millions of impressions for a campaign in a month is not very much.
*   Michael
    *   Even JavaScript on that happens 10,000 times a day, you’ll still get enough reports for you to emulate it on your browser and figure out what’s going on.
*   Harshad
    *   Debugging API is being used for creative registration
*   Michael
    *   Yes, we need to build a better API for creative registration. We know that people are using forDebuggingOnly to solve this problem. That problem has a much, much better privacy story than forDebuggingOnly.
*   Harshad
    *   Can we assume that the forDebuggingOnly won’t be deprecated until a new way to do creative registration is available?
*   Michael
    *   We’ll have news for you soon.
*   Alonso
    *   The forDebuggingOnly spec - the plan is to do the down-sampling at 3PCD. But we’re working on offering a way to test - during the testing period - whether a particular event would be sampled away with the logic that we’re planning to enforce for forDebuggingOnly at 3PCD.
    *   The hope is that ad techs can ensure that they have all of the samples they need.
    *   Once we have this label, you can look at a particular debug report. And you can provide feedback on whether your debugging needs are met or whether there are tweaks to the parameters.
*   Michael
    *   Yes. This label can tell you how well the downsampled version of the API will serve you needs.
*   Julien
    *   Like Fabian, repeating a point he made. One is getting enough reports. And the other is that reports are biased towards new devices.
*   Michael
    *   I think you’re certainly right that the cooldown/lockout means that new browser installs are more likely to be around for reporting versus those that have been around for a while.
    *   A few ways that you can correct for that skew in the ways you use that API, which relies on you looking at your data to make the distribution of reports you get most useful to you.
    *   When you add a user to an IG at the time of IG join, you have access to a lot of information on the site on which the IG was joined. And at auction time, you have lots of information about past behavior on the site on which the auction is run. Information from both is available during the auction when you’re deciding whether to request debugging. If there isn’t a lot of information about this user on either site, then perhaps you don’t want to request debugging for this user.
    *   Looking at statistical distribution seems like an interesting data analysis problem, but one you can look at the frequency with which you call the API.
*   Julien
    *   Would need to look to see if this could work or not.
*   Michael
    *   Thinking about how to use this tool given the strange hysteresis
*   Fabian
    *   Choose from our side exceptional conditions on when to call the API. But this is not always possible. What I need to know is how important is _this_ error. Call for both error cases, but also for success cases. Can’t only call for exceptional cases, or else I’ll miss things. I can’t just know that an error happens, but what percent of my revenue.
*   Michael
    *   I recommend you use the aggregate reporting API to find out what happens in broad sweeping terms and then use the forDebuggingOnly API to get more information for that.
*   Alonso
    *   This API assumes that you already know about the error, the sampling you need, and this gives you the detail you need to solve it. Gives you details on what happens in the auction or the bid.
*   Fabian
    *   Does the private aggregation API tell you about exceptions?
*   David Dabbs
    *   If I get this right, you’re actively working on two things
    *   A pre-flight assessment of what the change to forDebuggingOnly would be
    *   Some new service for an affordance for creative scanning, let people know about winning creative templates
    *   Do those have ChromeStatus features or tracking bugs in monorail, or are you going to produce something and post it as an issue? Or will it appear born anew at some point?
*   Alonso
    *   We always have the intent to ships.
    *   We also have a Privacy Sandbox quarterly published by our TPMs.
*   David
    *   You were talking about the current API for forDebuggingOnly. The new proposed sampling thickens at 3PCD?
*   Alonso
    *   Yes.
*   Brian
    *   Are there plans for global reporting for how often these APIs are being called?
*   Alonso
    *   I don’t know; is there a global use counter on this?
*   Paul
    *   I don’t think so, but part of what you can report in private aggregation can show if you’re in cooldown or 
*   Patrick
    *   In the chat - reading documentation on SharedStorage. Is this available for ads?
*   Michael
    *   Private aggregation is a great tool for all of your needs.
*   Patrick
    *   Does it need to be a Fenced Frame?
*   Michael
    *   Nope, you can use it anywhere.

**[Alonso] Upcoming FR Release communications:**



*   At the very least, an Intent 2 Ship message for all upcoming FRs
*   For features across the Privacy Sandbox we are also running a quarterly refresh of recently released and upcoming FRs here (working on finishing the Q4 2023 refresh right now): https://developers.google.com/privacy-sandbox/overview/status 
*   Changes to the explainers’ relevant sections 
