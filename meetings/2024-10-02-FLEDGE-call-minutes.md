# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Oct 2, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!	 



1. Michael Kleber (Google Privacy Sandbox)
2. Luckey Harpley (Remerge.io)
3. Roni Gordon (Index Exchange)
4. Sathish Manickam (Google Privacy Sandbox)
5. Paul Jensen (Google Privacy Sandbox)
6. Garrett McGrath (Magnite)
7. Matt Menke (Google Chrome)
8. Jacob Goldman (Google Ad Manager)
9. Orr Bernstein (Google Privacy Sandbox)
10. Sid Sahoo (Google Privacy Sandbox)
11. Fabian Höring (Criteo)
12. Brian Schmidt (OpenX)
13. Alex Peckham (Flashtalking)
14. Becky Hatley (Flashtalking)
15. Lydon, Jason (FT)
16. Tim Taylor (Flashtalking)
17. Matt Davies (Criteo | Bsw) 
18. Laura Morinigo (Samsung)
19. Yanush Piskevich(Microsoft Ads)
20. Felipe Gutierrez (Microsoft Ads)
21. Hillary Slattery (IAB Tech Lab)
22. Alonso Velasquez (Google Privacy Sandbox)
23. Priyanka Chatterjee (Google Privacy Sandbox)
24. Elmostapha BEL JEBBAR (Lucead)
25. Paul Spadaccini (Flashtalking)
26. Owen Ridolfi (Flashtalking)
27. David Dabbs (Epsilon)
28. Jeremy Bao (Google Privacy Sandbox)
29. Angela Crocker (Google Privacy Sandbox)
30. Sarah Harris (Flashtalking) 
31. Patrick McCann (S.H.I.E.L.D.)
32. David Tam (paapi.ai)
33. Victor Pena (Google Privacy Sandbox)
34. Kenneth Kharma (OpenX)
35. Koji Ota (CyberAgent)
36. Shafir Uddin (Raptive)
37. Miguel Morales (IAB TechLab)


## Note taker: &lt;Matt Davies>


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here (meetings with no items may be canceled)



*   Alexandra Peckham (need to push this to 10/9, due to attendance conflicts on our end again)
    *   Discussion of / update on this issue: https://github.com/WICG/turtledove/issues/1028 
*   Hillary Slattery - Deals (https://github.com/WICG/turtledove/issues/873)


# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!! :) 


# Notes 

## Deals, https://github.com/WICG/turtledove/issues/873

Hillary Slattery: 

High level summary from the Monday IAB Tech Lab meeting on product requirements for web advertising. 

As it relates to deal ids there are certain requirements in the Tech Lab monday meeting

Requirements

*   Support sending deal IDs provided by each seller to each buyer, where seller can be any permutation of ad server, SSP, and/or publisher
*   There must be a mechanism by which the buyer is able to communicate to the seller at bid time at a minimum the following attributes for sell-side decisioning: Seat ID + advertiser domain + bid price + deal id
*   The list is a minimum necessary for reconciliation, but other information may be passed 
*   Any information that influences the fees and/or the b

It is not possible to know ahead of time what deals are available to them at bid time, in the sandbox environment 

Jeremy Bao: When are they available - at bid time? 

David Dabbs: At any times and are fluid 

HS: If you know ahead of time then you need every permutations of deals and creatives etc and the maths become huge - 35qn combos etc

Michael Kleber: The presence of a deal id materially affects the priority in scoring and changes the money 

Deal ids as a concept has been massively overused and overloaded 

There is a huge difference between something that happens in the middle of the auction and also a publisher using the deal_id to label segments of audiences that a bidder might care about but doesnt affect the commercials 

Can we pull apart the two scenarios 

*   Which affect the commercials etc 
*   Which are more applied as a deal that is being labled 

DD - How often is deals curation vs audiences 

MK Inside of PSB they have two different techniques to get the information out of the auction 

*   Event level on the info on the auction, which can include 
    *   Values on the money 
    *   Winning creative 
    *   Deal ID 
*   Note that all this info has kanon constraints 
    *   This has high conflict with high cardinality fields 
    *   The more high cardinality fields added makes k-anon even stricter 
*   If there a million deal_ids that the buyer may choose, and also it needs to be specifically on the event level basis, then this has a k-anon problem 
*   If you are talking about the deal_ids can be reported post event _in aggregate_ then you are in a better position 

If the number of deals is all about who is needed to know where the commercials info is super important then you need less deals etc … 

HS: The deal object takes up more than half - 85% takes up the entirety the size of the bid request a very large amount of deals

MK: What are the dsps doing when you are getting a very large amount of deals in the request 

 
HS: All of the deal ids become heavily weighted inputs into the algos of the DSP’s bidding algos

MK: Nothing in PSB that restricts the number of deal ids and info into the auction and get corresponding in the response - there are no restrictions on this - info moves around in the auction - this is good news to a certain degree. 

What is the issue is getting the data out in reporting.  In aggregate reporting you can get this information.  In event-level reporting, there are privacy constraints.

Roni Gordon: Wether the information can or cannot be sent - the seller needs to know that the bidder has made this bid with the deal id and the sellers need the info 

MK: Who is going to use this information

RG: Buyers can do what they want with this information, they can use it for x number of purposes as this info shows up in the data and reporting. We need to know what is useful and what is not based on the reporting information. 

MK: Knowing this data in reporting then you can start to see predictions that are going to happen based on this information in the reporting. 

*   Is this reporting data allowing for better prediction model training this is a good use case 
*   Model training is a great aggregation story 

Alonso Velasquez: Anyone on the buy side please provide the usecases for deals 

*   Model training 
*   Attribution 
*   Reporting and billing 
*   Bid optimisation 
*   [add other use cases here] 

Please share with the details of the use cases 

HS: there is 10 bullets in the system req doc with the requirements for deal ids 

MK: the system requirements have a lot of discussions of the need to have deal_ids as a mechanism.  But we are not talking about the goals for what deal_ids are used for which makes these discussions much more relevant.  The Privacy Sandbox way is to understand the goals, then figure out a privacy-preserving way of achieving the goal, when it's possible.  Deal IDs are not the goal, they are the mechanism used to get to various goals today, and getting the same places in the future may require different mechanisms instead. 

DT: Would like to use the dealids for is to buy curated audiences from the buy side and is a mechanism to do that. The challenge is because the buyside is not creating the IG - as this is done by the seller how can the buyer use that deal / ids etc and need to work out the mechanism of how to do this. 

MK: Curated audiences is a good usecase - based on this audience in the curated deal - based on the presence of this deal id I will change my bidding strategy based on the presence of this deal Id 

Does this map 1:1 though - example I will bid when I see deal_id 123 and deal id 456 

Want to send this bid based on the presence of the intersection of both etc. 

Example - audience curation and the deal id provides extra useful information about this impression - this is very well supported in PSB 

This information can be sent in the auction and then bidders can pass this information to the sellers in the meta data and can be used for scoring - none of this requires the need for event level reporting 

David Tam: Agreed this may not be needed for the buyer’s event reporting.

MK: This is a use case where aggregated information and reporting is useful 

Elmostapha BEL JEBBAR: Troubleshooting use case: 

*   Troubleshooting a deal id is one of the biggest challenge in adops to understand why it doesnt work and having that information outside of ELT 

EBJ: Commercial data usecase - who is buying the deal_ids and operating the deal_ids on a daily basis and has enough granularity on a daily basis 

MK: Troubleshooting - the sort of reports we have related here is all about win reports - the PSB process on loss reporting - is already pushed into aggregate. There has not found a privacy story for loss notification on ELR makes sense - so all of this information needs to be within a aggregation. Need to discuss further around debugging 

Commercial data usecase - MK would like to understand this the most as there is the greatest risk of things not being supported right now and can think of a few ways to fix it and how this interacts with K_Anon. How many deal ids have this commercial extra requirements tha need ELR 

EBJ: Publisher which has different deal ids that represent different offers to different buyers and can then study the stats to understand the nature of what is happening and make better deals . 

MK: this does not appear to be restricted with the high cardinality - does not seem likely that the pub will offer the buyer 10000s of different deals and options for the buyers etc which could be that this is a low cardiality question - if they have their own set of 10 etc set of deals for each pub and buyers etc then this can be solved as is low cardinality 

Pub could prepare 100 deal ids each PMP packing their inventory differently and then expose to the buyers and the buyers would then bid / buy on the deal ids differently based on the package and the buyers / pubs would do some different analysis and packages based on the deal ids that they have. 

MK: combo of the different use cases, package up the details of the audiences AND change the terms of the commercial transactions 

Pull these two use cases apart to

1. Package up audience segments (aggregate) 
2. Change the terms of the commercial deals / details (low cardinality) 

HS: Deal id is a numerical representation of something that they have agreed upon 

MVP - at run time the buyer needs to communicate to the seller 

Buyer needs to know from the seller what is available 

Seller needs to know from the buyer the deal id so they can do their lookups

Deal ids also communicates details of price, priority etc 

Anytime an auction renders both buyer and seller knows who to cut the check to and for how much etc … 

MK: Trying to pull them apart - So long as the reporting has the details of the price, buyer seller and renders does it need the deal id 

HS: Push back it is needed as sellers need to know this information 

JB: There is a potential problem with the requirements for K-Anon and the high cardinality 

If there is this high cardinality issue - if we need to keep k-anon then the deal ids wont work, but if we remove k-anon then these things can become identifiers etc .. 

MK: The threat JB is talking about around k-anon - there is an ad and is stored in my browser then this ad could be an identifier - could the presence of the ad show that it is an indicator of who the add is shown to and then you get the event report then you know who the user is being sent to 

If you add in deal id this makes things much more likely to have a Identifier of the user and this is the fundamental thing we are trying to prevent 

Deal ids and commercial do not relate to users and is unlikely to see that the 

To identify the user we need to pick this ad and the deal_id no 37 etc as this is allowing us to identify a specific person or user as all we need to do is to get the ID is a combo of the ad and deal_id - how can we make the goals possible of packaging up the inventory with deals but not identify the users 

Why we need ELV on deals

*   Frequency cap 
*   Sox compliance 
*   Co-ordianation between parties - it solves for the need to coordinate between multiple parties 

MK: this is a problem and should be addressed within the IAB TL to handle co-ordianation 

We need to understand the cases where the bid values is interpreted and affects the commercials where it really does need EVL compared to the requirements for aggregate reporting 

PM: use case for deal ids where it affects the commercials 

*   Example there are multiple deal_ids where there could be different click through rates etc could mean for a difference in the fees requirements and the seller is deciding on how to distribute the moneys that relate to how this 

 
MK: Seller may identify the slice of inventory to declare to buyers this is a valuable  of inventory 

3 different sell side entities making this identification - who ever is making this identification changes what money is sent to which party (component / pub / other etc) 

It is not the source of the intel - but usually the staff on the ground selling this deal to the advertisers etc who are pitching the deals 

When party 1 attaches the deal there is a huge incentive of party 2 provide a copycat deal ids to gain the system? - Yes pubs and ssps do this all the time. 

JB: Pulse check - the current solution has limitations - how badly will this affect everyone

How big of a problem is this? 

HS: The location of the deal_id where it starts breaks everything downstream 

MK the list of the deal ids in ORTB come from the sell side where as in PSB it comes from the IG 

Is there any subset of the deal_id usecases where this could work or is this fraction 0? 

This appears to be impractical - the rest of the stuff we discussed lets put this in a logical way 

If the number of in play deal_ids is around 10 or so where this is commercially relevant - if that is possible then we can patch things together with our current workflow 

Now the interest group has to carry around the 10 different options that they can select. 
