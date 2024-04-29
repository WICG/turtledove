# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday April 3, 2024 

**NOTE there will be no call on April 10 (added later: or on April 17; next call after this one is April 24)**


To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	 



1. Paul Jensen (Google Privacy Sandbox)
2. Brian May (Dstillery)
3. Sven May (Google Privacy Sandbox)
4. Orr Bernstein (Google Privacy Sandbox)
5. Roni Gordon (Index Exchange)
6. David Dabbs (Epsilon)
7. Matt Menke (Google Chrome)
8. Laurentiu Badea (OpenX)
9. Brian Schmidt (OpenX)
10. Paul Spadaccini (Flashtalking)
11. Roni Gordon (Index Exchange)
12. Harshad Mane (PubMatic)
13. Isaac Foster (MSFT Ads)
14. Jacob Goldman (Google Ad Manager)
15. Pawel Ruchaj (Audigent)
16. Konstantin Stepanov (Microsoft Ads)
17. Alexandre Nderagakura
18. Tim Taylor (Flashtalking)
19. Owen Ridolfi (Flashtalking)
20. Laszlo Szoboszlai (Audigent)
21. Alex Peckham (Flashtalking)
22. Becky Hatley (Flashtalking)
23. Ricardo Bentin (Media.net)
24. Taranjit Singh (Jivox)
25. Miguel Morales (IAB TechLab)
26. Tim Hsieh (Google Ad Manager)
27. Arthur Coleman (ThinkMedium)
28. Alonso Velasquez (Google Privacy Sandbox)
29. Sarah Harris (Flashtalking)
30. Warren Fernandes (Media.net) 
31. Guillaume Polaert (Pubstack)
32. Russ Hamilton (Google Privacy Sandbox)
33. Joel Meyer (OpenX)
34. Abishai Gray (Google Privacy Sandbox)
35. Sid Sahoo (Google Privacy Sandbox)
36. Shankar Venkataraman (Intuit)
37. Shafir Uddin (Raptive)
38. David Eilertsen (Remerge)
39. Garrett McGrath (Magnite)


## Note taker: Isaac Foster (for first half), Arthur Coleman (for second half)


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here:



*   Isaac Foster:
    *   TEEs Flexibility: KV Support + Inter TEE Communication https://github.com/WICG/turtledove/issues/1140
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Warren Fernandes
    *   [Proposal to add support for analytics providers](https://github.com/WICG/turtledove/issues/1115)
*   David Dabbs
    *   Speaking of Brian’s reference to _crowdsourcing_, what is the team’s take on incorporating _Discussions_ in this repo (assuming WICG has greenlit their use)?


# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

**NO MEETING NEXT WEEK (April 10), because it’s at the same time as the PATCG in-person meeting.**


# Notes

Isaac Taking Notes till ~11:30 EST

PJ: announcements, No MK today que sad, meeting next week canceled to PATCG F2F (so don’t come here, go there); MSFT Edge folks are having WICG meeting about ADS/ASAPI every other Thursday, including tomorrow at 11 AM EST. Let’s get started…

Warren Fernandes: here!

PJ: let’s talk through your issue

WF: follow up to IAB fit gap response, tracking revenue independently, want to highlight that in the existing prebid world there is an analytics adapter that can listen to event stream and provide reporting functionality independently of other participants, so can we add a similar concept here in fledge, add an analytics URL field so that an SSP can specify something to provide the analytics to some party independently.

Cross talk about finding issue in IAB doc

PJ: 

IF: how would this allow the publisher to independently to confirm that something happened in Fenced Frame?  Publisher is still relying on SSP to report some of it.

WF: the idea is anyone can listen to some event that occurs inside the framework

IF: so anyone with tech on page can listen to the event?

WF: sort of…gives example of prebid adapter…sorry need more fill in here

DD: independent of how we grant the access, does Chrome see this as fitting in the privacy model?

PJ: security and privacy issues…security side meaning do we want to allow any third party to listen, might not want some other buyer to be able to listen to win information as it’s sensitive

DD: yeah getting kind of private

PJ: right that’s security question, who in the auction might not want to expose data…privacy issues as well, how many entities can see the information and how leaks could propagate…who is the “person” we want to send this to?

BM: Same concern about the “anyone on page” issue, should be controlled by the publisher, something well defined so publishers can develop contracts with partners and this will support those contracts in a way that is safe and acceptable for relevant parties. If we don’t have something for the publisher then I think it will discourage adoption as we will be asking them to trust partners without independent verification which they don’t do today because it was found to be unworkable in the past.

DD: Warren what is the Maslow's hierarchy of your needs?

WF: want consistency in the reporting experience…do have some key pieces, bid price and who won

DD: yeah could be a one-bit issue

WF: can be aggregated, need not be event level

PJ: how will that work for who won?

WF: ah, not looking for traceability, looking for the pub to know what the revenue was

IF: motivating use case is to check what SSP tells them for revenue

BM: publishers want the ability to check the numbers, without it they’ll be reluctant to adopt PA API.

DD: skip for now

PJ: so want to see what’s happening with a few important metrics?

WF: similar, can happen with prebid and independent analytics adapters…can’t happen with fledge as today

PJ: event level for now makes things easier, aggregated reporting later…concern of burden on users device, privacy leaks as more information going to more parties

WF: want to let pub nominate someone, not just tied to ad server/ssp

PJ: twixiness lies here…

BM: Want to be thoughtful about the details, but the immediate question is whether PAAPI would provide support for it.

PJ: don’t want to expose information that is sensitive to all parties, need some safe delegation…how would a publisher want to pick this person

(Note from AC: Notetaker shift occurred here from IF to AC)

IF: Dig into a couple of things:



1. On privacy piece, hypothetically if the publisher in their own domain had a well-known address where today event-level data and tomorrow aggregate-data could be sent
    1. Would that be safe from a delegation perspective
2. In terms of privacy, is there a marginal privacy leak here.  Anything sent to SSP can also be sent to the publisher.  Both now and when we get 128 bits that could be problematic in the aggregate reports.

Hypothetically if the publisher had that endpoint then the delegation would be ok and I don’t see a marginal privacy leak and then in framework we can control what is sent to the endpoint

PJ: Signal from publisher in secure fashion can be hard. HTTP response headers are one safe mechanic.  They could potentially delegate through that mechanic.  What is weird for me: this is information available to the sellers in event-level reporting today, so it could be shared onto the publishers.  If we feel like adding this to the web platform, some people may think of this as redundant

BM: Let me respond to that - “trust but verify” is the issue. My partners are working on my behalf but I want to verify that.

In regard to security, can we use cryptographic means to verify authenticity and provenance?  Participants can use crypto signatures to know someone is who they say they are.

PJ: HTTP header takes care of that.  And adding another encryption thing to the web is painful.  A lot of encryption things have verification but not authentication - don’t add secrecy.  HTTP headers have that optionality and browsers can expose stuff.

For example, if we want to know who has auctions on a page, we can expose that.  Versus doing that with a signature from the publisher it would potentially be visible to others.

BM: Suggesting using signatures to orchestrate verified agreements so that all parties can demonstrate agreement to use an analytics adapter.

PJ: HTTP headers already have this. In Javascript, someone could nefariously block it.  So we might be missing results.  Publishers might not be able to find out they are not reporting the results of the auction.

You mention something like this has been tried in the past and didn’t go well, any learnings?

BM: Historically there were cases in which publishers were asked to rely on reporting from partners, but found they needed independent verification in order to be able to settle discrepancy disputes. The general model that emerged was to use agreed upon 3rd-parties to provide analytics.

Specifically where it comes up is when the seller says they sold X impressions and the publisher thinks they provided Y impressions, or sellers say they sold X impressions and buyers say they bought Y.

PJ: I can see aggregation being a useful way to provide those numbers.

BM: If we came up with a standard set of reports as with specific fields that will be reported and granularity will only go to this level and thresholds will limit details reported.

PJ: What are bounds, do we think?

BM: We’d have to figure that out.  We could also trade entropy for timeliness.  We can provide less information more quickly: an event happened. More information in aggregate with a lag time.  If you want it fast, fewer details,  As the relationship between the partners matures, then providing more info at an aggregate level is meaningful.

PJ: What time frame are we talking about?

BM: Real-time or in a couple of minutes.  Specifically something has gone wrong with my configuration - a lot can happen in a short period of time that can be very costly and I want to know about it in as short a period as possible so I can fix..

DD: Sounds like the Chrome team thinks this is possible to do.  Wondering if there is already a readily available web reporting API that publishers can configure - say what the endpoints are - maybe using to report exceptions today - but the API doesn’t constrain it in that way.  May not be appropriate though.  The issue is if it can be done what are the privacy bounds?

PJ: (lost due to noise in the background from someone @Paul can you please fill in)

DD: Publisher is facilitating the SSP so they can find out what commercially is happening - to the publisher or someone they delegate.  You have to be a buyer to get any exhaust out of this.  But the publisher on whose behalf this is being done and permissioning it - it is perilous - they are not a participant.  That’s what I meant by a regression in pre-bid cases

BM: We’d be putting publishers Into a position where they wouldn’t be able to safeguard their interests and therefore would be reluctant to participate.

DD: Correct.  No one wants to put publishers in a corner

PJ: Understand what you are getting at.  Not sure I agree they are perilous

DD: Maybe “perilous” is too strong a word.  Sorry.

BM: Not that publishers are powerless, but that we’re asking them to participate blindly and without agency. That gives them only the option to either participate on faith or not participate at all rather than empowering them to make per-partner decisions.

DD: Not really - they are not really participants, other than having the machine on their page

BM: Exactly.  They are used for inventory, but have no agency.

RG: So I think this is where we always take the privacy leak stance when we talk about this, but I think we need to carve out a special place for the monetary aspect. Not everything needs to leave - but impression counts, $$ spent, that needs first-class treatment.  I would assume that is the primary interest in this discussion.  We need a privacy-safe way to exfiltrate that.  Making sure $$ trading hands is correct- that’s important

BM: We can use analytics reporting currently provided to publishers as a basis for determining what they would want provided to these analytic endpoints. Use what is exposed today as a guide.

PJ: Good next step.  Let’s see what people are actually wanting to learn.

Thinking about a Web API, what would delegation look like from a publisher to a provider they trust?  Does it look like a Private Aggregation API endpoint?

BM: Think that is reasonable, I expect a common model will be for Attribution Reporting to be delegated, this is similar.

PJ: Is it a one-to-one mapping?  Does a publisher have one analytics person they trust or is one-to-many possible?

WF: One-to-many possible

PJ: We need to know - if we expose these five things, we need to know how to bucketize.  Can get complicated.  Need specifics.

BM: Let’s start with a specific proposal

WF: I will put something together.

PJ: In that vein, I have to imagine contextual ads are as interesting as PA ads as protected revenue. 

WF: Don’t think we need that from an implementation perspective?  Publishers can delegate today to an entity.  The problem is when we get into PA - this ability does not exist.

PJ: Assuming that the seller is going to be taking some kind of a commission, we need to think about how sellers are going to expose the revenue they want visible that is related to their dealings

BM: I agree with that, we are already dealing with it in today’s world.  Take what we have today and move it into PA

PJ: That may be more data than we are exposing now.  Maybe it doesn't matter in aggregated format.

WF: Not sure if it is that different from what we are already doing today.  Outside PA, publishers can see post-auction details of various entities and what is coming to them from various sources.

BM:  You have reporting relationships with publishers that are independent of reporting relationships with buyers, right Roni?

RG: Yes, that is normal for us

PJ: Do you know the amount that is going to be reported to a publisher at that time?

RG: Conceptually yes.  Let’s take the post auction commercial terms out of scope because that has nothing to do with the transacted impression.  Do I know the clear price at result time: yes!

PJ: What you are asking is entirely focused on the publisher.  DD mentions situations that are partially due to the publisher.  You are asking about things totally due to the publisher, correct?

NEXT STEPS: Specify some more details about what gets reported.  

DD: Items to be included in a proposal should include:



1. What is to be reported
2. Time frame
3. Key dimensions
4. What delays

PJ: With aggregation you have a delay on reporting on some number of hours - not sure if that is acceptable

WF: We are also getting notes on forDebugOnly() use cases

BM: For the overall view of what is going on with revenue, most people are probably fine with 24 hour aggregation I’d assume.  For dealing with problems, if things are going wrong on my website - that is ASAP.

PJ: The more time correlation there is, the higher privacy risk.  But we are already thinking about real-time reporting APIs for use cases - may be appropriate here as well at least for impression counting.

BM: Tough but good problem to solve.

NEW TOPIC: DD About discussion in 

DD: Talk track on high level.  Sometimes it is difficult to know what real people have an interest in - which things you have - Discussions have up arrow - getting votes on issues basically like a community site.

AC: started web site - [www.theprivacysandbox.com](www.theprivacysandbox.com) - to educate the business side and pull all the deep technical discussions up to a higher level for consumption by a more general public.  However, that is not set up to organize issue threads - it references them where important.  Would be great to have organized discussions listed with links to issues.  Useful to see open issues in an organized, bucketed fashion.So for example, all issues around bid optimization.  (Add to the notes:  separately, this is somewhat difficult because many of these issues cross to the server side, so there may be a need to create this community information sharing platform  as its own area and allow referencing/bucketing issues from multiple API repositories.  But starting with just PA would work fine as a way to learn and get “product/market fit”)

PJ: Is github discussions a new capability in Github?

DD: Yes, I’m trying to beta it right now.  My idea would be to bring everything over to a discussion - and items tied to discussions can be issues or bugs that can be worked on.  Makes it easy to know - “yes we are working on that.”  Easy to see what is in flight.

BM: This would help sort out what is speculative and may be pursued from what is actively being pursued.  We can tell what the Chrome team has committed to developing and will be available vs ideas that may not be developed.  

PJH: Got it - let me see what we can do about it.

DD: Here is a [personal GH repo](https://github.com/dmdabbs/test/) where I set up _Discussions_.   \
_Discussions_ offer clear “up votes” and of course what the FLEDGE repo uses _Issues_ for today, discussion  threads, &c.

A repo owner can convert a _Discussion_ to an _Issue_ for that workflow, links to PRs etc. 

cc: pauljensen@chromium.org
