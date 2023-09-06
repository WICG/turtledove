# Protected Audience (formerly FLEDGE) WICG Calls: Agenda & Notes

Calls take place on ~~some~~ Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Aug 30, 2023


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Roni Gordon (Index Exchange)
3. Nick Llerandi (Triplelift)
4. Antoine Niek (Optable)
5. Xavier Capaldi (Optable)
6. David Dabbs (Epsilon)
7. Abishai Gray (Google Privacy Sandbox)
8. Paul Jensen (Google Chrome)
9. Tamara Yaeger (BidSwitch)
10. Orr Bernstein (Google Chrome)
11. Nitin Nimbalkar (PubMatic)
12. Sven May (Google Privacy Sandbox)
13. Alonso Velásquez (Google Privacy Sandbox)
14. Fabian Höring (Criteo)
15. Alex Cone (Google Privacy Sandbox)
16. Jonasz Pamula (RTB House)
17. Harshad Mane (PubMatic)
18. Sid Sahoo (Google Chrome)
19. Isaac Schechtman (BidSwitch) 
20. Isaac Foster (MSFT Ads) (2 isaacs!!! No way!!!) (agreed, only second time in my life that’s occurred!)
21. Don Ma
22. David Eilertsen (remerge)
23. Russ Hamilton (Google Chrome)
24. Martin Karlsch (remerge)
25. Christoph Armster (remerge)
26. Kevin Lee (Google Privacy Sandbox)
27. Priyanka Chatterjee (Google Privacy Sandbox)
28. Manny Isu (Google Chrome)
29. Caleb Raitto (Google Chrome)
30. Matt Menke (Google Chrome)
31. Andrew Pascoe (NextRoll)
32. Jeff Wieland (Magnite)
33. Sergey Federenko (MSFT Ads)


## Note taker: Tamara Yaeger


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here — no agenda no meeting!]



*   Isaac (MSFT) Issues (don’t all have to be mine, so grouping so we can easily filter)
    *   <span style="text-decoration:underline;">User IG view/delete interaction w/r/t BA payload optimization https://github.com/privacysandbox/fledge-docs/issues/56</span>
    *   <span style="text-decoration:underline;">Dynamic but K Anon Creatives https://github.com/WICG/turtledove/issues/729</span>
    *   <span style="text-decoration:underline;">Multiple Bids Per IG (checkin, was discussed a while back) https://github.com/WICG/turtledove/issues/595</span>
    *   <span style="text-decoration:underline;">PA Deals Phase: https://github.com/WICG/turtledove/issues/686</span>
    *   <span style="text-decoration:underline;">Cross Device https://github.com/WICG/turtledove/issues/607</span>
    *   <span style="text-decoration:underline;">Production Operations: </span>

        <span style="text-decoration:underline;">https://github.com/WICG/turtledove/issues/728 \
https://github.com/WICG/turtledove/issues/620</span>

*   Nick (Triplelift)
    *   scoreAd is provided adMetadata (ortb per industry discussions; BidResponse, Bid objects). Should reportResult also receive this? How else to report an imp against brand, creative, seat, etc?
    *   scoreAd can output a modified bid value in component auctions. Is it this modified bid that makes it to GAM for pub reporting?
        *   Non-component auctions: there is no modifiedBid output. Wouldn’t that still need to be required for a seller to implement a margin?
    *   ~~Clarifying how the attestations file lookup/check works (Seller perspective); is the “Seller” origin checked prior to every auction?~~
*   ~~Bosko (Optable)~~
    *   ~~[GAM throttling](https://github.com/google/ads-privacy/issues/77#issuecomment-1690872192)~~
*   Jonasz (RTB House): [3pc deprecation: ramp-up duration #717](https://github.com/WICG/turtledove/issues/717) 


# Notes

Michael (Chrome): let’s lead off w Isaac, choice of which issue to get into. 


## Creative Competitive Exclusions and Page Caps https://github.com/WICG/turtledove/issues/773 

Isaac F (MSFT): Start w competitive exclusion. The issue of a pub saying they don’t want category of ads showing up in page is accounted for w SSP key value flow (we can talk in more detail). He brought up w me is often advertisers will not want to appear on same page as other advertiser. Example Coke & Pepsi. It’s not something that you know at page existence time. Our SSP currently w multi slot pages, when SSP gets back w all the diff winning bids, it runs through category list of exclusions; based on some priority one of them wins. This is pretty important as I understand it on our platform. Was able to catch up some other folks who mentioned competitive exclusion is also important to them. The way the state is managed currently in auctions , I’m not sure how to do that, maybe run sequentially? Wanted to raise this. 

Michael: At a top level you’re correct there is not a way to do what you’re asking about. The model in PA is that every ad slot on device auction through PA mechanism happens independently of each other; no passing of info between them. It’s possible that some info is available between diff auction slots in same interest group, the IG could be aware of its previous wins. But there is no mechanism for one interest group knowing one ad that appeared because of previous behavior of a different interest group. So that's what happens today, now let's think about what we might change in the future. To be aware of scope, it seems like the best possible situation is two different PA auctions that are happening at the same time, same page, same seller or same buyer. If one of the ads is contextually targeted, for example, it is not possible to exclude itself from showing or being chosen based on previously chosen PA ad. The choice of which ad was the previous auction winner is not something we want to leak, part of privacy. Seems like this kind of exclusion could directly violate the privacy model. I have some questions about how things work today in ORTB - you Isaac just described what happens when single SSP is trying to handle multi auctions at once. What about SSPs who don’t interact w each other?

Isaac F: Sergey just joined. The pub's ultimate SSP is where any ad quality would be finally enforced. In our sys, creatives need to be registered even if coming from external DSP. We are able to do audits and ad quality settings. I would imagine it’s the same here; once the creative has won, each slot will have a category. Sometimes let’s not have the same ad on the same page. I believe the answer is that our SSP can require that a creative coming from our DSP or diff DSP have an audit status and is registered, therefore we are able to do this type of thing at final win time. In case when we call out to external thing, I don’t think we have them do anything with their creative, done at our time.

Michael: What I’m thinking about is if there’s multi SSPs that are each trying to do decision making. 2 ad slots on page, one filled by OpenX bringing OX demand to page, and you’re bringing different demand through entirely diff channel. How can they exclude against one another today? Seems very hard like it would be in PA case.

Sergey (Xandr): If that pub regularly engages in these things and it becomes problem for specific advertisers they will pull demand. Or they will bring that problem to them to solve. Brought up ORTB that competitive exclusions is not supported right now in bid response path, which prevents from enforcing the case you describe. In our platform if an advertiser says they want to exclude categories, we’d exclude from other slots. Another thing is that entire auction path is a cascading set of auctions that happens from DSP to SSP to previous server and so on. The earlier you filter out bids the better. You don’t clog whole system upstream. 

Isaac F: Are there cases where enforcement is not done technically or cannot be enforced technically at auction time. For 1P ad server clients, this is important feature for them, is that correct?

Sergey: Yes, for clients that manage own demand on platform and want 3P demand. Usually you cannot fill 100% supply w managed demand. Over years has tried to fill in various ways. Managed supply is where these controls are. They have a special relationship w advertisers, they have a line item that says they don’t want these categories displayed alongside me. 

Jeff W. (Magnite): The short answer is comes back to the pubs ad server to make final decision what renders on page. So it has some responsibility into which ads from which advertisers are there.

Michael: To take example we talked about here, if we have some set of demand coming through various SSPs integrated through prebid, then some other demand filled by GAM, how does meta data passing work to allow competitive exclusion?

Jeff W.: I think that in ORTB the advertiser is identified and GAM receives that in response from ad request.

Sergey: GAM would be final adserver in this case?

Jeff: Exactly

Michael: So it seems what you’re describing requires every SSP has to trust every SSP to send along accurate metadata about who the brand each ad is about?

Jeff: It’s sorta the purpose of ORTB to communicate using the same set of protocols. 

Sergey: There are two ways we are doing it: Creatives are pre-registered to go through audit to assign categories and brands to them. When DSP or SSP bids with this creative there is metadata attached that was assigned by the audit. Or for certain buyers that we trust we enable them to look at a domain in bid response, then that brand will define category where exclusion can be applied.

Michael: I was asking about order of this operation, my understanding from previous convo is that ORTB callout would go to a lot of diff SSPs in parallel, so each has to be making decision on multi ad slots on page, in absence of any other info from the SSPs that its competing alongside on the page. I don’t see the flow of info that can support diff SSP competitive exclusion working.

Jeff: GAM is the final decision maker on rendering

Sergey: GAM or Xandr SSP final arbiter to coordinate different bids. But it’s on the seller to set up rules in final arbiter SSP.

Michael: Seems like the thing that could work in PA environment is for every ad to come with some piece of competitive exclusion metadata to be understood by  browser to identify brand of ad. At the time that SSP’s score ad function is running, it might know about that piece of metadata of previous winners in other PA auctions on the same page, and then the SSP’s ad scoring logic could take that into account when they write their scoreAd JS. Is this what you’d wish for?

Jeff: Doesn’t seem right, seems like top level auction seems to be aware of component auction bids and brands, and then read any publisher brand exclusions from run ad auction and apply them. 

Michael: So it seems like info about competitive exclusion from previous winners on same page could be available either in component auction SSP scoring function or the top level SSP scoring function or both. Happy to consider either. Seems could also come in from contextual info from pub page. Maybe Coke ad shows up not through PA, but Pepsi from PA may want to exclude that. How to make it not subject to gaming is unclear to me. This doesn't seem impossible to maybe even end up in state better than today. Sounds like SSP 2 doesn’t have away to do competitive exclusion of ads from SSP 1 on same page.

Jeff: If you make it better than it is today you will get a lot more enthusiasm from community.

Michael: Doesn’t sound implausible. Make sure we’re not breaking invariants here. I don’t think anything we can do can have guarantees. At best, this will be best-effort exclusion. I don’t think browser is in any place to know that competitive exclusion metadata is correct. If metadata comes in about ads, it seems the competitive exclusion idea involves one SSP trusts other SSP’s metadata. Caveats aside, we can try to design it in the future.

Roni (Index Exchange): Top level seller needs to know state of multi bids at once. If we solve that problem we solve this. SSPs do best to assure their bids do not violate competitive exclusion. If top level seller can say only one of these, then I think we’re good.

Isaac F: I’m really into us finding ways to make this succeed I agree would be good. We should also think to not make current situation worse for 1st release. There will be cases where simple cases of competitive exclusion will need to be reliable. Need to understand short vs longterm thinking.

Michael: For any product that has unending stream of feature requests, some prioritization will be needed. This does not seem like something we should drop everything for and pivot as highest priority. These all seem like nice to have features for longterm roadmap. Hard to believe it’s a show-stopper.

Isaac F: My understanding is for cases where we can enforce in 1st party adserver context it’s quite meaningful. 

Sergey: Larger context on ad serving - in this situation, they curate demand very closely. It’s practically impossible to come up w ranking of single bids in isolation. Our ranking algo very complex. We take into account higher price bid most likely will not win. Some of the line items in managed demand influence where RTB bids can or cannot bid against managed demand. A lot of info during ranking phase. This problem is exacerbated by ad pods, where you have to fill not only single spot but duration and optimize for ad delivery based on PG deals and prices to fill entirety of ad pod.

Michael: I completely agree that there is a set of features that are not supported in PA, and that we don’t plan to put on roadmap, which are features that involve making choices about what ad to show based on more info from more different domains. The information flow in PA auction is specifically limited to using info from two domains: domain where user is added to interest group and domain where the ad will show. Purposely restricted. Using previous data could cause degradation to privacy model. Use case across multiple sites is not something we’re trying to support.

Sergey: This is about bidding against each other not knowing where bids are coming from. 

Isaac F: I recognize position you are in saying you need to respond to concrete issues and rank priorities. I think the nub that is good for us to clarify more is case where desire for mathematical proof is missing key use case. I’ll dig into this more, it would be good if in long run we have a system that enforces competitive exclusion in its core better. Understanding our use cases today, very important, if next step is to dig in more that’s fine.

Michael: Prospect of making it better than it is today in some ways, worse in others, some trade-offs, certainly is additional value.


## scoreAd is provided adMetadata (ortb per industry discussions; BidResponse, Bid objects). Should reportResult also receive this? How else to report an imp against brand, creative, seat, etc?

Nick from Triplelift, read us into your issue

Nick (TripleLift): It seems like the metadata is flowing into scoread. How do you actually report on this information? Industry says to ask ORTB for scoread functions but doesn’t look to be that in report results. What would it look like in the end? 

Michael: This is a deliberate difference between reportResult and socreAd. Motivation is that in scoreAd you can use large amount of info specific to user in picking for buyer how much to bid and for seller how to rank the bid. For the info that flows into report result, that info has to pass K anonymity barrier. Cannot pass it unless it is shared by large number of people. Can’t pass user identifier. Like website where they were added to interest group and where ad is displayed. Info that flows into report result includes what ad was the winner, based on that can look up information. Then there’s a second thing that flows into report results alongside ad render URL. Made changes recently. Used to be the second thing was the name of interest group, but that is subject to same K anonymity constraint. You always get render URL of ad that was winner, which we already know is safe, because ad cannot win w/out K anonymity check. If ad URL and interest group name are still K anonymous when you put them together, then we pass name of interest group into report result also. What we changed recently is there is a new piece of metadata to optionally supercede interest group name. New thing is called "buyer and seller reporting ID" to be attached an ad, which can be passed w render URL to both buyer and seller win reporting functions. That is subject to K anonymity constraint. If you pass buyer / seller reporting ID and combo is not K anonymous, browser will only pass render URL.

Next up, Jonasz from RTB House


## Jonasz (RTB House): 3pc deprecation: ramp-up duration #717 

Jonasz (RTB House): [Issue 717](https://github.com/WICG/turtledove/issues/717) to invite broader discussion and process of deprecation of 3P cookies. Right now the immediate concern for everyone is probably the 1% test in Q1; this is also our concern. It seems what happens next warrants discussion. Until recently the official Privacy Sandbox timeline mentioned the phase-out, if 1% tests go well, would take 2 months. This triggered internal discussion at our company. First concern is 1% tests may not reveal issues in broader deprecation, second if deprecation happens too quickly we may not be as successful if it happens more slowly (entire ecosystem). To maximize chances of success of Privacy Sandbox, it would be better for the 3pc phase-out process to be gradual, more prolonged, and to have clarity of the process ahead of time.

Michael: In looking at that issue 717, Isaac you also added text to the issue.

Isaac F: I agree, Xandr would like to see PPA succeed. Selfishly I do too. I think there is a way to move through privacy maybe even have better data in the long run. I’m fully behind spending my time to make this succeed. Mode B testing I don’t know that I believe we have the clarity that maybe is generally through infrastructurally. Much bigger deal than is being discussed. Difficult to go from 1% to 100% given how long each cycle will take to improve things, given this is an industry wide situation. 

Michael: Are there other Google folks here who want to answer?

David (Epsilon): Overarching Mode A Mode B, didn’t have people to talk about that. Do we go back to find that forum to talk about it? In yesterday’s IAB EU sponsored w Google folks, they said Mode B cohort will not be labeled in advance or otherwise. Is that something you can confirm or deny?

Alonso (Privacy Sandbox): The office hours piece is to expand to diff time zones. 

Michael: First, thanks for opening the issue for spelling out why ramp-up timeline that is slower might produce better outcomes to ecosystems. Happy to have it up on Github. This has already had some effect: timeline which once said cookies will be phased out over the course of 2 months now says "gradually". Gradual opens door for longer timeline but removes clarity. Apologize for fuzzy situation, more precision and clarity will make everyone’s life easier. We are eager to publish more info and will do that as soon as we possibly can. Let me be completely clear, though, that I will not announce any changes to anything on those subjects in this weekly meeting. This weekly meeting, about Protected Audience, will not be an announcements venue for timing of 3p cookies or any changes of details of testing period, mechanics, future timeline of remainder of Privacy Sandbox. Look at [www.privacysandbox.com/timeline](www.privacysandbox.com/timeline). We have agreements w regulators to update that site. Any time that changes, you will see it there first.
