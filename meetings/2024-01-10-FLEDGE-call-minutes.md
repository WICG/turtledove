# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 4pm UTC (during winter)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Jan 10, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)[first half only]
2. Roni Gordon (Index Exchange)
3. Brian May (dstillery)
4. Sven May (Google Privacy Sandbox)
5. Paul Jensen (Google Privacy Sandbox)
6. Jacob Goldman (Google Ad Manager)
7. Shankar Venkataraman (Jivox)
8. Timothy Taylor (Flashtalking)
9. Matt Menke (Google Chrome)
10. Alex Peckham (Flashtalking)
11. Harshad Mane (PubMatic)
12. Amit Gupta (Jivox)
13. Taranjit Singh (Jivox)
14. Matt Davies (Bidswitch | Criteo)
15. Laurentiu Badea (OpenX)
16. Laszlo Szoboszlai (Audigent)
17. Drew Schoentrup (Big Crunch)
18. Pawel Ruchaj (Audigent)
19. Becky Hatley (Flashtalking)
20. Anthony Yam (Flashtalking)
21. Luckey Harpley (Remerge)
22. David Dabbs (Epsilon)
23. Fabian Höring (Criteo)
24. Owen Ridolfi (Flashtalking)
25. Michael Gulak (Flashtalking)
26. Chris Nachmias (Flashtalking)
27. Abishai Gray (Google Privacy Sandbox)
28. Rickey Davis (Flashtalking)
29. Benny Lin (Bombora)
30. Ricardo Bentin (Media.net)
31. Wendell Baker (Yahoo)
32. Sid Sahoo (Google Chrome)
33. David Tam (Relay42)
34. Andrew Pascoe (NextRoll)


## Note taker: Matt Davies


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


## Suggest agenda items here:



*   Any continuation of the Jivox discussion that ran out of time last week?
    *   Previous week notes on GitHub: https://github.com/WICG/turtledove/blob/main/meetings/2024-01-03-FLEDGE-call-minutes.md#retargeting---adserver--advertiser-perspective-shankar-venkataraman-jivox 
    *   Shankar Venkataraman / Taranjit Singh / Amit Gupta (Jivox)
    *   <span style="text-decoration:underline;">Ad servers & IGs - https://github.com/WICG/turtledove/issues/924</span>
    *   <span style="text-decoration:underline;">https://github.com/GoogleChromeLabs/privacy-sandbox-dev-support/issues/201</span>
    *   <span style="text-decoration:underline;">We will review this deck - </span>
    *   FT Questions / Topics relating to last week’s Jivox discussion
        *   Creative logic and access to auction and interest group data
        *   Ad selection limits for Protected Audiences and Shared Storage in the future
        *   Practical participation by advertiser ad servers and DCO providers
*   Isaac:
    *   Buyer/Seller Reporting Questions: https://github.com/WICG/turtledove/issues/682#issuecomment-1710965068
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Roni Gordon
    *   Sensitive signals - https://github.com/WICG/turtledove/issues/824
*   Fabian Höring
    *   forDebuggingOnly availability - https://github.com/WICG/turtledove/issues/632


# Notes

Shankar: 

Where did we leave off last week 

Wanted a way where the ad server will get the winning interest group back to the x-header or as a query parameter

Would prefer the x-header model that was the proposal 

One area extra was the confidence in the ad confidence part and guidance and what to do 

MKleber: 

Key question: does Creative render know IG group name 

History of the interest group name: 

When first began the protected audience API  (when it was called fledge / turtledove), including the IG name was very possible as part of the reporting side of the API.  In the win log the when you get the creative render then you could add in the IG Name.

However, you need to meet the K-Anon requirement and the IG Name does not automatically get K\_Anon 

On Event level - will report the creative AND IG Name if they are K-anon together then it will be able to be recorded. If the IG name breaks K-anon then it will not be reported.

That was the original thinking. 

Nowadays our thinking about the event level logging has evolved. There are multiple labels, called the "Buyer reporting ID" and "Buyer And Seller reporting ID".  They have taken over some of the job that was originally for the IG name.

Those labels override the IG name as the thing that might be added to reports.  But still, only reported if they don't break K-anonymity.

Shankar 

Want to know if the IG has won the bid - 

MK 

Problem with having IG Name win the bid is it may have ppi as part of the IG Name 

Has to be K\_Anon and the ad has to be shown to x number of people and then it could break if the 

SV: 

If you creating the IG on shoes etc - there are multiple urls for many different shoes and urls and components within a singular interest group 

MK 

Info in the render url could include everything you need to know what creative should render and be shown 

That can be sent at the time of the IG registration

Michael Gulak 

Does not understand when it is not k-anonymous enough that he may have missed - question in chat 

“Why is the dsp allowed to know the interest group name but not the ad server? Am I misunderstanding?”

MK 

At reporting time the dsp reporting is allowed to know the render url and this additional label - either the IG Name or other label (Buyer and Seller reporting id) 

Sound like the real question is, could we let  other parties know about this if this is k-anonymous? - it may be possible but Chrome needs to think a little more around the privacy risk 

You're asking why is there a Difference:  The dsp who receives the report - that party is required to have gone through the PS registration process and make some attestations - the dsps who are using this and receiving the reports has to make this declaration about not using the info to identify the user across different sites 

The other parties are not subject to this - because the dsp is being held to this standard chrome can be more relaxed about the info is available 

Have to be more conservative about the info available inside the iframe environment differently as there is more risk etc … 

SV - why is the adserver not getting this kind of information - if the AdServer can make this kind of attestation would this be possible. 

MK -if the adserver gets the information and the information affects the rendering environment, then basically every party involved in rendering the ad gets the information also.  We did talk about the rendering environment to go through the attestation process - seems dangerous as it is not the top level domain - everything else also needs to come from an attestation approved env.

What is much easier is to restrict the info available than to check and know that everything on the rendering process has gone through the attestation etc.  If you start rendering an ad, and then half way through it tries to load something from a non-attested domain, then the ad would break.

SV: 

Even in this basis is it not on the dsp to ensure that everything is correct etc… 

MK

The way we chose to manage risk on this is that the rendering env only gets to know the rendering info. What the render url should have is info on what to draw on the page - all the mixing of the data can happen in the rendering etc…. And the reporting goes to the dsp who has made this. 

This was deliberate in the design process - the dsp gets the correct info and more detailed info and the rendering process gets less info for privacy design sake . 

Other hands that are up: 

David Tam 

Trying to simplify how this works practically: 

Scenario - 2 users go to Nike website - User A is interested in trainers and user B interested in boots - can you add this to the same IG

MK - Yes - it is fine for the same interest group to have different userBiddingSignals for each user and a different set of ads for each user etc…. 

Brian May 

How is the K-anon monitored - adding any info to the creative URL would add complexity 

MK 

2 things that the browser and our k-anon system keep track of for each render url 



*   Is the render url (creative) over the K-anon threshold 
*   Is the render url + other label over the Kanon threshold 

Which extra label we choose depends on this process, could be IG name or one of those reporting IDs

Anthony Yam

Getting to the core of the issue - the dsp is being made responsible for things it doesnt normally do 

An ad server is usually making decisions on which creative to show - this is complicated 

When setting up IG the DSP doesnt know which creatives are going to be shown etc

MK 

There is no requirements in the way we have built PA - where the bidding and buying decision has to be made from the same company 

The truth of PAAPI - the creative has to be shown and the decision has to be shown has to be done without the identity of the user being known on multiple sites etc … 

AY 

Could there be a way for another system / operator could happen to make the decision of the creative etc… 

MK 

The key part of this is that the decision on the creative etc and what is being set up is that some of the creative selection happens before the user turns up on the publisher page etc… 

AY 

This is different to now - make you bid at the time of setting up the IG 

Why can't the dsp use other things to take into account in real time such as time / geo etc … 

MK 

Yes you can have those!  geo / time etc are fine.  what you don't have is info on the users across multiple sites. 

_Michael Kleber had to leave and passed onto Paul Jensen_

SV 

The bid logic determines what is the interest group and which render url is being presented at the time using the bid logic etc 

PJ 

What kind of info are you looking for at the time of bidding 

AY 

You can add lots of rich information at the time of the interest group - 

Why can’t you use the same set of info for creative selection decisions 

PJ 

There is no requirement that the decision is being made by a single instance - what would be the most useful decision 

AY 

What would be useful is history on what products have been shown in the past etc 

PJ 

All of this information is available in the interest group and can be adjusted in the IG

AY 

Not if everyone 

Alonso Velasquez

What represents the additional information to be added for creative selection etc? 

SV

The crux of the prob is the dsp is winning the bid based on the IG Name - using the nike example 

The IG is winning on shoes - how does the dsp know that the creative is for trainers vs hiking shoes etc … 

AV 

What information is needed for the ad selector from the bidder 

David dabbs 

The dsp could have lots of rotated creatives in this world which doesn't exist in paapi world - now you may have to merge the two companies 

SV

The dsp is all about getting what IG wins - the dsp knows he has won the ig called shoes 

How will he pass this to the ad selector who is not the dsp and how will the browser know which to select 

PJ 

There is one script that comes from somebody and the browser doesn't get a choice on this 

DT 

A lot of things are theoretically possible - but there is no plan for this at the moment 

SV 

If you look at the DV3 PS docs you have to upload all the static ads into DV3 

This is all that is being asked for by DV3 today 

This will pretty much break the dsp > adserver relationship currently etc 

PJ 

Part of this comes from the privacy restraints of not wanting to leak privacy information at ad render time - only way to do this is via the K-Anon and you have to know the render urls ahead of time 

SV 

Agreed but you have to have something after the script is rendered on the browser but the ad selector can then jump in at this stage 

DM 

Sounds like there is a need for the adselector to be part of the process that says we have won x and now we need to select which ad is rendered as part of the adselector

DT 

What would the campaign manager do - this is what they do - how do they integrate with DV3

SV 

There is a lot of docs on this - sounds like dv3 will call out to campaign manager to do this 

DT 

Sounds like this process is essentially broken 

PJ 

How does this happen today?

SV 

RIght now there is a lot of datapass back  at the decision point right now - but unf you need to do this with being k-anon you need to do this at creative selection point 

PJ 

What is the data passback sent to the creative selector?

SV 

There is a lot of information passed back either in realtime or ahead of time 

Need to know the context of the winning bid which could be impossible in privacy sandbox

PJ 

Could be impossible / v difficult for two entities to create the script that is needed then?

SV 

This should in part be at the point of advertising etc 

BM 

DSPs make bidding decisions / Adservers make creative render decisions 

AY 

Someone makes the IG 

States - this is the bidding owner - can make decision based on bidding 

Other - This is the creative owner and the creative owner should be able top update the creative etc 

SV 

If this was the case then the amount of IG can be reduced etc 

If the DSP is the owner example could create an IG on behalf of Nike etc… and then Nike change dsps the dsp could then use on behalf of Adidias?

PJ 

This likely happens now - PJ would like to hear from dsps 

Seems that a dsp calculating a bid should have some information based on the creative information as well 

BM

When you bid on an impression you don't necessarily know why you bidding on this impression 



*   At the point of bidding you may need in the moment info etc 
*   Example: if raining may show ad for a movie on vacation - if sunny could be an ad for sunscreen? 
*   What you need is the contextual information to know what is the bid context 

PJ 

Discussion is the difference between the bidding calculation and the creative decisioning 

PJ had imagined you would need to know the context of the creative when bidding is this the case?

DD 

Sometimes yes there is strong data that can be used at the bidding process however not always etc 

Sometimes you don't have that info and have a rotating option of creatives that could possibly be shown - however the context of the bid will add to the info needed by the ad server to create the choice of the data 

BM 

example - the dsp may set up a bid for shoe buyer interest group to always bid on a shoe buyer - but would not know in advance (especially at IG creation time) which particular creative is going to be shown at that exact time - change of seasons / new product etc…. 

The dsps may need to have to create 100s of interest groups with each creative for one line item for each IG

SV

if you have a product catalogue of 40,000 items you could have a minimum of 40000 creatives even before you get to offers and deals etc 

PJ

The idea of PA is that you narrow this down at the time of joining or updating the IG

 

BM 

The dsp may be able to pick that - but the dsp is not responsible for that, the creative person is responsible for that etc… 


### Notes from the chat 

Michael Gulak

4:15 PM

Why is the DSP allowed to know the interest group name but not the ad server? Am I misunderstanding?

Re: Shankar's question

Luckey Harpley

4:16 PM

does someone have a link to the documentation outlining the ad metadata fields 'reporterid' and 'sellerandreporterid' (I think they were called) and how they relate to k-anonymity?

Sid Sahoo

4:16 PM

The IG name is also not available to DSPs at render time

Sven May

4:18 PM

@Luckey, I think this is what you are looking for:[ https://github.com/WICG/turtledove/blob/main/FLEDGE.md#:~:text=buyerAndSellerReportingId%3A](https://github.com/WICG/turtledove/blob/main/FLEDGE.md#:~:text=buyerAndSellerReportingId%3A)

Michael Gulak

4:18 PM

That is (at least part of) what I am asking

Shankar Venkataraman

4:19 PM

https://docs.google.com/presentation/d/1DhgWHIQ29rNik5JQ-k6mVxVqIkMyAYYcmg0HPLrfZus/edit?usp=drivesdk

Michael Gulak

4:20 PM

Then shouldn't that privacy sandbox registration process account for the very valid role of the independent ad server?

Michael Gulak

4:23 PM

Can't it just be made a requirement that the assets and such are also hosted on a domain that went through the process? and prevent calling out to any domains that aren't vetted?

Michael Gulak

4:26 PM

I don't agree that this is required in the interest of privacy. The system can be architected to respect privacy while accounting for current use cases.

Anthony Yam

4:26 PM

I think this is the core of the issue. DSPs are not typically responsible to select creatives.

Nor to register which creatives/ads should could be seleted.

Rickey Davis

4:28 PM

With this conversation, what is the opportunity to rethink the structure here? with the explicit goal to enable independent ad servers to perform the tasks that advertisers expect and value from them? inclusive of creative decisioning at run time?

Sid Sahoo

4:29 PM

https://github.com/WICG/turtledove/blob/main/FLEDGE\_k\_anonymity\_server.md

Brian May

4:29 PM

Thanks Sid.

Amit Gupta

4:30 PM

Follow up question: If we are saying that interest group could be specific to end-user. In such cases, how do we expect third-party ad-servers to generate render URL specific to interest groups? Also, does that imply that there is no creative decisioning and all the creatives to be rendered against each product are known up front?

Michael Gulak

4:39 PM

The ad server can't access that information as they aren't a part of the attestation process as I currently understand the model

David Dabbs

4:44 PM

We / PAAPI talk about the renderURLs as an "ad" but often this is an endpoint for sequencing, rotation, customisation of the advert content from a selection of one of more groups of content instances.

Michael Gulak

4:44 PM

this would be potentially more navigable with a (much) longer timeframe for the rest of the industry to respond to these changes

Rickey Davis

4:45 PM

I don't think we're arguing with the privacy process - I think the discussion is why the access to that privacy safe information is limited to the bidder and not the renderer

Michael Gulak

4:46 PM

Respecting privacy does not mean that only huge companies can make informed decisions about what ad to serve

David Dabbs

4:46 PM

WWCMD

Michael Gulak

4:48 PM

I think all actors in the mix are compatible/happy to embrace k-anon. The problem is that the attestation process doesn't account for that being anything other than one monolithic company

David Dabbs

4:50 PM

Not sure I agree with that blanket on DSPs and creative selection, &c.

David Dabbs

4:52 PM

I'd say that's a contractural issue - similar to "what does the no-longer-engaged DSP does with any advertiser data, &c.

OWEN RIDOLFI

4:53 PM

DSPs will potentially control the "placement" being associated to a bid but within that placement hundreds if not thousands of different creative versions can exist.

OWEN RIDOLFI

4:54 PM

for example, it is common for a DSP to control the make and model of a vehicle winning a bid, but which of potentially thousands of unique combinations of ad elements are handled by the creative selector.

OWEN RIDOLFI

4:58 PM

other even more "up to date" functions ad servers handle are things like flight pricing which changes constantly. No DSP is real time onboarding flight prices and changing the creatives on the fly.

well, few. I can't say none.

Roni Gordon

4:59 PM

have to drop

David Dabbs

5:00 PM

THat's where one might leverage "adCOmponents"

Laurentiu Badea

5:01 PM

the more there are the harder to reach k-anon
