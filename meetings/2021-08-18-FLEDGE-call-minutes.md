# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC.

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Aug 18 2021


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Basile Leparmentier (Criteo)
3. Newton (Magnite)
4. Brad Rodriguez (Magnite)
5. Erik Anderson (Microsoft)
6. Michael Coward (Arcspire)
7. Caleb Raitto (Google Chrome)
8. Paul Jensen (Google Chrome)
9. Manny Isu (Google Chrome)
10. Brendan Riordan-Butterworth (IAB Tech Lab / eyeo GmbH)
11. Brian May (dstillery)
12. Cory Underwood (Search Discovery)
13. Russ Hamilton (Google Chrome)
14. Russell Stringham (Adobe)
15. Jeff Kaufman (Google Ads)
16. Chris Evans (NextRoll)
17. Valentino Volonghi (NextRoll)
18. Matt Menke (Google Chrome)
19. Dom Farolino (Google Chrome)
20. Fred Bastello (Index Exchange)
21. Jason Xie (Adobe)
22. Wendell Baker (Verizon Media)
23. Don Marti (CafeMedia)
24. Joel Meyer (OpenX)
25. Nathan Jia (Nielsen)
26. Przemyslaw Iwanczak (RTB House)
27. Phil Eligio (EPC)
28. Vincent Grosbois (Criteo)
29. Anthony Molinaro (OpenX)
30. Paul Farrow (Xandr)
31. Bill Landers (Xandr)
32. Aswath Mohan (Microsoft)
33. Tamara Yaeger (IPONWEB)
34. Aditya Desai (Google)
35. Andrew Pascoe (NextRoll)
36. Aleksei Gorbushin (Walmart)
37. Viraj Awati (Amazon)
38. Isaac Schechtman (IPONWEB)




## Note taker: Joel Meyer


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

 		


## [Suggest agenda items here]



*   Discuss issue 213 : reporting on user bidding signals (Basile Leparmentier)
*   Discuss issue 214 : how to authenticate reporting event (Vincent Grosbois)

Standing Agenda Item: Open Issues, or Issues that Need Opening

https://github.com/WICG/turtledove/issues?q=is%3Aopen+is%3Aissue+label%3AFLEDGE


## Notes

MK: Michael Kleber | BR: Brad Rodriguez | FB: Fred Bastello | PF: Paul Farrow | WB: Wendell Baker | BL: Basile Leparmentier | VG: Vincent Grosbois

Michael Kleber: Queue maintained using “Raise my hand” feature. Please use it. There are some agenda items already listed. Prior to getting to agenda items, based on notes from last week it sounds like there was a chance that AdTech folks might be presenting on their needs from the browser to run a sequence of auctions that result in choosing the winner for a single ad slot.

Brad Rodriguez: Can say a few words, though didn’t get a chance to put together anything formal. Will prepare something for two weeks from now. Please contact him if you are interested in participating.[ https://github.com/brodrigu/fledge-ad-tech-model/issues/1](https://github.com/brodrigu/fledge-ad-tech-model/issues/1)

Michael Kleber: I have seen some one-off discussions that maybe highlight a misconception. I originally explained it, thought about it, as needing to support the buyer winning Auction A needing to be a participant in Auction B. It is perhaps more appropriate to think of it as the Seller who runs the first auction becomes the Buyer in the second auction. If that is the case, it helps clear up some confusion about buyers participating in multiple auctions and the dynamics of that. Is that the case? Can others with more familiarity speak up on that?

Joel Meyer: I agree that's a good mental model and can help to clarify things.

Brad Rodriguez: Seconded. Would like to think about it more. Wouldn't object to that as a place to start.

Michael Kleber: Great. Look forward to hearing more in the future meeting.

Fred Bastello: In terms of running the auction in Fledge, you have multiple SSPs participating so it’s not necessarily a cascade of auctions that go on, but in a way it’s almost like the seller is the publisher running a big unified auction.

Michael Kleber: I agree that thinking of the pub as the one who makes the final decision is very reasonable and inline with the original intent of the Fledge explainer. If the implication of that is that the final auction is a much simpler auction/decision process - perhaps calling it an auction is misleading - that seems fine to me. Whatever concepts we need in order to support the use case is what’s important. Thinking of it as an auction is easier, because it allows us to reuse a lot of the same machinery, but if that’s not the case then we’ll learn that in due time.

Michael Kleber: Do people want to have more discussion now or shall we wait for Brad to come back in two weeks? Contact Brad, discuss on GitHub, etc - this has been an open question in the design of Fledge from the beginning and it is good to get to resolution on this.

Mehul Parsana: Last week John and Mehul wrote something up that happens in the two-step process at the end of the Parakeet proposal. Maybe they can also speak to that next week.

Michael Kleber: Happy to discuss in two weeks or do it next week in the Parakeet call. I’m hoping that a lot of the explanation of what we’ll get is from the ad tech world about the interactions between parties and how those needs can be met in Fledge, Parakeet, and/or a combo. Fully understanding the parties and the roles they play in subsequent steps is the first step.

Paul Farrow: Going to talk a little bit, I think that the SSP going from seller to buyer role kind of makes sense. It seems like that’s what Prebid is doing. Happy to be in the loop with discussions with Brad and Joel.

Michael Kleber: Agree - sounds like what we were talking about in previous discussions. Perhaps it’s too subtle, but the issue of who determines price in the second auction is important. To what extent is that the DSP and to what extent is it the SSP. Is it the DSP who won the first auction, or is it the SSP who ran the first auction who now chooses how to bid against the other SSPs in a final publisher made decision. In other words, whose Javascript runs to determine the bid price.

Paul Farrow: This definitely needs to be explored. First response is that it seems like it would be the SSPs JS in the second auction and then Pub making final choice.

Wendell Baker: I’m hearing that there is legitimate misunderstanding. There is a thing called OpenRTB 3.0 that is not widely adopted but it has a lot of theory. It does have the chained auction diagrammed. There is no defined way to run auctions. We’ve bounced back and forth First-Price Auciton, Second-Price Auciton, hybrids, etc. First-Price Aucitons don’t compose and Second-Price Aucitons have some dynamics as well. What tends to happen is that these are commercial relationships between supply chain and it depends on who thinks what a fair shake is determines that. Chrome is going to be party to these trades - you will own the merchandise. Like UPS, there is verbiage that says who owns the package when it leaves the receiver. Sometimes it’s the receiver and sometimes it’s the carrier. If multiple sub-contractors are involved it gets resolved in accounting and book keeping. I’ll harken back to RM era, fractioning out the payments has been a value in the industry. Running the auctions and stewarding the inventory as it goes from purveyor to buyer with user helplessly watching, you guys will be party to these trades. You can shed responsibility by publishing what you do, but that pushes responsibility elsewhere. Maybe you should take a point of view, get something going, and then industry can sort out the mess. Something like in OpenRTB 3.0. There’s lots of theory here.

Reference to OpenRTB 3.0 spec: https://iabtechlab.com/standards/openrtb/

Michael Kleber: Thanks very much, that is a helpful POV. I think you’re right that we don’t want to design a system in which the browser is forced to be an owner of the inventory at some point during the ad trading process whatever our inclinations may be. We are trying to build something that is a web standard that other browsers will adopt as well. In order to be a browser it doesn’t seem like you should have to take part in the ad tech industry - that should be left to the people engaged in buying and selling on the device using a set of well defined primitives that are available. If the answer is that these things are not fixed and is an ongoing tussle, then we the browser are stuck with a situation where we need to write something that is powerful enough to let other people determine their own auctions, that is somewhat dismaying. Would vastly prefer to provide a foundation that enables many solutions.

Wendell Baker: Let me jump in. You suggested something back in 2019 that the metaphor is like a hypervisor that sits between hardware/OS and operates on behalf of the rights-holder. The W3C seems to hold a position that this shim should operate on behalf of the user, though the reality is that it operates on behalf of the media provider - see DRM as an example. The hope is that browser makers would find a way to sell media and the shim would be adaptable in reasonable ways. It seemed that the sandboxing being developed and things like WASM were helpful ways of achieving that with signing and encryption to guarantee that the layer was trustworthy(?).

Michael Kleber: Digesting what WB said. Go ahead, Brad.

Brad Rodriguez WB - do you have links on the proposal you alluded to?

Wendell Baker: Let me see what I can find. This was a F2F hosted by Google at Google in Sept 2019. There were some notes, slideware, and proposals recorded. Check GitHub.

Michael Kleber: In terms of who is acting on behalf of what parties, I think the division of responsibility that I would be looking to recreate is that the browser's interest is in ensuring privacy. The reason the browser intrudes into this patchwork of interactions is because there’s an interest in restricting information flow. So whatever mechanisms the browser can use to ensure that there is restriction of flow of information so that you can’t do, eg, cross site tracking, but at the same time allows the business relationships to flow forward and enables innovation, that is welcome. It’s just harder to do the right privacy thing as the solution enables more general processing & communication. The goal of restricting arbitrary execution and communication is because it’s hard to protect user privacy (e.g. prevent cross site tracking).

Michael Kleber: I’m very interested in OpenRTB3 and its take on chained auctions. It might be that someone else has put a lot of work into parameterizing the solution space. If so I would love to hear about it in two weeks.

Wendell Baker: It’s a deep body of work, breathtaking in its scope. It’s called Open Media in general. How do we buy, sell, etc. As a tutorial it might be interesting to get someone to rep that. But the industry has largely chosen not to adopt it. Though a lot of thought was put into it. It’s worth spending some time there.

Michael Kleber: If anyone is a  principal in that discussion feel free to invite them and we can get some additional wisdom. Good discussion - let’s continue it. This is a good area to discuss and something we’ve deferred long enough. There are two items on the agenda.

Michael Kleber: Let’s move to issue 213: reporting on user bidding signals by BL.

**Issue 213 : reporting on user bidding signals (Basile Leparmentier)**

**https://github.com/WICG/turtledove/issues/213**

Basile Leparmentier: This is also an issue that has been going on a long time on all the signals that aren’t available in reporting. You have contextual signals but you don’t have user signals. In general, you won’t have reporting, even in an aggregated fashion, that you need. Basically, we can’t report on the signals we need to bid. So we can’t know why our bid is what it is. It’s also impossible to do ML because you can’t report on outcomes. No reporting == no training. TL;DR: We can’t report on the signals we need, which means we can’t validate/train models, so that’s a problem. We really need them, please add them, otherwise we can’t do what we need to.

Michael Kleber: The answer is that, yes, we do expect the sort of reporting you’re talking about to be possible using an aggregated mechanism. There is a very old explainer on the old/generic AR approach but it doesn’t capture any of the work done recently in the context of aggregated attribution measurement. But that same amount of thinking that went into that applies to the more general problem as well. We definitely need to publish an updated explainer or description of how we will handle reporting for the on-browser bidding signals as long as they pass through the noise aggregation layers that the browser provides.

Basile Leparmentier: Does this mean that in the origin trial we could expect this? We need it at the beginning of the OT otherwise we can’t do a true comparison of cookieless vs cookie.

Michael Kleber: We can expect it, but not sure on timing.

[brief scribe outage due to surprise reboot]

Michael Kleber: The other agenda is issue 214

**Issue 214 : how to authenticate reporting event (Vincent Grosbois)**

**https://github.com/WICG/turtledove/issues/214**

Vincent Grosbois: The way it works is that the report win function is implemented by the client, you basically call a URL with a list of params and the only thing that Fledge will do is send the request. The question that I have is that it seems like there is no way to validate the data that is sent to this URL. For instance, as basically in Fledge everything is public, anyone can access it, because most of the computation is done in JS so it’s quite easy to figure out what is done in the reportWin function. So basically anyone can call that endpoint and corrupt your data. Anyone could duplicate this and do the same thing but with wrong data. Two parts: 1) is there any way to authenticate the data/call to the endpoint? 2) a proposal for authenticating the data, but for it to work it requires that in the reportWin function we receive data that is coming from the trusted bidding signal server that we generated. In other words, if everything is done on the client, how can we trust it?

Michael Kleber: It is certainly true that there is data that comes from servers, both trusted and contextual response, that feeds into the event level reporting capability. I apologize, I’m not familiar with the issue so I’m not sure what you proposed.

Vincent Grosbois: The idea is that if we can call a trusted server in the reportWin function, in the call to the reporting endpoint we can pass a signal through or hash data we send. When we receive the reportWin we can undo the hash and compare it and know that it’s authenticated. It’s a quite common way to do this. The whole point is that if it’s all done in JS on the client it’s not effective because it can be reversed & faked.

Michael Kleber: If there is a way that this would allow someone to be uniquely identified, then we can’t do this. There are a few mechanisms that have been discussed and studied a lot. Charlie Harrison has looked at a lot of this in the context of the bounds checking machinery. For example, the contextual signal could come back with a set of cryptographically verified bounds within which the bid is expected to fall that can be verified in the context of MPC. But it sounds like you’re thinking about event-level reporting, not aggregated reporting. We could use the same-origin policy to offer protection and the unique event must come in the same context as the page, but then some 3p could also look at it. The generateWin env should be the only environment in which this is sent, so the browser should be able to offer some protection that the call is only made from your code and not someone else’s. You are quite right that some other user could open Developer Tools and duplicate the call, but it would protect against an attack at scale in which some actor does this across many unsuspecting users browsers. We should talk more on issue 214. We also haven’t mentioned trust tokens, but they are one sort of way to sign something and pass along the fact that something was signed in a way that gives you the confidence that it really is something you signed in the past without allowing tracking. That may be a solution as well.

Vincent Grosbois: My main concern is the fact that the endpoint for this reportWin call is a really simple server call and anyone can discover it and call it. TL;DR: we need to be able to trust the call to reportWin.

Michael Kleber: Perhaps I answered a different question, apologies. The contextual response in which your server learned the URL and got to send back signals for on-device bidding is also a way to send a unique ID associated with this auction/page and when you get a reportWin call you can pass that through.

Basile Leparmentier: Jumping in. I’m really concerned about the contextual request because it means we’d have to know in advance if we want to bid this and we only reply to a small fraction of requests. A small player can’t handle the volume of ad requests that come through. TL;DR: Any scalable solution can’t rely on the contextual request. It would be too limiting/expensive operationally.

Michael Kleber: Thank you, I understand. There are other solutions I was proposing that would also work.

Alexi: Are you talking about the spoofing? That someone can generate calls to you?

Basile Leparmentier: We don’t want to receive all calls. Today we can say what opportunities we want to be called on. So we only receive a small portion of the requests even though we are quite big. We won’t know what users are in what IGs, so we can’t know in advance if we should respond to a contextual request with info for an IG request.
