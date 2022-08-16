# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday May 11, 2022


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Christina Ilvento (Google Chrome)
3. Fred Bastello (Index Exchange)
4. Garima Dhingra (Google Ads)
5. Philippe de Lurand Pierre-Paul (Google Chrome) 
6. Paul Jensen (Google Chrome)
7. Brian May (dstillery)
8. Grant Nelson (TripleLift)
9. Don Marti (CafeMedia)
10. Christina Brauer (Capital C)
11. Chris Starr (Hearst Autos)
12. Russ Hamilton (Google Chrome)
13. Wendell Baker (Yahoo)
14. Marco Lugo (NextRoll)
15. Sergey Tumakha (Microsoft Ads)
16. Tamara Yaeger (Iponweb)
17. Bartosz Marcinkowski (RTB House)
18. Phil Lee (Google Chrome)
19. Andrew Pascoe (NextRoll)
20. Jonasz Pamula (RTB House)
21. Sangharsh Kamble (Walmart)
22. Sven May (Google Web Platforms team)
23. Caleb Raitto (Google Chrome)
24. Fabian Höring (Criteo)
25. Ryan Arnold (P&G)
26. Brad Rodriguez (Magnite)
27. Katherine Wei (Zeta Global)
28. Angelina Eng (IAB/IAB Tech Lab)
29. Aditya Desai (Amazon)
30. Supraja Sekhar (Google Ads)
31. Stan Belov (Google Ads)
32. Matt Menke (Google Chrome)
33. Alex Cone (IAB Tech Lab)
34. David Tam (Relay42)


## Note taker: David Tam


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here]



*   Marco Lugo (NextRoll) [Issue 287](https://github.com/WICG/turtledove/issues/287), Auction Performance Testing
*   Matt Wilson (NextRoll) [Issue 145](https://github.com/WICG/turtledove/issues/145), Interest Group Data in Report Win
*   Phil Lee (Google Chrome) [Issue 290](https://github.com/WICG/turtledove/issues/290), FLEDGE Key/Value Server requirements
*   Andrew Pascoe (NextRoll) [Issue 269](https://github.com/WICG/turtledove/issues/269), Mechanism for Retroactive Segmentation
*   Andrew Pascoe (NextRoll) [Issue 270](https://github.com/WICG/turtledove/issues/270), Scalable Account Targeting Mechanism
*   FLEDGE latency reporting to sellers [Issue 299](https://github.com/WICG/turtledove/issues/299) (Stan Belov, Google Ads)


## Notes



*   FLEDGE latency reporting to sellers [Issue 299](https://github.com/WICG/turtledove/issues/299) (Stan Belov, Google Ads)

Michael Kleber: Fledge latency reporting as requested by Stan.  Not on the call.  Will push to end of agenda and circle back if we get to it.



*   Marco Lugo (NextRoll) [Issue 287](https://github.com/WICG/turtledove/issues/287), Auction Performance Testing

Marco: Should be short as Matt Menke responded to the issue and I will be continuing the thread.  The concerns are about if the API is heavily solicited, timeout can be quite large, this behavior is unexpected to me because it can make the delay cumulative before the timeout as opposed to timing out all at once. Issue #293 by Jeff Kaufman seems like a step in the right direction.

We did open source test harness linked in the issue, if any one wants help to use it or is interested please reach out.  

Also, Fenced Frame broke when showing ad in more recent release of Chrome.  

Michael: Please file an issue if something is not working or stopped working.  If problem is related to flags please report those flags.  

On the subject of performance issue, may be make timeout cumulative.  Definitely file bug.  

Brian: Have folks on the Chrome team considered putting a limit on participants in auction.  

Michael: Should be the job of the seller to limit participants, and we should give them the tools they need.  Not all interest groups want to participate in an auction.  Right now we've implemented a way sellers can do capping by buyer, based on buyers assigning priority.  Something on these lines.  It would be useful for us to offer a better declarative language, so that buyers can do a better job at self selecting.  Have some ability for the seller to limit the number of buyers / interest groups.  That is why we have those caps in place. Make it clear which interest groups have priority.  

Brian: Something required to oversee the process rather than self-selection. 

Michael: The seller specify the auction config.  That person has the power to say who are allowed and how many interest groups.  Seem to be placing the responsibility at the right place.  If the browser decided, that is something Michael is skeptical of.  Sort of thing is that seller is best place to make these decisions.

What additional tools are available to the buyer.  That would be a fruitful discussion to have.  

Probably want to take a look at the tools available to the sellers.  

Tools for seller to set limits and buyer tools.

Q: From a tools perspective would the buyer have some indication why they lost the bid, even if they have a priority segment, is too low, or is it because there are too many bidders. Even though some audiences are lower priority many buyers are able to forecast allocated spend.  Anything to allow buyers to optimise bids would be useful.

A: There is an existing issue on post attribution reporting.  The set of outcomes should be talked about.  "This interest group did not make the top set", that seems like a valid thing to report on.  If there are 1000 interest groups and allow 20 in the auction does that mean that the other 980 get the same notification?  This is not entirely clear, whether we should report auction by auction.  

Should maybe think about in-bulk reporting not by-interest-group reporting.  Might make this more useful to the buyers and less heavy process on the browser.  

Angelina Eng: When prioritising based on deal IDs there are so many buyers, is there tools available to seller to manage inventory?  How do they manage 1000s of interest groups. 

They may not have clear view of what those interest groups are.  The seller does not have visibility of the interest groups so they do not know which ones they should prioritise.  

Michael: The model as it stands, the sellers decide how many interest groups / buyers are allowed.  They are responsible to place limits such as many milliseconds.  The buyers are totally in charge of the interest groups.  

If not all sellers in the open web do not know the buyers.  

The SSPs choose which DSPs.



*   Matt Wilson (NextRoll) [Issue 145](https://github.com/WICG/turtledove/issues/145), Interest Group Data in Report Win

Andrew Pascoe: Older issue where essentially, when setting interest group, you set user signals which are used in bidding function.  Can we have those signals, like the whole interest group, passed in the report win function?  Needed for machine learning to use those signals. 

MK: Is this related k-anonymity filter or some other issue?

MK: There are two different reporting paths, first there is the long term discussion about how FLEDGE should work.  Separately there are a bunch of debugging APIs that are used for testing.  These are not long term.  The long term support for win version, while it is event level reporting, intent is to be able to report on contextual information with full fidelity.  Full interest group object, that would easily contain the ID, and cross site tracking, clearly that is not compatible with FLEDGE.  

MK: When we have aggregate reporting functionality that is when we should allow interest group object reporting.  

AP: Are you open to a PR?  When we generate interest group in the bidding signal, some probability, calibration of models is important.  How to make sure that models are performing well.  Without ability to track transition state is problematic.  

MK: Probability of click or probability of conversion model?

AP: Both.  

MK: If conversion stuff then you probably want the attribution API.  This is different from reporting about what happened in the auction.  

AP: Any machine learning signal want to report back on.  Other models like will a person be likely to see an ad.  

MK: Aggregate reporting API is intended to the right answer.  We need to make clear how integration between the aggregation APIs and FLEDGE works.



*   Phil Lee (Google Chrome) [Issue 290](https://github.com/WICG/turtledove/issues/290), FLEDGE Key/Value Server requirements

We're working on a first reference implementation for a more trusted version of key/value server to add in real time signals in the auction.  Soliciting feedback on what features to build first.  Without building out complicated feature set.  Threshold for requirements.  We'd like to hear how much data, what latency is acceptable, and so on ….



*   Andrew Pascoe (NextRoll) [Issue 269](https://github.com/WICG/turtledove/issues/269), Mechanism for Retroactive Segmentation

Several issues here, which are all about different ways of using the FLEDGE API that are maybe unexpected.  Let's examine just this one for now.  Customers are visiting our clients sites, we segment those users based on their past browsing behavior and create campaigns.  Write interest groups into the browser while the user is on-site. What if we create empty interest groups?  They could save some information about the user's behavior. Users go off site and one of clients that target users that exhibit certain browsing behaviour.  This does not seem to violate privacy.  

Michael: Are you talking about behavior just on the advertiser's site?  Or behavior of the user as they browse some other website?

Andrew: Only on the advertiser's site.  Suppose we log every URL visited on the advertiser's site, and then I really like to target everyone that visited this and that URLs.  Look at the all the URLs, timestamp … and then bid in the buying signal.  

Michael: This is completely compatible with how FLEDGE was designed.  

Q: After the fact the advertiser creates another interest group and how does the owner add to that interest group.  

Michael: Why do we need a blank interest group to do any of this?  When someone is visiting pets.com, suppose you add them to an interest group, whose job is to bid a small amount of money, to bring previous customers back.  Then if that site wants to run a specialized campaign for dog lovers, it does not require another interest group.  That very same interest group could show different ads to different people.

You could do another approach more like what you are thinking, where there may be different interest groups for dog lovers, cat lovers, hamster lovers, etc.  But you could also have a single interest group with dog, hamster …. ads , then you can still use that single interest group along with the per-user signals to show different ads to different people.  

David Tam: Does that mean you can effectively have just one IG, for everyone that visits that site?  And then personalize based on user attributes?

MK: Yes!

Andrew: Personalization is still limited on k-anonymity of the ad itself, not of the IG

David: How many ads can an IG hold?  Is there a limit?

Andrew: There is a limit to # bytes in the JSON

Paul: I think that's the only limit, don't recall others.

Russ: Update size is the main limit.  There are limits on the size of the whole database, but you're not likely to hit those.

David: Some e-commerce company might have many different products.

MK: Agreed, you wouldn't want to push 10,000 items into one IG.  This would be an engineering decision, how to best manage interest groups, could make them more specialized or have fewer IGs that are each more flexible. 

Roten Dar: Pretty similar to targeting a user based on a site list — but you need the user to pass through the advertiser's domain first, to have some sort of basis

MK: The fact that an interest group can store lots of metadata about the user on a particular site means it is different from just a site list.  The way we talk about user list lots of times is as a simple binary thing: is a person a member of this user list?  An interest group is much richer as it contains more information to inform buying decision.  

Thank you, great discussion, see you in two weeks.
