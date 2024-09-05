# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Sept 4, 2024


To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!

1. Paul Jensen (Google Privacy Sandbox)
1. Luckey Harpley (Remerge.io)
1. Ricardo Bentin (Media.net)
1. Harshad Mane (PubMatic)
1. Matt Kendall (Index Exchange)
1. Arthur Coleman (IDPrivacy)
1. Pooja Muhuri (Google Privacy Sandbox)
1. Matt Davies (Bidswitch | Criteo)
1. Ivan Staritskii (Bidswitch | Criteo)
1. Laurentiu Badea (OpenX)
1. Kevin Lee (Google Privacy Sandbox)
1. Laura Morinigo (Samsung)
1. Alexandre Nderagakura (Not affiliated)
1. Yanay Zimran (Start.io)
1. Matt Menke (Google Chrome)
1. Brian Schmidt (OpenX)
1. Garrett McGrath (Magnite)
1. Chris Nachmias (Flashtalking)
1. Shivani Sharma (Google Privacy Sandbox)
1. Alonso Velasquez (Google Privacy Sandbox)
1. Lydon, Jason (Ft)
1. Josh Singh (MSFT Ads)
1. Alex Peckham (Flashtalking)
1. David Tam (paapi.ai)
1. Paul Spadaccini (Flashtalking)
1. Joshua Prismon (Index Exchange)
1. Shafir Uddin (Raptive)
1. Yanush Piskevich(MSFT Ads)
1. Jeremy Bao (Google Privacy Sandbox)
1. Abishai (Google Privacy Sandbox)
1. Jeroune (Google Privacy Sandbox)
1. Kenneth Kharma (OpenX)
1. Andrew Pascoe (NextRoll)

## Note taker: Orr Bernstein	/ David Tam


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here (meetings with no items may be canceled)

* [David Tam] Add seller information to the KV server request - https://github.com/WICG/turtledove/issues/1249 
* [Matt Davies] Follow up to https://github.com/WICG/turtledove/issues/1220 to understand the next steps in getting reporting for third parties outside of the component auction but within the billing chain 
* [David Dabbs] Chrome Status indicates that Chrome M130 will ship ~mid-October with Protected Audience Bidding & Auction Services enabled by default.  The active (extended) Origin Trial runs through M129. Can you speak to this ‘release,’ its parity with on-device, &c, or is the PAS call series every other week after this call the place to inquire?

# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!! :) 


# Notes 

Announcements
* TPAC in about a month’s time
  * We’ve requested a meeting to talk about Protected Audience API and Microsoft’s Ad Selection API

## Add seller information to the KV server request - [Issue 1249](https://github.com/WICG/turtledove/issues/1249)

David Tam: One observation - what we want to be able to do is to bid from certain sellers from the buy side. While the seller can determine who the buyer is, vice versa is also useful. Buyer may want to bid for certain sellers. Request is “accepted” - only question is, when is it likely to be supported?

Paul Jensen: You mentioned that buyers may need to bid based on hostname of the publisher? The request is about the hostname of the seller.

David Tam: Yes, for the moment, it has the hostname along with some of the keys for the macros. What we’d really like is to see who the component seller is in that case along with that request. The way it’s proposed is as an attribute on the InterestGroup. We avoid unnecessary bits.

Paul: Not in the IG. 

David Tam: That’s correct. In the IG, we’d add to the IG a list of sellers, and prioritize those you want to buy from.

Paul: The priority vector - what you want to bid and the priority of that bid.

David Tam: Yes, and sometimes not to bid.

Paul: Two different sections per buyers. The key section - the DB lookup of a key. And an IG section. The IG section is the one with the priority vector in it. The priority vector allows you to specify different values in a vector, which gets dot-producted, and if the priority is negative, then the IG doesn’t get to bid on a particular request. One of the changes is to allow the buyer to specify which component seller each of the elements in the priority vector applies to.

That seems fine. We don’t want to duplicate the request for different sellers. Two parts to this. The response part sounds fine.

David Tam: Don’t really have a preference, just want this use case.

Paul: I think we could add that support to allow the priority vector response to indicate which seller it applies to.

David Tam: Yes, that also works.

Paul: That would be fine then. Michael is aligned with that, and Matt also said it sounds reasonable. Do you need the request to include the list of sellers to not?

David Tam: The reason for having it in the IG is to minimize the number of bid requests that we’ll receive, or the number that hits the K/V server. Has a number of advantages. But it could be part of the K/V response instead of the request. We could make different logical decisions based on how we intend to bid. Also has certain advantages. Having in the IG minimizes the number of K/V requests.

Paul: What do you mean in the IG?

David Tam: This is the list of sellers for this IG that you want to buy from. If we happen to see a request from a seller that’s not on that list, we don’t bid for it, or we don’t see that request at all.

Paul: That’s fine. So the only change needed from the browser is to be able to break up the priority vector by component seller?

David Tam: Yes.

Paul: I’d be interested to hear if others are excited about this as well, to help us prioritize this.

David Dabbs: Will folks like David who want to use this use the trusted signals slot size mode to do this? I don’t see us changing the way we bid, and wouldn’t want the seller to mess with caching. You’d want a way to opt into it and not have it happen uncontrolled.

Paul: Last part of the conversation is that we don’t have to modify our request. David had the idea that you could include in the IG the sellers that you’re bidding against.

David Dabbs: So, you’re taking up space in the IGs. Why wouldn’t it be in your bidding logic?

David Tam: More unnecessary bidding, invocations of the generateBid() function. If you don’t intend to bid, why are you invoking it.

David Dabbs: Bigger cost to you is the trusted signals call.

David Tam: Yes, if I don’t intend to bid, I don’t want the request.

David Dabbs: But you’ll still get the request.

David Tam: Not if it’s in the InterestGroup. Then we don’t get the request at all.

Paul: Specific priority vector would be specific to a given component seller.

## Follow up to [Issue 1220](https://github.com/WICG/turtledove/issues/1220) to understand the next steps in getting reporting for third parties outside of the component auction but within the billing chain

Matt Davies: 5 or 6 meetings back, chatting to Alonso, the addition of adding 3rd party reporting.  Some one in the middle is part of the bidding chain, but not necessarily the only route to traffic.  Some way to go through bidswitch.  Can SSP and DSP on behalf of bidswitch, require on 3rd party for accurate reporting.  Feedback on simplified option.

Paul: Bidswitch as component Seller.  Situation when bids pass through bidswitch.  Beacon that allows one URL per bid.  

Matt Davies: Potentially get a worklet to run.  Basically impression and click cost data. Verified by the SSP/DSP so we can bill on that transaction.  It does not make sense for us to be a component Seller.  Then SSP cannot be a component Seller. Just want to receive accurate information going through Bidswitch.  When going through Bidswitch we can simply add our reporting URL.  This is the Bidswitch reporting URL then we would have access to the data.  

Shivani:RegisterAdMacro is a functionality that is available.  Key value pairs can be registered by the DSP and reported in an ad frame to a URL which could be the 3P URL.  That request can go to Bidswitch.  It does require coordinate between Bidswitch and buyer.

Ivan: Requires some initiation with the buyer.  Seller and Buyer agrees on using event name impression.  Can trigger an impression event.  If go via Bidswitch we can trigger the impression event.  To keep the logic of impression inside the worklet.  

Alonso: Summarise the gaps.  Mostly a problem on coordination between Bidswitch and the buy side.  

Matt Davies: In order to have accurate tracking.  Be independently track impression from a DSP.  There is no way to coordinate with the DSP.  If something went wrong then we Bidswitch need to be able identify what went wrong.  When it goes through Bidswitch then it needs to be done by us.  It could go through a SSP, but it ought to go through our logic code.  

Alonso:Makes sense.  Infrastructure we have today for 3rd parties.  Rely report for the larger buy side.  When there are massive campaigns are run.  They have 3rd party reporting needs.  DSPs fulfill the campaign etc.  In fenced frame reporting, quite useful in supporting 3rd parties.  Somewhat similar capabilities to sell side reporting.  Gap maybe, from a workflow, for you Bidswitch as a reporting vendor.

Matt Davies: We need to bill and invoice.  From a purely financial trade this breaks.  This causes problems.  Either we disappear as we become irrelevant.

Alonso: Here we have a reporting Use Case that is not clearly for Buy or Sell side.  This particular Use Case is a hybrid situation.  Similar of adjacent realm needs to be encourages.  Different companies have a different role.  We can internally understand demand and priorities.  Let’s see what are the demand.

Matt Davies: We are somewhat unique.

Paul: Which buyers signals would that be.

Matt Davies: What happens, DSP is connected to SSP via Bidswitch.  DSP would respond with  and we would submit this to the SSP.  We have the option from a logic point of view, this went via Bidswitch. The details is that we could place a reporting URL. Through conversations in the past the buyer can insert multiple reporting URL. It would be simple if that bid went via Bidswitch.  This went via Bidswitch.  When we receive the IG bid we have theoption we can add x t this.  If this goes down the chain then we know that this took place.  Then we get the reporting on the impression.

Paul: Which auction, PA or contextual.  Which buyer is adding this.

Matt Davies: Open RTB we get contextual .. The IG for the PA auction, what ever response we get from DSP we pass to the Seller.  It is part of PA auction.  ORTB we are fine that flow is sorted.  We would know automatically.  When it comes to PA version the reporting is only sent to the reporting API.  

Ivan: FContextual bid, in the seller extension provides the reporting URL. The worklet of seller side send report to Bidswitch.  It is only possible to call only once. We cannot use this functionality.  Discuss with SSP to send report on our impression URL.  

Paul: Last call in July. Bidswitch works with a whole bunch of DSPs. Some component seller would need to on the buyer list.  Bidswitch conveying this list of buyers to seller.

Ivan: Buyers that are willing to participate,  SSP create the auctions. 

Paul: You give the list to Sellers.

Ivan: Not contextual bids.  perBuyerSignals to each DSP.  

Paul:Assuming integration with SSP, providing a list of DSPs as potential buyers. Bids coming from these buyers would go to Bidswitch.  

Ivan: Allow to call sendReportTo function multiple times.  Buyer to add another event if possible.  Another way to sendReportTo to call the event name only once but multiple times.  One for th SSP another for Bidswitch.  Also we can solve reporting without complicated mechanics.  

Paul: Sounds possible. Do foresee that may not work great to parallise with PA auction.  

Ivan: We can provide a list of buyers.  Run auction with a list of our buyers.  

Paul: I think it could work.  If we avoid some latency.  

Shivani: SQuestion, if there was support for 3rd party reporting then what about ad rendering.  

Ivan: Impression that happen inside of the creative is important that happens inside the worklet.  Central reporting tool is optional.  

Shivani: Without any changes …. If this is registerAdBeacon with a unique event name e.g. “impression_3p”, called inside the ad frame then there are no changes.

Ivan: It is impossible to agree something with the DSP but hard (possible) to coordinate with SSP.

Shivani: DSP calls the event, then it is hard to coordinate.  We also have automatic event.  If it is part of the ad rendering request, then entity that this registered for automatic beacon thn they will get the event.

Ivan: We are only interested in the impression, not click.  

Paul: Ponder about the privacy properties to address this.  Not sure about the implications. 

## Chrome Status indicates that Chrome M130 will ship ~mid-October with Protected Audience Bidding & Auction Services enabled by default.  

Paul: When is Chrome supporting BA services.

David Dabbs: It is on default version 130. Is this real?

Paul: Chrome dev stuff is forward looking.  I did not put the 130 in there.  Do not want to go past 12 months on the testing. Discussing this soon.  We will have to make some announcement soon.

David Dabbs: Process question.  There have been a lot of changes to the web API.  

Paul: Happy to talk about the web API. in terms of published timeline.

David Dabbs: Sometime in Q3 of this year.

Paul: Timeline for server side, will place a link.
