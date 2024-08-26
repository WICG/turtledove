# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Aug 21, 2024


# NO MEETING AUGUST 14
due to a lack of agenda items, Happy Summer

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!	 



1. Michael Kleber (Google Privacy Sandbox)
2. Patrick McCann (Raptive)
3. Brian May (Dstillery)
4. David Tam (https://paapi.ai )
5. Matt Davies (Bidswitch | Criteo) 
6. Ivan Staritskii (Bidswitch | Criteo) 
7. Arthur Coleman (IDPrivacy)
8. Laurentiu Badea (OpenX)
9. Brian Schmidt (OpenX)
10. Alex Cone (Google Privacy Sandbox)
11. Anthony Yam (Flashtalking)
12. Becky Hatley (Flashtalking)
13. Sven May (Google Privacy Sandbox)
14. Paul Jensen (Google Privacy Sandbox)
15. Sathish Manickam (Google Privacy Sandbox)
16. Jeremy Bao (Google Privacy Sandbox)
17. Tomer Ben David (Taboola)
18. David Dabbs (Epsilon)
19. Matt Menke (Google Chrome)
20. Isaac Foster (MSFT Ads)
21. Tamara Yaeger (BidSwitch)
22. Abishai Gray (Google Privacy Sandbox)
23. Shafir Uddin (Raptive)
24. Bharat Rathi ( Google, Privacy sandbox)
25. Koji Ota(CyberAgent)
26. Courtney Johnson (Privacy Sandbox)
27. Paul Spadaccini (Flashtalking)
28. Kenneth Kharma (OpenX)
29. Andrew Pascoe (NextRoll)


## Note taker: Tamara Yaeger	


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here (meetings with no items may be canceled)



*   [David Tam] I would like to return to this issue - https://github.com/WICG/turtledove/issues/1196 and how we can come up with a solution that does not require Publishers to delegate IG creation to a DSP.  Instead enable Publishers to curate and sell their IG in an open auction. 
*   [David Tam] Add seller information to the KV server request - https://github.com/WICG/turtledove/issues/1249 
*   [Matt Davies] Follow up to https://github.com/WICG/turtledove/issues/1220 to understand the next steps in getting reporting for third parties outside of the component auction but within the billing chain  



# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!! :) 


# Notes 

Michael (Chrome):

W3C's annual meeting TPAC is coming up, https://www.w3.org/2024/09/TPAC/, third week of Sept, Chrome will ask for MSFT to have a joint session to discuss convergence between PAA and Ad Selection API. Great job of converging previous 2 years. Might be Tuesday or Thursday that week, subject to schedule availability. Remote attendees will be welcome, but will use different infrastructure for TPAC than for our normal weekly calls.

Upcoming meetings – Michael Kleber unavailable next week (Aug 28); will decide whether to keep it at end of this call by show of hands. 

ANSWER: Meeting as usual next week, Paul will direct while Michael is away ("See you in September!")

No automated note-taking, has not been approved by W3C :]


## [Issue 1196](https://github.com/WICG/turtledove/issues/1196): Solution that does not require pubs to delegate IG creation to DSP.

David Tam ()

Long-standing issue, want to find out where we got to.

David Dabbs (Epsilon)

I think we got hung up on creation vs ownership. Do you want the pub to execute as owner? Anyone can delegate creation, but not anyone can own it. You really want the pub to own it.

David Tam

Yes

Michael 
I think ownership has some problems. There is a field in IG that has the name ‘owner’ is my unfortunate choice from back in 2020. I’m in favor of separating out diff roles and letting diff parties do diff roles. Would prefer to avoid saying “ownership” and being more clear on the roles, and when they happen, and how they are delegated. 

Brian May (Dstillery)

I like the def of ownership to understand what kind of control is exercised over IG.

Michael Kleber (Chrome)

We had some discussion of diff roles, back in the July 10th call ([notes](https://github.com/WICG/turtledove/blob/main/meetings/2024-07-10-FLEDGE-call-minutes.md#david-tam-seek-feedback-on-proposal-for-how-seller-can-set-interest-groups-for-buyers-to-bid-on---httpsgithubcomwicgturtledoveissues1196)). We can pick up from there. The thing that I call “owner” is the thing we should describe as the person who does the bidding. An IG is something that bids, and it holds some JS bidding logic and the info used at the moment of auction to combine data to produce bids; when I imagined IGs, the “owner” would decide how bidding worked. Could be “party responsible for bid”

David Dabbs

Anyone can place the initial content of IG, but only the “owner/bidder” update it (in addition to bidding). Perhaps that’s what MK meant by other roles. 

Patrick (Raptive)

I heard a lot of parties and in supply side circles, particularly entities who don’t bid but have intimate relationships with their audiences. It seems the solutions have pointed to additional output gates, but a lot of entities are on the sidelines waiting for fundamental architecture change. Party doing bidding + party setting storage on browser exist of this user; if that has to be the same party, that should be clearly communicated to the supply side. 

Michael

Those 2 entities can be different, and the mechanism has existed as long as TURTLEDOVE / FLEDGE proposals. There is an entity that does bidding, “owner” for now, and the creation of the IG does not need to be done by that party. They can delegate who creates audiences used for bidding later on. The person doing the bidding wouldn’t expect anyone to allow this, especially malicious parties, but they can delegate the ability to make audiences to whoever they choose. There is already a mechanism that exists with permission files and special locations. If the bidding party wants to [delegate the permission to create audiences](https://github.com/WICG/turtledove/blob/main/FLEDGE.md#13-permission-delegation) that they use later, it can happen.

David Tam

How the IG is set, you can’t separate bidding from IG, so you have to do both at the same time. My question is maybe K-Anon issue, but can we separate that out, w/out having to say they are the bidder? 

The pub can create the IG, and what I’d like to see is to say to every buyer is that the IG and they’re open to bid, and advertiser / DSP provide bidding logic as buyer. 

Michael

This is indeed possible today! Could be pub who is IG creator or could be any other party in that role, let's call them something like the "audience identifier", some kind of party that is expert in saying X person belongs in Y audience. I’m an expert and can recognize that, that’s one role. That role can say they saw something happened on a website (whether they are a pub or 3P), and can figure out audiences, potential bidders, etc. The mechanism in PA is: at the moment of audience identification, the audience identifier party can off-the-browser say to everyone who likes bidding on their audiences, "we identify this person can be in this audience, and I can create an IG to let you bid on this audience". This could happen in a server-to-server convo between audience creator or future bidder, or in browser where IG creator creates various iFrames, server-to-server calls to potential bidders. If multiple bidders want to take that audience creator up on the offer, then those bidders can use different interest groups to bid on that same person, because each has different bidding logic for IGs. Independent JS do independent things, but the various bidder IGs can all be created at the same time by a party that can identify person that can be in each of those audiences.

Brian

The interest is not in being able to create IG, but to create IG for best return and control relationship w person as it relates to IG

Michael 
Selling this IG to who will benefit the most happens at the moment of IG creation, not in IG use in the middle of auction.

Brian 
I wish I could create an IG, sell it for some days, to another for other days, and purge bidding logic in between.

Michael 
Selling an IG for a period of time is feasible right now.  Having an IG start later for an unknown buyer does not work, no way to do that today.

David Tam

I would like to see that the pub can say here’s my IG, maybe 5-6 number of bidders, and they bid dynamically. The pub sets IG, and says to all the buyers, they have this particular audience I can sell. They can be any number of buyers interested in buying that audience. They can all submit that bid, today the only exception is that buyer puts that in there…

Isaac (MSFT Ads)

I’m just curious, maybe abstractly we can say these are objects owned by entities w actions that can be performed. There exist diff kinds of access control options. Sounds like we’re talking about an action we’d like to delegate in a more subtle way. How would you start to plan a framework in that direction where the initial creator can set privileges. 

Michael

That’s a direction I would be happy to go, if we have a clear understanding of delegations and rights. For performance reasons, browsers will have a strong preference of _not_ doing delegation based on a new round of delegation logic that runs during the moment of auction. It’d be better not to happen during those precious millisecs. 

Isaac

Hypothetically if I create IG, I can grant bidding privilege; maybe I can do something specific per IG. Would that help what Brian was talking about; I do agree having to dupe objects to do delegation will result in sadness. I’m not sure of the run time use case; whether this direction would help or not.

Michael

That’s in line with my thinking, just nailing down description on what we want clearer would be useful.

Isaac

Do you agree that even absent of knowing a certain privilege, sounds like we’re moving in the more resources-actor privilege?

Michael

Interested in exploring, adds complexity to API implementation, need cost analysis. 

Brian 
Want to suggest exploring this model w/out tying it to client side implementation of PAAPI. Server side may be able to do this with appropriate capabilities. Prior to auction can be done on server side relatively easily, then present to server side final auction what should be represented. 

Isaac

Doubt anyone disagrees, maybe the connection to client would be that the client needs to make decision what data and decryption keys will be released. Important to align w these guys because if we build the best feature on server side, but insecure, it doesn’t matter.

David Dabbs

The agent there is the owner-bidder in either case? 
 
Isaac

Can’t get away from something creating a thing, but it’s nice to be able to delegate an action to other entities. Could be more object management friendly, it would also open up the possibility of additional things in the future, have to start somewhere.

Jeremy (Google Privacy Sandbox) 
Can you describe more of your biz case? What parties are involved?

David Tam

At the moment looking at retargeting, primary way of using protected audience. If we can open pub audiences it will give us bigger reach so we can target across funnel; targeting what appears on pub’s website not just advertiser’s. Gives more audiences, we can drive more performance for advertisers. Advertisers who want to reach new customers, for example, they’ve been asking for this because in a cookie-free environment this would be the best vehicle for retargeting campaigns. 

Michael

That’s a great use case, can you explain why that use case is not addressed by handing IG from creator to bidder at time of creation? Brian just identified a use case about serially using IG groups, is that what is missing?

David Tam

You’re negotiating a deal beforehand, but I’m saying why not let the auction decide who the best bidder is? 
 
Michael

You can absolutely use an auction, at time of IG creation, if you want. If you choose to accept 5 diff bidders for IG audience, then you can create 5 diff interest groups. They can bid in every auction and might have opp to add to that audience. 

David Tam

Say a pub has an audience they want to sell to a variety of diff buyers, how would they say they have an audience who wants to place a bid on it? How does the mechanism work? Would the seller have to tell all the buyers, not w/in the browser, that they have the audience. So what I would like to see is for it to happen in the browser.

Michael

You can, you can write your own JS that does it in the browser; call all the other ad techs you hope to bid, announce the opp, and ask if they want to create the audience or not.  Or you could do it server-to-server outside the browser, either way works.

David Tam

The seller has to write the JS? 
 
Michael 
Yes, the party that wants to sell the audience, at the time you recognize you want to put a person in the audience. IG event can be sold to as many people are interested in buying at moment of IG creation. Up to your biz negotiations.

Jeremy

Let’s say I built an audience 3 mos ago, what if I have a new buyer? 
 
Michael 
Handing an audience to another person happens at audience creation; you can’t put pre-existing people into an audience for a new bidder later, unless you see that person again. That is right and closely related to Brian’s point. We could add a new mechanism to support that, but that sort of delayed-reaction audience assignment is not possible in PA today. But if what is supported is adding someone recognized to audience event being sold, all of whom will be able to buy and target that audience in the future, that already works. 

Brian 
Who can access IG and for how long reflects deficiency in ad tech, if a cookie is allowed to be set, it can be found on many other sites. One of the things that PA and IG ownership in PA can do is preserve the value that pub can develop audiences they have by controlling means of access to those IGs.

Michael

Back in July, we talked about: what sort of joint control can be exercised by a combo of IG bidder and the audience creator? What sort of control should be retained when bidding ability is given away? We talked about contractual agreements and reporting, so that the audience creator can find out how the audience was used, and can monitor any breaches of agreement. If after the fact reporting is not good enough, and IG creators want to retain some amount of control of when / where / how IG gets used, let’s talk about scope and implementation w performance characteristics.

David Dabbs

(Addressing David Tam) Do you need this to utilize PA? Your description is echoing Michael’s seller defined audience and having many people bid on it, with the caveat that they can monetize that wherever they want to. Implicitly you want the ability to, whenever you want, assign a buyer and make sure that you know and get a piece of the action? Or in the (your) model does the seller of the ‘rental’ make the money?

David Tam

When I buy 3P audiences, they will be based on cookie IDs or email addresses that audiences are built around. The beauty of PA is that it doesn’t require an ID. It happens in the browser, I don’t need to know any identity, I just need to know that this person visits a pub and the pub thinks that person should belong to a certain audience. These audiences do not have IDs built into it. I could still buy 3P audiences that use cookies or email to bid on this.

David Dabbs

Do you want the purchasing of your audience to only happen on your property? Would it be limited to impressions on your property? 
 
David Tam 
I think pubs would welcome monetization; given that the utility of 3P cookies is diminishing, how else to drive revenue? A lot of pubs don’t have that many 1P identifiers.

Michael

Often we get into trouble when one party speaks towards others’ interests. We don’t have Patrick McCann's pub POV right now, so right now we might be deficient to speak to his audience creation use cases.

Brian

I spent lots of time with pubs, I think the ideal model is where a pub doesn’t have to try to sell IG outright. Pubs will always over-value their IG. Might mean that buyers are not interested in IGs. If someone means that the IG owner could provide IG to the buyer, buyer gains whatever benefit and decides whether to keep using it, it would benefit the entire PA ecosystem.

David Tam

I concur

Brain

The IG creator has identified until of value, which will go to another context and be purchased, the value of that purchase will be given the provider of context. There needs to be a way of taking value of interest and context and combining them so both parties gain value at point of activation, not just creation.

Michael

It seems you are arguing for IG creators running an auction to find out what the value of IG bidding opp is going to be to bidders. That would be fine with me, auctions are great. Imagine pubs or other audience creators announcing to bidders their opinion of some kind of performance characteristics, and then ask for bids on how much the bidder would pay to have the opportunity to bid on that audience. Potential bidders needing to decide how much to pay for IG at time of creation, that requires some kind of prediction. But in-auction bidding ALSO requires prediction; advertisers are paying money now but are still hoping for value later on. I don’t think these are hugely different; showing an ad to a user in the moment often isn’t what gets value, it’s the conversion event. Prediction conversions based on audience attributes, it’s the uncertainty when you only know 1 domain worth of behavior.

Brian

There should be some kind of competition to gain access to IG. How much ownership / value does IG creator have to give up and is it a terminal event?

Michael

This is getting back to the July discussion.  We discussed 2 things that IG creator might want to retain after the fact so as to not give up everything during IG creation: (1) money: a piece of the action that is spent later on that IG, which can be done through after-the-fact reporting.  (2) control: is any control retained on how the IG is used? For the first one, the money part: If the audience creator runs an auction for who gets to have an IG created for them, then the bids in the auction could include whatever kind of payment you want.  Audience creators could accept bids of the form "$X right now plus a Y% cut of the action when this IG wins the auction" for example.  This still requires us, the browser, to build some new reporting mechanism so that the audience creation party can get reports when the IG layer wins.  But that's OK, an entirely reasonable additional feature we could build.

Brian

You mentioned that the risk taken in IG creation auction is equivalent to risk in OpenRTB auction. An interest group once won has much higher potential value over 1 bid. 

David Dabbs

In today’s world the creation of IG on pub’s site is all or nothing prop? They can do it through policy header, they would put in origins of delegates they would be permitting to create IG on their property

David Tam

Issue is tons of tags on the publishers’ website and this has been one of the biggest challenges of programmatic today.  
