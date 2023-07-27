# Protected Audience (formerly FLEDGE) WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday June 7, 2023


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Gianni Campion (Google Ads)
3. Fabian Höring (Criteo)
4. Sven May (Google Privacy Sandbox)
5. Roni Gordon (Index Exchange)
6. Harshad Mane (PubMatic)
7. Sid Sahoo (Google Chrome)
8. Paul Jensen (Google Chrome)
9. Russ Hamilton (Google Chrome)
10. Maciek Zdanowicz (RTB House)
11. Andrew Aikens (TripleLift) 
12.  Kevin Lee (Google Chrome)
13.  Risako Hamano (Yahoo Japan)
14.  Jonasz Pamula (RTB House)
15. Stan Belov (Google Ads)
16. Orr Bernstein (Google Chrome)
17. Tianyang Xu(Google Privacy Sandbox)
18. Youssef Bourouphael (Google Privacy Sandbox)
19. Isaac Schechtman (BidSwitch) 
20. Tamara Yaeger (BidSwitch)
21. Caleb Raitto (Google Chrome)
22. Alfred Wong (Remerge)
23. David Dabbs (Epsilon)
24. Isaac Foster (MSFT[Xandr])
25. Supraja Sekhar (Google Ads)
26. Ardian Poernomo (Google Ads)


## Note taker: Orr Bernstein


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here — no agenda no meeting!]



*   [How to handle k-anonymity fallback #592](https://github.com/WICG/turtledove/issues/592) [Fabian Höring]
*   [Support more than one ad candidate and bid from generateBid #595](https://github.com/WICG/turtledove/issues/595) [Gianni Campion]
*   [Leave all IGs for a given domain #475](https://github.com/WICG/turtledove/issues/475) [Gianni Campion]
*   [Click related data in browserSignals #579](https://github.com/WICG/turtledove/issues/579) [Maciek Zdanowicz]


# Notes



*   [How to handle k-anonymity fallback #592](https://github.com/WICG/turtledove/issues/592)
    *   Fabian Höring
        *   Started looking into different renderURLs with different granularities
        *   Two URLs - one targets a very broad audience, less targeted, easy to pass the kAnon threshold, pretty sure we’ll have something to display; another that’s more targeted, harder to pass the kAnon threshold.
        *   What would be the strategy to implement to be sure to always display something with the kAnon requirements implemented?
        *   How to bootstrap at the very beginning when I have no URL to present.
        *   What happens whenI stop displaying one URL and want to make sure that I’m still above the threshold.
    *   Michael Kleber
        *   Background for kAnonymity in FLEDGE.
        *   Here was the plan for how to deal with kAnon in FLEDGE when I wrote the explainer in 2021.
        *   One of the privacy protections in what was once called FLEDGE is that ads shown to users cannot be targetted to a single user, needs to be targetted at a large enough group of user.
        *   Especially important with event level reporting. If there were a Michael Kleber ad that was shown only to Michael Kleber, they’d immediately know that I’d visited that page, could reconstruct my browsing history. Need a better technique.
        *   In the long-term, the better technique is that all reporting coming out of the Protected Audiences API is aggregate reporting, so people don’t know exactly what ad appeared at each page.
        *   But in the meantime, kAnon provides some initial protection. Requires that an ad is shown to at least 50 people in a particular week. If you see the ad, only a 2% chance it’s person X because it’s also been shown to another 49 people.
        *   But has a bootstrap problem. How can you show an ad for the first time?
        *   Proposed was, you can bid and say I want to show this ad, and if it’s not kAnon, it still counts as 1. So, you _try_ to show the ad to 50 people, and then on the 51st, you’ve met the threshold and you can show the ad.
        *   But during the ramp up, it seems unfair that the InterestGroup can’t show any ad. So there’s this fallback option, where the InterestGroup gets a second opportunity to bid when it tries to show an ad that’s not kAnon. The second time it gets an opportunity to bid, it can only put forth ads that are kAnon.
        *   That’s the two-step bidding process we have today. You bid once using all your ads, and a second time using only the ads that are kAnonymous.
        *   Fabian points out, if your strategy is, first try a very targeted ad, and then fallback to a generic ad, how do you get the generic ad past the kAnon threshold in the first place?
    *   Fabian
        *   Proposed solution. Some discussion in the ticket. Idea to increase the counter of multiple ads. Agree that it wouldn’t be a good idea to increase the counter for many ads, ok to increase the counter for only the ads which I have bid.
        *   Discussed strategy would be to start with the most generic ad. Then look at logs, and when it starts to be displayed, then start displaying the more specific ads.
        *   Let’s say I start on day 1, and I put forth my generic ad 50 times, but if on day 2, I display a less generic ad, on day 3 the displays from day 1 will expire. 3 day rolling window.
    *   Michael
        *   True. If you put forth the effort to bid with the more generic ad to begin with, then after seven days, it will pass the cliff, and now again you don’t have a fallback ad to fallback to.
    *   David Dabbs
        *   Would you be open if we could construct some sort of traceability mechanism to allow us to understand how we’re failing as an early temporary mechanism?
    *   Fabian
        *   Don’t see a difference of having a fallback option. Everything happens on the device.
    *   Michael
        *   If you allow the bidding function to know which ad is kAnonymous and which is not, then you’re asking the bidding function to say how much money you would put forth, but there’s no chance they’d have to pay that amount of money because it isn’t kAnonymous, their behavior would not reflect real bid behavior. You’d bet a million dollars for an ad that’s not kAnonymous because you know it can’t win the auction.
    *   Isaac Foster
        *   Degree to which kAnonymity is protected, up front by design. Has many positives, but also, whoever owns this introduces some level of accountability into the system. Why couldn’t you take a post-hoc or a mixed approach where ads can have a second state, which is “under review” or something that you could inspect, where the keys or the bidding function could be shut down.
    *   Michael
        *   At a higher level, the idea would be that anyone abusing the system by driving up numbers for bids for non-kAnon bids could be penalized. Certainly, this is something that’s technically feasible. Unfortunately, if someone were trying to do something abusive like that and also be subtle, they could do so. Not sure that something that is risky will necessarily be as flagrant as the attack I just described.
        *   Better for there to be technical limitations than to have to create structures and penalties around the lack of those.
    *   Isaac
        *   Certainly could detect straight-up abuse in that way. But also could do something like, a newly created URL that would have to go into this interesting bidding scenario, different state, if it didn’t get kAnonymity in a certain time, it goes back to the current state. May be cases where  the tradeoffs benefit from the technical limitations not being so bulletproof.
    *   Michael
        *   Would prefer something like a solid technical answer first. Can fallback to the latter.
    *   David Dabbs
        *   Can we reach about the behavior of PAAPI auction mechanics if we had two concrete implementations? Doubletap two-tier - all creatives are eligible and then only the kAnon ones are eligible. The B&A case would look the same?
    *   Michael
        *   Yes, we don’t want people to have to think about both cases separately.
    *   Jonasz
        *   Would it be enough to attach the metadata, observed through reporting mechanisms, about how often each ad was shown to the InterestGroup?
    *   Fabian
        *   When you see display, you don’t know if the kAnon counts as 1 or 50.
    *   David Dabbs
        *   Also, the goal would be to move away from anything that makes a network access, move to things that use web bundles?
    *   Jonasz
        *   Even if one impression of an ad is being observed. You can play it safe and you can observe that there is at least 100 impressions, and then display the more granular ad. This would also work in future with aggregated reporting, though perhaps with different dynamics.
    *   Fabian
        *   Works for the beginning. When you see the ad for the first time, you know it passed the threshold. You can check the logs.
    *   Michael
        *   This is the hacked-together strategy I proposed in issue 592 for using the key-value server to see if the fallback ad is good, or should I occasionally use the fallback ad to ensure it stays above the threshold.
        *   Maybe related is the next topic on the agenda, about allowing each InterestGroup to return a list of ads, ordered by preference, it’s possible there is a way to do that, where it’s possible that the most highly preferred ad that’s above the threshold is the one you bid with in the auction, and the one that’s the least highly preferred ad is the one that gets to increase its kAnon.
    *   Paul
        *   Want to better understand the problem. Is the problem, if we’re looking for a kAnon of 50 counts of 7 days. That when the next 7 days starts, it gets reset?
    *   Fabian
        *   Let’s say I show the ad 50 times, and then over the next 6 days nothing, then on day 8, it would be ineligible.
    *   Paul
        *   Yes, if you do all of the displays at one time. Seems like an odd use case.
    *   Fabian
        *   Yes, but the strategy Michael recommended would work with existing APIs - ping to see if the generic ad is above the threshold.
    *   Michael
        *   Less of a concern if you have a lot of campaigns using the same fallback ad, because then each of the more specific ads helps to increment the kAnon number of the fallback ads.
*   [Support more than one ad candidate and bid from generateBid #595](https://github.com/WICG/turtledove/issues/595)
    *   Gianni Campion
        *   Imagine you have two InterestGroups - one very big, one very small
        *   We return top candidate from each of these IGs
        *   The one from the big one loses to the one from the small one. The big IG can have a lot of other ads that would have done better. We lose the ability to make money from the big IG.
        *   Three problems
            *   With bigger IG, might bid with lower quality ad.
            *   In a sense, we’re throwing away a lot of ads when we return only one ads. With kAnon, chances are in the kAnon-enforced pass, you’ll do the same thing as you did in the non-kAnon enforced run.
        *   Would reduce the effect of kAnonymity, because if we have 10 ads, the first five not kAnon and then the next five kAnon. Could return them all.
    *   Michael
        *   If you have an IG that has 50 different ads. Not enthusiastic about taking all 50 of those and passing them to the SSP’s ScoreAds function. Now the SSP has to score 50 times as many ads, need to load SSP bidding signals for 50 times as many ads.
    *   Gianni
        *   Resource constraint
    *   Michael
        *   Yes, and surely something that DSPs have to deal with in RTB land today. DSPs can’t return an unlimited number of bids for the SSP to pick through. IIUC, DSP returns one or two ads to an SSP today, not more. You all would know this better than I do.
    *   Issac Foster
        *   Curious what the design concern is here. From a privacy architecture perspective. Outside of the resource constraints. I’ve worked at Microsoft and Xander for the past 11 years. Typically try to handle this by some sort of object limit, and not just 0 or 1. Bids come in, there’s some cap - not 50 but not 1 either - strike the right balance between publishers, advertisers, SSPs, ADSPs, and consumer. Come up with a number - maybe 5?
    *   Michael
        *   Not talking about a privacy issue here. As Gianni pointed out, the same as taking one big IG with 50 ads and turning it into 50 IGs with one ad each. So primarily talking about resourcing issue, and what’s feasible.
    *   Isaac
        *   Might have missed the core of this request. Didn’t understand the request to be that each bid would be submitted with one ad. But rather, that there’s matching that’s returned with some keys, and determines some set of ads to bid.
    *   Gianni
        *   If you have large IGs, might have large number of ads, some of them are dropped, some go off to bidding. Agree the number is 50. Minimum number is 2 - my idea would be that the generic bid picks all of the ads, and the browser picks the maximum one and the maximum kAnonymous one. Throw a couple more just for safety, and now you’re up to the five.
    *   Michael
        *   On the resource constraint side, if there are 50 ads, and you attach a bid to each of the 50 ads - to be clear, Chrome is not doing any evaluation of the merits of the ad - the DSP can rank those by whatever they want to rank them by. After the SSP has evaluated the top five bids coming out of the IG, if the SSP doesn’t like these as much as bids from some other IG, no value of the SSP evaluating the remaining ads from that IG. Don’t want the DSPs dropping any responsibility for selecting relevant ads, and leaving it to the SSPs.
    *   Issac
        *   Agree, we don’t want to make the DSPs return each ad with a shrug emoji. Ranking is not always straightforward. The idea that SSP would rank the DSPs bids in a different way than the DSP is not surprising. We would return to the SSP, which does the ranking.
        *   Even with a reasonably low object limit, if you can return up to five, increasing the degrees of freedom on how people can use this in a privacy safe way.
    *   Russ (in chat)
        *   To balance it, could the SSP place a limit on the number of ads per owner like they do now on the number of interest groups that bid?
    *   Michael
        *   Maybe top five and top five kAnonymous ones, then those are the ones we have the SSP evaluate. Right now, it’s one per InterestGroup. Obviously we need some change to the API that allows generateBid() to return more than one bid - that’s a purely mechanical issue - if you see a reason that’s a bad idea, please speak up.
    *   Gianni
        *   The cost of running more than one generateBid is more expensive than returning more than one bid from a single call to generateBid. So, we’re saving resources. And in cases of no kAnonymity, would save the cost of rerunning.
    *   Michael
        *   Agreed. Many ads that get evaluated twice in the rewriting of kAnonymity case. As long as we can avoid the case of, you didn’t like the first five bids, but I’ll have the SSP evaluate the rest of the 45 bids.
    *   Paul Jansen
        *   Like this for several reasons. Especially if some are kAnon and some are not, could yield good performance. We already group together TrustedScoringSignal fetches. The per-buyer limit - makes sense for the SSP to pick the number - if they picked 5, and it’s a per-buyer thing - it’s complicated if the buyer is multiple InterestGroups - how you distribute those five across InterestGroups. It could be different deals for different InterestGroups. If one IG uses the whole allotment of bids for a bidder, and there’s another IG that has lower value ads but ads that the SSP would evaluate as stronger.
    *   Paul/Michael
        *   Could use different strategies. For example, could pick the top 5 bids by value, or ensure that we spread out bids across more IGs.
    *   Isaac
        *   Enable the participants to solve their own problems sometimes through configurations or whatever. If this creates problems through different IGs, the answer doesn’t need to be that the implementation limits things perfectly.
    *   Michael
        *   Looking forward to continued conversation on GitHub. Hope to see you all back here in two weeks.
