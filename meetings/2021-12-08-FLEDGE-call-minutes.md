# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 4pm UTC (during winter).

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Dec 8 2021

**_No meeting Dec 22 — following meeting will be Jan 5, 2022_**


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Don Marti (CafeMedia)
3. Brain May (dstillery)
4. Christa Dickson (Dotdash Meredith)
5. Tom Houriez (PEReN)
6. Paul Jensen (Google Chrome)
7. Supraja Sekhar (Google Adx)
8. Fred Bastello (Index Exchange)
9. Isaac Schechtman (Iponweb) 
10. Stan Belov (Google Ads)
11. Ryan Arnold (P&G)
12. Sergey Tumakha (Microsoft Ads)
13. Erik Anderson (Microsoft Edge)
14. Bartosz Marcinkowski (RTB House)
15. Bartosz Łoś (RTB House)
16. Jeff Kaufman (Google Ads)
17. Jonasz Pamuła (RTB House)
18. Martin Gruau (Captify)
19. Aleksei Gorbushin (Walmart)
20. Wendell Baker (Yahoo)
21. Tamara Yaeger (IPONWEB)
22. Yuval Tanny (Oracle Advertising)
23. Sven May (Google)
24. Joel Meyer (OpenX)
25. Irlinta Mavromati Terzopoulou (Google Chrome)
26. Andrew Pascoe (NextRoll)
27. Richard Anton (Amazon)
28. Aditya Desai (Google)
29. Przemyslaw Iwanczak (RTB House)
30. Sam White (LiveRamp)
31. Brad Rodriguez (Magnite)


## Note taker: Sergey Tumakha


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

 		


## [Suggest agenda items here]



*   (Paul Jensen) Discuss FLEDGE worklet language needs
*   (Supraja Sekhar) Losing bids reporting for sellers ([#218](https://github.com/WICG/turtledove/issues/218))
*   (Supraja Sekhar) Reporting functions in case of Ads Composed of Multiple Pieces ([#153](https://github.com/WICG/turtledove/issues/153))


## Notes



*   (Paul Jensen) Discuss FLEDGE worklet language needs

Paul Jensen: performance of worklets when doing scoring. JS. Ways to improve. Some of the browser's performance issues have to do with isolation. Need to parse, compile bidding code, which comes from sources the browser might not trust. We run untrusted code in a different process: if untrusted code finds an exploit to run the code that is not JS, the browser needs to protect against it. This is a trade off. By choosing JS, we make a trade off, choosing more flexibility but at a cost of performance. We need to push ML model though. Two dimensional arrays of floats should be parsed safely. JS has isolation requirements, but if all you need is matrix multiply, it does not have that same risk, could run it much faster.  Question: Is there a happy medium on isolation? Hybrid model? Do folks just want to do matrix multiplies or more? Any other efficient ways to do it?

Jeff Kaufman: matrix multiply. Something else like checking a few bits before. Worried about client side performance

Paul J: Declarative and matrix multiply methods ?

Jonasz Pamula: we have a lot of code beyond matrix mult. If we replace, we have to evaluate if feasible. Also, if we interleave JS and Matrix operations, we might need to do a lot of context switches, to fully evaluate an ML model. There will be calls to matrix multiplication. 

Paul J: Support of WebNN ML 

Brian May: We may be getting ahead of ourselves by looking at this as we’re still figuring out the overall models. Maybe meaningful to provide a Real time and a background resource where the first was at bid-time and the latter could precompute values between bids.

Andrew P: there is a lot of code that requires multiple math ops. Embedded ML framework is hard, but would be very beneficial. Even WebNN doesn’t cover all use cases; not everyone uses NNs. A lot more than just matrix operations: parsing data, vectorizing to return bits quickly. Decision trees before NN is a technique seen in literature, may not be easily linearized.

MK: taking raw signal and vectorizing may be on server side. Either at the time user is added to the interest group or contextual call

Andrew P: however, frequency, recency etc will need to happen on browser, so not all vectorization can happen server-side.

MK: Anything else on this topic? Thank you

Next topic: 



*   (Supraja Sekhar) Losing bids reporting for sellers ([#218](https://github.com/WICG/turtledove/issues/218))
*   (Supraja Sekhar) Reporting functions in case of Ads Composed of Multiple Pieces ([#153](https://github.com/WICG/turtledove/issues/153))

Supraja: two issues. Report loss functionality. Creative scanning. Set of creatives that were disapproved. Previously we thought API can help. This might not scale. 1. Buyers need to integrate with similar APIs provided by every seller they work with - adds friction 2. Sellers need to do sampling to accommodate resource constraints in their scanning system. Thoughts on loss of functionality.

KB:  when we are talking about the info we have at the time seller is evaluating the bids, we have to control the flow or we will reveal browsing history. Example you mentioned with creatives that were .. not strongly tied to the information that . Tried to support . Allowing reporting the info that comes from one source is appealing. If mult sources d-dangerous

SS: what the report loss might be .. might need to mention

MK: all info comes from single context .. choice . need to be careful, can influence set of choices

SS: association between render URL and add component URL. rather separated . gives us a mechanism that scans a subset of products per ad template..

MK: join top level ad and component .. is the event level reporting we can do. But if you look at the whole set, it might be too much information

Stan B: question - how contextual information is influencing . win reports as well. Here info about render URL. One more - it might be interesting if minimal form of loss reporting functionality existed in the origin trial, as it may influence how easy (or not) it would be for sellers and buyers to work together on ad quality check, and whether some sort of special creative submission API would be needed

 MK: we are looking to make things available as soon as we can

Brian: maybe have some level of entropy allowed, a reporting budget. Report x number of fields and do not go above a certain level of specificity until there is sufficient volume to maintain anonymity. For example, just report by campaign ID at the start, add additional values, such as contextual data points as campaign matures. Would allow for some depth of understanding on low volumes and more detailed insights as volume increased.

MK: adaptive and hierarchical. Can help but adds complexity. Flexibility vs complexity. Bunch of ideas that we need to ..

Brian M: Would be of value to allow people to indicate whether they need detailed level reporting or are OK with just getting high-level counts.

SS: Report loss. Component render URL vs ad comp url

MK: no exact explanation in the origins trial yet. Report . This gets into difficult territory as for exposing info. Exposing renderUrl and adComponentRenderUrl pairs might be possible whereas renderUrl and the entire set of adComponentRenderUrls may not work during event level reporting due to privacy concerns.

MK: anything else? Meet again on Jan 5
