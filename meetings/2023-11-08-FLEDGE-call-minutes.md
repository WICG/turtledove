# Protected Audience (formerly FLEDGE) WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 4pm UTC (during winter)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Nov 8, 2023


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Brian May (dstillery)
3. Roni Gordon (Index Exchange)
4. David Dabbs (Epsilon)
5. Russ Hamilton (Google Chrome)
6. Orr Bernstein (Google Privacy Sandbox)
7. Isaac Schechtman (BidSwitch) 
8. Sid Sahoo (Google Chrome) 
9. Paul Jensen (Google Privacy Sandbox)
10. Harshad Mane (PubMatic)
11.  David Tam (Relay42)
12. Stan Belov (Google Ads)
13. Don Marti (Raptive) 
14. Isaac Foster
15. Jeffrey Wieland (mMGNI)
16. Neil Haack (Criteo)
17. Dmitry Stropalov (OpenX)
18. Laurentiu Badea (OpenX)
19. Matt Davies (BidSwitch)
20. Fabian Höring (Criteo)
21. Tamara Yaeger (BidSwitch)
22. Kevin Lee (Google Privacy Sandbox)
23. Matt Menke (Google Chrome)
24. Risako Hamano (Yahoo Japan)
25. Andrew Pascoe (NextRoll) 
26. Marco Lugo (NextRoll)
27. Caleb Raitto (Google Chrome)
28. Abishai Gray (Google Chrome)


## Note taker: Tamara Yaeger


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


## Suggest agenda items here:



*   Roni Gordon
    *   Macro substitutions - https://github.com/WICG/turtledove/issues/817
    *   API versioning - https://github.com/WICG/turtledove/issues/823
    *   Sensitive signals - https://github.com/WICG/turtledove/issues/824
*   Isaac:
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Buyer/Seller Reporting Questions: https://github.com/WICG/turtledove/issues/682#issuecomment-1710965068
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736


# Notes

Michael (Chrome): We have a standing list of agenda items. Isaac to nominate a topic.

Isaac (Xandr): I need to refamiliarize myself w reporting issues, I should go 2nd.


## Macro substitutions - https://github.com/WICG/turtledove/issues/817

Roni (Index): Issue #817, macro substitutions, this is about understanding what the expectations are from API, I suppose also from buyers, about how macros will behave. We have macro support via replacing URI / URN, my understanding it’s limited to render URL and not what ends up evaluated. I’m trying to understand – today programmatically as sellers we’re responsible for indicating clearing prices and other macros not only in the URL, and I have not seen any indication that what is generating response will understand these macros, so I don’t know where they go anymore. Is there another mechanism?

Michael: There are two halves to the question (1) what we design and what intention is on how to use macro substitution functionality of PAAPI, (2) how that should interact w IAB expectations about macro subs ought to work. I think the clear answer is that the way we design things to work right now is not same as a set of assumptions that IAB expectations were written, so something will have to change. Won’t be a seamless perpetuation, will involve discussions w IAB and changes that ad ecosystem will need to get in on. 

You’re right that there are 2 diff types macro substitutions. First there is replace in URN function, intended to take something in k anon render URL and allows some subs in the URL that incorporate info on pub’s page. They are not about subbing info about what happened in auction, just info that is already available on pub page. No matter what you do w that replacement system, you will never get more info than what is available in standard event level reporting path, k anon render url + pub page. That’s doesn’t help w learning clearing price or store of privileged info.

The second kind of substitution is the one that's part of the reporting APIs in which it is possible to deliberately send reports, could be set up to be sent inside of auction, and when you set up reports inside auction like reportWin or reportResult in auction, you can include macros in those URLs to sub later stuff that happened in creative rendering time or what happened as result of auction (px). Reporting API mechanism we have is where macro sub is available.

That is different from substituting macros in body of rendered creative. No support for that right now, deliberately. Rendered creative does not have any imposed restrictions on what domains resources can be loaded from or where info can be exchanged with. But the combo of info about things like what happened in the auction, that is much more sensitive info, that we want to only send to parties making attestations that they will not circumvent Privacy Sandbox restrictions. We want to only allow this macro substitution going to domains that are signatories to attestations, therefore do not want to allow inside rendered body of creative. Would prefer to remain k anonymous and not follow secret info so that we don’t need to worry what domains are contacted from inside rendering frame. This is essence of why Protected Audience API makes a distinction between rendering and reporting. In the ads world today, when the creative is rendered, you’re also doing reporting at the same time, there is no difference between rendering activities for putting pixels on the screen and ones for telling servers what happened. In PA auction we separate those two things and treat them differently. Screen has to be k anonymous, can load stuff from arbitrary domains, but can’t get lots of extra info as a result. The stuff that needs info about auction outcomes should go to reporting flows.

Roni: Let’s say today auction price macro is included, what I’m hearing is there is intentionally no facility for replacing that macro as a seller, it will remain unreplaced and that is something that the buy side will have to deal with. In other words OpenRTB spec requires replacement, even if with nothing. I can’t do that as a seller. Therefore buyer’s macros cannot involve what the API will not involve me to replace. Signaling needs to be outside creative / markup. Is there a signals for winner between report results / report win, so I believe if we wanted to signal that we would need to devise a standard for meta data. We would need to talk about mechanism for communicating these macros between report result / report win. Onus could then be on report win buyer to make subs on their own. Is that a fair assumption?

Michael: Almost right, it is still possible to get the reporting that you want from inside the rendered creative, but not by loading some URL as part of the markup. The mechanism we have where party that shows up in rendering flow can get info involves their use of fenced frame that reporting API. So inside rendering there is new JS API that says send this report to this URL, that report can include macro values in it, the browser will automatically sub them w values that come from the outcome of the auction and the values that are provided by buyers / sellers, that whole flow is possible but it doesn’t involve loading HTML, gotta call JS API. In addition, the domain has to be explicitly listed along w the creative by the buyer as a domain it is ok to send reports to. That domain has to be done the attestation work to be allowed to get info from auction, so as not to circumvent Privacy Sandbox. 

Roni: Even w/in context of today iFrame example, even if pathed signals from winner, still no facility for IFrame to replace that macro even if buyer has that info.

Michael: Correct, doesn’t get replaced through HTML, has to go through reporting flow. 

Roni: To summarize, any expectation that buyer has that macro will be replaced in markup, are false assumptions now, that will never happen. If they want to do it the “new way” they report event and all requirements. If they want to use event level reporting, there needs to be an agreed upon meta data signals for winner between report results and report win. That’s where we’re at. 

Brian (Distillery): The implication is that formerly evidence of what happened on browser, result of having my macro pixels subbed and sent to me, is now going to have to have to be presented in another way. We have a fenced frame where something is happened, trusting what is happened, reporting coming from another channel, I’m concerned we are not getting strong signal that what we paid for is what we bought. Should we think about some means of producing signals to buyers to provide evidence of the thing getting billed for? Can we do it in privacy preserving way?

Michael: It has always been the case that any info received about browser was because browser sent request over network. The different now is instead of one request, there’s two types of requests. Some of them are regular old HTTP requests for rending things, using pixels, those still exist. They don’t have macro sub involved in them. Macro sub was always a black box, some man in the middling HTML to insert stuff into it, asking unknown party to change what you sent. What is in the browser is exactly what you send now. Before many unknown people could mutate HTML before it gets rendered, now there are not. From that POV a lot less behind the scenes black box happened in this model. 

Brian: In old model, when event happened, details associated happen at same time. Now they will be provided at a later time? In my pixel fires it can have context from which it fired in macro substitution, at point at which it fired. When I have to go through diff channel, I have to assume the event that happened to produce info, but w/out direct evidence. Later I’ll get info about that pixel fire.

Michael: Not how I think of it. There were always different events.

David (Epsilon): Just to clarify, embedded in there, there won’t be an impression time YoY, won’t be an impression time fetch creative because vision for fenced frames is there is not network, right?

Michael: That was our vision early in this effort, but not any more. Infrastructure required to realize old vision is not how the web is moving. When we started this journey and described this journey in 2020, we liked the idea of rendering with web bundles and without using the network, but it seems that is no longer likely. However it is still the case that the place where we should get to in 2026 is one where even though a creative that renders loads over the network, it should have very little info inside the fenced frame. What we just talked about is fenced frame doesn’t have info on clearing px of auction. That info is known inside auction but tries to be careful about releasing info that could be used for tracking. The vision for the future of fenced frame is one where it is still possible to render creative, but the creative has little information beyond the fact that it rendered. A pixel can reveal that this showed up on a screen, but it would be great if it didn’t reveal info about person who saw the ad. Anything that involves cross-site info, sense of user’s ID, that kind of info should go through dedicated reporting API with privacy protection built in. This means something about aggregation, differential privacy noise, delaying time at which things are sent. Fenced frame rendering is not a privacy preserving API, just a boundary to not allow user info to flow through pub page.

Roni: Getting back to original ask for clearing price. I understand the report result / report win, gap is in decision - what price to use, conversions from net to gross - all of it based on pub host name and rendered URL and associated meta data. If I make decision that changes bid price, no way to get it into report result, no way to tell what I need to tell to buyer. Today there are mechanisms for leveraging auction px macros to provide fee reductions and buyer incentives. They may not charge you full bid submission, I can communicate the discount. No way to change $10 to $9.50 because Scoread could not send info to report result because of privacy. There’s a gap where the info I know is in Scoread. Modified bid is about the net price, not the buyer’s price. MOdified bid is to be used in top level seller auction. There are 2 parties in this transaction, and those prices can be modified at any point.

Michael: The intention is that the person running the auction gets to output - how much pub should get paid, how much advertiser should get charged. We want to make it possible for you to make those decisions and communicate them at reporting time. Is there a third number that is different? 

Roni: There is, so the take rate adjustment is modified bid. Both bid and modified win are available, but that doesn't communicate to buyer what they were charged. They might submit a $10 buy I may not charge them $10 independently of my take rate. Submit $9, actually charge $9.50. I don’t have a way to signal that piece, because all I have is incoming bid from buyer, and modified bid going to seller. Only known in Scoread if I give buyer discount. No way to indicate it to report result. 

Stan (Google Ads): You’re saying some discount could be based on individual creative

Roni: Assume buyers will make decisions based on render URL, exact same on sellers side. Right now no trusted scoring signals going into report result. What I now in Scoread should be in report result. The real question is, can I get trusted scoring signals in report result?

Michael: [Sorry, my Chrome crashed, back now but I missed some of the conversation.]

Paul Jensen (Privacy Sandbox): I just want to understand, do they know they will pay lesser amount?

Roni: Not at auction time

Paul: How does the seller know who to give discount to

Roni: Based on pub host name and render URL, the info Scoread has. All discounts handled post-auction. 

Paul: But in 2nd price auction you would know?


Roni: There are no second price auctions.

Paul: Trying to understand how / what is the discount that the buyer knows is coming, they’ll bid not knowing there’s a discount?


Roni: Bidding behavior is not based on my margin. They bid in gross terms before all adjustments. Separate from how buyers / sellers pay and charge, we can make intentional incentive programs based on volumes. The question is, all the info that we need is in the trusted scoring signals. If they are ok for Scoread, I assume they’re ok for report results, and it’s just not there because we didn’t come up yet.

Michael: It seems there shouldn't be any new privacy problem in giving you the info you’re asking for because it’s the signals that are fetched from sellers key value server based on looking up render URL and the host name of pub site, and you’re taking about making those signals available in report result, an environment that already knows the render URL and the hostname in question. Yuu knew keys and you wish you knew value. Browser has already looked up value based on those keys. We’ve never piped it through to get you that info where you want, but I think that's just because nobody has asked for it until now. I don’t see a reason why it can’t be there. 

Matt (Chrome): If all info in request to seller could be used to generate response, could potentially learn other data. Data has to be generated on per URL basis. 

Michael: What Matt is saying, if the key value server is behaving like expected (only based on each individual URL plus the host name), then no new privacy problem to make it available in report results. If owner of key value server is cheating and packing info in response for one URL based on other URLs, then would be channel for leaking cross-site info. Someone who wants to cheat in their key value service by processing as one big thing. 

Isaac (Xandr): presumably you can cheat by combining render URLs but if you export… attestation … (?) . It could happen but it’s covered by the same thing, I don’t know.

Matt: My understanding is that discussion around info from same joining origin in signal call, which would break anonymity. So we’d have to revisit whether we allow key value server to process all seller URL from the same joining origin. Also note it does not apply to bidder key value info.

Michael: The only reason that giving what Roni is asking for is feasible is because URL is already k anonymous. Not a collection of keys that buyers key value server, the already k anonymous render URL. 

Roni: Made progress, solidified understanding. 

Michael: It didn't occur to us that it’s a valuable thing to have. 

David: Roni would it be fair to describe this as 1st class feature request/ Now that we seem to have alignment, we were actually talking b4 meeting about relaxing single origin. How do we go about saying we’d like you all to have 1st class feature request? Anything special we need to do process wise?

Paul: We have a GitHub label for features that don’t break things, it could go there. 

David: So we have to tag one of you to put it there? Fine, good to know. 

Kevin (Privacy Sandbox): I’ll let you know

Michael: Scoread still has more info that report result does, even with this change. Scoread can get lots of info handed to it from Generate Bid. Generate Bid has arbitrary 2-sites info about user. Still the case that could violate privacy. This particular info that report result can get we know is based on k anonymous data, not on individual user tracking data. 

Laurentiu (OpenX): Not necessarily based on K values, could be based on some machine learning or computational intensive work. 

Michael: the fact that we can’t give you full Scoread contents. I’d be delighted if this doesn’t introduce privacy problem. 
