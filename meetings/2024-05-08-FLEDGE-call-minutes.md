# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday May 8, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	 



1. Michael Kleber (Google Privacy Sandbox)
2. Roni Gordon (Index Exchange)
3. Brian May (Dstillery)
4. Sven May (Google Privacy Sandbox)
5. Kevin Lee (Google Privacy Sandbox)
6. Don Marti (Raptive)
7. Matthew Atkinson (Samsung)
8. Alex Cone (Google Privacy Sandbox)
9. Paul Jensen (Google Privacy Sandbox)
10. Caleb Raitto (Google Chrome)
11. Orr Bernstein (Google Privacy Sandbox)
12. Andrew Pascoe (NextRoll)
13. Laura Morinigo (Samsung)
14. Wendell Baker (Yahoo)
15. Youssef Bourouphael (Google Privacy Sandbox)
16. Pawel Ruchaj (Audigent)
17. Matt Menke (Google Chrome)
18. Isaac Foster (MSFT Ads)
19. Laurentiu Badea (OpenX)
20. Brian Schmidt (OpenX)
21. Paul Spadaccini (Flashtalking)
22. Kenneth Kharma (OpenX)
23. Arthur Coleman (IDPrivacy/ThinkMedium)
24. Jacob Goldman (Google Ad Manager)
25. Tamara Yaeger (BidSwitch)
26. Shafir Uddin (Raptive)
27. Taranjit Singh (Jivox)
28. Jason “Excellent time” Lydon (MO)
29. Matt Davies (Bidswitch | Criteo) 
30. David Dabbs (Epsilon)
31. Garrett McGrath (Magnite)
32. Aymeric Le Corre (Lucead)
33. Yanush Piskevich (MSFT ads)
34. Sathish Manickam (Google Privacy Sandbox)
35. Jeroune Rhodes (Google Privacy Sandbox) 
36. Abishai Gray (Google Privacy Sandbox)
37. Risako Hamano (LY Corporation)
38. Nick Llerandi (Kargo)
39. Felipe Gutierrez (MSFT Ads)
40. Courtney Johnson (Privacy Sandbox)
41. Leeron Israel (Privacy Sandbox)
42. Yanay Zimran(Start.io)
43. Sid Sahoo (Google Chrome)


## Note taker: Tamara Yaeger


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here:



*   Isaac Foster:
    *   Patch’y Updates When on Joining Origin: https://github.com/WICG/turtledove/issues/1162
    *   Any guesstimates on XOrigin would be quite helpful, asking for a friend (https://github.com/WICG/turtledove/issues/813)
    *   Attestation Process  https://github.com/privacysandbox/attestation/issues/53 
    *   Brief revisit the “coarse information sharing” thing, we had talked about setting up time but never did, all got too busy…can even answer here
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Jacob Goldman
    *   Multi-seller component auction input validation: https://github.com/WICG/turtledove/issues/1168 
*   Warren Fernandes
    *   Tweak to the signalValue returned by aggregate reporting functions to support cross-component auction loss reporting: https://github.com/WICG/turtledove/issues/1161
    *   Scope reductions to the analytics entity proposal discussed last week: https://github.com/WICG/turtledove/issues/1115
*   Roni Gordon - https://github.com/WICG/turtledove/pull/1156 - cross-site TSS fetches
    *   Looking to better understand the performance penalty


# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.


# Notes


## Isaac (MSFT Ads): Updates on 813? (https://github.com/WICG/turtledove/issues/813)

Paul (Chrome): Implementation landed a day or 2 ago under a flag in Canary.

Isaac: Can you help understand Chrome process of Canary → 120x?

Paul: Looks like right now you can pass flag to Chrome and try it out yourself. We can put directions in issue 813 how to do that. Next steps are Blink release process, documented on dev.chromium.org . Need to figure out how to release this to get turned on in Canary, either 50% or 100%. 2-4 weeks. Then Beta 2-4 weeks. At last Chrome stable 1-4%, eventually get 100% stable. Testable w flag today, later testable in Canary w/out flag. 

Michael (Chrome): Moving new features forward thru this path is often gated on features getting used, and being able to look and see that someone is trying it out and whether it’s crashing. It’s easier to understand features that are getting used. 

David (Epsilon): As component sellers, not in position to query feature capabilities to know to try to enable it. Am I capturing that Roni? We’re trying to sort out in ad tech how to plumb the pub probing the feature to tell component sellers to use it to test it. This may inhibit our ability to test.

Roni (Index): Don’t know if I can build auction config the same way. Until it’s always active in some minimum version of Chrome for the large percent of Chrome it can’t be enabled at scale because it won’t work. I can test on my own with a flag, which we will do, but to Michael’s point, my one request is not the sanity check you’re looking for :]

Paul: I’ll add the feature detection document: https://github.com/WICG/turtledove/blob/main/PA\_Feature\_Detecting.md. For 813 we did add extension to feat. detection mechanism. 

David: For component seller we have to figure out how to do it on their behalf. 

Roni: I don’t have code on page to build auction configs. There is way to know through JS but that’s too late to do reliably on scale. 

Michael: If it requires new code in prebid, then requires updates by pubs, understand it is a difficult process, so we may rely on someone who does have code on page to do testing necessary to ramp up availability of feature. Then it will be widely available to those who do not have code on page to do feature detection directly. This is a sticky point for parties building the config who don’t touch browser directly. 

Paul: We added feature for PA where you can query support. I suspect we can expose fields, maybe iterate through JS.

Michael: So prebid could change their code once to get the features that are available and send that along, instead of having to change their code every time to look for that feature. If this is an important blocker, please file github issue. 

Paul: Call query feature w wildcard

David: Discussed in 156

Brian (Distillery): Easily accessible and consistently queried ; doing feature detection is complex process, can we get something to pull string out of browser?

Roni: The other aspect is, it would be nice if we’re aware of the fact that we’re building these things in a seq auction setup and don’t have power of JS available to us. Happy to wait until end (I did drop comment), but I don’t know if now’s the best time to dig into that. 

Isaac: This has gone from plug to discussion and can give up my next spot and get in later. Since it’s a discussion, might as well finish it. On the cross-origin stuff, currently in Canary. Need testing in Canary. Sounds like it would be 4-8 weeks to his stable, at which point it’d be behind flag, then go to 50% then 100%. Imagine before it is fully available to IGs, we’re looking at 3mos. 

Paul: I think it’s faster, probably branch point middle of month. Right now, would reach stable on Jun 11th, that would be behind a flag. We’ll start our release process asap also, it’s likely we can’t ramp out w – ready Blink release process, to get feature default there is a process – can’t do between now and Monday. Will likely have to go the more cautious release plan. I don’t think 3mos assuming things go smoothly.

Michael: Roni, you have more about this topic? Is this https://github.com/WICG/turtledove/pull/1156 your issue?
 
Roni: Yes, it’s the explainer for 813. I want to understand, it calls out there about that the trusting scoring signals fetch weight for that response header. Makes sense first time, unclear if penalty is on every auction, every fetch, every whatever. What does it mean?

David: For bidding and scoring, if there could be carveout for subdomain. If it’s completely diff domain, but if same control seller / subdomain, why not?

Roni: Let’s pretend I’m on my decision logic, my seller is sub-1, and my trusted scoring is on sub-2, I understand if we need to authorize it. But for every auction? Every browser? How does it persist?

Paul: A few things– this is something we thought alot about when I designed this; this requirement is specific to the trusted scoring signal. That is due to trusted scoring URL being provided in auction config, and we don’t have any origin reqs on who requires that auction config. We don’t want to add more iFrame reqs. We had to add this dependency for sellers to check this header that gives permission to fetched trusted scoring signal from diff domain. I was skeptical but convinced myself it’s not an issue, for actual latency to result, would mean that we had somehow to get header response from static resource, that’s racing w fetching the bidding script and getting the body response, parsing, creating JS context, executing—- and then realizing we need to score it. Includes setting up process to run bidding script and fetch. I suspect it’s rare that interlock will result in actual delays. Racing against a lot more, may not intro actual latency. But to the question of can we re-use that? It’s similar question to when can we use http cache? That’s something PSB is focused on, we have to be careful about cross-site identifiers. Requests initiated from one site, we have to be careful about reusing. Likely we can’t cache across sites.

Paul (con't): To David’s question, can we trust more from same site? Typically answer is no– there are generally only trusted web securities boundaries origin unless there is attestation from both sides to trust both sides. 

Roni: So you’re saying, is that since the response header is waiting for is supposedly on decision logic url, presumably already cached, … 

Michael: It’s not that it’s already cached, it’s requested early in the flow and a long time before we need to make actual trusted signals fetch that it unlocks. 

Paul: Keep in mind probably most of the trusted scoring signals latency is not from the bandwidth; we can initiate those things ahead of time, a pre-connect, to the trusted scoring signal URL ahead of time since we’re not sharing any info not already on pub page.

David: Pre-DNS but doing it implicitly?

Paul: We already connect the bidder / seller scripts and hopefully scoring signals as well. We try to preconnect as much as feasible. We can do it in parallel to bidding or process creation. 

Roni: It would be great if the context is provided on the doc, I heard that we’re delaying the fetch and it sounded like that meant delaying everything connection to origin. But it’s the HTTP request, not establishing the actual connection between user agency and origin we need to connect to.

David: yes that doc has been helpful to understand what’s going on.

Paul: Will add it

Brian: Isaac’s question about availability in feature, what should we anticipate in stand-still period? We’re almost at end of testing period, which means 8ish months before stand-still period has been gone through.

Michael: Can’t announce anything in this call, check privacy sandbox website :]

Isaac: I understood Brian’s questions more about these types of things that aren’t changing the API, is that included in the stand-still? I assume it is. 

Brian: We’re all racing to get this implemented, and there are helpful features being added, if stand-still means we can’t count on them it will slow everything down.

Michael: Sorry I misunderstood.  The standstill has nothing to do with pausing Chrome’s addition of new features to PSB, in fact the end of CMA testing period will make it easier to add additional features to the various privacy sandbox API. We have tried to be cautious to add new features during CMA test to Mode A and Mode B traffic so we don’t mess with the test. Once it’s done, features will roll out on all Chrome traffic. Standstill may be somewhat misleading, it refers to the period during which Google has said we want to get rid of 3P cookies and CMA has asked for a period of time 60-120 days when we pause before we actually start getting rid of cookies. Standstill only applies to the process of cookie removal, it does not have implications for ongoing feature development for PSB. 

David: So we’re inferring stepping on gas of feature dev. 


## Jacob Goldman: Multi-seller component auction input validation: https://github.com/WICG/turtledove/issues/1168 

Jacob (Google Ads): I just wanted to bring up auction config issue - - we had a bug that was fixed quickly in component seller, malformed auction configs that would crash – while that bug was quickly fixed, were there mechanisms to protect top level seller and all other component sellers when there are malformed inputs? 
 
David: Two aspects– (1) how to fix, (2) feedback / visibility to a broken seller. 

Jacob: Yes, two diff aspects there. Chrome could ignore broken component auction and continue as usual, but there is no way to instrument browser warnings. You can come up with a few different mechanisms to get feedback. Lifting API that top level seller can invoke and write code to ping back entities that are broken and interface over JS would be one option, we can think of other reporting options. Maybe there is a report 2 style angle where component sellers can add error feedback endpoint to their configs. 

David: Pubs will have most to say. I would think pub wants to get some auction going, even if an engine is out on component seller auction. 

Jacob: Agreed

Paul: Putting on API designer hat, I don’t want API to silently fail or cover up failure. I’d rather it throw an exception so alarms are raised. Thinking about auction configs, we’ve had this weird case where there were different people submitting different pieces, in a perfect world browser tells them what’s going on. We can design a lot but it’s not easy to give feedback from an API. There are basic JS exceptions; inside string / type error that points out what is wrong, it may not be easy to parse at run-time. There is a bunch of middling solutions that aren’t great. We can continue on, running the auction. From API pov exceptions may get ignored, like you would other error codes. We can try reporting mechanisms, but network fails or sometimes they break. We could change return value of runAdAuction to be a sub-type error with the component auction config, but it gets quite complicated. It can be implemented now by call-running auction multiple times: If it fails, you search by calling again and again with the different component configs, and see which one is causing the problem. We could add a dry-run to check if the auction config works, but actually there are already ways you can nullify auction config so that it doesn’t actually have an effect, like removing buyer list. I like the critical path of when everyone is submitting good auction configs to work efficiently. As a top level seller, if that first auction doesn’t work I can fall back to slower logic to figure out which part is wrong. That way the main path would keep working quickly like today. My question is, what is the blast radius of something like this? Does it affect one pub, or every pub with this component seller?

Brian: I think this is the type of error that can take out subsets of pub inventory for as long as it takes the error-creator to figure it out. It’ll be a distributed failure.

Paul: Is there any way to make it less of a distributed error?

Michael: May be a question for us– should Chrome implement severability to preserve the auctions that are running ok? While the broken one is trying to fix themselves. Or do we rely on top-level seller to detect which component is bad? Maybe we could support a severability flag, so that top-level seller could run auction once and see error result and then try running a second time with the severability flag. Then Chrome can let the rest run. I like the idea of a dry-run flag to get reporting.  There is a problem with  the idea of the severability flag though: people might just use the flag all the time, and not be aware of a failure, and then some auction config is broken and parties who have JS on the page ignore it; then the entity making the mistake doesn’t find out.

Paul: Problem is when 1% of 1% of pages don’t notice and everyone gets hurt by that.

Michael: Hmm, we could make the severability flag to only work the 2nd time. Might be too bossy of the browser.

Brain: Seems there is fairly high degree of variability across PA based on interest groups and pub / seller relationship. This means it can crop up intermittently across diff scenarios. I’d be uncomfortable running my biz on this. If there was some way to isolate and report when it happens, it would be preferable model. 

Michael: The kind of errors are not based on IG availability in browser. This is about auction config to judge good or bad while avoiding sensitive info leaking. That's good, it means we can be more candid about what went wrong when there is no privacy leakage.

David: Is this an accurate playback; if it’s lint-free it just runs the auction? It throws the exception and you get a clean config? Or you just get a….

Michael: We haven’t designed it yet. We could try to design something, makes sense if the browser helps you figure out what / when went wrong. Avoiding silent failure matters also, you end up with problems that never get fixed.

Roni: 2 weeks ago we were talking about reporting API for exceptions. In this particular situation, if some aspect of a component seller's config isn’t correct, how would the component seller know it’s not correct? That loopback is very important for feedback.

David: Much of this will be in the hands of whoever the top level seller is, which isn’t always the pub. 

Michael: I think there is a large design space in helping educate about validation problems with the input. Looks like there are additional ideas, we’ll have to go back and think about implementation. One question I’d like to hear feedback on is what are error paths today when someone does something messed up, as part of participating in an auction?  What are well-established lines of comms and info flow about the fact that something is broken? It’d be great if we can do what’s familiar today instead of trying to invent a new comms channel. There’s many ways we can hope to make info available. What is most convenient for you? Certainly there’s lots of ways browser can invent new things. Greenfield development has lots of possibilities and risks. If we’re talking about a config object that is very malformed, any reporting also might not be available because the part that tells the browser where to send the report could be malformed also. The ways in which problems are learned about today are probably more tired and true than something new. 

Brian: One of the basic ways is to be in comms w other parties, if other party doesn’t get back what we expect from them, in browser we don’t get opportunity to see that.

Michael: What about in Prebid, if an SSP's response in on-device request that came from prebid JS, how do they learn if it is malformed? 

David: Varies by SSP. When we experimented we screwed up subtlety of OpenRTB no bid response; 

Michael: I withdraw my point about reusing existing channels, if the answer is that this all fails silently today :]

Jacob: Most actors on page in pub context should have some JS error system, this how we were able to detect overall run-ad auctions. Problem is other actors, don’t know if there is enough interoperability to make sure one vendor can find everyone else. Maybe individual sellers specifying endpoints, if that config can make it through, it would be a good browser-vendor channel.

Michael: Browser to vendor is tricky is because the thing that could be malformed may have info that’s needed.

Roni: If the auction config is so broken…

Michael: All that needs to happen is to miss replacing a quote but a double quote

David: The linting that Jacob found a particular well-formed value was not right.

Roni: It’s more about the noisy neighbor problem.

Brian: I would say we generally use multi strategies to monitor things.

Michael: Will work on getting error reporting more feasible and accessible. Expect to talk about it again in the future. 
