# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 4pm UTC (during winter)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Jan 3, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Brian May (dstillery) 
3. Shankar Venkataraman (Jivox)
4. Taranjit Singh(Jivox)
5. Kevin Lee (Google Privacy Sandbox)
6. Ricardo Bentin (Media.net)
7. Sven May (Google Privacy Sandbox)
8. Paul Jensen (Google Privacy Sandbox)
9. Amit Gupta (Jivox)
10. Sid Sahoo (Google Chrome)
11. Andrew Pascoe (NextRoll)
12. Matt Menke (Google Chrome)
13. Harshad Mane (PubMatic)
14. David Dabbs (Epsilon)
15. Joshua Prismon (Index Exchange)
16. Zach Edwards (Victory Medium)
17. Abishai Gray (Google Privacy Sandbox)
18. Orr Bernstein (Google Privacy Sandbox)
19. Jonasz Pamuła (RTB House)
20. Matt Davies (Bidswitch | Criteo) 
21. Don Marti (Raptive)
22. Becky Hatley (Flashtalking)
23. Isaac Foster (MSFT Ads)
24. Timothy Taylor (Flashtalking)
25. Kenneth Kharma (OpenX)
26. Christopher Nachmias (Mediaocean / Flashtalking)
27. Jeroune Rhodes (Google Privacy Sandbox) 
28. Laszlo Szoboszlai (Audigent)
29. Daniel Rojas (Google Chrome)
30. Jacob Goldman (Google Ad Manager)
31. Leeron Israel (Google Privacy Sandbox / Chrome)
32. Rotem Dar (eyeo)
33. Nick Llerandi (Triplelift)
34. Alex Cone (Google Privacy Sandbox)
35. Alex Peckham (Flashtalking)


## Note taker: Orr Bernstein


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


## Suggest agenda items here:



*   Isaac:
    *   Buyer/Seller Reporting Questions: https://github.com/WICG/turtledove/issues/682#issuecomment-1710965068
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Jonasz (RTB House)
    *   3pc deprecation timeline: https://github.com/WICG/turtledove/issues/717#issuecomment-1847118918
*   Roni Gordon
    *   Sensitive signals - https://github.com/WICG/turtledove/issues/824
*   Matt Davies (Bidswitch) 
    *   Origin / Traffic shaping -<span style="text-decoration:underline;"> https://github.com/WICG/turtledove/issues/951</span>
    *  (Update: can remove from agenda, this has been addressed by conversation in the GitHub issue)
*   Jeroune Rhodes (Google) 
    *   **Join Privacy Sandbox Developer Webinar: Protected Audience API Reporting**
        *   The Google Privacy Sandbox team will be hosting our next series of webinars on the Protected Audience API. This set of webinars will focus on reporting and how you can measure data related to a Protected Audience auction. The first **Americas friendly session** is happening on** Jan. 16th 3-4 pm ET**. A second **EMEA friendly session** is happening **Jan. 18th 12-1 pm GMT**. A third **Japanese language session** will be held on **Jan 30th 9-11 am JST**.
        *   To join, please register below: 
            *   AMER-friendly: [Register Here](https://rsvp.withgoogle.com/events/protected-audience-webinar-3-reporting-amer)
            *   EMEA-friendly: [Register Here](https://rsvp.withgoogle.com/events/protected-audience-webinar-3-reporting-emea)
            *   Japanese language: [Register Here](https://rsvp.withgoogle.com/events/protected-audience-office-hour-3) 
*   <span style="text-decoration:underline;">Shankar Venkataraman / Taranjit Singh / Amit Gupta (Jivox)</span>
    *   <span style="text-decoration:underline;">Ad servers & IGs - https://github.com/WICG/turtledove/issues/924</span>
    *   <span style="text-decoration:underline;">https://github.com/GoogleChromeLabs/privacy-sandbox-dev-support/issues/201</span>
    *   <span style="text-decoration:underline;">We will review this deck - </span>


# Notes


## 3pc deprecation timeline: https://github.com/WICG/turtledove/issues/717#issuecomment-1847118918



*   Jonasz (RTB House)
    *   Can we get more guidance on the timeline for 3pc deprecation?
    *   We have a lot of coordination, monitoring, adjustments to perform before phase out.
    *   Start a discussion, get some clarity, thoughts of others on how this could look. Bump from 1% to 10% would be a great step in Q3 or Q4
*   Michael Kleber
    *   The actual answer to the question, at what time will be first be able to perform deprecation and removal of 3pc is not entirely in our own hands. Oversight of the UK’s Competition Market Authority. Testing that will be happening over the next five months, hope that many of you will be participating. After testing period, Chrome may say that they would like to remove 3pc. CMA might say yes, they might say you have to wait 60 days, or other answers.  We cannot predict what is going to happen.
    *   When we start ramping things up, there’s a few things I can say. Not specifically about removal of 3pc, but generally on the rollout of potentially dangerous features on the web. There’s no chance that we’ll go from 1% to 100% directly. 10%, 25%, 50%, 100%, like Jonasz suggested, is the kind of thing we have done in the past. Ramping to something like 10% and holding there and watching and waiting to see any reports of breakage is consistent with the way we do things.
    *   The question about exactly when things are going to happen even aside from the CMA, we know that ads folks don’t like things to change in Q4, especially before Thanksgiving and between Thanksgiving and Christmas, we’ll try to be sensitive to those kinds of considerations as we choose the times at which things change.
*   Jonasz
    *   Understand that further phaseout is conditional on CMA approval
    *   But we’d like to know what’s being considered if CMA agrees. 
    *   When it comes to stages, it’s good to hear that that sounds good to you as well. But how long do stages last? If it’s a couple of weeks, that’s not very helpful. If it’s months, that seems more reasonable.
    *   There was a blogpost that announced the 1% on January 4th. But in that blogpost, it says that this is an important milestone on the path to full deprecation of 3pc in 2024. Is Google’s plan to do the full phaseout in 2024? The lack of clarity really paralyzes the whole effort.
*   Michael
    *   Anything that involves what the CMA is going to do and how quickly is going to do it is a difficult conversation. I understand that further clarity would be helpful, we’ll try to say everything we can, but won’t be able to say everything you want to.
*   Brian May
    *   The first CMA "60 days" pause is mandatory, not optional
    *   Is the goal of the Google team to try to identify benchmarks that would guide the deprecation?
*   Michael
    *   Have not heard anything that’s based on a metrics-style feedback loop. We’ll have some timeline for when we plan to ramp up, and if something surprising happens, like if we learn about a large unexpected pocket of web breakage, we’ll reevaluate and potentially delay the ramp up. But I don’t know of any other example where that kind of closed loop thing has happened, wouldn’t expect it here either.
*   Isaac Foster
    *   The thing that’s tricky for me to understand is, I would assume that you guys could potentially say, we’re not going from 1% to 100%. We’re going to express that timeline. There’s something about the agency being all on the CMA that I’m not understanding. Why can’t we just say, hey, let’s go with Jonasz’s proposal or something like that.
*   Michael
    *   I have no desire to speak for the CMA, and wouldn’t want to even if I could. I have no idea what they may say about things in the future.
*   Isaac
    *   It’s being presented as if all of the agency is on the CMA, and that doesn’t sound quite right to me. Is that the case?
*   Michael
    *   The CMA is very interested in taking input from a large number of parties, and a large number of parties have strongly held opinions about the right way to do everything involving third-party cookie deprecation. So we will try to listen to what everybody says about the timing and be responsive to all of their needs. Whether it’s Chrome or the CMA or some combination thereof, the goal in general is to hear everybody’s needs and figure out some path between them all. Nobody is making a unilateral decision.
*   Shankar Venkataraman
    *   When we do the rampup, my question is, how do we get the advertisers involved? Every advertiser has campaigns running. How do they build up their interest groups for that test campaign? If we have a timeline, we can provide our customers with guidance.
*   Michael
    *   We understand that clarity will encourage everybody else in the industry that hasn’t yet taken the steps to start to do things. For example, as Jonas pointed out, when we go to 10%, when 10% of the traffic is without 3pc, that’s a very good time for those who haven’t yet taken the steps to start doing so.
*   David Dabbs
    *   Very clear that it’s completely the CMA’s call, the timeline, in their recently quarterly notice to you all. The question here is, is the way you conduct the deprecation, can you say that? Are they calling the shots, approving or disapproving?
*   Michael
    *   The only thing I can say is, the CMA is a regulator that is welcome to have authority over any part of the process they want to have authority over. If they want the timeline to be up to them, it’s entirely their choice for them to do so.
*   Jonasz
    *   It seems that we haven’t learned much after today. Is there anything we can do to unblock this? It seems the decision really involves the CMA. My thinking is that being vocal about this - get this message to the CMA.
*   Michael
    *   What I can tell you is that the CMA is surely aware of this discussion. They are paying attention. Whatever happens on GitHub they are very aware of. Further discussion on GitHub repository is good. Linking to the blog post from the GitHub issue may bring stuff to their attention if they don't already know. Direct feedback straight to the regulator in any fashion you choose.
*   Harshad Mane
    *   Will Chrome take a couple of months to declare after the CMA decision?
*   Michael
    *   We’ll all know that when it happens.
*   David
    *   If you are going to write a letter or make a case to the CMA, they have a pretty stringent timeline. If you don’t get it to them by a certain time, they don’t look at it until the next quarter. So if you’re going to make a case, you should do it soon.
*   Michael
    *   True that as a government regulator they have a clear process when they are in public comment, but they are always listening.
*   Brian May
    *   Just to clarify for myself - we have until the end of June for the testing period, then the mandatory 60 day holdout period, which puts us at the end of August and the beginning of Q3, and the Q3 advertising period. If we go out another month, that puts us right at the door of Q4. Is it reasonable to assume that there will be a pause, or should we expect some possibility that in spite of code freezes and Q4 we might have additional deprecations.
*   Michael
    *   As I said before, we (Chrome) understand that late Q4 is a bad time to do things that everybody wants to be in code freeze for the last six weeks of the year. Feel free to raise that feedback on GitHub and via all of the usual channels.
*   Brian
    *   Is what I said about timeline correct?
*   Michael
    *   What I can say is that [privacysandbox.com/timeline](privacysandbox.com/timeline) - middle of Q3 to middle of Q4 is what’s highlighted there as 3rd party cookie phaseout.


## Join Privacy Sandbox Developer Webinar: Protected Audience API Reporting



*   The Google Privacy Sandbox team will be hosting our next series of webinars on the Protected Audience API. This set of webinars will focus on reporting and how you can measure data related to a Protected Audience auction. The first **Americas friendly session** is happening on** Jan. 16th 3-4 pm ET**. A second **EMEA friendly session** is happening **Jan. 18th 12-1 pm GMT**. A third **Japanese language session** will be held on **Jan 30th 9-11 am JST**.
*   To join, please register below: 
    *   AMER-friendly: [Register Here](https://rsvp.withgoogle.com/events/protected-audience-webinar-3-reporting-amer)
    *   EMEA-friendly: [Register Here](https://rsvp.withgoogle.com/events/protected-audience-webinar-3-reporting-emea)
    *   Japanese language: [Register Here](https://rsvp.withgoogle.com/events/protected-audience-office-hour-3) 
*   Jeroune Rhodes (Google)
    *   Raising awareness about this.
*   Michael
    *   This webinar is about the reporting aspects of the Protected Audience API at a variety of times in an attempt to be friendly to people from around the world.


## Retargeting - Adserver / advertiser perspective (Shankar Venkataraman, Jivox)



*   <span style="text-decoration:underline;">Ad servers & IGs - https://github.com/WICG/turtledove/issues/924</span>
*   <span style="text-decoration:underline;">https://github.com/GoogleChromeLabs/privacy-sandbox-dev-support/issues/201</span>
*   <span style="text-decoration:underline;">We will review this deck - </span>
*   <span style="text-decoration:underline;">Shankar Venkataraman / Taranjit Singh / Amit Gupta (Jivox)</span>
*   Shankar Venkataraman
    *   (presenting https://docs.google.com/presentation/d/1DhgWHIQ29rNik5JQ-k6mVxVqIkMyAYYcmg0HPLrfZus/edit#slide=id.p)
    *   Retargeting as it exists in the 3pc world
        *   User A and user B go to Advertiser TopShoes.com website.User A browses "boots" and user B browses "trainers”.
        *   The advertiser creates a retargeting pool against this behavior. The pool granularity could be “shoes” or “shoes.boots” or “shoes.trainers”. The campaign strategy decides which granularity to use when ad is served. 
        *   The DSP usually combines these pools into a macro pool (to maximize reach), but the ad servers keep the granular pools. The ad server can use the granular pools across multiple DSPs.
        *   An ad is served to the user when visiting a publisher site like news-site.com. The creative served will have different product images (and corresponding offers / price etc) based on the pool to which the user belongs to.  i.e. User A sees Boots and User B sees trainers. 
    *   First question - are these namespaced to the advertiser domain?
        *   Advertiser works with multiple DSPs
        *   If each DSP is going to register multiple IGs, then the number of tags just multiplies
    *   What happens when advertiser stops
        *   Currently, interest groups are owned by the DSPs.
        *   Seems to be a problem for anybody who’s operating as an ad server
*   Harshad
    *   Wanted to answer that right now in Protected Audience auctions, DSPs are invited to participate. IGs have to be owned by DSPs because advertisers are not directly invited into the auction.
*   Shankar
    *   The audience is actually owned by the advertiser. If they put a promotion on their site by putting a new category, it’s almost impossible for a DSP to keep up with all this stuff.
*   Harshad
    *   In the current cookie world, .
*   Shankar
    *   The InterestGroup is not determined by the DSP. The advertiser tells the DSP that it wants to retarget to this audience.
*   Harshad
    *   Maybe the advertiser will need to inform the DSP to generate interest groups based on a certain action.
*   Shankar
    *   But that’s not the way it works. On an ecommerce site, there’s something called new &lt;content?>. Could we make it easier for advertisers? They’re dependent on each of the DSPs to change the interest groups. Worst case, if the DSP of the advertiser changes, the DSP still owns the interest group. They could use the same interest group to target the competitor.
*   Michael
    *   Let me answer the question you ask about granularity of targeting, e.g. shoes vs boots vs trainers. I would like to be clear that a single interest group can do all of those things. You do not need to think of this as putting a person into three different interest groups in order to signify three different details of what the person showed interest in. It’s not a boolean. Instead you could put a person into a single interest group that’s related to the advertiser site that they were visiting, inside the interest group, the user bidding signals could have a list of all of the subcategories of shoes and how much the person seems to be interested in each of those categories. And all future ads or ad campaigns that buy using that interest group can customize their targeting. An interest group is a much richer object, and it has the ability to hold a lot of information and can serve many different ad campaigns based on that.
*   Shankar
    *   We couldn’t figure that out from the API spec.
*   Michael
    *   Happy to talk about this here, happy to talk about this on the GitHub issue. Don’t feel like you need to put a user into a lot of interest groups in order to advertise on a lot of facets of the user behavior.
    *   The second question is about namespacing and multiple DSPs. You’re correct that the way we designed the PA API, every interest group is owned by a single DSP. The DSP also has bidding logic. If an advertiser wants to have three different DSPs, exactly as Harshad was just saying, there would be three different joinAdInterestGroup calls on their page, from all three different DSPs, so the user can end up in all of their interest groups, each out representing a single DSP’s ability to bid on behalf of that advertiser.
*   Shankar
    *   Impression control - if I want to hit the user only once per day, but there are three DSPs running.
*   Michael
    *   How does that work today? If you have three DSPs each independently making buying decisions, and each doesn’t know the buying decisions of the other ones.
*   Shankar
    *   Right now, it’s messed up too, I was hoping you could solve it.
*   Michael
    *   We have not tried to solve exactly the question that you’re asking about. But it’s not implausible that we could do so. Right now, everything is scoped at the DSP level.  Any kind of cross-DSP rate limiting in the auction would be new work.
*   Shankar
    *   Why I was pushing for one interest group per advertiser is because of the corollary. If you own the shared storage domain at that point, then you …
*   Michael
    *   Right now, the way things would work, you could only do the control at the advertiser level across different DSPs in a post-auction sort of way. Shared storage is available, and when it goes to show the creative, it could check shared storage, and if it’s already shown the ad, it could show a different ad. This works well for a goal like "creative rotation": An ad could look in different ways depending on what’s already been shown that day, using the Shared Storage and selectURL functionality.  That is available today and would work across DSPs.
    *   Within the auction, information is at an interest group-by-interest group level, and cannot be shared across interest groups, and thus cannot be shared across DSPs either. But if we have multiple layers of  ownership, an advertiser layer and a DSP layer, that is a feature we could add. It could be compatible with how Privacy Sandbox works because you’re asking for interest groups to be either in or not in the auction based on information on the device. It’s not a feature we’ve already built. We’ve already built negative targeting based on interest groups for contextually targeted ads. The thing you're asking for is related, because it’s negative targeting of interest groups based on other interest groups.
*   Taranjit Singh
    *   To cover Shankar’s point, we already discussed that we could use negative targeting, But not sure if the spec itself explains it. When a regular interest group wins an auction, there’s no mapping in terms of that based on the bidding signal or user input signal.
*   Michael
    *   This is getting at a change that happened over course of discussion of the PA API.  Each ad in the IG can carry a BuyerAndSellerReportingID that might help with this.
    *   The interest group name could be very coarse. The user data in that IG could be everything that IG knows about what I’ve done on that site. It’s possible to push ads into that IG based on the full set of information about my behavior on 1 site. When those ads are rendered, they can’t have that identifier, because none of those ads would then have met the kAnon threshold. You could have all of the ads and all of the ad campaigns playing together in a single interest group and then part of that reporting structure, and those all could be targeted based on different facets of my behavior on a single site.
*   Anthony Yam
    *   DSP is also the ad server, kind of critical for us, when you’re saying that ads can be added at creation and update time, the tech partner knows what the ads are, not the DSP. Back to the comment before, can only the actual creative partner use shared storage?
*   Michael
    *   Already over time, so can’t discuss in detail. Already an issue that was opened by the index exchange folks that is about allowing things to load from different domains so that it’s possible for an IG to talk directly to an ad server of a DSP to get a creative. But the metadata for the creative has to be enough to know how to do things at auction time instead of post auction render time.

## Here are the in-call sidebar chat comments…

Don Marti

11:11 AM

https://manifold.markets/DonMarti7bd2/will-google-chrome-support-thirdpar

 \
Harshad Mane

11:13 AM

This year :)

 \
David Dabbs

11:13 AM

And an epic election cycle!

 \
Kevin Lee

11:17 AM

jonasz, would you be able to link that article you just mentioned?

 \
Jonasz Pamuła

11:17 AM

Sure:[ https://blog.google/products/chrome/privacy-sandbox-tracking-protection/](https://blog.google/products/chrome/privacy-sandbox-tracking-protection/)

 \
Kevin Lee

11:17 AM

thank you!

 \
Jonasz Pamuła

11:17 AM

" We'll roll this out to 1% of Chrome users globally, a key milestone in our Privacy Sandbox initiative to phase out third-party cookies for everyone in the second half of 2024"

 \
David Dabbs

11:22 AM

We are all empowered to write "amicus brief" memos to the CMA making suggestions about how the rampdown is conducted.

 \
Warren Fernandes

11:25 AM

Is there a resource available that details which privacy sandbox APIs require enrolment?

 \
David Dabbs

11:25 AM

Yes. The ensollment GH repo.

 \
Sid Sahoo

11:25 AM

https://goo.gle/privacy-sandbox-enroll

 \
Warren Fernandes

11:25 AM

Thanks

David Dabbs

11:25 AM

https://github.com/privacysandbox/attestation/

 \
Brian May

11:29 AM

Perhaps an open letter from industry participants?

 \
Harshad Mane

11:29 AM

+1

 \
Joshua Prismon

11:29 AM

Will need to drop for a moment. Be back in 5 minutes.

 \
David Dabbs

11:32 AM

Can and will be used against you...

 \
Warren Fernandes

11:33 AM

Wrt the enrollment mentions of "site" could this apply to SSP domains that span across websites?

i.e. can a single domain enroll and use the APIs across websites?

 \
Isaac Foster

11:34 AM

the domain that would be checked is the on of the IG owner or auction runner

so fairly sure the answer to your question is yes

 \
Owen Ridolfi

11:34 AM

IG stands for?

 \
Warren Fernandes

11:34 AM

Perfect, thanks Isaac

 \
Isaac Foster

11:34 AM

interest group

the owner of the IG is, presumably, the DSP, generally not the advertiser themselves

(it can be ofc)

 \
David Dabbs

11:35 AM

"Third-Party Cookie Phaseout" sung to the tune of "Tenth Avenue Freeze out"

 \
Jonasz Pamuła

11:35 AM

If you like, please add your thoughts about timeline & the importance of knowing it early in[ https://github.com/WICG/turtledove/issues/717](https://github.com/WICG/turtledove/issues/717) to strengthen the message

Brian May

11:36 AM

Thanks Jonasz.

Isaac Foster

11:39 AM

sorry ntd for internal fledge meeting

thank you all

Warren Fernandes

11:42 AM

BTW is there any guidance on the role GAM plays in the FLEDGE process

Sid Sahoo

11:47 AM

https://github.com/google/ads-privacy/tree/master/proposals/fledge-multiple-seller-testing

Warren Fernandes

11:49 AM

Thanks

B. McLeod Sims

11:49 AM

is there a link to the spec explaining that?

Sid Sahoo

11:49 AM

That = GAM's role?

B. McLeod Sims

11:49 AM

sorry the IG being able to have non boolean behavior

Sid Sahoo

11:51 AM

https://github.com/WICG/turtledove/blob/main/FLEDGE.md#11-joining-interest-groups

Owen Ridolfi

11:52 AM

the ad server would control it.

Paul Jensen

11:52 AM

@B. McLeod Sims: The IG contains a userBiddingSignals field that can contain arbitrary information.  This information is available at bidding (i.e. generateBid() time).  This is described in the Explainer link that Sid provided, and is also in our spec.

B. McLeod Sims

11:53 AM

cool, i missed that bit, thank you for the explanation

David Dabbs

11:54 AM

renderURLs do not need to be same-origin with the bidding DSP(s), right?

Paul Jensen

11:54 AM

Right

David Dabbs

11:54 AM

Different "creative expression. of that ad.

David Dabbs

11:56 AM

Tweak of negative targeting

Harshad Mane

11:57 AM

is there a way to create an IG under advertiser-1-domain and delegate it to dsp-1-domain to participate in auction?

Sid Sahoo

11:58 AM

https://github.com/WICG/turtledove/blob/main/FLEDGE.md#13-permission-delegation

David Dabbs

11:59 AM

@Harshad the bidding logic URL should be same-origin with the IG owner. I suppose an advertiser could use an origin they control and delegate control to specific PAAPI-oriented subdomains.

Sid Sahoo

11:59 AM

When you delegate permission, the IG is owned by advertiser-1-domain

Yes @David, and the bidding script needs hosted on this domain as well

Matt Davies

12:00 PM

Isn't there a minimum amount of users that can be targeting within an interest group?

\*targeted

David Dabbs

12:00 PM

Oh, Harshad you were asking about making the joinAdInterestGroup() call on the advertiser's property but the IG owner is dsp.example. Yes this is possible with the correct permissions files, &c. in place. See the explainer.

Party over, oops, outta time.

Sid Sahoo

12:01 PM

@Matt: There is a k-anon requirement, not at the IG level; here are more details:[ https://developers.google.com/privacy-sandbox/relevance/protected-audience-api/k-anonymity?hl=en](https://developers.google.com/privacy-sandbox/relevance/protected-audience-api/k-anonymity?hl=en)

Matt Davies

12:01 PM

Thanks

Harshad Mane

12:02 PM

Thank you for inputs Sid, David and Matt... I will check it offline

Shankar Venkataraman

12:04 PM

https://docs.google.com/presentation/d/1DhgWHIQ29rNik5JQ-k6mVxVqIkMyAYYcmg0HPLrfZus/edit?usp=drivesdk
