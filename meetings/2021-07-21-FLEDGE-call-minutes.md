# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC.

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday July 21 2021


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Paul Jensen (Google Chrome)
3. Erik Anderson (Microsoft)
4. Mehul Parsna (Microsoft)
5. Brian May (dstillery)
6. Brendan Riordan-Butterworth (IAB Tech Lab / eyeo GmbH)
7. Allen Fung (ShareThis)
8. George London (Upwave)
9. Russ Hamilton (Google Chrome)
10. Cory Underwood (Search Discovery)
11. Julien delhommeau(Xandr)
12. Wendell Baker (Verizon Media)
13. Fred Bastello (Index Exchange)
14. David Tam (Relay42)
15. Tamara Yaeger (IPONWEB)
16. Newton(Magnite)
17. Christa Dickson (Meredith) 
18. Isaac Schechtman (IPONWEB)
19. Nathan Jia (Nielsen)
20. Fabien Benoit (Criteo)
21. Vincent Grosbois(Criteo)
22. Valentino Volonghi (NextRoll)
23. Michael Coward (Arcspire)
24. Joel Meyer (OpenX)
25. Przemyslaw Iwanczak (RTB House)
26. Brad Rodriguez (Magnite)
27. Ryan Arnold (P&G)
28. Paul Farrow (Xandr)
29. Caleb Raitto (Google Chrome)
30. Matt Kendall (Blockthrough)
31. Danny Doyle (hearst)
32. Amol Deshpande (WireWheel)
33. Martin Gruau (Captify)
34. Viraj Awati (Amazon)
35. Andrew Pascoe (NextRoll)
36. Aditya Desai (Google)
37. Paul Marcilhacy (Criteo)
38. Jason Xie (Adobe)
39. Matt Davies (Iponweb) 




## Note taker: Brendan Riordan-Butterworth


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.



[<img src="https://i.insider.com/5fc55f5350e71a00115580e8?width=700" alt="Screenshot showing location of the Raise My Hand button">](https://www.businessinsider.com/how-to-raise-hand-on-google-meet)



# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

 		


## [Suggest agenda items here]



*   @Vincent Grosbois : I’d like to discuss issue 204 about Feature-Policy HTTP-header and potential alternatives https://github.com/WICG/turtledove/issues/204
*   ~~"shallow merge" from https://github.com/WICG/turtledove/pull/185 (if Jonasz and Russ are both here)~~ (nope — will continue on GitHub)

Standing Agenda Item: Open Issues, or Issues that Need Opening

https://github.com/WICG/turtledove/issues?q=is%3Aopen+is%3Aissue+label%3AFLEDGE


## Notes

Michael Kleber: Initial meeting administrivia.  Join WICG, location of notes, hands up for queue.  

MK: Let’s get started on the agenda.

MK: Issue 204 from Vincent


### [Issue 204](https://github.com/WICG/turtledove/issues/204)

Vincent Grosbois: The main issue we’re waiting on is the timeline of FLEDGE and the rollout for that.  I wanted to talk about the issue 204 a bit because it didn’t get much attention.  This is about the system of permission delegation.  For FLEGE there is such a system, intended to work with a new system of HTTP Headers (Feature Policy).  We did survey with diff clients at Criteo.  There are a few clients that can’t really have that much control over their HTTP headers, specifically through high level services like Shopify.  Not that much control, like custom HTTP headers.  

VG: Question is about this - I realize that this behavior of feature policy HTTP headers could be done as well with a file on the server?  Permission delegation done by thing at well-known-URL?  Is it true that this could be done with a static file, like is done with other systems that depend on well-know-URL?  

VG: Could we switch from HTTP Header to well-known-URL?  

MK: Just to make sure that I’m clear about what you’re talking about - the things that rely of feature policy for delegation from FLEDGE is about being able to add people to interest groups?  

VG: Yes, a given domain can delegate permission to different owners.  

MK: Domain A can allow Domain B to run the JS to add a user visiting Domain A to be added to a group by Domain B.  

MK: 2 different scenarios.  Permission 1, expected to be used by publishers, if a visitor is on publisher.com, then publisher.com should be in control of whether users can be added to interest groups, while they are on that site.  This is in response to “audience stealing”, ensuring that site owners have control while the visitor is on the site.  

MK: The model for how that works does involve FEature Policy.  The usage, as far as I remember, is when you create a cross-domain iframe, then adding people to interest groups inside the cross-domain iframe, then it’s only allowed when Feature Policy specifically allows it.  It can be set via HTTP header on top level document, but it can also be set at the time that the iFrame is created (as an HTML attribute).  

MK: So, perhaps that you can do it in HTML addresses the need?  Or there might be additional ways that iFrames might be created where the publisher site having a .well-known file might be useful?  If there’s a need, it does seem fine, and aligns with the publisher being in control.  Adding it as another method of control seems fine.  

VG: Don’t see the HTML one, can get pointed to it?  

MK: Details at [URL](https://developer.mozilla.org/en-US/docs/Web/HTTP/Feature_Policy/Using_Feature_Policy#specifying_your_policy) , the “allow” attribute of “iframe”.  

VG: Questions about JS?

MK: Is Feature Policy not good enough to lock it down?  The way it works is that if a document has permission, then that document can pass it along, using Feature Policy, to iframes.  The top level page has the permission by default, and it can assign it to sub elements when those are included, either in the DOM of the main page, or as allowed iFrames.  Maybe this is too much?  Opinions sought.  

Mehul Parsana: Clarification Question - when you delegate, does it keep going recursively?  

MK: I believe that if an iFrame has the permission delegated, then it has the ability to create deeper iFrames and delegate permissions further.  

MK: There were 2 diff types of delegation.  That was the first.  Delegating to a 3rd party.  The other is a delegation by advertisers who want to be interest group owners (or buyers, or other owners), and who are not explicitly recogized as a first party.  The complication is where there’s an SSP that the pub has deal with, while DSPs want to add people to a list.  The pub doesn’t have a tie to all the DSPs that they might want to give access to.  There is therefore a permission that points in the opposite direction.  The DSP delegates to the SSP permission to add people to an interest group that remains owned by the DSP.  This is permitted through a .well-known resource.  

MK: This is the DSP assigning trust and power to the SSP.  Normally, it’s only the owner of the interest group who can change the membership of the interest group (describing the types of actions taken on an interest group).  

VG: For a given advertiser who wants to go through us, the owner has to be criteo, because all of the endpoints are owned by criteo, and so all the advertisers need to delegate to us?  I’d be interested in people replying on issue 204 to have additional feedback on this.  

Brad Rodriguez: From the SSP side, I can def see publishers being concerned about unauthorized parties adding people to groups.  Having more granularity would be good!   

MK: When permission delegation happens, yes, it should be specific.  This is why there’s 2-way.  The current system does put the control in the hands of the SSP, which mimics the current trust model for the most part.  


### [Issue 185](https://github.com/WICG/turtledove/pull/185)

Michael Kleber: About shallow merging, how to update interest groups at product-level FLEDGE.  Do we have RTB House people here?  

MK: Maybe not?  

Przemyslaw Iwanczak: Cannot speak for the others.  

Russ Hamilton: Discuss further on GitHub.


### Open Items


##### FLEDGE vs PARAKEET

Julien Delhommeau: Thank you.  Wanted to know if there are more detail on timeline for FLEDGE testing?  Especially since there’s need to compare FLEDGE and PARAKEET.  The testing of PARAKEET is messaging for Q3/Q4.  

MK: The quick answer, we don’t have an announced date.  We do expect it to happen this year.  We will announce timeline on privacysandbox.com, which will have the projects under development.  More information soon!  

JD: About the evaluation as to what is better for the industry, is there going to be Google taking a look at which solution and deciding, will WICG have input, how will it happen?  

MK: Yes, this is the place to have the conversation, and now is the conversation.  Also make sure to attend the PARAKEET calls (that alternate weeks), publish blogs, be in the GitHubs.  Give reasons as to why what you want.  One of our goals is to make it as easy as possible to try out both of the APIs, since they’re similar enough, to make is easy for the stuff that happens in the browser easy to experiment.  

MK: Discussion about the tradeoffs, and the opinions on those tradeoffs, is really interesting conversation to the Chrome team.  The main difference, from my perspective: “In the FLEDGE proposal, there is on device bidding based on full fidelity signals (about the user and context) and bids can be based on those signals.  In PARAKEET, bidding doesn’t have to be on the device, can still use server-to-server, but makes a tradeoff in that the signals are no longer full fidelity signals, meaning that the information available server-side is less full fidelity.”

MK: Then there is the MACAW follow-up.  

MK: That’s the difference, on-device with full fidelity vs on-server with less fidelity.  This is a tradeoff, and understanding which is better for who isn’t easy.  

JD: Great, this does feel like the right place to debate, but it hasn’t been clear that decision making hasn’t been happening here.  Would be great if we could have a mode of control, influence.  

MK: Yea, the W3C doesn’t get to be the boss of anyone, the decisions are going to be made at each of the browsers.  The goal isn’t to have one winner and everyone else losing - the goal of W3C is to put out a bunch of ideas, understand the positives of each, and come to a chimera that has the most necessary features.  Every browser seems to want convergence, long term, so figuring out how to make convergence happen is important.  


##### Delegation to READERS

Erik Anderson: We’re looking at what types of changes to make to Chromium to get PARAKEET functionality in.  Not an endorsement, just making it easier for anyone who wants to include things.  

EA: TURTLEDOVE had a “READERS” concept, but doesn’t anymore.  PARAKEET still does, because when the is sent to the ad system, the interest group is encoded with the key associated with the reader.  So an interest group with multiple readers gets encrypted multiple times, so that everyone who should be able to read it can.  

EA: If we had one generic “join” function, we’d have some alignment with these.  Is there a strong reason why FLEDGE no longer has a “READERS” concept?  

MK: Yes, the reason TURTLEDOVE had a READERS concept - the browser talked through SSPs, so that SSPs got to add their data.  The READER concept was necessary so that the periodic background updates of the user’s interest groups could be sent to the SSPs that were intermediaryies.  This isn’t necessary anymore in FLEDGE because the interaction happens inside the browser, so there’s no need to encrypt over the wire.  

MK: The role of READERs in the FLEDGE model I would think as a way of BUYERS being able to disclose at time of creation which auctions I’m interested in participating in.  There might still be a feature request for this - it does present optimization opportunities!  We should be happy to add back the notion of READERs.  Both because it might be useful, and because storing it even if ignored would be fine.  

Mehul Parsana: In the FLEDGE model, the assumption is that the one who writes the interest group to the browser, they’re the buyer and have full control.  But if there’s a party that representing multiple different brands or systems.  If a user visits a page, then they might need to inform 4 systems that they are interested in Nike, for example, because of intermediaries?  We’re looking at ways to optimize this?  

MK: Maybe?  Are you looking at READERs as a way for a buyer to delegate to another buyer (like a DSP) the ability to target?  A bunch of different buyers can assign the interest group to another buyer?  

MP: Yes, this is delegating responsibility, the user information that I pulled, the READER can read it.  The READER would have a public key, encrypt over the wire, and only the intended party can access it.  

MK: READER might need a new name, revise FLEDGE so that the delegation from advertiser can be supported, and align with the PARAKEET server-to-server model.  

Viraj Awati: just note that between Turtledove & Fledge; other meta-data got added to Interest Group like biddingLogicUrl which would be different for each reader

MK: This is a good point.  There might be complexity here.  

MP: I think that the interest group is a targeting mechanism, the metadata is things that would remain server-side.  If user is interested in athletic shoes, different bidders have different valuation.  Nike as advertiser wants to give all the buyers something that they can all use?  You might need to create different interest groups.  

MK: In FLEDGE, the Interest Group is the part that does the bidding, since this has to be on client.  In PARAKEET, then there’s possibly the separation - though I think that different buyers might want different S’?  

MP: Yes, they get their own copy.  

MK: Maybe not redesign FLEDGE to have a one interest group  to many buyers at this time.  Multiple different interest groups with different buyers, this should work.  

MP: In PARAKEET, there’s no restriction on duplicating interest groups.  So whatever FLEDGE does should be fine.  

Brian May: Where there might be a problem, is if the publisher wants to control, and when one advertiser is presented by multiple buyers, there might be problems.  

MP: Yeah, PARAKEET might have some capability to control this.  

BM: But FLEDGE would have partitions between things that make this more difficult.  

MK: Yes, on page logic to determine when a bunch of diff buyers rep the same advertiser is not easy in FLEDGE.  

MK: To EA - there’s at least one YES, but there remain open questions about how to make the right choice for both PARAKEET and FLEDGE.  
