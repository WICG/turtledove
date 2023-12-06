# Protected Audience (formerly FLEDGE) WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 4pm UTC (during winter)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Nov 29, 2023


## Attendees: please sign yourself in!	

1. Michael Kleber (Google Privacy Sandbox)
2. Roni Gordon (Index Exchange)
3. Sven May (Google Privacy Sandbox)
4. Orr Bernstein (Google Privacy Sandbox)
5. Paul Jensen (Google Privacy Sandbox)
6. Brian May (dstillery)
7. David Dabbs (Epsilon)
8. Fabian Höring (Criteo)
9. Harshad Mane (PubMatic)
10. Maciek Zdanowicz (RTB House)
11. Russ Hamilton (Google Privacy Sandbox)
12. Kevin Lee (Google Privacy Sandbox)
13. Wojciech Biały (Wirtualna Polska Media)
14. Miguel Morales (IAB Tech Lab)
15. Pawel Ruchaj (Audigent)
16. Jeroune Rhodes (Privacy Sandbox)
17. David Tam (Relay42)
18. Abishai Gray (Privacy Sandbox)
19. Youssef Bourouphael(Google Privacy Sandbox)
20. Andrew Pascoe (NextRoll)


## Note taker: Orr Bernstein


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


##  Suggest agenda items here:


*   Isaac:
    *   Sec-cookie-label for no label: https://github.com/GoogleChromeLabs/privacy-sandbox-dev-support/issues/153 
    *   Buyer/Seller Reporting Questions: https://github.com/WICG/turtledove/issues/682#issuecomment-1710965068
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussi  loop on and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Roni Gordon
    *   API versioning - https://github.com/WICG/turtledove/issues/823
    *   Sensitive signals - https://github.com/WICG/turtledove/issues/824
*   Fabian Höring:
    *   Protected Audience AB testing (https://github.com/WICG/turtledove/issues/909) 10 min prez + discussion


# Notes


## Protected Audience AB testing from a DSP perspective - https://github.com/WICG/turtledove/issues/909



*   Fabian Höring:
    *   Slides presented: https://github.com/WICG/turtledove/blob/main/meetings/2023-11-29-FLEDGE-call-minutes-slides-ab-testing.pdf
    *   Why do we need AB testing?
        *   Increase performance of PA
        *   Measure technical changes - when we roll out stuff on 1% of users and make sure we don’t break anything
        *   Measure stuff in a consistent way
    *   If we can’t do AB testing the right way - for e.g. sales and conversions - cannot improve the performance of PA
    *   User scenario	
        *   User browsing shoes / browsing phones / visit his famous blog / checks the weather / clicks on shoe ad / buys shoes.
        *   With AB testing, split the the user into two populations: A and B
        *   Split by user - currently using 3rd party cookies. Consistency exposed to A or B behavior. Can change the bidding behavior, and then check to see if this user had more sales/less sales.
        *   Throughout the user scenario, always in the same group.
        *   With first party on publisher site, in some websites, exposed to the A group behavior, in other websites, exposed to B group behavior. Not perfect because you don’t see the full impact of being in a group because some publisher pages would be on the “other” group.
        *   With 1st party on advertiser, then different campaigns for the same user would end up in different groups.
    *   Existing mechanism with ExperimentGroupID
        *   Can assign an ExperimentGroupID, and based on this ID, decide if the user is exposed to population A or B.
        *   Cannot be assigned by the buyer anymore, can only be assigned by the seller.
        *   In this scenario, could attach the first party user ID to the interest group. Could decide based on this realtime call if we are exposed to population A or B. Could encode when we get render URLs which population we chose.
    *   In this scenario, cannot handle as much 
        *   If we split by tagging, could get some bias.
        *   Could get leakage, as the same user will change for different advertiser websites. If the advertise has two campaigns, can see if if one campaign works better than the other.
    *   Proposal: inject a global Chrome ID, reduce entropy as much as possible and not leak too much information to negative side effects to identifying the user.
        *   Could use 3 bits - which would allow for 8 groups.
        *   User behavior could be impacted by being a member of a group; constantly rotating the population helps to mitigate this.
    *   Reporting
        *   For each population, can look at e.g. clicks vs sales.
        *   In the short term, could use existing mechanisms - e.g. having more renderUrls, and using one based on the population. But all of these things have privacy mechanisms in place, so always come with some tradeoff.
        *   Fully compatible with aggregated reporting (with DP noise). Can still bucketize with population.
*   Kleber
    *   Two different types of discussion we could have here, and I would like to do both:
        *   Understanding better - asking questions about whether the existing capabilities really don’t meet your needs, or
        *   Talking about the proposal to introduce more bits.
*   Brian May
    *   Would like to better understand where the confounding factors are in the current model, why AB tests can’t be utilized as they currently are given PAAPI constraints.
*   Kleber
    *   Pick A vs B impression by impression - no consistency
    *   Pick A vs B using 1st party identifier on publisher site - different treatments for the same user when on different sites.
        *   Makes sense why this doesn’t do a good job of letting an advertiser know the relative effectiveness of different ad campaigns, because same person might have seen both.
    *   Pick A vs B at the time you put the user into an interest group.  Now you have separate A and B populations.  IG would contain renderUrls which indicate whether you were in A or B, which means you do pay a factor-of-two cost in your k-anonymity counts.  It seems this does let you do consistent experimentation of different bidding models on different ad campaigns.
*   Fabian
    *   Too much noise to distinguish whether A or B is better. In some scenarios, it could work, if you have one large advertiser testing A vs B on their own campaigns. If one advertiser wants to test, it could work. But if you want to get raw events, means you would need to split on all traffic. Means you need to change behavior of all campaigns. If you split ads, you could see both behaviors. For e.g. adding a button - users may like that and click on those more - then it works. For bidding behavior where there’s no visual impact, can’t necessary see this - e.g. if there was a case where both A and B were shown, how do you know if it was because B was shown or because both were shown?
*   Kleber
    *   So this is the case where a DSP wants to run an experiment on something like the bidding model across all advertisers. Not enough traffic from a single advertiser. Want enough data that you can tell the difference between things you want to tell the difference between.
    *   But, if you’re a user and you’re seeing some ads with A and some ads with B, then why isn’t this enough? You can’t control all the ad campaigns that a user will see, because some are run by other DSPs, so even when you’re changing your bidding model, you’re only affecting the ads affected by that bidding model.
*   Fabian
    *   Don’t have an example right now. The signal you’re using for bidding is not a simple signal. If you cannot expose a user consistently to the same behavior.
*   Kleber
    *   What I was expecting to hear is, what if you add a user to an ad campaign if they’re on different websites. Different advertisements from advertisers joined on different sites, then could get lack of consistency. Then I would say, negative targeting on interest groups thing could be part of a solution.
*   Fabian
    *   Scenario is mostly a retargeting campaign. For upper funnel campaigns.
*   Paul
    *   If you have a three-bit identifier, still breaks down if you have other buyers involved that won’t respect another person’s campaign.
*   Fabian
    *   But only measure what Criteo does. Only want to modify my bidding behavior and optimize my bidding algorithm.
*   Paul
    *   3-bit mode, or split by 1st party advertiser, or by 1st party publisher. Wondering what the efficacy of each is.
*   Fabian
    *   Can do a technical AB test on experiment group ID. Just want to know that it’s not completely broken. Don’t care about noise or confidence intervals. For this, group ID is enough.
    *   User ID is perfect for single-advertiser campaigns, where one advertiser wants to propose a single advertiser campaigns, where one advertiser wants to propose a different creative to see how they behave. Lots of noise.
*   David Dobbs
    *   Might some constrained controlled application of shared storage work in the bidding worklet?
*   Fabian
    *   No, because shared storage cannot be used inside FLEDGE. Also checked this out; shared storage can be used after rendering for creative AB tests. But not completely random, could be aligned with the 3 bits. Every time you use shared storage selectURL API, you can leak three bits. This is why three bits. Shared storage is even worse, because it’s 3 bits for each time you call. Vs these three bits - it’s the same three bits every time. Budget capping mechanism.
*   Kleber
    *   For enabling shared storage in FLEDGE, if we do this in a reckless way, could enable a lot of tracking. Would need to be done with care, where you could maybe access only a few bits of information from shared storage. Can’t give an answer, but could give it some thought.
*   Brian
    *   What if the browser had essentially one bit. Bidding algorithms could have one bit. If the bit is 0 don’t bid, if the bit is 1, do bit. Some level of consistent audiences. Some way for the browser to divide the audiences, without the advertiser seeing the browser level split.
*   Kleber
    *   Seems unlikely that a single bit will remain a single bit. Seems likely that that the three bits are needed. And there’s a slippery slope, in which this could later become 6 bits or 9 bits. Could add noise, but then each bit is less useful and you need more bits. Tradeoff mechanisms we’ll have to talk to researchers about. Noise would make your confidence intervals larger. Not an easy thing to answer, going to have to think hard to think about this. And why things that are already possible - e.g. A/B split at the advertiser level - A vs B version of an interest group - has some advantages. It just works. That’s Brian’s “I just want one bit”; just do that at interest group time. As soon as you want things to be cross-site, you have trade-offs against noise. People like Charlie and Josh who have thought a lot about differential privacy and noise on the shared storage side of things will have a lot of thoughts, but they’re not in this meeting, so we can’t come to any conclusions.


## API versioning - https://github.com/WICG/turtledove/issues/823



*   Roni
    *   Short version - Paul and others are very active about getting changes into everything. B&A services has a version of this. So do a bunch of other surfaces. We’re in the middle of changing things. We’re changing which headers are required to be returned. Subresource bundles to other HTTP headers. Additional bids from negative targeting. No easy way to know when things are added programmatically. Neither client nor server have a way of knowing what’s going on. Even release notes don’t align with all of the changes, and there’s no way to introspect that programmatically. Basically, what support exists for what origin. And when can we expect a stable API surface, not have to write code to inspect this.
*   Paul
    *   I share the goal of a stable API. We want to make changes, and it’s going to be a living API in that way. There’s going to be a need to feature detect what thing is there. Versioning is not that easy. A lot of the way we launch these is 50/50 on canary/dev, then 50/50 beta, then 1% stable. Necessity to make sure we’re not breaking things as we roll them out, so not as easy as “what version is this”, because there’s a bunch of things rolling out at the same time. The explainer/spec are being updated as we update things as well. Because there are a few things being rolled out at a given point in time, there is no one version number that will tell you the answer. Knowing which 50/50 branch you’re in in dev/canary or beta, or which 1/99 branch you’re in on stable.
    *   So, to help with this, I published a document https://github.com/WICG/turtledove/blob/main/PA\_Feature\_Detecting.md that shows - for everything we’ve launched since the APIs reached General Availability - how to detect for any one of the things that you can use it. Once it’s at 100% stable, then at tip of tree, you can expect that it’s there. But until then, feature detection will be necessary.
    *   The other thing to keep in mind is that we’re trying to keep things as stable as possible, and avoiding breaking backwards compatibility. Deprecating things on the web is pretty hard. All of the things we launched at GA, and all of the things we added in the origin trial over its 15 months, we added in an optional way. It’s very rare that the web breaks backwards compatibility. Many of our APIs are dictionaries of, “what do I want to turn on”; you can add new thing that the browser doesn’t know about yet.
    *   In short, feature detection is a good practice to know what’s available
*   Kleber
    *   Or ignore new features. If you keep using the API the way it was, it’ll still work. That’s what the goal of maintaining backwards compatibility is all about.
*   Roni
    *   I get it, but maybe we’re not being complete about what feature detection means. Some of these things have an impact on sellers or buyers. Some of the KVs get parameters; if they’re missing, I know they’re missing, but not everything. If I as a seller want to know that negative targeting is enabled, or size on the kAnonymity checks. Because I have to generate my auction config somewhat independently of what the browser is doing, I have to build something that can get sent from client to server, need to know about capabilities. I don’t have a list of everything that needs to be detected.
*   Paul
    *   Some of these things are harder than others, e.g. sizes and knowing whether that’s in the kAnonymity check. We’ll work harder to better surface what features are enabled to different parts of the auction.
*   Kleber
    *   Definitely note to us when we there are gaps and we can try to 
*   Brian May
    *   Roni’s concern is a symptom of an underlying concern that I’m hearing regularly, in which we have an extremely complex machine we’re not familiar with, where we’re trying to make sense of it, poking it and we have a requirement that we do some assessment of this machine vs our current status quo, and how changes in the model will impact our businesses. We’re not sure what this thing is, we have a couple different expressions of the model in various states of completeness and it’s regularly getting updated and changed. The challenge we’re facing is that we need to figure out how it works before we can begin to figure out if it works and we’re also facing testing deadlines.
*   Kleber
    *   I hear what you’re saying, but this weekly meeting alternates between people requesting a stable machine and here are 5 new feature requests.
*   Brian
    *   I understand, but we need to get to a place where we can keep it stable long enough to figure out how it works so we can do that assessment.
*   Kleber	
    *   During the CMA testing phase in the first half of 2024, good time for a relatively quiet period, not rolling out substantial changes. But that said, the web is a dynamic platform, nothing is written in stone, things constantly changing.
*   Brian
    *   Another concern is that we slow down on development, and when we’ve come up with conclusions based on that, we make significant changes to the APIs, and all the things we figured out about business impacts are invalidated.
*   Kleber
    *   Hope is that your prior evaluation should be a lower bound on how well this API works for your needs. From testing, are hoping to get feedback on ways to improve the API to make it better satisfy your needs.
*   Brian
    *   Can we get help from the Chrome folks on how to test these things, things like feature detection.
*   Kleber
    *   Yes, e.g. Paul’s doc, in the notes and chat.
*   David
    *   Release notes stopped being updated at M114. If one were to read the spec or the human readable version, does that reflect what’s going to be live at M120, e.g. in mode B labels where the cookies are being taken away, which is where people trying to get consistent numbers will be testing.
*   Kevin Lee
    *   Alonso and I are working on improving change logs.
*   Paul
    *   FYI, the explainer will often have things that are not yet shipped, since we need to make these changes as part of the Chrome release process for IntentToShip.
*   David
    *   More clarity/transparency is a good thing.
