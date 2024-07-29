# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday July 17, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!	 



1. Michael Kleber (Google Privacy Sandbox)
2. Brian May (Dstillery)
3. Tomer Ben David (Taboola)
4. Sven May (Google Privacy Sandbox)
5. Roni Gordon (Index Exchange)
6. Andrew Pascoe (NextRoll)
7. Fabian Höring (Criteo)
8. Russ Hamilton (Google Privacy Sandbox)
9. Paul Jensen (Google Privacy Sandbox)
10. Sathish Manickam (Google Privacy Sandbox)
11. Laura Morinigo (Samsung)
12. Orr Bernstein (Google Privacy Sandbox)
13. Matt Menke (Google Chrome)
14. Harshad Mane (PubMatic)
15. Laurentiu Badea (OpenX)
16. Ricardo Bentin (Media.net)
17. Alex Peckham (Flashtalking)
18. Owen Ridolfi (Flashtalking)
19. Rickey Davis (Flashtalking)
20. Becky Hatley (Flashtalking)
21. Tim Taylor (Flashtalking)
22. Arthur Coleman (IDPrivacy)
23. Matt Kendall (Index Exchange)
24. Matt Davies (Bidswitch | Criteo)
25. Tamara Yaeger (BidSwitch) 
26. Isaac Foster (MSFT Ads)
27. Warren Fernandes(Media.net)
28. Anthony Yam (Flashtalking)
29. Kevin Lee (Google Privacy Sandbox)
30. Jeremy Bao (Google Privacy Sandbox)
31. Achim Schlosser (Bertelsmann)
32. Luckey Harpley (Remerge)
33. Koji Ota(CyberAgent)
34. Alonso Velasquez (Google Privacy Sandbox)
35. Antoine Niek (Optable)
36. Denvinn Magsino (Magnite)
37. Abishai Gray (Google Privacy Sandbox)
38. David Dabbs (Epsilon)
39. Felipe Gutierrez (MSFT Ads)
40. Ken Gordon (Azure)
41. Jacob Goldman (Google Ad Manager)
42. Kenneth Kharma (OpenX)
43. Shafir Uddin (Raptive)
44. Maybelline Boon (Google Privacy Sandbox)
45. Laszlo Szoboszlai (Audigent)
46. Hari Krishna Bikmal (Google)


## Note taker: Tamara Yaeger	


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here:



*   Isaac Foster:
    *   Brief revisit the “coarse information sharing” thing, we had talked about setting up time but never did, all got too busy…can even answer here
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736 

*   Matt Davies: 
    *   Seek feedback on third party reporting

        https://github.com/WICG/turtledove/issues/1220  


*   Warren: 
    *   [Addition of an analytics/reporting entity to enable centralised reporting · Issue #1115 · WICG/turtledove · GitHub](https://github.com/WICG/turtledove/issues/1115) 

*   David Dabbs
    *   Request for `updateURL` processing to support the leaving/joining of negative interest groups via the [nascent header feature](https://github.com/WICG/turtledove/issues/896) 
(entered as issue [#1228](https://github.com/WICG/turtledove/issues/1228))

        In the “shell IG” pattern a buyer joins an IG with a minimal, updateable attribute set. This centralizes and defers page latency inducing processing (computing and rendering IG content) to updateURL time. (Proposed) nNegative targeting adds a wrinkle. Scenario: a buyer intends to negatively target a positive interest group. The Chrome-suggested approach has the buyer pair the excluded positive IG with a negatively-targetable ‘negative shadow.’ At site visit time, the buyer joins the browser to “shells” of “EXCLUDED_POSITIVE,” the excluded IG, “EXCLUDED_NEGATIVE,” its negative ‘shadow,’ and “EXCLUDER,” an IG that seeks to negatively target the other group. EXCLUDER’s update will configure EXCLUDED_NEGATIVE in `negativeInterestGroups[]`. If EXCLUDED_POSITIVE’s update determines that IG membership condition doesn’t hold (or the update doesn’t run &c.), the buyer has a partial, incorrect IG set.  
 
        The buyer’s ability to join or leave EXCLUDED_NEGATIVE during EXCLUDED_POSITIVE’s update would smooth this wrinkle. The primary can ensure its ‘shadow’ is always appropriately joined when it is processed. 

    *   Request for nascent header join/leave capability to support <code>[fetchLater API](https://developer.chrome.com/blog/fetch-later-api-origin-trial)</code> 
        (entered as [comment](https://github.com/WICG/turtledove/issues/896#issuecomment-2233667864) on #896) 
 
        Chrome is [migrating](https://issues.chromium.org/issues/40236167) keep-alive request handling from the “renderer” (front-end) to the “browser” process (back-end). [Explainer](https://docs.google.com/document/d/1ZzxMMBvpqn8VZBZKnb7Go8TWjnrGcXuLS_USwVVRUvY/edit#). Attribution Reporting API (ARA) supports event and trigger header registrations on background requests, and this will move to the browser. ARA team has [extended that hook](https://issues.chromium.org/issues/40242339#comment48) so that <code>fetchLater</code> API requests will also be able to set ARA headers. Requesting that Protected Audience header processing also support <code>fetchLater</code>.  



# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!! :) 


# Notes 

[Github: Third Party Reporting #1220](https://github.com/WICG/turtledove/issues/1220)

Matt Davies (BidSwitch): 

Applies strongly to BidSwitch but sure many others in the supply chain would require access to this. Idea is to additional reporting party URL w/in PA response that comes back from oRTB where credentials are provided to the SSP / seller, to then be able to create their own PA auction. Idea would be to add a few extensions for 3P IG signals (HTP, URL, DATA URL) which would allow 3Ps to have access to auction results. You may have a direct participant or non-direct chain, it might be that the IG allows DPS A to trade with SSP B directly. The same IG may also be used on another transaction that goes through BidSwitch or 3P operator where they need to register that they are in the chain. It would be useful to understand if it’s a worthwhile use case.

Michael Kleber (Chrome): Let me ask some questions to understand the relationship of parties. Trying to scope the story; in the PA world, we tend to think about there being 2 parties, buyer and seller, and we have possibility of component vs top-level seller, but don’t need to worry about that now. The way that we think about things like how do ppl get onto the pub page? The pub page is owned by the pub (1st party) and then the fact that anyone else is on the pub page is the result of some delegation / invitation, so we think about it as the pub invites some list of sellers to sell inventory - SSPs - who then invite buyers to buy into their auctions. If pub delegates to seller, and seller to buyer… where does BidSwitch use case fit in chain of delegation? Who is it that invited BidSwitch to be a participant in this auction? 
 
Matt: Depends how much you want to separate oRTB from PA. Basis is that oRTB side of equation starts that process; oRTB request goes to DSP, they send a response with PA credentials, from there seller configs; after that it’s sent to the seller and the auction is between just the buyer and seller. Part of where BSW comes in is in oRTB; many DSPs will have 15 top supply partners w direct integration, but work with 40-50 smaller entities like BidSwitch or others e.g. Magnite. When you see application traffic you see SSP partners inviting other SSPs. There are chains of oRTB transactions where it goes through, but impressions and spend need to be recorded; agreement w DSP / SSP. Required to have certain live accurate data around giga transactions. For sake of argument, imagine SSP ABC, and XYZ DSP. They run through BidSwitch; so the SSP sends oRTB request, transmitted to dSP, the DSP would respond via BidSwitch about auction participation. We will then pass to SSP, who will create component auction, which will then invited DSP directly in component in auction. Once impression created, noone in the chain would be able to record and have access to transactions. 

Alonso Velasquez (Google PSB): Ultimately we need to understand whether this reporting is in the service of the buy or sell side, or representing both in some cases as it may be with BidSwitch. I gather that the way to scope this problem is, that there are adtech parties that are not the seller or buyer and are still in the service of the Advertiser or Publisher, that certain reporting capabilities are missing from the PA auction  in order for these parties to get the reporting needed to render their services to the Publisher or Advertiser. 

Matt: While BSW can’t run component auction….

Michael: It seems one natural answer is, why doesn’t BidSwitch run a component auction? If BidSwitch is a player that gets some amt of awareness of demand, and propagates demand to some collection of buyers that buy proxied through BidSwitch, then why isn’t it that BidSwitch is a component buyer in PA auction directly? What would prevent BidSwitch from being component seller?

Matt: But the SSP we’re repping would also want to work their own component auction. There’s only top-line level seller (GAM / Prebid), then pub , seller, maybe exchange, buyer, etc. Only 2 components can run a component auction and no one else (?) 

Michael: Agreed, but why can’t we flatten that out and have BidSwitch be a component buyer directly in top level?  Ah, but you’re saying BSW is only here because SSP invite, so SSP would get cut out of reporting chain?

Matt: Yes, that would be the problem. There might be data providers that may need to also get access to this data. IGs belong to the DSPs not us, and we’re not privy what DSP wants to run in campaign at any given time. 

Michael: It seems like everything you are trying to do involves some kind of winning bid that went through BidSwitch during OpenRTB phase; that annotation is something winning component SSP is in good position to add, if they choose to. They know everything about their supply lines, when winning SSPs evaluates each bid they know they got it because of BidSwitch’s involvement in oRTB side. It’s possible for the reporting to happen, but only if the component SSP does the right thing. What if SSP doesn’t annotate the bid correctly? 
 
Matt: We wouldn’t want to rely on 3P for access…

Michael: But isn’t it the SSP that invited BidSwitch? 
 
Matt: Yes but the tech or them to be able to feed the info back to us, would they add us to reportWin? It would be very useful, but it would also be useful to be able to state that we were def part of the auction instead of relying on the original SSP to confirm… what if it doesn’t happen 100% of the time? 
 
Harshad Mane (PubMatic): Another use case. Currently pubs rely on component sellers; let’s say pub is working w 5 SSPs, pub needs to combine all these reports offline. Pub doesn’t have way to get real time reports from auction.

Michael: I agree we have been talking about getting direct reporting to the pub. Seems different from Matt’s use case; we all know why pub is entitled to reporting. They’re top of the chain of delegation of right to be on page, but now we’re talking about more steps removed from that. How do we even know who the parties are who are supposed to get reporting seems like the difficult part. Difficulty is – if BidSwitch is only in the auction because an SSP invited BidSwitch, then that SSP is the channel that sits between you and the browser. If SSP is not trustworthy to include BidSwitch, then anything I can do as a browser would only be in the reporting configuration proxied by the SSP. 

Brian May (Distillery): There is a question of trust, but also of people making mistakes and not recognizing it. If the SSP is missing something that needs to be visible. 

Alonso: Might be worthwhile to say we have infrastructure for both sellers and buyers to declare 3P destinations and which data from the auction they can receive (fenced frames, ad reporting infrastructure). Config needs to be done by seller / buyer still, but it’s feasible. [Additional edit from Alonso: we know that the ability to declare additional destinations is present for the buyer and not the seller, so this is perhaps an area to get feedback on below]. For more than a year we have known of 3Ps that need to get reporting; about a year ago enhanced flexibility for different kinds of reporting. Will share the explainer here: https://github.com/WICG/turtledove/blob/main/Fenced_Frames_Ads_Reporting.md . I encourage everyone to consider how far the reporting capabilities listed here from what you’re envisioning as the need and give us feedback on that. 

Warren (Media.net): Makes sense on buy side, but gap is on sell side; we don’t have a similar / easy way to have list of 3P behind use to get similar reporting. There’s no easy way built into the system for sellers to do what buyers can do.

Michael: So the problem is that sellers would want an endpoint for reports to get sent to, but not be sent there all the time. There should be a step which makes it different from pubs getting duplicate reports, which could plausibly happen all the time. Somehow someone needs to trigger a win to have an additional party to get dupe copy of reporting.

Warren: How do we trigger it, even when we figure out whose impression it is? 

Michael: As a browser person, I have to ask: why doesn’t the SSP server that receives the win report serve a response that is an HTTP redirect to deliver that report to another host in addition? Why can’t the SSP doing a browser redirect solve the problem?

Matt: In our experience HTTP redirect leads to big discrepancies, 2-3%, which disappears when you have server to server APIs, then it’s 0.1%. It also leads to extra processes. We trust our SSPs implicitly, but as with anything, it is easier to do inhouse. Imagine chain, DSP send bid response to us, imagine there is something that shows which request it should be applied to.

Roni (Index): Problem is if BSW represents buyer origin, there is nothing in reporting results to show it came from BSW. There is no notion of a  “representative”.   reportResult() only has IG owner and renderURL – it would be indistinguishable from a bid from the ultimate buyer that BSW is representing.  So it’s not just a matter of calling sendReport() multiple times – there’s no way to know at reportResult() time that BSW was involved.

Michael: If I understand correctly, the contextual response that was put there by BSW as part of OpenRTB contextual response, that should be enough info so that the SSP partner can look at contextual response as part of seller signal, and it should know all the bids coming from buyer, are bids that are in auction because of BidSwitch as an intermediary. Is that right? That the seller and buyer and seller signals are enough for SSPs to figure out whether to trigger BidSwitch reporting.

Matt: Theoretically, unless the SSP knows which IG group won. They won’t know which path it went to.

Michael: But they know who the buyer is, sounds like there is enough info.

Matt: We don’t add anything to to PA response, we just send it back to them. There is nothing in that response that we would add to; if we were able to give a field to fill in in advance, they can put into that auction config to get automatically called when impression occurs. 

Michael: Part of your GitHub proposal is about wanting some new JS worklet coming from a new party that gets to execute on device.  For that part of your request, as browser we’d prefer not to have additional worklets to contribute additional JS; possible but worst case scenario, in making the API more heavy. Having additional destinations is more appealing if the report can be built by the browser based on some static declaration, from an API design POV. Sending a browser-crafted report is a lot nicer than downloading someone’s JS to run a worklet. It would be great to know if there is a version of this proposal that could be implemented as a static config. I’m unclear on how triggering works, how the SSP can know which bids should get BSW reporting and which do not. Let me re-read notes and responses; if it’s really just a function of buyer / seller component auction and something that can be written in OpenRTB response.

Roni: This is not merely about notifying BidSwitch, the seller needs to know that BidSwitch is involved at all stages.   SSPs need to collect money from BSW in this scenario, so everything in the APIs – KV, scoreAd, reportResult, etc. – would need this awareness.

Matt: At end of day we will still need to pay the SSP

Michael: No one mentioned money changing hands goes through BidSwitch, not only an info flow.

Matt: We are the proxy, we would invoice and pay SSP out is how we operate. We would be in billing chain as well. The only other option would be for SSP to be calling with new URL…

Michael: The fact that a particular bid / buyer was proxied through BidSwitch, that info is knowable inside the auction, ScoreAd function, report win, report results, hopefully that’s still the case. Roni said that info needs to be known in the seller's key value server, which right now learns just the render URL, that adds further complication. Also Roni’s question is another complication – what if the same buyer has a direct relationship with the SSP AND a connection through BidSwitch?

Roni: Nothing prevents me from onboarding a direct relationship, that can happen any day. I can map Brian, tomorrow it may be indistinguishable if Brian goes through Matt. The same IG fires, the same IG bid gets submitted, so I would continue to charge Brian. By design once that bid wins, that relationship is lost. 

Michael: It sounds like you’re increasingly making the case that the right answer is for BSW to be a component SSP.

Matt: If we could do that, that would be amazing and we would build it out; we’ve been looking at it from the beginning, but it wasn’t in the spec. If we could run a component auction, passing details back and forth, that would create a third level auction. 

Michael: Point is that maybe we can flatten the tree and BidSwitch can be a component auction. Anyone bringing demand is a component auction. 

Warren: Comes back to previous issue about SSP then not having reporting… one or the other

Michael: BidSwitch could send it reporting to the SSP that invited it to the auction.

Brian: I think BidSwitch is more of an alias than a replacement.

Paul (Chrome): What if 2 component auctions

MIchael: I was thinking more linked component SSP that runs component auction could list each buy as a buyer or buyer-becaeuse-of-BidSwitch, making that level of aliasing more obvious. Then winning bid would come from buyer X or buyer X-because-of-BidSwitch. IG would belong to buyer X. If same buyer has both relationships, does that buyer bid twice in same component auction?

Roni: Nothing prevents that from happening, multiple bids into the same auction. The KV doesn’t get the IG owner, I have to guess who owns the render URL. Now it would be another level of indirection. It’s not just who owns the IG, it’s who’s representing the buyer origin commercially?

Brain: Suggest for Matt to mull over and come back 

Michael: If we propagate the buyer as part of reporting, and maybe each alias /annotation of buyer gets automatic reporting, maybe it’s enough to address this. 

Paul: What reporting are you looking for?

Matt: Impression, spend, and click – did a billable event occur? From there we can run at least a record of the transaction occurring. 

Brian: If we’re reporting 3P data there needs to be consideration that the data wasn’t tampered with.
