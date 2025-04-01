# Protected Audience WICG Calls: Agenda & Notes


Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Oct 16, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!	 



1. Michael Kleber (Google Privacy Sandbox)
2. Roni Gordon (Index Exchange)
3. Luckey Harpley (Remerge.io)
4. David Dabbs (Epsilon)
5. Harshad Mane (PubMatic)
6. Laurentiu Badea (OpenX)
7. Fabian Höring (Criteo)
8. Isaac Foster (MSFT Ads)
9. Orr Bernstein (Google Privacy Sandbox)
10. Diana Torres (MLB)
11. Aymeric Le Corre (Lucead)
12. Courtney Johnson (Google Privacy Sandbox)
13. Paul Jensen (Google Privacy Sandbox)
14. Alonso Velasquez (Google Privacy Sandbox
15. David Tam (paapi.ai)
16. Jeremy Bao (Google Privacy Sandbox)
17. Sathish Manickam (Google Privacy Sandbox)
18. Miguel Morales (IAB TechLab)
19. Brian Schneider (Google Privacy Sandbox)
20. Arthur Coleman (ThinkMedium)
21. Abishai Gray (Google Privacy Sandbox)
22. Sid Sahoo (Google Privacy Sandbox)
23. Hari Krishna Bikmal (Google Ads)


## Note taker: Orr Bernstein 


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here (meetings with no items may be canceled)



*   Anthony Yam (Flashtalking)
    *   Discussion of / update on this issue: https://github.com/WICG/turtledove/issues/1028 
    *   [kleber: Maybe continue with topic in the future — there was still more discussion to have]
*   Pat McCann: https://github.com/prebid/Prebid.js/issues/11730 and solution at https://github.com/prebid/Prebid.js/pull/12205 [also resolves https://github.com/WICG/turtledove/issues/1093 and https://github.com/WICG/turtledove/issues/851]
*   Jeremy Bao: Deal ID Discussion - questions for DSPs/SSPs/anyone else with insight:

    1) How many Deal IDs do you usually see in a \*PMP Auction on average?\*


    2) How do you currently handle processing many Deal IDs in oRTB today?


    3) We understand that Deal IDs are used to represent a business agreement (for a PMP, Preferred or Programmatic Guaranteed Deal) or to label traffic (for reporting only purposes) - are there other uses that have not been surfaced so far?


    4) For Deals reporting only use cases is Aggregated Reporting sufficient?



# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!


# Notes 


## Prelim announcements



*   Speakers make sure your points / comments are captured accurately in the notes. 
*   Join the WICG to contribute.

Michael Kleber: Congratulations again to the Microsoft Edge folks, who have made their version of the Protected Audience API, called the Ad Selection API, available as an Origin Trial.  They are holding their every-other-Thursday call tomorrow at this same hour, great place for further discussion.

Isaac Foster: Thanks to all the Edge folks who put huge effort into this launch.  I know at least one ad has been served!

Kleber: Looks like we don't have the right people in the meeting today for two of the suggested agenda items, the Creative Ad Server discussion (continued) or for the Prebid parallelization discussion.  So we will pick up the Deal ID discussion and associated questions.


## Jeremy Bao: Deal ID Discussion - questions for DSPs/SSPs/anyone else with insight:



*   Jeremy Bao
    *   Roughly two weeks ago, we discussed deal IDs in this forum. Folks from IAB tech lab raised a few questions and concerns around our current deal setup. There are so many different use cases that use deals, having one solution to solve all of them is challenging. Want to learn more about those use cases. A few follow-up questions about how folks are currently using deal IDs.
    *   How many deal IDs do each of us have in a typical auction or private marketplace auction?
*   Isaac Foster
    *   Deals in the broadest possible sense? How are DSPs, SSPs, publishers and advertisers using this? Or specific slice?
*   Jeremy
    *   Broadly for now. Trying to figure out what kinds of deals we could support with which potential solutions. How do you use deals in your world?
*   Isaac
    *   Pretty common on a programmatic exchange that offline - not in the context of a bid request - a buyer and a seller will make an agreement, codified in a deal object. Deal object has industry-wide significance. Interoperable.
    *   In terms of the number in general, kind of high. Can find the number of deals served on the platform.
    *   For a given bid request, what would happen is - at least for our case - we will determine "deal eligibility" - what deals are eligible to serve on this inventory? Deal can be targeted on a combination of inventory and user characteristics. Seller might give buyer preferential pricing on a particular slice. Lets the SSP notice, I will accept this as a private marketplace. Generally not going to be the same Deal ID across all DSPs. Those get sent out individually to the DSPs and the bid requests, DSP can choose a particular deal. Can be capped, can be budgeted, at rendering can notify that this was for this deal, this private marketplace.
    *   It is possible that some deals will be based only on 1p information. One of the use cases of curation would be to package a deal doing eligibility targeting from data collected on other places; would be possible in a k-anonymous fashion through sandbox, just not yet.
*   David Dabbs
    *   Enrichment scenario. As a seller, you're enriching and labeling with a deal ID as a high-level attribute.
*   Isaac
    *   In Deal classic, the seller might be in control of all of that information. Something a bit more advanced, three parties in that transaction -  buyer auction service charge, seller auction service charge, and curator auction service charge.
*   Jeremy
    *   Who do each of you represent
*   Isaac
    *   Xander side
*   Alonso
    *   You're wearing mostly a buy side hat?
*   Isaac
    *   Xander is both buy and sell. A bunch of people here can give more details and more specifics.
*   David Dabbs
    *   Roni responded that he, "just ran a query and saw over 318 going out to a single DSP in a single OpenRTB bid request"
*   Michael Kleber
    *   From the chat…
        *   Laurentiu Badea: hundred
        *   Roni Gordon: many hundreds
*   David Tam
    *   This is probably more of a question for the exchanges. I kind of understand that deal ID is in terms of the contextual side of the auction. How does it work in a sandbox world?
*   David Dabbs
    *   I would assume that it's similar.
*   David Tam
    *   On the advertiser side, they can create a bunch of interest groups on the fly.  How would they know which Deal ID is applicable for which interest group?
*   Isaac
    *   Right now we can do some deal support with something we could detect as eligible through the contextual auction, if we see the inventory, pairing and characteristics of the publisher. The thing I was alluding to was something that would be like sandbox support, going off the inventory and then possible other website segments or user data from a specific site. It would be a restriction to say that you only get the deal ID, could be a multi-level thing, might be challenging.
*   David Tam
    *   And seller-defined audiences? Publisher-owned assets, on the publisher side not on the audience side.
*   Michael Kleber
    *   Maybe we should avoid the term "Seller Defined Audience", which has an IAB meaning based on a specific audience taxonomy. What you're describing is that the publisher can create an audience.
*   Issac
    *   "Publisher audience extension"?
*   Jeremy
    *   So, not using deals just for business agreements, but also to represent your advertiser's audience.
*   Isaac
    *   It sounds like you're trying to pull out the major bits here, one of which is information hiding. Package this thing up for you, and it gives you this pricing, but it doesn't tell you how we got there. A lot of it is about preferential pricing, somewhere between direct and indirect.
*   Jeremy
    *   Question is - you need to use PA to do the remarketing, and you want to use deal IDs. Do you still believe there needs to be hundreds of deal IDs involved?
*   Roni Gordon
    *   Not sure - hundreds where? Do buyers still need hundreds? Assuming it's audience-powered, and audience is powered by interest groups, I don't see why there wouldn't be just as many or more. Whether it's server side or client side, none of that has changed.
*   David Dabbs
    *   A bit of color from the buy side: Speaking from our platforms perspective, in a lot of CTB, usind a deal ID is pretty much the only way for a lot of inventory, especially certain kinds of device inventory. This is for video, which PA doesn't do yet, but the only way in advance a line item can target it, the only way to have it embedded and know about it - the deals are there to keep our partners happy, lets us know what kind of traffic to send you, but some cases where a deal is essential.
*   Issac
    *   Yes, you need deal IDs. Trying to find the avg number of impressions we transacted on for that. I'm pretty sure the number Roni and Laurentiu were referring to is the total number of deals that go out, or per DSP?
*   Laurentiu
    *   Per DSP, and hundreds
*   Isaac
    *   Yes, but there's only one you'll need for reporting. One deal per slot. It's not as if the request here is to add 80 new IDs coming out of reporting IDs. Earlier, I meant total served across the platform.
*   David Tam
    *   While we flesh out the deal IDs, in the interim, can we not get this data out using perBuyerSignals? To describe what this deal is without a deal ID?
*   David Dabbs
    *   The plan that Hillary from ope RTB and PA has is for the seller to pass deal IDs through perBuyerSignals. It's getting them out. As Roni has said, it's not a problem to get the information in; it's getting it out.
*   Jeremy
    *   Yes, but event level reporting, you can't guess what deals would be needed for event-level reporting.
    *   As a follow-up, we're talking about hundreds of deal IDs, not millions, right?
*   David Dabbs
    *   On a request to a buyer, it's hundreds. Isaac just said (in chat), "~50K deal ids transacted on our platform last 24 hours (total, not per bid)". On our system, 10/12/15-thousand through various programmatic partners, either because we have to bid on it like some video cases, or because we choose to.
*   Alonso
    *   In the ORTB back and forth, buy and sell side for contextual request reply, maybe we can map out an actual graphical workflow, and ask the ecosystem how many deal IDs are on each leg of that workflow. Maybe millions of deal IDs, but in essence, a whittling down of that number, the closer we get to PA configuration, when I hear that feedback, if we can specifically, graphically, figure out at each point how many there are. If we can map that out, maybe we can show these guys and they can react.
*   Jeremy
    *   To that point, I'm asking for "per auction" or "per bid", and I'm hearing hundreds.
*   David Dabbs
    *   In past calls, Isaac just said, coming out from a winning bid, it's either 0 or 1. Coming in, it might be as many as 318 (as Roni said in chat) metadata for deals that are appropriate from that seller to this buyer for this slot at this time. Buyer may choose to apply one of those deals to one of those bids.
*   Jeremy
    *   Do we send all 300 of them, or do we shrink the number of deals so that bid size can be smaller?
*   Isaac
    *   A given exchange might have limits on the size of requests it sends out. If there are 10,000 eligible deals, maybe I cut it down to 1000. There's no standardization.
*   Roni
    *   PMP object is not the biggest; we have other giant arrays of strings.  And OpenRTB support HTTP compression as well.
*   Isaac
    *   Your charge here is to help figure out what we need to do for deal  or kinds of deals in sandbox, one thing we're happy to collaborate on in the ASAPI side. One thing that's worth separating is - there's no standard around , you will oiptimize your ORTB request in these ways; slightly bespoke arrangements. When an SSP receives a bid request, will have its own internal indexing. Similarly, on DSP side, has its own indexing.
    *   Do you mind if I ask you, the space of things you're looking to understand and explore, from a Sandbox and an ASAPI perspective, probably worth decoupling deals entirely from contextual information and sandbox supporting appropriate reporting on that downstream, and deals that determine eligibility in what we call interest group information. Are you thinking strictly about the former case, or the latter case, or looking to get started?
*   Jeremy
    *   In contextual bids, in PA framework, there are ways to hand bids and the reporting of that. The problem is on PA bids, the key problem is around privacy and k-anon. If we want deal ID to be reliably available in event level reporting, then K-anon is required to protect privacy; the huge number may not help K-anon, and pre-registration, as we previously discussed, is needed to support K-anon. You guys mentioned preferential pricing, also specific audiences and specific inventory, and your deal ID will impact business terms. But if a deal ID is only used to label an audience - and not to have a specific business impact - if event-level reporting is not required, k-anon and preregistration may not be problems. So I'm curious, does this categorization make sense to you?
*   David Tam
    *   Maybe more of a clarification around k-anon. My understanding is K-anon can only be calculated if the attribute is in the interest group?  Is it possible to calculate k-anon at auction time?
*   Michael Kleber
    *   K-anon is a tool to achieve a goal, the goal is not having the event level report imply the user's identity on multiple sites, there are lots of ways to be smart about how we can use k-anon to achieve that goal. This is why Jeremy is asking about all of these things.
*   Isaac
    *   A little bit confused, at the beginning of a long exploration here. The k-anonymity need for deal ID is well understood among the people who have been working on this for the last few months. I agree with what Michael said that there's a way to provide deal IDs in reporting while respecting the privacy needs we’ve all been discussing.
    *   Deals do affect pricing and are therefore a huge part of billing and discrepancy. I don't think we have anything on our side for the terms of a deal, e.g. we'll give you this preferential pricing if you get 100 thousand impressions on this deal or something like that.
    *   I don't think anyone is pushing back on the idea that a deal ID needs to be part of a k-anonymity calculation and we should be able to do that at auction time.
*   Jeremy
    *   Do, so we see any cases where we don't need event level reporting and aggregated reporting might suffice?
*   Alonso
    *   Just put into the chat - when you're talking about a financial transaction, reporting only use cases - summaries, territories, sale persons. Deal ID is a contract, and one company may want to audit, want to understand if the terms are being respected.
*   Isaac
    *   Absolutely one of the cases. For quite a while I ran our reporting platform. We would break down our types of "reporting" into different categories - general analytics (am I serving); inventory analysis (how much inventory is available, might use for forecasting); bill and discrepancy analysis (as Alonso says, but if you're a bigger company for GRC)
    *   MRC requirements-  not yet tuned to an aggregated world, not a thing yet. The discrepancy analysis, fine-toothed comb - not sure how you'd do that in an aggregated fashion.
    *   When you ask, is there a reporting use case where deal ID is not required, kind of the wrong framing.
*   Roni
    *   Why have a unique deal ID in the auction at all? When we're talking about discrepancy - not just talking about billing - within a platform, the reach of this deal is going to be 10M, it should activate on this page at this time - want to see if that's true. In aggregate, can't necessarily see that. Need to be able to see that it activates when it needs to activate.
*   Michael Kleber
    *   Wait, what you're saying is at the time of the auction, you need to be able to say does this deal ID apply, and then you need to be able to answer questions in aggregate.
*   Roni
    *   For arbitrary granularity.
*   Michael
    *   Sure
*   Roni
    *   Billing is the last step.
*   Issac
    *   I'd encourage Jeremy to think about what are product level use cases that may not require a signal in event level reporting. I just think deal ID is not one of them, yet.
    *   I can make that case that the size thing in event level reporting - the abstraction or the rule would be, if a product level use case - and an attribute - has an impact on pricing, it likely has a strong desire to have event level granularity.
*   Jeremy
    *   You're saying that all of those affect pricing? When it's used as a label for the audience, does it still affect pricing?
*   Isaac
    *   I'm not aware of any use case on our platform, certainly one that's common, where a deal ID doesn't affect pricing. One of the attributes of a deal is inherently that it's creating a different marketplace.
    *   In theory could create something where for some instance of the deal, maybe it magically disappears. The notion of deal has some standardization, and it fundamentally has something to do with pricing. Could be in a particular case it's irrelevant, but it's not typical.
*   Jeremy
    *   So deals are related to pricing and sales, and those 300ish deals are all relevant to this one bid?
*   Isaac
    *   SSP who receives a request will determine which deals are sent on that request. Before campaign targeting. Saying, first, eligibility determination based on ad request, and then pass that down into the next level of targeting. A given request, then a bid request - the concept of scale.
*   Michael Kleber
    *   3 minutes over the end of our period.
    *   For the first time, I'm baffled by the question of, why would an SSP let a particular bid with a particular deal ID win the auction if it's not valid for that buyer. Why are there discrepancies, if they can decide in retrospect that it's not valid.
    *   But since we're over time, we can't probe that or any other questions.
