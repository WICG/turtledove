# Protected Audience (formerly FLEDGE) WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Oct 11, 2023


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Russ Hamilton (Google Chrome)
3. Sid Sahoo (Google Chrome)
4. Brian May (dstillery)
5. Harshad Mane (PubMatic)
6. Roni Gordon (Index Exchange)
7. Fabian Höring (Criteo)
8. Antoine Niek (Optable)
9. Paul Jensen (Google Chrome)
10. Isaac Schechtman (Criteo/BidSwitch)
11. Kevin Lee (Google Privacy Sandbox)
12. Orr Bernstein (Google Privacy Sandbox)
13. Isaac Foster (MSFT Ads)
14. David Dabbs (Epsilon)
15. Manny Isu (Google Chrome)
16. Brian Schmidt (OpenX)
17. Laurentiu Badea (OpenX)
18. Risako Hamano (Yahoo Japan)
19. Stan Belov (Google Ads)
20. Lukasz Wlodarczyk (RTB House)
21. Matt Menke (Google Chrome)
22. Marco Lugo (NextRoll)
23. Jeff Wieland (Magnite)
24. Caleb Raitto (Google Chrome)
25. Elias Selman (Criteo)


## Note taker: Manny Isu


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


## Suggest agenda items here:



*   Roni Gordon [held over until Roni can join the call]
    *   same origin constraints - https://github.com/WICG/turtledove/issues/813
    *   K/V lookup timeouts - https://github.com/WICG/turtledove/issues/814
    *   Macro substitutions - https://github.com/WICG/turtledove/issues/817
    *   API versioning - https://github.com/WICG/turtledove/issues/823
    *   Sensitive signals - https://github.com/WICG/turtledove/issues/824
    *   Multi-size banners - https://github.com/WICG/turtledove/issues/825 **_+1000 ANSWER UNSATISFACTORY!!!!!!!!!!:)!!!!!!!_**
    *   Additional browser event beacons - https://github.com/WICG/turtledove/issues/826 
*   Isaac:
    *   Multi Tag Support via “Mixed Ranking”: https://github.com/WICG/turtledove/issues/846
    *   Temporary surge in discussion opportunities, twice a week for a bit? 3 every 2?
    *   Buyer/Seller Reporting Questions: https://github.com/WICG/turtledove/issues/682#issuecomment-1710965068
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Jeff W:
    *   PAA Market Test Phase Labels
        *   Given the language [here](https://developer.chrome.com/docs/privacy-sandbox/chrome-testing/#cookie-deprecation-value) implies users need to consent to a label transiting the bid stream (leaving the device) how would you suggest we address this? 
        *   What if user consent skews the test/control ratio? 


# Notes


## Multi-Size Banners https://github.com/WICG/turtledove/issues/825



*   [Roni Gordon] This is the majority of the banner inventory seen by SSP and configured by publishers. Why does the existing API surface only support a single size? Can you enlighten the broader group on the privacy nuance? What does this mean for the early phases as it pertains to CMA testing and eventually, where we intend to go with multi size banners?
    *   [MK] Fundamentally, the issue to be dealt with is… the API involves some store of information for the user kept secret and stored on the device. All the protections about the outcome of the OD auction is about preventing the leakage from the OD information store. At one extreme, if the x and y sizes are unbounded and chosen by the auction, then it's hopeless, the choice of x and y can leak all the secret information. So we certainly need some constraint on what can come out of the auction. The goal is to have the results not leak any information at all about who the user is. Ultimate goal is the reporting to happen at aggregate level and not the per-event level. The reality right now is some place in between the two - certainly more revealing than not learning anything about the user… The place where we are trying to put all of the "Yes we are still leaking information but we’ll fix in the future" are the reporting APIs like the aggregate API, event level reporting API; those reveal more for now, but we will cut down later. Also, there is 1 bit of information that is leaked as part of the API surface itself - was there an ad that was part of the PA API auction or not? The bit is leaked to the surrounding webpage. People have promised not to abuse that leak through attestation. The 1 bit of information is an uncontrolled 1 but that we want to get rid of… If we were to allow the results of the ad auction for different sizes, then that 1 bit will turn into many bits of information.This is why we cannot allow different size ads to come out of the auction
    *   [Brian] Is the concern about Publishers?
    *   [MK] Yes and.  It's about publishers and everything else that can observe the outcome, for example any third parties on this webpage
    *   [Brian] I think it will be difficult to create usable signals if it is only standard sizes - the idea that you can use this to generate a user ID seems far fetched
    *   [MK] The concern is cross site tracking, creating a user ID here is not hard
    *   [Brian] If we are doing this on a million sites, are we really providing meaningful signals for folks to use?
    *   [MK] Yes.
    *   [Brian] I wonder if there be ways of allowing different sizes - maybe one size per interest groups
    *   [Isaac] How much of this is related to a subtle but important distinction that PS as a principle wants to limit sharing of data across sites as opposed to stopping re-identification as the main principle? Can we apply some of the diff privacy techniques like k anon, noising of the size, etc.?
    *   [MK] The various other places for noising are in the realm of reporting APIs, which want to be aggregated in the long run and in the short term, we are using attestations to circumvent that… Noising does not seem applicable for the size ads that is the winner of the auction.
    *   [Isaac] For noising, can we apply it to the size (the width and height)?
    *   [MK] Noising is not relevant. It will not help anything at all. For K-Anon, there is a protection on the URL that comes out of the auction. However, it does not help with cross site ID.
    *   [Roni] As a seller, need to continue to run the business. Publishers are configuring multi size ads slot. The intent is tall and short rectangle; and that’s the intent of the seller that is represented. Where do we go from here?
    *   [MK] There are things you can still do even with the current restraints. You can run two auctions; if an ads wins a tall rectangle, then you can show it, otherwise I’ll show the short rectangle. All you are doing is running 2 auctions and having 1 bit come out of that auction.  This only works, though, if you are willing to say that you always want the tall rectangle if there is one; you don't get to render whichever of the tall or short has the higher bid.  As a second thing you can already do, you can also run a multisize ad auction in which there are 15 different sizes with purely contextual signals which don't use PA API; and you can say here is my contextual winner. And then the PA API has the opportunity to beat the contextual winner, but PA gets to fill only a single predetermined size.
    *   [Russ] If you can run 2 auctions and pick 1, then could the auction run the GenerateBid twice and have two sizes chosen independently, and then let the auction pick the winner?
    *   [MK] Running 2 auctions uses twice the budget. Then it lets the 2 compete against each other.
    *   [Harshad] Multi size is so common amongst publishers. How will top-sellers receive the information?
    *   [MK] Every PA auction is an auction of 1 size
    *   [Paul] You can have a FF on the page that has both an ad and non-ad content. You can then reflow things that will let you render ads of different sizes without the surrounding page knowing it
    *   [MK] This will amount to some bizarre gymnastics and will be unappealing.
    *   Isaac: please give details about the bits attacks, want to understand better
    *   [MK] Let's keep discussion going in #825


## [Isaac] Is there a way to make things a little different but in a private way for ScoreAd and parallelize things?



*   We have done a lot to allow OD auction to parallelize and have the auction start and all the parts that doesn’t depend on some piece of information can run.


## PAA Market Test Phase Labels



*   [Jeff Wieland] If the label is gated by user consent, how should we explain to the user what we are asking them to do?
    *   [MK] Let me try to answer, but afraid I will not have a satisfying answer to this question. This is not legal guidance and I am not a lawyer. That said, here is some context: There is an A/B test that Chrome will facilitate people running.
    *   Link: https://developer.chrome.com/docs/privacy-sandbox/chrome-testing/#cookie-deprecation-value
    *   There are two different tests: Mode A and Mode B. In Mode A, Chrome wil label some segments of the population with Control or Experiment groups. Chrome will not do anything different with those people. However, adtechs can run some kind of experiment with chunks of labeled traffic for their experiments. That is setup for the Mode B experiment. Now, in Mode B, there will be 4 different chunks of traffic, 3PC will be disabled for these chunks, and people can make comparisons. Now, the problem is that “accessing labels involves accessing information that is stored on the user's device…”, which brings up questions of consent under ePrivacy or GDPR.
    *   So how can you run an experiment if you need to ask for consent to know whether a person is in the experiment or control group?  As a scientist, as long as you have asked people for consent in the same way irrespective of the branch they belong to, and once you get consent you read the experiment-vs-control label, this does not seem to me in a situation where you have to worry about consent affecting the two groups differently. However, if you have to ask for consent in one jurisdiction over another jurisdiction, it seems like you will then need to slice your experiment by jurisdiction.
    *   [Russ] You need to set/get cookies to get the label anyway, so you should already have a consent flow.
    *   [MK] I would recommend you talk to your lawyer.
    *   [David D] You mentioned “percentage of users” in the devrel document, but I believe you mean “instances of chrome browsers”... just want to suggest you disambiguate that because it gives the impression that this may refer to actual people behind the keyboard.
    *   [MK] You are 100% correct.  It's the percentage of Chrome instances.
        *   [Kevin] I'll pass that comment along and update this doc
    *   [JW] Does Chrome plan to ask the user for consent to write the label to the browser cookie jar?
    *   [MK] No, it is not in the cookie jar. It does appear as a js API or cookie header
    *   [JW] Is it stored on the device?
    *   [MK] Yes, it is stored on the device. Chrome has a built-in random assignment for experiment purposes. This is another example of the Finch trial and which Finch trial you belong to is a random number between 1 and 8K.
    *   [JW] I have concerns for how we are going to phrase this to the user. I don’t know what type of language to use here
    *   [MK] That makes sense. Talk to your lawyers
