
# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 4pm UTC (during winter)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Dec 20, 2023

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	



1. Paul Jensen (Google Chrome)
2. Brian May (dstillery)
3. Wojciech Biały (Wirtualna Polska Media)
4. Amit Gupta (Jivox)
5. Shankar Venkataraman (Jivox)
6. Roni Gordon (Index Exchange)
7. Szymon Gajda (Wirtualna Polska Media)
8. Isaac Foster (MSFT Ads (there in 5))
9. Laurentiu Badea (OpenX)
10. Youssef Bourouphael (Google Chrome)
11. Orr Bernstein (Google Privacy Sandbox)
12. Harshad Mane (PubMatic)
13. Sid Sahoo (Google Chrome)
14. Owen Ridolfi (Mediaocean)
15. Antoine Niek (Optable)
16. Caleb Raitto (Google Chrome)
17. David Dabbs (Epsilon)
18. Ricardo Bentin (Media.net)
19. McLeod Sims (Media.net)
20. Brian Schmidt (OpenX)
21. Fabian Höring (Criteo)
22. Chris Nachmias (Mediaocean)
23. Tamara Yaeger (BidSwitch)
24. Anthony Yam (Mediaocean/Flashtalking)
25. Jonasz Pamuła (RTB House)
26. Jeroune Rhodes (Google Privacy Sandbox) 
27. Drew Schoentrup (Big Crunch)
28. Maciek Zdanowicz (RTB House)
29. Nick Llerandi (Triplelift)
30. David Tam (Relay42)
31. Becky Hatley (Mediaocean/Flashtalking)
32. Stan Belov (Google Ads)
33. Abishai Gray (Google Chrome)
34. Matt Davies (Criteo | Bidswitch)
35. Alex Peckham (Mediaocean/Flashtalking)


## Note taker: &lt;please volunteer>


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


## Suggest agenda items here:



*   [General Announcement] Jeroune
    *   The Google Privacy Sandbox team will be hosting our next series of webinars on the Protected Audience API. This set of webinars will focus on reporting and how you can measure data related to a Protected Audience auction. The first **Americas friendly session** is happening on** Jan. 16th 3-4 pm ET**. A second **EMEA friendly session** is happening **Jan. 18th 12-1 pm GMT**. A third **Japanese language session** will be held on **Jan 30th 9-11 am JST**. To join, please register below: 
        *   AMER-friendly: [Register Here](https://rsvp.withgoogle.com/events/protected-audience-webinar-3-reporting-amer)
        *   EMEA-friendly: [Register Here](https://rsvp.withgoogle.com/events/protected-audience-webinar-3-reporting-emea)
        *   Japanese language: [Register Here](https://rsvp.withgoogle.com/events/protected-audience-office-hour-3)  
*   Isaac:
    *   Buyer/Seller Reporting Questions: https://github.com/WICG/turtledove/issues/682#issuecomment-1710965068
    *   <span style="text-decoration:underline;">Persistent Opt Outs, Maybe CHOPS - https://github.com/WICG/turtledove/issues/915</span>
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Shankar Venkataraman
    *   Interest group ownership construct missing for Third Party Ad servers whitelisted by Advertiser ([#924](https://github.com/WICG/turtledove/issues/924)). We will discuss and explain the model that we would like supported in the context of Protected Audiences. 
*   Jonasz (RTB House)
    *   3pc deprecation timeline: https://github.com/WICG/turtledove/issues/717#issuecomment-1847118918
*   Roni Gordon
    *   Sensitive signals - https://github.com/WICG/turtledove/issues/824
*   Matt Davies (Bidswitch)
    *   Origin / Traffic shaping -<span style="text-decoration:underline;"> https://github.com/WICG/turtledove/issues/951</span>


# Notes

Paul filling in for Michael.

Paul (Chrome): Jeroune has announcement

Jeroune (Sandbox): Sandbox team hosting webinars on PA on reporting and measuring data, including auction, user engagement, attribution data from conversion. Links will be posted. Next sesh is Jan 16th, EMEA Jan 18th, Japan Jan 30th. 
 
Paul: Isaac’s issues

## Persistent Opt Outs, Maybe CHOPS - https://github.com/WICG/turtledove/issues/915

Isaac (MSFT): Potential k anonymity issue between creative URL and reporting, but today we have a need (legal type) to provide privacy center. Xandr has one, MSFT has one too, but we’re not merged yet. A user will be able to go to our privacy ctr and wish to opt out of all Xandr-based advertising. We do get couple diff flavors of that, the idea is to not show ads from our platform. How we do that today is to opt out their cookie, and we will in some cases record it to expand to graph-type deal. I think that’s a challenge in partitioned world. We can opt out of ads in our privacy ctr, but not every website they go to, can’t get it shipped across partitions. Interesting challenge. Someone mentioned GPC but if there’s someone who can explain I didn’t quite get it. If there was a way for browser to expose API for enumerated list of flags to be sent to domain. In case of opt-out it could be helpful. 
 
Shankar (): TCF consent string will tell us if we can sell ads or not to specific user.

Isaac: How would the TCF string be shared across partitions? 


Shankar (Jivox): Should come in w ad call after auction is won from publisher. Ultimately user is sitting on pub site. Then coming to Xandr site…

Isaac: We’re talking about diff cases. In case of user vising NYtimes. Com… popup asks what to do w cookies. I say reject everything. I’m referring to case where someone goes to Xandr privacy ctr and asks to not see ads from our platform on on any partition.

Shankar: That model is gone, as long as pub is sending right consent and they’re not targeting someone who has not given consent, why bother? 


David D (Epsilon): In today’s world we have to have all these industry user portals or our own way to opt out because there isn’t a global BRS (big red switch). In this new mechanism there is a way for user to control this and not worry about individual companies they can turn off ads targeting. To point of wanting control, to Shankar’s point, if in jurisdiction where there is site-based consent, you can get that signal, and if you buy into whole GPC thing that will be available to you in browser signals. Not sure something will exist as global buyer-seller.

Shankar: Even if on site level, like happens in Germany, I would say right model is to have something for browser to….. at point of selling ad, it’s the only way to secure.

Roni (Index): There’s always a way to have global 3P cookie opt-out (https://optout.networkadvertising.org/). I hear question whether we need such a facility if not based on cookie. If you read through the lines it’s interest based advertising. The question for Chrome is if we can’t put it in 3P cookie, where to put it?

Paul: Chrome does offer ways to turn off targeting mechanisms. In settings you can turn off PA or other parts of Sandbox. Any sort of middle ground with less-global, this is a space that ppl tried to explore a few times and failed a few times. They devolve into fingerprint mechanism that we haven’t thought about when creating solution, having to go w more global solution in the end. I don’t know if there is a great middle ground. 

Shankar: What is proposed is that pub stuffs consent into \_\_\_ storage which is available for all white listed advertisers and DSPs. At this point everybody can read if the consent is there. 

Brian (Distillery): I was going to suggest this as formal project, anyone is subject to receiving an opt-out request. Other parties in chain may or may not have access to add origin. We should look thru use cases to see who will get opted out of, rather than trying to quickly come up w solution.

David D: Tracking of opt-out becoming fingerprinting vector, is it at all on the PA horizon to … every ad will be in a fenced frame. Is it on the horizon for browser to say it knows that PA ad is being rendered in this frame, have an affordance to some kind of transparency to the user. We know boatload of who was participating, including who won the auction. If there isn’t a concept of some opp to provide transparency for these frames facilitated to browser, then there’s not much to discuss. 

Paul: This is something we thought a bit about, haven’t chosen UI direction yet. In general browser knows evtg, including origin and interest group. Problem is some things are harder to communicate to user. The user was on an origin in last 30 days, so they probably have some familiarity of that origin. The opt-out Isaac was talking about, per adtech, user may not have familiarity w owner of interest group. Very hard thing for browser to convey to user.

David D: They don’t today but we try to make it possible to understand that triangular thingy on top of ad.

Isaac: I just want to make sure I’m being clear, the thing I care about is, looking at our privacy center, we don’t mention cookies anywhere. It’s referring not to cookies, it’s referring to sales / sharing of data. If the solution I called out is a terrible one it’s fine, the problem I’m more interested in – today our lawyers told us we must have way for user to have control over how data is processed / stored / shared. The way we have done that has relied on cross-partition, which is going away. Not a Jan 4th emergency, but could be real problem. To Brian’s point, may need to take elsewhere. Hypothetically if 3P cookies became partitioned everywhere always, and no opp to have them across anywhere, it would be interesting to see whether to exempt that. 

Harshad (PubMatic): Maybe can be made available in PA whether user has opted out.

Paul: This is the middle ground that devolved into fingerprinting. We can uniquely identify people that way. 

Brian: This question is multi-dimensional. If you give users a list, they will opt out of all. The next level down is opting out of specific advertiser. How do I opt out of that? You don’t. You opt out of all. My point is we have to contend w data subject rights at some level w/in context of PAAPI and PSB. We’re going to have to deal w how to comm to user why they’re seeing what they’re seeing. I’ve been working w IAB group on how that gets communicated, but how does that slot into PAAPI universe? I don’t think we need to start a new group, but we need to focus on data rights. If we determine not to take on directly, still need to figure out how to support those who are.

Paul: maybe we should encourage people to offer any solutions they have in Isaac’s issue.

Brian: I suggest we make Isaac’s a part of larger data subject rights issue.

Harshad: If only specific ad tech player knows about opt out, can there still be case of finger printing?

Paul: Only if adtechs are not sharing lists they’re receiving.

David D: Isaac, what did lawyers have to say about Safari cookies being in the dustbin? We should put this in as something to discuss, but unlike ad targeting, there is “big red switch” not on a granular level. It affords us some time to do someone more granular. In Maslow’s hierarchy of needs we need to focus on getting nose off tarmac to deliver on guarantees we have before all cookies are gone.

Isaac: I did put a semi-answer, I can ask lawyers for their real legal opinion. 

Brian: Just want to add last note about doing opt-outs too quickly, damage of massive ppl opting out of system is difficult to recover from. Suggest to move into territory slowly. 

Paul: Shankar do you want to talk about interest group ownership?

## Interest group ownership construct missing for Third Party Ad servers whitelisted by Advertiser ([#924](https://github.com/WICG/turtledove/issues/924))

Shankar: The way we operate in 3P cookie world is DSP wins, on PSB PA at point of registry, supposed to give not only bid logic but creative. This is done by DSP, usually creative comes much later. Even in current world people transfer then creative runs. Creative is ready probably weeks after \_\_\_ has been put in place. IN actual execution of model, no way for advertiser to have 2 diff DSPs. Unless I gave creative URLs to advertiser, they could have multi campaigns running on same interest group. Why is it that creative 

Paul: We built interest group creative system to be able to add creatives later. Meant to address problem of not having creatives. The other part is, interest groups are fairly flexible. We don’t dictate that the advertiser has to be owner of interest group. Almost anyone can create interest group and use them to bid. We also don’t require ad creative URL is same origin as anyone else. Whether it’s the DSP or advertiser that owns interest group, they can still delegate rendering of ad to third party entirely. 

Shankar: The second part is when the bid is won, will the ad get context of who won the bid? Bid logic is gained by somebody else. There are 2 providers in the ecosystem. How will URL know which interest group won (?) For every interest group, 

Paul: Any info that’s needed at ad rendering time can be put into URL. 

Shankar: We have macros that get passed to DSP. 

Paul: Not really any restriction on k anonymity. Pretty free-form. Could be specifying to ad server to render ad #4 for campaign #5.

Shankar: For example, on e Commerce site, they have category and subcategory. I can create an interest group at product level and sub level… 4 interest groups for 1 visit. Do we have to find out bid logic for each interest group, ad calls, lots of data.

Paul: We had a k anonymity restriction, but we dropped that designed a long time ago. You can create 1 interest group that has 4 diff ads. Then add or remove ads that target that. If you want you can put in as much 1P info from joining side. You can write user journey in interest group. Then if you want to start ad campaign you can push down to that interest group.

The way k anonymity bootstraps itself, the hope is that people don’t have to keep fine tuning which ads are shows ot how many ppl. If your ads are broadly targeted it should fall into place and browser will worry about what to show.

Shankar: I’ll update the ticket based on issue.

Amit (Jivox): I think what Paul is suggesting, the only issue is adding multi ads in same interest group means we are targeting same user. Literally 1:1 is gone so we are saying our cohort is now based on interest group, which is same for category, sub category, and individual product. It has to be multi interest groups in that case?

Paul: You can have 1 broad interest group, in there there is user bidding signals, where you can put your info including user flow. You don’t have to split across interest groups.

Shankar: We have party that provides bid logic is different from party that \_\_\_\_

Paul: With k anonymity restriction we’re avoiding microtargeting. 

Shankar: I should be still be able to show the right ad based on product, but if I not 

Amit: What we are leaning towards is ad tech cohort to resolve this issue. We cannot use the same ad tag to target multi cohorts itself. 

Paul: What is an ad tag? 
 
Amit: The render URL, hits ad server directly. Bidding logic belongs to DSP. 

Shankar; Unless we do another round of bid logic to resolve.

Paul: Is problem that you want more info at bid time or ad serving time?

Shankar: Ad serving. We need to know which interest group won the bid and parameters around that.

Paul: Anything you want can be put into ad created URL.

Harshad: Shankar, why not putting two interest groups? 


Shankar: That’s exactly what we have to do. For example, suppose red color sofa. Interest groups are created for furniture. 

Paul: You may want to break up your ad tags like that, not interest groups. Say user saw red sofa, say update time you send that person 3 ads, then at ad serving time you get the URL that says they are interested. 

David T (Relay42): We work w a lot of eCommerce companies w huge product catalogs. We’ve always been able to do that using campaign manager w Google studio. Google studio manages prod catalog, we provide prod ID to render specific asset w/in the HTML creative. To do this currently you have to create loads of interest groups. The question is how ot create one interest group where the users are placed in that group that actually see the right product. If you have to create loads of interest groups, it’s just not feasible.

Shankar: Possibly feasible but not sure how it would work. We are trying to replicate what we are doing today, let us go back to spec to review. The other problem is that most eComm ads are in sequence. Multiple products show in sequence. 

Brain: This is going towards day of reckoning for PAAPI. In legacy model of advertising, big servers figure out what to bid for each user with. PAAPI will take all that intelligence and ship it down to browser or auction server. If you take all the intelligence serving ads across internet, the space will be overused very quickly. Paul said a couple times that you can put as much data as you want in the interest group. We need to be careful because there are limits on a browser. We need to figure out constraints.

Paul: You don’t have to store all that info in browser, you can assign an identifier and put that in the interest group. Different from today’s model

David T: You mentioned that ad components, are they URLs? Can those URLs feed into JPEG image or just HTML?

Paul: Haven’t thought of it, guess they could be either one?

David T: If it’s a bunch of assets, pics of diff products, how does the ad itself know what to pick?

Shankar: We thought about to actually at point of sending HTML, could be JPEG. If you want to send JPEGS to browser, need to register worklet as part of interest group, worklet decides what to render. 

Paul: It could just be an HTML wrapper, at bidding time is when the products would be selected. 

Shankar: Unless you hard code it into URL. 

Paul: You might have overall fenced from that is carousel ad, and individual components would be some kind of headphones. 

Shankar: Our problem is with millions of URLs. 

Brian: Propose for Shankar to create formal discussion w slides
