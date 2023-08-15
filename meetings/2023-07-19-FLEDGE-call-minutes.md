# Protected Audience (formerly FLEDGE) WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday July 19, 2023


## Attendees: please sign yourself in!	



1. Paul Jensen (Google Chrome)
2. Dennis Yurkevich (AirGrid)
3. Youssef Bourouphael (Google Privacy Sandbox)
4. Roni Gordon (Index Exchange)
5. Brian May (dstillery)
6. Sven May (Google Privacy Sandbox)
7. Sid Sahoo (Google Chrome)
8. Nick Llerandi (Triplelift)
9. Andrew Aikens (TripleLift)
10. David Dabbs (Epsilon)
11. Fabian Höring (Criteo)
12. Andrew Pascoe (NextRoll)
13. Gianni Campion (Google Ads)
14. Martin Pal (Google Privacy Sandbox)
15.  Isaac Foster (MSFT Ads)
16. Tamara Yaeger (BidSwitch)
17. Marco Lugo (NextRoll)
18. Matt Menke (Google Chrome)
19. Priyanka Chatterjee (Google Privacy Sandbox)
20. Arun Nair (Google Privacy Sandbox)
21. Alex COne (Google Privacy Sandbox)
22. Danny Rojas (Google Chrome)
23. Stan Belov (Google Ads)
24. Alonso Velasquez (Google Chrome)
25. Russ Hamilton (Google Chrome)
26. Jeff Wieland (Magnite)
27. Manny Isu (Google Chrome)
28. Caleb Raitto (Google Chrome)


## Note taker: Martin Pal


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here — no agenda no meeting!]



*   [Isaac Foster] 673 - 675 (IG Filtering via OpenRTB, Capping, Topics)
*   [Isaac Foster] 607 (XD Targeting, few minute discussion)
*   [Isaac Foster] TEEs in Azure Public, Azure Private, and Xandr DCs (I will find the appropriate tickets and inject here)
*   [Gianni Campion] https://github.com/WICG/turtledove/issues/475 (deleteAllIGs api change)
*   [David Dabbs]  Issue [319](https://github.com/WICG/turtledove/issues/319), _Supporting Negative Interest Group Targeting_


# Notes

Isaac Foster 673–675: I filed a bunch of issues, didn’t expect all to make it to the agenda. Was there further discussion on on-premise TEEs. Criteo and Xandr brought this up.

Arun Nair: Some folks who thought about on-prem not present. Current POR is that for Privacy Sandbox services we’ll support AWS and GCP. DIscussing with Azure. We do not have a plan to extend this to on-prem right now. Our position has not changed since patcg.

Isaac: Can you delineate what we believe public cloud has that is secure that private one doesn’t. What are the physical infrastructure requirements? Lots of subtleties. Treat this as a longer term discussion. 

Jeff Wieland: Add what is bad about on-prem. Using a public cloud is very costly. Have the reasons an on prem solution is not allowed written down in a public place.  

What are the security requirements needed by a public cloud to be approved?  These are not available publically to my knowledge. 

Arun: Fair points. We’re thinking about on prem on our side as well. To share our position, AWS, GCP, Azure. Based on security practices of these cloud providers to secure data. Includes physical access, policies. 

Jeff Wieland: you should be open to the notion that a non-public cloud can qualify.

Isaac: What if someone builds a cloud called AppNexus2 (Isaac Edit: AdNexus2, actually, AdNexus->AppNexus->Xandr->whatever we are now) who offers TEEs (and general private advertising cloud infra). 

Arun: I’m hearing that publishing a clear set of requirements would help. Sounds reasonable. We don’t have the right folks present. Just to clarify, AWS only a year ago, today GCP, Azure down the road. We’re open to adding more providers. 

Martin: considering on-prem solutions requires a deep dive into what else the machines are doing and what the hypervisor looks like.  What level of intent can we consider?  Deep dive on the stack is required.

Isaac: If we don’t know what the requirements are, we don’t actually know if public cloud implements them. The set of requirements would be pretty stringent I assume.

Brian May: From PATCG meeting: how do we ensure a cloud provider doesn’t serve their own advertising business. FOlks in the ecosystem would be interested in that. One problem with on-prem is that it’s not only the TEE, but also environment the TEE is in (software and physical). We should define TEEs as necessary for advertising but not sufficient.

Stuff happening between the device (point of origin) and the TEE. Needs to be monitored. If that link is not secure, we leak privacy. 

Paul Jensen: For B&A server design, my primary concern in the API design was what we’re exposing. Even sizes of packets may leak info. I used to work on the Chrome networking team. http2/http3. We wanted everything to work with tls, best security practices. 

Brian May: I have a background in netops. Attributes of the traffic can expose info about the traffic. 

Have you considered the requirement of amazon advertising not running on aws / google ads on gcp? 

David Dabbs: How does this fit in if cloud and adtech are the same company..

Arun: Paraphrasing Michael Kleber. At this point in time Amazon can use AWS and Google can use GCP. Mitigation is that the cloud provider is sufficiently independent and has sufficient reputation on the line. 

Isaac Foster: No consensus necessarily, but backing up Arun. Folks from MIT, Martin Thompson (?), Kleber seem (Isaac edit: I was just backing up the notion that there was a discussion a few different fronts, a) is public cloud the right standard b) a little bit on what that standard might include, in particular around physical security of machines and premises and c) whether any tech should be able to use their own servers, regardless of whether those servers are public cloud or no. There was not any “established consensus around any of those, Kleber said he believes (or is at least taking the position) that Google can use GCP and more generally a tech can use their own servers. I would say there was “a lot of folks wanting to discuss it more”)

Arun Nair: &lt;scribe missed>

J Wieland: I find the position [that Google can use GCP and Amazon can use AWS for its B&A] incredible. That Google and Amazon can use their own cloud but you don’t allow other ad tech companies to use their on-prem. CMA will take note.

Isaac Foster: Publishing on-prem requirements would help resolve this.

If the standard is that you don’t run on your own infrastructure. Let’s come up with a set of standards. If aws/gcp/azure are the only ones then so be it. (Isaac Edit: Isaac was agreeing with Jeff’s position but saying we need to split this into a number of questions 1) is “public cloud” the right technical specification 2) if it’s not, what is the right technical specification and with that 2a) should that include a requirement that the host of an ad tech’s TEEs must not be the ad tech and the 3) if it really is the case that those can only be accomplished by GCP, AWS, and Azure…well, OK I guess)

Brian May: Martin Thompson said in patcg that large cloud providers have strict security protocols.Suggest we identify the attributes being identified as “cloud providers” specifically so that others can develop compliant services.

Arun: Going back, publishing reqs makes sense. When we’re ready to publish we’ll give heads up.

Jeffrey Wieland: we’re racing a deadline. We don’t have requirements published, and it is hugely problematic for smaller companies. 

Paul Jensen: On-device version of Protected Audiences doesn’t require cloud tees.

Isaac: Let me characterize this a bit differently. COnversation w. Alex & Kleber at patcg. Going on a tangent. Agree the API is reasonably well defined. If 1.0 is going to satisfy competition requirements. The implementation may not be where we want to be. We can say API is defined, but infrastructure is not fully in place. (Isaac Edit: lot of nuance here so want to amend: I think we’re all having emotional reactions to the “readiness” discussion, which I think has two-to-three dimensions to the it: (1) is whether the API as imagined is a reasonable v1.0 replacement for 3rd party cookies, and while I have my own opinion on that (which is, ehhhhhh mayyyybbbee), I’ll say that is a different kind of question then the other two; (2) is the infrastructural one, and my point is that the logical API of a system being at v1.0 but the infrastructure and operations of it being &lt; v1.0, does not add up to the system being at v1.0…in this case I’d guess that the reason B & A came about is that we’re realizing Edge Compute in general isn’t ready to meet ad tech functionality and SLAs, and so now we’re trying to get B & A sorted out and it’s not yet; (3) is that we’re coupling the initial production release of this system preeeettty tightly to the deprecation of the old system (3PC), which means we’re leaning heavily on this new system working out of the box, and (2) makes me (and I think others on the call) quite concerned, even if we ignore differences on (1)).

Andrew Pascoe: For Protected Audience there’s on device path forward. Our problem is that the SSP decides on B&A vs on-device. Hence a DSP needs to implement both. If the SSP declares B&A, then DSP is required to set up B&A stack in public cloud.

Paul: Right now, on device is the path forward.

Arun: To Andrew’s point. If SSP sees a mix of DSPs on-device and B&A, they can split auctions. Can run a component auction – one on-device one B&A, then merge results. THis architecture could accommodate both on-device and server DSPs. WOuld love to hear feedback from DSPs on this.

Isaac: Getting into deep things. Challenge/reaction: one thing if we release the APIs and build towards a competitive solution. But we’re coupling this to 3pcd. Jeff: things are still evolving. Diagram on the page is reasonably fixed. Problem is the hard coupling and sequencing with 3p cookie deprecation. Physical infrastructure and operational things. WIll discover issues. Fix cycles pretty long. This may be a disconnect ppl are feeling.

Brian May: some of us at smaller adtechs are feeling severe resource crunch.

Paul: Thanks Isaac & Jeffrey. Arun has an AI to give clarity on requirements. Isaac has posted other issues.

Isaac: let someone else go. We can discuss my other issues next time.

Gianni, Issue 465. Deleting all IGs.

Gianni: 1 minute summary. User goes to advertiser many times. Advertiser polulates many IGs. Then we want to remove IGs. That requires keeping track of which IGs are on device. This is doable but not practical. Don’t want to build infra to keep track of all IGs. On-server and on-device state may get out of sync. Or, tagging fails for some reason bust server doesn;’t know. Or, we ask Chrome to delete IG, but delete fails.

I’d like to request a feature that drops all IGs from a given origin, except a few. Thoughts?

Paul: You want an API that would leave all IGs from the origin that are owned by one owner. 

Gianni: yes, except I want to be able to specify “leave all, except for IG 7”. Can call it leaveandJoin() if you like.

Paul: From privacy angle this seems OK to me (could implement this via 1p cookie)

Brian May: sounds familiar from other domains, such as databases. Given the lack of insight into what’s on the browser, such primitive might be useful. It would be a swap – replace what’s there with a new set. I think having the option of excluding existing IGs might create a potential for information leakage and we should only allow a complete swap.

Paul: question for Gianni. The API would remove all IGS from given site and given owner, except for an optional list to be left alone. What if some IGs on the list are from different origin/owner.

Gianni: I’m thinking mostly about “group by origin”. Anything from different origin is to be left alone.

Paul: anyone else want to comment? Implementation wise, this seems not difficult to implement. There may be a function that kind of does this.

Paul: David, Issue 319, negative targeting.

David Dabbs: I pinged Orr Bernstein, Max Orlovich, the two engs who proposed this. Seems like something that’s moving forward? I’d like them to talk about it.

Paul: Orr, max may not be on the call. I’ve helped design the feature. This is about negative targeting. IGs are for positive targeting. Steven brought up negative targeting. Ex: don’t advertise to ppl who already signed up. Put them in a ‘negative’ interest group. Can a bid be submitted at auction time.

The SSP would pass bids into runOneAuciton. In Protected Audiences, assumption is money will change hands. Normally bid comes from a bidding script from an origin that is expected to pay money. We use TLS to verify identity of the bidding script. 

David: want to plant the seed to enhance the feature. Wafer thin interest group. Name and bidding URL. The IG will have no ads. What about a header based interest group. Set a tombstone. Header bidding can add negative targeting.

Paul: What would be the advantage of header vs js api. 

David: you can do it with a pixel rather than javascript. ARA uses http headers, shared storage may too.

Brian May: http headers are more trusted – more under control of endpoints and harder to manipulate.

Concern with negative IG: potential to leak info about the browser. Leaks info that this user is being left out. Negative targeting may ‘cast a shadow’ visible in logs.

David: ads being served are not interest targeted. Based on IG data, but not the same way. I’d like to be able to engage on this.

Paul: idea that iframes have significant cost resonates with me. Process creation for new render. I don’t know if this ask is specific for negatively targeted IGs.

David Dabbs: ability to manage IGs via http headers without the user needing to visit the advertiser site sounds useful. 

Paul: recommend filing an issue for the idea of joining an IG via http headers. There may be issues that I’m not seeing immediately. 

David: I’ll file  an issue. 

Paul: Worth filing. Need to think through how policies will be applied.
