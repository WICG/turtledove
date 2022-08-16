# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Apr 27, 2022


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Andrew Pascoe (NextRoll)
3. Brian May (dstillery)
4. Joel Meyer (OpenX)
5. Fred Bastello (Index Exchange)
6. Peiwen Hu (Google Chrome)
7. Chris Evans (NextRoll)
8. Paul Jensen (Google Chrome)
9. Ryan Arnold (P&G)
10. David Dabbs (Epsilon)
11. Russ Hamilton (Google Chrome)
12. Matt Wilson (NextRoll)
13. Don Marti (CafeMedia)
14. Wendell Baker (Yahoo)
15. Brian Schneider (Google Chrome)
16. Caleb Raitto (Google Chrome)
17. Jonasz Pamuła (RTB House)
18. Bartosz Marcinkowski (RTB House)
19. Bartosz Łoś (RTB House)
20. Chris Starr (Hearst Autos)
21. Martin Pal (Google Chrome)
22. Eric Trouton (Google Chrome)
23. Sergey Tumakha ( Microsoft Ads)
24. Angelina Eng (IAB / IAB Tech Lab)
25. Matt Menke (Google Chrome)


## Note taker: Paul Jensen


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


## [Suggest agenda items here]



*   Access to data from FLEDGE OT for folks who are step removed. (bmay)
*   Andrew Pascoe (NextRoll)
    *   [Issue 283](https://github.com/WICG/turtledove/issues/283), Use of Cross-Site Data in Browser
    *   [Issue 269](https://github.com/WICG/turtledove/issues/269), Mechanism for Retroactive Segmentation
    *   [Issue 270](https://github.com/WICG/turtledove/issues/270), Scalable Account Targeting Mechanism
*   FLEDGE latency reporting to sellers (Stan Belov, Google Ads)
*   Marco Lugo (NextRoll) [Issue 287](https://github.com/WICG/turtledove/issues/287), Auction Performance Testing
*   Matt Wilson (NextRoll) [Issue 145](https://github.com/WICG/turtledove/issues/145), Interest Group Data in Report Win
*   Phil Lee (Google) [Issue 290](https://github.com/WICG/turtledove/issues/290), FLEDGE Key/Value Server requirements


## Notes



*   Access to data from FLEDGE OT for folks who are step removed. (bmay)

Michael Kleber: first topic: Access to data from FLEDGE OT for folks who are step removed. (bmay)

Brian May: Interested in what kind of data is accessible outside of the origin trial.  Data needed to inform efforts to target ads.  We analyze traffic coming from web sites to see where conversions coming from to inform buying decisions (e.g. bid price, where to bid). Need to know where to onboard data.

MK: Relevant API to this data: Shared Storage (https://github.com/pythagoraskitty/shared-storage).  We haven’t discussed it a lot here.  Shared Storage API is a way to more flexible to get aggregate reporting about whole populations. Based on user of API writing data to on-device storage location.  Similar to FLEDGE in that regard.  In FLEDGE data is used for auctions.  In Shared Storage the data flows into aggregate reporting mechanisms.  This seems like the kind of data source you might be interested in.  A way to gather aggregated population data, in ways that don’t track individual users, but can inform decisions.

BM: where is this being discussed?

MK: In separate explainers.  Not in FLEDGE Origin Trial.  Aggregation reporting API is part of this: https://github.com/alexmturner/private-aggregation-api.  Shared Storage is the way to dump data into the on-device data store, then aggregated reporting API is the way to report it off-device.

BM: During Origin Trial, that whose aren’t directly participating in OT, how can get they get information on impressions etc.

MK: This data may come less from Chrome, and more from those parties involved in early fledge testing, and how they may provide their data to interested parties.  Chrome as part of doing OTs, doesn’t require sharing OT publicly, but we encourage folks to talk publicly about findings from OTs.  Any testers that want to speak about their sharing of data?

BM: They can go to the issue to talk about it.

MK: Could also be the topic of 1:1 conversations not in public.



*   Andrew Pascoe (NextRoll)
    *   [Issue 283](https://github.com/WICG/turtledove/issues/283), Use of Cross-Site Data in Browser

MK: next topics from Andrew Pascoe.

AP: Issue 283 is best to start with.  283 is less technical, more principle.  Context: last November, asked “can we throw anything in userBiddingSignals?” MK said we need to be careful.  A month ago we had a discussion about auction mechanisms, “can cross site data inform auction outcomes?”  response was “no cross site data is prevented from being used”, but pointed out that some information (e.g. interest group values) are informing auction.  Can prevent that from leaving the device by using aggregated reporting.  Can we use this data for informing prices as perhaps this doesn’t affect user privacy?

MK: the first italicized bullet points in issue “Anything that is allowable by an API should be considered acceptable.”  The case of exploits highlights the complexity.  Origin trials is example of how we test to make sure things operate correctly.  FLEDGE API is still malleable at this stage (OT).  How can we affect the API to support the necessary functionality without allowing exploitation.  “What counts as exploit” is challenging, may be different for different users.  One-bit leak in FLEDGE is example of addressing necessary use but also exposes a leak.  Something that we want to improve over time.  Using FLEDGE for exploiting the leak, can expect this to break.  It’s a temporary state that we’re aware of.

AP: In Nov example, arbitrary userBiddingSignals can be piped into IG which then goes to generateBid().  This is a dataflow that could contain unique identifying data.  Spec doesn’t say you shouldn’t put identifying info into that.  Should spec call this out as unallowed?  Do we need restrictions to control this.  Impractical for us to design the API and then whack-a-mole to change way people use the APIs.  We’re preparing for OT.  Mapping existing products to FLEDGE spec.  May use APIs in ways this group not expecting.  Don’t want to be in a position where everyone is asking if things are okay.  Not sustainable.

Angelina Eng: IAB tech lab had meeting with Barb Smith.  One of the things discussed looking at open measurement specs.  Should we look at things in bid requests and protocols.  How can this affect how it connects with browser.  Tech lab to discuss are there protocol standardization work that should integrated.  Potentially from taxonomy work.

MK: Ways API can be used should not be limited to use cases explained by proposals.  People need to be free to innovate.

Stan Belov: Didn’t understand allowable versus intended use of APIs.

AP: No examples yet.  Hypothetical: NextRoll SPURFOWL kept trail of user events, e.g. ads they’ve seen.  Live on browser could extract bidding signals features from trail.  Wait, do we want to use this cross-site data?  If the data isn’t escaping the browser is it problem?  Why are we saying this isn’t allowed?  Some signals may be valuable for bid pricing and modeling.

MK: Bid pricing doesn’t seem like an example of allowability.  Different discussion between 

AP: Example: the other two issues.  For example user comes to site, perhaps know who they work for.  In userBiddingSignals, could log their employer.  Could influence their bidding decisions.  API might allow.  If implemented, could later discussion disallow this.  If spec changes it could break significant part of business.

SB: Current spec, FLEDGE, only data from advertiser and pub site are used in bidding decision.

MK: FLEDGE currently written to work, bidding decisions inside FLEDGE are influenced by data from joinAdInterestGroup() calling site, and from contextual site data calling runAdAuction().  Advertisers isn’t required to be the joinAdInterstGroup() caller.  Intention is that these two pieces of data are combined.  IG membership includes some association from the site adding user to IG.  Seems inline with design to allow the data from joining site.

AP: Previous purchase data might include all sorts of other data.  Is it inline with FLEDGE spec to put that into IG?

MK: Yes. When user shown ad and they are surprised, browser can tell them why the ad is shown, in particular which site provided data that put you into IG.  Browser can trace this data back.  Browser can tell the tale of this trace.  Browser can explain this.  Different from a trail of events on hundreds of web sites, where it may be hard to trace data back to some specific site.  Large user experience different.

AP: so this is more about transparency?

MK: What does privacy mean is a deep question.

AP: Too philosophical to discuss here.

Chris Evans: Users of API going to exploit to max profit.  Auditor could look at data being passed back and raise issue.

MK: Don’t want to create arbitrary data channel if we can restrict reasonably.  In the middle of discussion of doing coordinated experiments.  Best answer for coordinating across contextual and key value server, was to pass experiment ID to k/v server.  Right now with BYOS step, the experiment ID could be used to correlate events.  Hesitant to create a data channel like this.  Reason for existence is clear.  When k/v server is trusted, it won’t be able to correlate.  Will have restrictions on how it works.  This is an example of doing a perilous thing which we need to be clear about how it will change in the future.  Don’t want to disrupt things as much as possible.  Need to be open to comments.

Mathew Wilson: Consistency across proposals.  Topics API is using cross-site info.  But in FLEDGE using cross-site information isn’t allowed.

MK: Topics API does use some cross-site information.  Topics API has a lot of layers of protection.  Protections speak to large number of threats.   Hundreds of issues opened against FLoC and extensive public discussion.  Set of things we needed to do to FloC to transition to Topics was quite extensive.  I don’t know of any way for us to open up Topics API so that it could be replaced by something where other people get to do arbitrary things to assign topics that doesn’t break many of those privacy guarantees.  We could talk about privacy protections, but there was a lot of design work that went into giving Topics those protections.  If there was something like FLEDGE that had the same protections and sent the same info to server, we might be open to it, but we’ve spent years testing other proposals against this public test of protections provided.

AP: I understand Topics has the protections.  The protections in FLEDGE in the design spec prevent cross-site data leaking to other parties.  It seems like in FLEDGE we’re more concerned about the transparency aspect.  Should we consider this another design goal.  We say someone can’t detect someone went between sites, but should we discus transparency.

BM: Transparency factors into privacy discussion.  Need to know how data used to discus privacy.

MK: Consent 2022 keynote from Robin Burgeron (sp?) is outstanding and does a good job of thinking about all aspects of privacy and data and autonomy issues that this discussion is bordering on.  Andrew, you’re bringing up, the set of norms of data use are not clear right now.  The way in which we as a community come to this set of norms is not clear yet.  The answer we have now is that privacy regulators write laws and tech firms try to abide.  Not a collaborative industry wide discussion, which would be a healthier to arrive at these norms.  Robin’s GARUDA proposal is an attempt to put some technical footing behind this community discussion.  The answer we have in FLEDGE right now, “you can store data from site joining IG, and use that plus contextual site” isn’t the right final answer, but is a starting point for now.  Maybe something broader than that might be possible and might be expandable.  Might be able to agree on what is allowable for advertising.  Discussion is beginning.  Hope that conversation continues.  May increase and modify the scope of current APIs.

Don Marti: A lot of that policy decision is driven by current events.  When there are high profile stories of companies participating in a community intending to monetize productive web content, but also participating in problematic user targeting practices or business practices then those front page news stories tend to guide laws, regulations and norms.  We can all make these conversations work better by cleaning up existing relationships and practices so that the stories used to explain laws and norms are not just the worst ones.

AP: Like broadening direction.  Limited ability to contribute new ideas to these specs and point out when we’re not in alignment.  Guiding principles help guide discussion.  There’s been a lot of talking past each other in the past.

BM: A lot of folks spread too thin.

MK: This is a deep thought process.  Need to make things easier to think about.  Good discussion to have.
