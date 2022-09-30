# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Sept 28, 2022

(No meeting Sept 14 because of W3C TPAC)


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Brian May (dstillery)
3. Paul Jensen (Google Chrome)
4. Caleb Raitto (Google Chrome)
5. Fabian Höring (Criteo)
6. Russ Hamilton (Google Chrome)
7. Sven May (Google Chrome)
8. Stan Belov (Google Ads)
9. Andrew Aikens (TripleLift)
10. Sid Sahoo (Google Chrome)
11. Charlie Harrison (Google Chrome)
12. Sasha Gavrilov (LinkedIn)
13. Alex Cone (IAB Tech Lab)
14. Andrew Pascoe (NextRoll)
15. Joel Pfeiffer (Microsoft)
16. Anthony Molinaro (OpenX)
17. Martin Pal (Google Chrome)
18. Jonasz Pamuła (RTB House)
19. Sangharsh Kamble (Walmart)
20. John Mooring (Microsoft) 
21. Supraja Sekhar (Google Ads)
22. Matt Menke (Google Chrome)
23. Joel Meyer (OpenX)
24. Bartosz Marcinkowski (RTB House)
25. Sergey Tumakha (Microsoft Ads)
26. Phil Lee (Google Chrome)
27. Shivani Sharma (Google Chrome)
28. Zheng Wei (Google Ads)


## Note takers: Charlie Harrison & Martin Pal


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here]



*   [Martin Pal (Chrome): Discussion of Data-Version (context: issue [146](https://github.com/WICG/turtledove/issues/146), [slides](https://docs.google.com/presentation/d/1QpDQVxWcjIP4mK9Sf9VXMC4tObPLNFpHxhSnecXkWtw/edit?usp=sharing))
*   Charlie Harrison (Chrome): Aggregatable ARA reports and FLEDGE ([issue 289](https://github.com/WICG/turtledove/issues/289), [slides](https://docs.google.com/presentation/d/1uFNyCBXDEihkqSdSXIeNSQxeYLq-2jnoNZ9IdfEJS7s/edit?usp=sharing))


## Notes



*   Kleber: We all met at W3C TPAC in Vancouver 2 weeks ago. Thanks to all the people who joined us there in person or remotely. There was a get-together specifically discussing FLEDGE + PARAKEET. I think most of the people who come to FLEDGE call regularly were there, so won’t do a recap. The notes from that meeting are available through W3C and there’s a copy in our repo in the usual meetings directory. If you want to see the notes, here is a link  https://github.com/WICG/turtledove/blob/main/meetings/2022-09-16-TPAC-Vancouver-FLEDGE-PARAKEET-Breakout.md
*   It was a great chat and gave me a lot of hope for the prospects of the FLEDGE + PARAKEET converging in the not-too-distant future.  We talked through the small remaining # of differences, and heading towards a path to convergence.

**[Martin Pal (Chrome): Discussion of Data-Version (context: issue [146](https://github.com/WICG/turtledove/issues/146), [slides](https://docs.google.com/presentation/d/1QpDQVxWcjIP4mK9Sf9VXMC4tObPLNFpHxhSnecXkWtw/edit?usp=sharing))**



*   Wanted to talk about data versioning in the FLEDGE KV server and summarize where we stand, my understanding of what’s roughly needed for and what the privacy risks are, where we can go from here.
*   What is Data-Version?
    *   We have the KV-server which holds some data, required to run for both bidding logic (buyer) as well as KV server for the seller to run the auction
    *   Idea of the Data-Version field is that it represents the state of the KV server at a given time (loaded Dataset #17 into the KV server), so everyone that looks up knows its from version 17.
    *   Important property: should not depend on what the browsing is asking. Don’t want evil KV servers to echo data from the interest group back into the Data-Version field
    *   It’s a number. It’s in the browserSignals object, available to generateBid() and buyer/seller reporting functions
*   Use-cases for Data-Version
    *   Monitor data freshness. 
    *   I want it to be included in events coming from fledge (event-level reports). If I’m a buy, I’m getting reports saying it was done with version #17. Know I loaded #18 yesterday, cause for concern and need to debug. Can monitor data push latency
    *   More important use-case, is joining event-level reports with KV-server data. Let’s say I have an event-level report of a win, I know the renderURL, but the report is restricted in information. Could do lookups in the KV-dataset (e.g. budgeting information, etc.), which $$ to pay for this impression.
    *   Could do a lookup and say which budget to use for this URL. Important use-case for billing, and troubleshooting, e.g. want to simulate my bidding logic post-fact and see if it aligns with what happened.
    *   In both of these use-cases, want to link the event-level data with the KV server
    *   Data-Version could be used as a join key
*   Privacy risks
    *   If we have an arbitrary server that implements KV interface, no guarantee that Data-Version represents the data, instead of some information retrieved from the interest group. Lots of data can be packed in a 64 bit integer.
    *   Want to think about the privacy abuse here
*   Show of hands, how important is data-version?
    *   A few hands raise (Brian May, Jonasz Pamula, John Mooring)
*   How important is 64 bits? OK with lower resolution?
*   Brian May: Hard to say exactly what the landscape will look like when this gets operationalized. Knowing what version of data the browser was working from is going to be useful. In terms of the size of Version number, 64 bits way bigger than what we need. Reasonable to assume something considerably smaller, and increment with a rollover.
*   Jonasz Pamuta: Not at a point where we would be using this but the use-cases are definitely useful. Is this global or is there a separate data-version per each key?
*   Martin: Excellent question. The way FLEDGE talks about this right now is a single number that gets returned in the header from KV server. There are reasons why you might want this for a specific key, but tricky thing is privacy, how to support without violating user-privacy. Some technical proposals that could address this but require a trusted server. BYOS is harder but trusted server is more tractable.
*   Jonasz: How will this work with user-defined functions?
*   Martin: BYOS you can implement whatever you want. For trusted KV server you can still have a global data version number that evolves over time. I think data-version is compatible with UDFs. Per-key version we would need to discuss details
*   Brian May: If we have a single global version it will be considerably less useful, and constrain what things we can do in FLEDGE simultaneously, how many different campaigns we can run / know what’s running. We could allocate a budget of different version numbers that somebody can have so that the compound version # doesn’t give away too much information. We might need a separate version # to know what code is running in a UDF environment.
*   Sasha Gavrilov: I think a single global version is sufficient. If you represent changes into the store as a stream of events and use version as a pointer in history. Same way as streaming technologies like Kafka. It’s a little more expensive but the data is here, it’s sufficient. Ideally it should be there for any diagnostic debugging / maybe even training purposes
*   John Mooring: Issue 356 in FLEDGE repo, request to add cost to what’s available in reporting. That would be a very useful feature here. RIght now what we would use the version header is recomputing the ad cost we computed inside generateBid. Also has this limitation that we can’t incorporate browser signals like join count or previous win.
*   Martin: I’ll acknowledge the issue. I don’t personally have a comment. It’s an issue we’re aware of and discussing solutions
*   John Mooring: Thank you
*   Martin: coming back to Data-Version. Right now it’s a 64 bit integer and one way to mitigate privacy harm is to enforce it is slowly incremented. When we looked at this, implementing “slow incrementing” is hard, don’t know how to do it. Some kind of prober technique, if a KV server returns a wide range of versions at a time, it indicates something fishy. Making this concrete would be difficult. I am arguing against this option. If we decide this type of enforcement is not feasible, need to look at other options.
*   Sasha Gavrilov: Trying to understand reasoning. Say we fix the trusted server URL, it means that it has the global version, it cannot link any information specific to the interest group, it is just monotonically incremented.
*   Martin: Draw a distinction between BYOS and trusted server. BYOS could have a malicious actor echo information to the interest group. In an environment where we have to deal with that, it is not feasible to have enforcement. Let’s split out this into “have trusted servers” and “do not have trusted servers”. The first couple of items here are for the untrusted server. While we have untrusted servers maybe we should reduce the entropy of the version # (e.g. 8 bits), or a timestamp (which can be skewed between the current time - generated vs. server push). Maybe we can think of it like a unix timestamp rounded to whole minutes
*   Brian May: Can you clarify where the timestamp is published?
*   Martin: If you run a KV server and wish to use Data-Version that looks like a timestamp. I am happy with that but would like logic in the browser that says it has to be rounded / truncated to be near the current time (reduce the bits).
*   Brian May: have you considered adding a mediator between the version increment request and KV store
*   Martin: Can’t say we have seriously considered this. An answer you might be looking for is the ability to use a trusted server. Once we have a trusted kv server many of these restrictions can go away. Once we have a server where we trust the data version we can be much more relaxed about the value
*   Brian May: I was thinking about a server we communicate with that is trusted to do the right thing, and that server communicates with untrusted KV store. Sensitive actions like incremented data version goes through trusted server
*   Martin: E.g. trusted server gives you a signed certificate on a timestamp, something like that?
*   Brian: Something like that, something that enforces version # isn’t incremented at an unacceptable rate
*   Martin: Sounds doable but complicated. We wnat to enforce a limit on the total possible # of version numbers in circulation. E.g. 256 for the privacy params I am thinking of.
*   Sasha Gavrilov: Time wise what should happen first. 3p cookie got removed from browser or BYOS moves to trusted environment
*   Martin: Good question, my expectation is that BYOS might last past 3p deprecation. That’s my thinking
*   Sasha Gavrilov: Otherwise maybe there is no problem.
*   Martin: Yes while 3p cookies are around you can use 3p cookies. This would be for the setting where we are still operating untrusted servers without 3p cookies
*   Sasha: How long? 
*   Michael Kleber: Want to make things as private as possible. Answer for how soon to migrate entire ads ecosystem to something new (trusted server inside TEE) is hard to predict, don’t know exactly what the timeline will be about when we have a requirement to use trusted server, want something reasonable before that
*   Martin: Want to draw distinction between requiring a trusted server, and making a trusted server available. Likely we will make a version of trusted server before 3p deprecation time
*   Sasha: In this future is it possible to avoid limitations for trusted server than then have different requirements for BYOS.
*   Martin: Yes I would imagine you might be able to get better resolution information from a trusted server
*   Supraja Sekhar: Similar to Sasha, trusted servers are trusted. Why the data version is any different that all the other things we trust BYOS server to do
*   Martin: I would call this trust but verify. BYOS we have no real way to enforce some things. For some sensitive pieces of data we are choosing to be more cautious and restrictive
*   Martin: Last slide, so I’m proposing that for untrusted KV servers we are looking at reducing entropy of the data version, 8 bits.
    *   8 bit integer or unix timestamp scheme two main options
*   Once we move to trusted server, it becomes much easier, can verify Data-Version response does not depend on request, can be much more permissive. If you want a structured response, that’s also not out of the question.

**Charlie Harrison (Chrome): Aggregatable ARA reports and FLEDGE ([issue 289](https://github.com/WICG/turtledove/issues/289), [slides](https://docs.google.com/presentation/d/1uFNyCBXDEihkqSdSXIeNSQxeYLq-2jnoNZ9IdfEJS7s/edit?usp=sharing))**



*   Integrating attribution reporting api and fledge reporting. Goal is aggregatable reports. We identified a mismatch between information in fledge and what the existing APIs allow to report.
*   Main problem: Normal behavior of ARA is about joining 2 contexts: source (impression) and trigger (conversion). With FLEDGE, we have 3 contexts: pub level top level page, fenced frame with the ad, user behavior (viewability, click, ..) trigger context (conversion)
*   ARA is configured with HTTP network response headers. To conform with ecosystem, redirects, to avoid having to re-tag. 
*   In FLEDGE, environments have extra information that can't be easily revealed to a server. We need a different approach. Keep good parts of the network response API: security aspect (ping adtech server, get header response, know identity of the origin)
*   Strawman solution: move away from network-based registration, at least for fledge. Move the heavy lift report generation logic in generateBid. This way, the buyer is in charge of both bid computation and report data generation.
*   generateBid can design whatever data it wants to put in the report, if event happens. "Pending source registration". The report will be fired if an event (say engaged view or click) is fired. May be possible to register multiple "pending sources".
*   Binary signal (did source fire?) comes from the fenced frame. If FF dies, all unfired pending sources are discarded.
*   In some cases the fenced frame is not 100% trusted. In that case FF should not be allowed to modify contents of the report, only decide whether to fire. 
*   Tension between trust in FF context vs expressive power. How often it happens that the FF will be untrusted? Sounds prudent to have buyer in control of what gets sent.
*   Potentially the trigger event can be triggered by browser itself rather than the FF
*   Open question: how to support 3p reporting (entities other than the buyer receiving reports). Any design seems to require huge coordination effort between buyer and the 3p reporting entity. 
*   Pixels not going to cut it.
*   Jonasz Pamula: for RTB house, this looks great, and seems it will satisfy our use cases. I have 2 technical questions: (a) how many pending sources can we register -- is 20-40 ok? We may want to use custom names for the pending sources.
*   Abuse vector: register event from the FF: haven't thought about this much.
*   Charlie: 20-40 source types: this looks very non-ergonomic if you need to define 40 of these in js. Need to understand structure, whether this is the right api.
*   Jonasz: I expect redundancy in the 20 sources. Perhaps we will discuss more in GitHub
*   Martin: In ARA, there is the notion that each side should contribute a piece of the key — e.g. how does some feature of the IG correlate with some property of the click.  In that case I might want to generate a key piece from the IG context and a key piece from the click context.  Does that make sense here?
*   Charlie: Does make sense, was something I considered.  The FF could have almost as much power as the other contexts in generating key-pieces.  It felt like overkill to me — this is why I wanted to present this to the group, to see how much information we need from the FF.  Letting the FF do arbitrary bitwise-OR's gives it lots of power to do bad stuff also, and is additional more complicated.  If this design, or small tweaks on this design, satisfy all the use cases, then I would prefer it.  What you said is totally possible, and we can go there if we need to.
*   Shivani: One of the things we have in reportEvent is that the fenced frame has information about user behavior which goes through the click event parameters.  Maybe anti-abuse would be a reason that we would indeed want a channel to get user event data into the aggregation reporting keys.
*   Charlie: I'll ask the anti-abuse folks.
*   Brian May: did you consider separating the 3 contexts: get 3 separate reports, one for each context.
*   Charlie: I did not think this is powerful enough to satisfy use cases. Brian are you saying maybe you don't need to join contexts?
*   Brian: just started thinking; don't know
*   Fabian Hoering: question about convergence of this api, attribution reporting, fenced frame reporting apis. For abuse, I'd ideally want to detect jointly before bid
*   Charlie: yes, we will look into converging theser APIs. In terms of abuse, conversion spam is also an issue
*   Fabian: it would be nice if all apis could handle abuse consistently.
*   Charlie: totally agree; devil is in the details of the privacy model. This should be our north star though
*   Charlie: let's table this for now; hopefully can revisit soon.
