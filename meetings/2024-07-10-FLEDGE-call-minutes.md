# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday July 10, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!	 



1. Michael Kleber (Google Privacy Sandbox)
2. Roni Gordon (Index Exchange)
3. Brian May (Dstillery)
4. Matt Menke (Google Chrome)
5. David Dabbs (Epsilon)
6. Matt Kendall (Index Exchange)
7. Sven May (Google Privacy Sandbox)
8. Orr Bernstein (Google Privacy Sandbox)
9. Brian Schmidt (OpenX)
10. Harshad Mane (PubMatic)
11. Ricardo Bentin (Media.net)
12. Fabian Höring (Criteo)
13. Arthur Coleman (IDPrivacy)
14. Laurentiu Badea (OpenX)
15. Kevin Lee (Google Privacy Sandbox)
16. Matt Davies (Bidswitch | Criteo) 
17. Victor Pena (Google)
18. David Tam (Relay42)
19. Owen Ridolfi (Flashtalking)
20. Laura Morinigo (Samsung)
21. Jeremy Bao (Google Privacy Sandbox)
22. Paul Spadaccini (Flashtalking)
23. Omri Ariav (Taboola) 
24. Alonso Velasquez (Google Privacy Sandbox)
25. Neha Gautam (Carnegie Mellon)
26. Becky Hatley (Flashtalking)
27. Kenneth Kharma (OpenX)
28. Aymeric Le Corre (Lucead)
29. Andrew Pascoe (NextRoll)
30. Abishai Gray (Google Privacy Sandbox)
31. Maybelline Boon (Google Privacy Sandbox)
32. Alex Peckham (Flashtalking)
33. Sid Sahoo (Google Chrome)


## Note taker: David Tam (or when he's talking: Arthur Coleman)	


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here:



*   Isaac Foster:
    *   Brief revisit the “coarse information sharing” thing, we had talked about setting up time but never did, all got too busy…can even answer here
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Jeremy:
    *   Seek feedback on Negative IG solution and usage budget 
https://github.com/WICG/turtledove/issues/896 

*   David Tam: 
    *   Seek feedback on proposal for how seller can set interest groups for buyers to bid on - https://github.com/WICG/turtledove/issues/1196 
*   Matt Davies: 
    *   Seek feedback on third party reporting

        https://github.com/WICG/turtledove/issues/1220 



# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!! :) 


# Notes 


## JeremyBao: Seek feedback on Negative IG solution and usage budget, https://github.com/WICG/turtledove/issues/896 

Jeremy Bao - product manager working on PAAPI with Privacy Sandbox. Seeking feedback on negative IG proposal.  Supports IG targeting on contextual bids, can support negative targeting in PA bid.  Add a field in IG.  Must have the same owner. When the auction is initiated then the negative IG is filtered out. For negative IG set maximum 3 per IG, for contextual the maximum is 10. 

David Dabbs - Great.  I wanted to bring your attention to Fabrice's question about needing additional bid key in PA bids

Orr Bernstein:  Don’t need additional bid key in negative IG.  

Additional bid keys are needed for additional bids.

David Dabbs - Potential expansion of this feature.  Is it possible to have negative targeting for a single positive IG.  Don’t participate if the user is in a positive IG.  

Jeremey Bao - I assume the solution will solve this.

David Dabbs - Is it beyond the pale of privacy protection.

Jeremey Bao - Negative IG are tiny.  Is there any reason why you just do not use negative IG.

Michael - [Please add your comment] Positive and negative roles are separate from each other.  It seems wise to keep them that way.  I don't want to deal with questions of what happens when positive IG A is filtered out because of a negative IG B but is also being used as a negative IG by positive IG C.

Brian May - What is the implication for k-anon.  

Michael - None at all.  K-anon is not based on who is eligible to participate in an auction, only on winners of an auction.  If an IG does not bid in an auction then it has nothing to do with k-anon-  If you are not a potential winner of an auction then there is no k-anon.  Only candidates that won an auction.

Omri Ariav - adding negative targeting to the paapi flow can help in the negative IG native advertising use case.  How to follow these announcements.  

David Dabbs - Watching the repo and the issues. 

Omri Ariav - Suppression or negative IG in PA flow.  In terms of timeline what is the expectation of providing feedback.

Jeremey Bao - we are still working on that piece.  Timeline is not finalised.

Michael - There are many github issues around the same set of questions. #1072, #1092 are similar issues.  The negative IG work is also relevant.  #1199 is more about interactions of ads on the same page.  

David Dabbs - Negative IG has to have the additional bid key.  

Michael - To remove from the auction contextually targeted ads.  Extending the negative targeting capability it can also apply to bids from other IGs.  

David Dabbs - There was a negative IG could not have an updateURL.  Requesting that the scope of the lightweight creation of IGs via HTTP headers encompass this new negative targeting use case. A neg IG is a lightweight IG use case. 

Jeremy Bao - Don’t know much about header joining.  Need to explore more.  Negative IG still additional bid keys.  IG may still need to make additional bids.  Additional bid key is part of a negative IG, though not needed for this new PA bids usage. 

Orr Beinstein - You should set an additional bid key in your negative IG, that way you can use it in both ways, contextual and other-IG.

Michael - Even if you intend to use negative IG exclusively for negative targeting from other IGs, and you're sure that additional bids are irrelevant, the additionalBidKey is still needed to make the negative IG work.  That's how we recognize a negative IG in the first place.

Orr - It still needs to be there. 

David Dabbs - Not proposing that neg IGs be updatable.  As I recall you have a validity check on addlKey (so passing empty string or even some non-key token value) probably not work. 

Orr - Join negative IG instead of join IG.  

Michael - Let negative IG filter out positive IG. Additional bid is a required field. 

David Dabbs - Over time the special-case single value `negativeInterestGroup` attribute would be unneeded and be deprecated (since you are eliminating the same-origin restriction). 

 
Orr - Yes, we can simplify things.  


Jeremy Bao - Agree that you do not need that special thing.  An attribute maybe.  No need to read origin domain.


## David Tam: Seek feedback on proposal for how seller can set interest groups for buyers to bid on - https://github.com/WICG/turtledove/issues/1196 

[David Tam] Came up with a proposal on how publishers could create interest groups.  Idea is that we add additional attributes to the IG to signify that it is a seller defined interest group. Based on that we can populate the dynamic attributes.  Want to know if that is a possibility.

[Michael]  Looking at your proposal and who is here.  No one from RTB House seems to be here.  Their prime audience product is a seller-based audience launched on top of Protected Audiences - so it sounds like they are doing what you are proposing.  This is a use case we have discussed in the past.

[David] RIght now only the buyer can create an IG.  But according to the specification there are three parties involved.  The advertiser, the tech vendor that an advertiser can delegate to, and the publisher.  So when a publisher creates an interest group, how does a buyer buy that interest group?  The only way that is monetized today is that the advertiser or the delegated tech vendor of the advertiser creates it.  It implies that all the attributes of that audience are known at creation time of the renderURL which, if this were a publisher-created interest group, you would not know in advance if you don’t know who the buyer is.

[Michael] An interest group inherently has to contain some bidding logic since the interest group is the element that produces bids.  If you want to use a model in which a publisher wants to participate in the creation of an audience, people who visit their site, then the model we have discussed before is one in which the publisher is going to need to work with some ad tech or multiple adtechs involved in the audience use business.  So you are quite right that at some point there needs to be a delegation from the publisher that is involved in audience creation to the person who is actually doing the bidding.  In the Protected Audience model we have today, that happens at the time of interest group creation. So if a publisher wants to create an interest group and make it available to different buyers, we recommend they have a set of different interest groups, one for each buyer they are offering these interest groups to. We are not set up to have anybody build one interest group that holds onto different pieces of bidding logic that come from multiple different ad techs.  We put everything together by having a bunch of different interest groups - one each for each of the potential buyers.

Second, for what you asked about the ad creatives not known in the browser ahead of time but only something you learn as a response in the middle of the auction as a response from the key value server:  It is a problem because it does not play well with the k-anonymity constraints.  The way that a browser knows that an ad is k-anonymous and therefore knows that it has appropriate privacy properties to show to a user, involves looking information up on the k-anon server which is an operation that generally happens earlier in the process, not in the critical path. So it is not viable from the latency point of view for the browser to only learn what the candidate ads are after all the bidding, key value return and and then Javascript execution of the bidding has taken place.   That is late in the process.  A lot of other issues - poor performance properties and poor privacy properties, for example.  There is a issue in Github repository by Martin Thompson that points out the kind of privacy leakage that you could exploit if we made the change you're asking for.

[David] So in the context, how would a publisher create an interest group to sell to different buyers that want to bid for it?

[Michael] Exactly the reason I am talking about this.  I think it is extremely reasonable for anyone to create and monetize an audience. But the transaction in which there is some kind of deal between the audience creator and buyer of that audience is not something that happens in the moment of the auction.  Like many parts of PAAPI, things should happen ahead-of-time even though in ORTB we think about them happening at auction time. With PA we think of them happening earlier at interest group creation time or update time, for example.  This is a good example of that.  The audience creators' choice of what buyers they want to work with given an opportunity to buy this audience in exchange for dollars is something we feel should take place ahead of time and if that requires some new sort of reporting for this audience at bidding time, then we are interested in providing additional reporting functions.  A lot of discussion on this topic has happened in this group.  We certainly can talk about shortcomings.  But handing an audience from an audience creator to a buyer in the Protected Audiences world has to happen ahead of time, not in the moment of the auction.

[Daivd] Does that mean you are effectively agreeing that in some kind of offline world, that you want to buy this audience. So the publisher or whoever they delegate to create the audience will sell it while the creative… that all of that is basically agreed to in advance before that interest group is created?

[Michael] I think that the sort of thing you are talking about could happen in an automated real-time way at the time of audience creation.  Most things that involve financial transactions between two people tend to need some kind of setup to happen ahead of time outside the browser, of course, because it involves money changing hands.  So I don’t think that is something dramatically new and different.  Aside from the issue of how the payment channel gets established, the idea of creating a new audience and making it available to multiple parties to bid on is absolutely something that can happen in real time between two parties that have agreed on audience creation and then audience delegation based on whatever protocol and financial consideration they want.  Again, I'm sorry the RTB House folks aren't here because they have worked on this and I expect they would have interesting opinions to contribute.  Maybe we can revisit this on a future call when folks with more experience are here.

[David] Last question,  in the case of the publisher creating the interest group, who initiates the auction?

[Michael] Exactly as they are today.  Initiated on a publisher page by a publisher or ad tech that publisher works with.  

[David] The interest group  will basically have the owner initiate some buyer.  And then the bid requests will go to that buyer.  Correct?

[Michael] Yes that is the way it works today in Protected Audiences.

[David]  Do you have a reference to the RTB product?

[Alonso] Yes, Prime Audience.  [www.primeaudience.com](www.rimeaudience.com)

[Brian] Wavering back and forth because I’m not sure how far into this we want to get. I assume the concern for publishers is that I want to maintain control over the value that I provide to my buyers via my audiences.  We may want some way of metering the value that I am providing a buyer so that I know what my interest groups are.  Are some interest groups more valuable on one browser versus another?  And if someone is doing something I am uncomfortable with, can I block them from using those audiences?  Publishers want to abide by a set of rules, and those are maintained by the goodwill of those who are using them versus any control you have directly over things.

[Michael] That makes a lot of sense.  Offhand I can think of three plausible approaches to deal with that set of questions:



1. Some contractual arrangement between audience creator and audience buyer on what those rules are and they enforce the contract however they want.
2. There is some mechanism in the browser that allows for _reporting,_ so that the party that was involved in audience creation can get a stream of information about how the audience later gets used.  That lets them monitor proper usage vs.the contract.  That is a monitoring approach wiIth the possibility for after-the-fact repercussions if someone is acting in the wrong way - after the fact reporting. 
3. There is some mechanism in the browser that allows for _control,_ so that the use of interest groups is not solely in the hands of the buyer.  Instead there are other controls - multiple parties involved in real-time that ensure that the interest group  is used in the way it was supposed to - buyer and seller have independent control over what the interest group does at the moment at the auction.

Seems to me like 3 is rather difficult - not sure if it's impossible, but at least relatively difficult and different than how Protected Audience mechanics work today.  On the other hand 2 - seems very compatible with the way Protected Audiences works today.  This may be fully supportable today with our additional reporting endpoints, or we may need to add some additional reporting so that parties involved in audience creation can be sure they have an ongoing ability to monitor how the audiences are used.  If there are holes in reporting that make it not suitable for this use case, then we can talk about adding additional reporting features.

[Brian] I think for the second option I would want to be able to kill my interest groups as well.  If someone doesn’t abide by contract, shut them down and make them non-functional

[Michael] I understand the desire for a kill switch, but I’d have to think about how to implement that. Doesn’t seem like something that naturally exists today.  Less demanding than full control, kind of an option 2½ .  So I would need to think about it.

[Brian] Maybe KV server could allow the owner to do that

[Michael] The KV server touching the browser at the time of the auction is in control of the buyer, so the wrong party in the trust model you're worrying about.  I will keep thinking about this.  It is an entirely reasonable use case and one we are happy to talk about supporting.  If features are missing, we can certainly find them.

[Roni]  Brian captured a lot of it.  A difference between “this is my audience and you can use it” and “I am creating an interest group on your behalf” - which is what the browser lets you use today.  Sure it can be done post- auction - but that is a bit problematic.  Sure reporting should tell you what happened but there should be better responsiveness.  There are then scalability challenges.  Also the issue of making 100s of interest groups for every buyer they have seems “heavy” and problematic.  In the interest of time, I will leave it there but want  to noodle more.

[Michael] Yes, interesting things for us to think about.

[Matt Davies] In generic terms, what about third parties that want to create an interest group?  Maybe we give it to an agency, who then might want to have multiple options for DSPs to do that.  Does it just have to be at time of creation or could there be other use cases?  Where a data provider can create the interest group and then have it duplicated into multiple SSPs/DSP?

[Michael]  Right now it has to be done at creation time.  Willing to think about your use case, but am concerned about cloning an interest group.  That sounds like a way to get runaway interest group creation.  Something we need to pay attention to.  Figuring out how to defer…how to allow a third party to take the interest groups or to allow them to default to them. You can’t have a set of different bidders each running their own code.  Not interested to have every interest group having an auction that feeds into itself.  Not interested in creating taller and deeper trees of IGs at auction time.

[David]  Agree with Roni.  Does not make sense for publishers to create hundreds of thousands of interest groups.  And if only one buyer can actually bid on an interest group then why is there a need for PA?  The aim is for the seller (publisher) to create an interest group and auction this interest group to a number of buyers.  

[Michael]  Are we talking about a buyer bidding on an interest group created by a publisher when visiting that same publisher's site, or when they leave the publisher site and see an ad elsewhere?  If directly on their site, you don’t need Protected Audiences at all!  That can just be done with information passed between the publisher and the buyer, and it doesn’t touch any of these APIs at all. Protected Audiences only applies if you want to create an audience on site 1 and use it on site 2.  If you're using it on site 1 as well, for example a "Seller Defined Audiences" use case, then this whole discussion is not relevant.

[Brian]  So agree with Roni about the explosion of interest groups to gain a foothold on browsers.  We need to find a way to prioritize interest groups.

[Michael] It is what we already do.

[Brian] What I’m concerned about is you hit a publisher page, it creates a bunch of interest groups, it washes out a number of interest groups - and we end up constantly churning through interest groups.

[Michael] Excellent reason that interest group creation should be in the hands of buyers who are actually using the interest group.
