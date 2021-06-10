# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 4pm UTC.

That's 11am Boston = 8am California = 5pm Paris time.

(This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload)


# Next meeting: Wednesday June 9 26 2021


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Michael Lysak (Carbon)
3. Brian May (dstillery)
4. Brendan Riordan-Butterworth (IAB Tech Lab / eyeo GmbH)
5. Jeff Kaufman (Google Ads)
6. Allen Fung (ShareThis)
7. David Tam (Relay42)
8. George London (Upwave)
9. Matt Wilson (NextRoll)
10. Valentino Volonghi (NextRoll)
11. Andrew Pascoe (NextRoll)
12. Brad Rodriguez (Magnite)
13. Newton (Magnite)
14. Wendell Baker (Verizon Media)
15. Nathan Jia (Nielsen)
16. Jonasz Pamuła (RTB House)
17. Michael Coward (Arcspire)
18. Les Cochrane (Audigent)
19. Paul Selden (OpenX)
20. Fred bastello (index Exchange)
21. Afolabi Adenuga (Nielsen) 
22. Vincent Grosbois (Criteo)
23. Phil Lee (Google Ads)
24. Phil Eligio (EPC)
25. Ryan Arnold (P&G)
26. Viraj Awati (Amazon)
27. Julien Delhommeau (Xandr)
28. Erik Anderson (Microsoft)
29. Mehul Parsana (Microsoft)
30. Joel Meyer (OpenX)
31. Abhinav Sinha(PubMatic)
32. Tamara Yaeger (IPONWEB)
33. Don Marti (CafeMedia)
34. Anthony Molinaro (OpenX)
35. Christa Dickson (Meredith)
36. Luke Whittington (Glimpse)
37. Paul Farrow (Xandr)


## Note taker: Brendan Riordan-Butterworth



## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.



[<img src="https://i.insider.com/5fc55f5350e71a00115580e8?width=700" alt="Screenshot showing location of the Raise My Hand button">](https://www.businessinsider.com/how-to-raise-hand-on-google-meet)



# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

 		


## [Suggest agenda items here]



*   Jonasz Pamuła: open pull requests:
    *   PLTD clarification
    *   aggregate reporting
        *   https://github.com/WICG/turtledove/pulls 
*   Vincent Grosbois : 
    *   Discussion on status of opened PR
    *   sendReportTo() feature in fledge code

Standing Agenda Item: Open Issues, or Issues that Need Opening

https://github.com/WICG/turtledove/issues?q=is%3Aopen+is%3Aissue+label%3AFLEDGE


## Notes

Intro Session: Michael Kleber with the administrative things.  

MK: Please remember to sign in to the list of the attendees.  Don’t need to order it alphabetically.  

MK: We have attendees, note taker, use Raise My Hand for queue.  

MK: Agenda Items.  


## Product-level and prioritization of features

Jonasz Pamula: It is a question from us. How can we stay engaged?  We see 2 areas where FLEDGE has open questions.  1 is product level TURTLEDOVE, where spec needs to be updated.  No trace of product level support in the current CHROMIUM implementation.  Propose a productive update to the spec?  Is this the best way to make product level support as a first class citizen of FLEDGE during origin trials?  

JP: Second is what was discussed last call, this is an update that extends FLEDGE with agg reporting, to anybody else that may be interested, please leave a comment.  

Brian May: Links to the pull requests please?  

JP: Will add them to the Suggested Agenda Items.  

MK: How to stay involved and make page level stuff first class citizen?  There are some engineers on Chrome doing first end-to-end implementation of the very basics of FLEDGE.  Lacks a bunch of features that are necessary for privacy and functionality.  This is minimal set of features that is “something”,.  It is quite likely that the minimal implementation won’t have your fave feature.  After this end-to-end implementation is in a state of understanding the most basics, then we look at the outstanding requests and improvements to add these in to the Explainer, Future Spec, Chrome Implementation.  This will include actual Chrome engineers triaging the order of these things.  

MK: Sensible ordering based on people who actually know the code.  We’re getting closer to this point, but not there yet.  

MK: Submitting pull requests that suggest changes to the API shape in order to support more features, including pull requests that describe features needed, that’s good.  Aggregating pull requests is also great.  But it’s going to take some time to add features.  

Brad Rodriguez: Given Chrome team is working on a subset, would versioning the proposal be possible?  So that we can know what’s coming first, what will happen later?  

MK: This is a great idea, tried initially, but failed to keep up.  Agree with this.  Don’t have one yet.  This would be easier to maintain during active triaging / prioritization.  

BR: Yeah, the reporter of an issue has no current insight into what’s going on with the feature requests. 

JP: Thanks for the clarification.  What’s different about product level support - the CHromium implementation seems to diverge from current explainer.  Maybe we’re overcommunicating.  But let’s make sure that Product Level Support is not treated as a small feature that can be added later.  If it’s not there during Origin Trials, we can’t take part in Origin Trials in a meaningful way.  PRoduct Level Support is of critical importance.  

MK: Yes, there’s a variety of parties that have different priorities.  We have to figure out these, alongside dependencies and complexities.  Product Level Support is more complex, as it affects auctions and new features on the fenced frame side of things, so it is not the lowest hanging fruit in terms of complexity.  Understand its importance, want to support a wide range of use cases.  

JP: Is it possible that origin trials start without a basic Product Level Support?

MK: No comment on what’s going to be in or out at initial Origin Trial.  

JP: Timeline?  Can share any details?  

MK: Understand that this would be useful.  PM is not available.  Don’t have anything to say about timeline at this time.  Want to communicate more on that front.  


## Trusted servers and GARUDA

Brian May: GARUDA was made in W3C that incorporates server side components in a privacy preserving way.  There are a number of proposals that include a server side component.  Is there thinking about including a server side component? 

MK: For folks that are not familiar with it, link with GARUDA.  https://darobin.github.io/garuda/

MK: GARUDA as ROBIN described it yesterday, is probably more a companion proposal with PARAKEET and MACAW approach to doing the ad targeting that is described in FLEDGE.  FLEDGE doesn’t include an idea for a server side component.  This is a difference between FLEDGE and PARAKEET proposals.  GARUDA is very interesting in that it seems to offer a very appealing way to integrate a PARAKEET-style server that is trusted by multiple parties in a way that we didn’t leave a hole for in the design of FLEDGE.  I guess: if we try to modify FLEDGE to take a GARUDA style server into account, then we’d have something more like PARAKEET.  One could imagine another FLEDGE variant that would take the on-device worklets and ship that work off to a server.  This would not proxy requests to the outside world, just talk back to the browser.  It’s another plausible way of taking the key/value server and adding some functionality there.  Some interesting discussion about how much trust that can happen on the server, this is not an easy change to FLEDGE.  

Brad Rodriguez: I agree that switching to a model where computation is done server side changes things significantly.  Once we have some performance data about costs on client, then there’d be an interest maybe?  FLEDGE says that both the trusted selling and trusted buying would be handled server side ultimately - do you feel that something like GARUDA for those trusted servers would be appropriate?  

MK: The trusted servers in FLEDGE are designed to require as little trust as possible.  The server has minimally trusted server side support to make sure that some really minimal functions are available.  There’s a lot of research in oblivious databases, “PIR”, that is where FLEDGE tries to live.  

MK: The more you trust the server, the more you can do on that server.  FLEDGE tries to be at the least-trusted end of the spectrum.  If you trust the server to talk only with the browser, some stuff can be offloaded.  PARAKEET trusts the server even more, gathers a global view of data, proxies data to the outside world.  PARAKEET is on the other end of the spectrum from FLEDGE.  

MK: GARUDA is about setting up the trust infrastructure for the trusted server - how to assign trust to the server through governance.  IF this works, then this opens up a wide range of more powerful uses of a server.  I’m not sure that something as complex as GARUDA is necessary for the very low trust in the server that FLEDGE envisions for key/value lookup.  Having GARUDA working allows servers to do more sensitive things.  

BR: If I understand, you believe there are crypto ways of ensuring that trust is available for FLEDGE.  If those don’t pan out, then a model like GARUDA could help, too.  

Mehul Parsana: I want to chime in about the trust model, PARAKEET, and FLEDGE, and what it means.  There are 3 parties involved overall in the transaction.  User / Pub / Advertiser (DSP).  In PARAKEET&lt; we assume that that the user trusts the browser.  Doesn’t assume pub/advertiser is trustworthy, but doesn’t upload data.  In FLEDGE, the Bid Server is publisher components, so the trust of the lookup means that the user trusts the publisher a bit, and the advertiser (DSP) trusts the Bid Server.  There is talk about BYO Bid Server and MPC / 2PC, that takes away trust and makes it computation.  What about this trust model - the user is trusting the Bid Server, and the DSPs are trusting the Bid Server as well.  Wouldn’t this need some oversight, like what GARUDA offers?  

MK: You’re right, we were talking about BYO Server way to test out FLEDGE and allow these Key/Value servers to provide real time data.  As we were just talking about, in response to Brad’s question, that would require some sort of more trusted model to keep good privacy practices.  The information seen by the real time Key/Value server, allows the recreation of some insights available to third party cookies.  This means that the Key/Value server would need to move to a more trusted model to support FLEDGE.  

MK: What you’re asking, Mehul, isn’t the trust required higher, but it’s multiple parties that need to trust it in FLEDGE.  That’s an interesting point.  I don't know the answer to that.  I mean, certainly we want to do things that are compatible with a crypto way of establishing trust.  Policy is an option - it’s lighter computationally, but harder to ensure compliance.  Crypto is more computation, but technically enforces compliance.  Interesting tradeoff.  

MP: Yes, we are moving that way.  Whatever the PARAKEET server is doing can be done with Federated Learning.  We are not yet complicating the whole scenario with a step 0.  The functionality moves quicker if the basic functionality is done.  

MK: Yes.  I appreciate this!  PARAKEET is picking a different point on the valuation between crypto and policy.  You are taking a position that the user trusts the browser and therefore also the server that the browser operates.  This is not the position of FLEDGE.  But GARUDA puts us in a position where this is potentially more feasible.  Chrome might have a hard time fully embracing a position where the user trusts Chrome therefore trusts Google to hold their data.  

MP: We make 1 step forward with the framework, but then it makes it easier to take 2 steps forward when we put something like the trusted CDNs, ensuring k-anonymity in a global way.  

MP: Currently the bid server requires a trust.  Would GARUDA oversight be ok?  Obv with MPC that’s not necessary.  

MK: Yes, GARUDA oversight would be good!  

Robin Berjon: HELO

RB / MP / MK: Quick discussion on recap.  

MK: Any more points about this trust in servers?

David Tam: My question about trusted server?  Aren’t we just fetching data?  How far does the FLEDGE proposal go?  

MK: In the FLEDGE model, we’re trying to think of the servers as just what you’re fetching from, but requesting data inherently includes asking for some particular information.  We are working on ways to fetch data in ways that keep the server from knowing what is being fetched.  

MK: If all we need is that the server remain blind to the data that was requested, then it’s easier and different than pushing data to the server?  

DT: Can the advertiser have a trust with the DSP separately?  

MK: I think that’s out of scope of the conversation to date.  The browser is trying to express opinions about maintaining user privacy.  

Brad Rodriguez: Trust and the FLEDGE model.  Browsers are most interested in the trust users have about their data leaving the browser.  Pubs need to trust that the browser won’t send data to a place that they don’t want, and advertisers need to trust that the data has appropriate access control (they aren’t accessing data that they don’t have permission, and that their data isn’t accessed without permission, sortof).  

MK: There are 2 different questions.  (1) What is the flow of information, who gets access to what information, if the system works as it is designed.  

MK: For (1), if there are problems with this, then we need to change design.  

MK: And (2) How can we tell if a system is working as intended?  For (2), you can look at the code of the browser, the browser can be observed via debugging proxy and etc.  But a server side component is harder to have independently validated.  Information flow problems where the server isn’t behaving as specced is a different bucket.  

Mehul Parsana: The bid server, that gets same amount of information, user location and etc, do you envision that BYO server comes?  Ignoring MPC, since this makes compliance technical instead of policy.  What / how do you think that Bid Server will evolve.  Will this be hosted by DSP or ad providers?  Targeting bid request bid-by-bid could be adversarial.  How do we know that the data uploaded is being used appropriately?  I understand that this can be open source code, but verifying that the data is being used appropriately. 

MK: Yes.  The Key Value server in FLEDGE needs to be trusted in some way.  Like, can’t record what key/values were looked up.  BYO Server is only feasible initially, when we don’t expect things to improve on privacy.  In a post third party cookies world, FLEDGE would need to rely on a trusted server - centralized that trusted, GARUDA verified, MPC, one of these trust models.  BYO Server can’t be the long term plan - there has to be trust.  

MP: Yes, we seem aligned.  

Brian May: When you talk about the ecosystem - Ad Tech or the larger Internet?  If the latter, considering other use-cases?  

MK: Very interesting.  Privacy Sandbox is looking at non-ad scenarios.  These don’t come up much here, since it’s Web-Adv or FLEDGE meetings.  But yes, anti-fraud, trust tokens, APIs for federated login, etc.  Same issues about establishing trust, ensuring that state is broadly available.  Some of these are well known, like the DNS servers.  IF a DNS server decided to assign different IPs, then there could be data leakage.  HTTPS Certificates could also leak privacy.  

MK: many of the other cases it’s possible to observe when someone is doing something wrong.  In this case, much harder to observe the trusted servers we’re discussing here.  The need for other ways to establish trust is very important because of this.  

Mehul Parsana: Apple announced Private Relay.  I know there’s [Gnatcatcher](https://github.com/bslassey/ip-blindness).  These will probably impact FLEDGE and PARAKEET.  Thoughts?  

MK: No direct comments about direct competitors.  Phone using VPN makes sense.  Google One has similar functionality - Google run VPN that hides IP.  The Gnatcatcher question is about IP address hiding for everything, rather than for a set of people who have opted in to it.  Some of the specific pieces of the FLEDGE would benefit from IP Address anonymity.  Gnatcatcher would potentially allow global anonymity of IP addresses, maybe?  This work is difficult, would change a lot of how the internet works, force a lot of rethinking how the web works.  

MK; The version we’ve talked about here, scoped to a small set of advertising related scenarios, is probably launchable sooner and more immediately valuable.  


## Topics for next time

Valentino Volonghi: Two topics we can get to later: Issue 175, need feedback, especially understanding feedback and latency.  Second topic: how is billing going to work?  The exchange/SSP takes care of it now.  When the auction takes place on device, how does that get out?  

MP: We can tee up the notification discussion to next week (issue 175).  The billing is slightly easier for PARAKEET, since a lot happens server side.  

MK: I have questions!  There is privacy questions.  

MP: The logic runs on a server using encrypted data, but the win notification is binary.  
