# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 4pm UTC (during winter).

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Feb 15, 2023


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Brian May (dstillery)
3. Alonso Velasquez (Google Chrome)
4. Russ Hamilton (Google Chrome)
5. Tim Hsieh (Google Ads)
6. Caleb Raitto (Google Chrome)
7. Itay Sharfi (Google Chrome)
8. Paul Jensen (Google Chrome)
9. Manny Isu (Google Chrome)
10. Fabian Höring (Criteo)
11. Elias Selman (Criteo)
12. Martin Pal (Google Chrome)
13. Sven May (Google Chrome)
14. Orr Bernstein (Google Chrome)
15. Connor Doherty (Amazon)
16. Joel Meyer (OpenX)
17. Matt Menke (Google Chrome)
18. David Dabbs (Epsilon)
19. Marco Lugo (NextRoll)
20. Kevin Lee (Google Chrome)
21. Wendell Baker (Yahoo)
22. Julien Aimonino (Criteo)
23. Danny Rojas (Google Chrome)
24. David Tam (Relay42)
25. Andrew Aikens (TripleLift)
26. Jonasz Pamula (RTB House)
27. Andrew Pascoe (NextRoll)
28. Michał Kalisz (RTB House)
29. Stan Belov (Google Ads)


## Note taker: Manny Isu


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here]

Held over from previous call:



*   Interest group size https://github.com/WICG/turtledove/issues/402 , Julien Aimonino

New items:



*   https://github.com/WICG/turtledove/i[ssues/166](https://github.com/WICG/turtledove/issues/166) [link](https://github.com/WICG/turtledove/issues/166#issuecomment-1419831197)
*   FLEDGE: custom reporting dimensions https://github.com/WICG/turtledove/issues/165, Tim Hsieh, Stan Belov
*   iframes' usage in FLEDGE going forward; communication from container to nested iframes; resizing (ctx: https://developer.chrome.com/docs/privacy-sandbox/fledge-api/feature-status/), Jonasz Pamuła


# Notes

Interest group size https://github.com/WICG/turtledove/issues/402 , Julien Aimonino



*   [Julien] Any progress on this?
    *   Update from Paul Jensey yesterday on issue #361 which is related:
        *   https://github.com/WICG/turtledove/issues/361#issuecomment-1430069343
        *   For Interest Group updates, we're going to try something different from k-anonymity — each IG is just one site's data, so really these are privacy-safe as long as we address side channels, which we're actively working on for other parts of the web.  We'll go other directions for addressing those
        *   The k-anon req for IG updates was causing lots of problems, as discussed on that #361 issues, and we have other ways to address the privacy goals, so we will be removing the k-anon req.
        *   So now is a good time to reopen the question about size limit: with k-anon req gone, should be possible to have fewer IGs, e.g. only one per owner per site the user visits — but yes, we should raise the limit on IG size to compensate for this change
        *   Browser does need to be concerned about lower-end devices that don't have as much storage; can't store 50GB of data on a little Android device, so some kind of global limit will need to be in force.  But we can relax limits to something per owner instead of something per IG, for example
        *   There are other things browser does with similar limits.  Cookie size limit is something like 4K, while cache limits are high.  We'll need to figure out the right balance.
    *   [Jonasz] Our position is that the daily update without k anon is a trusted update. In the future it will unlock certain use cases. We view it as a separate issue compared to the IG size. It will work better if there is a single limit that is applied globally
        *   [MK] That is very much aligned with Paul’s proposal - what you are doing seems to be the emerging consensus and it is exactly right. We will remove the K anon threshold and figure out the best limit. If there is anything as a FLEDGE user that you find as a barrier, please bring it up and we can discuss it.
    *   [Paul] What do you see as the biggest contributor to the size?
        *   [Jonasz] Will need to double check, but my recollection is that it is indeed the list of ads and ads components. We have to specify the entire url each time and it adds to storage space.
        *   [David] RTB often responds to some SSPs with a bid - might it be possible to provide a token or placeholder to add urls 
            *   If it turns out that the actual ad url is a significant contributor to the IG size, it seems like an excellent thing to add that will make the system better.
*   FLEDGE: bidding currencies,  https://github.com/WICG/turtledove/issues/166, Tim Hsieh, Stan Belov
    *   [Tim] Let’s discuss bid currency which was a carryover from last time. In summary, the issue is that there are certain limitations - provides other mechanisms for sellers and buyers to bid in different currencies. If we do decide to support multiple currencies, the highest other bid currency becomes ambiguous. My proposal is to add the currency on the Generate bid result and maybe change the highest order bid to be…
    *   https://github.com/WICG/turtledove/issues/166#issuecomment-1419831197 suggestion, "Option 2" there
        *   [Paul] Who does the currency conversion if bids are coming in different currencies?
            *   [Tim] It will be the seller. Under option 2 where we are choosing the currency, it will still be the seller.
        *   [Paul] Does the buyer know what the currency conversion rate will be?
            *   Not sure if a buyer will necessarily need to know
        *   [Paul] Generate Bid to Report Win, we risk people using those fields to pass information that could be user identifying. Can the rate be provided to bidders at auction time so we do not have to pass it along in reporting? What's the problem with having the seller specify this?
            *   [Joel] Today, sellers take on the currency conversion role and take on the risks as well. We do not send all the currency conversion rates to all of our buyers today - it is handled by some currency conversion service.
            *   [Martin] There is a billing relationship between buyer and seller. These two parties can agree on terms including the currency
                *   [MK] The problem is that if specified as part of IG, it could still be different for each individual browser. 
        *   [Tim] Is Chrome thinking that all buyers need to be placed in a standard currency such as USD?
            *   This is the current GAM experiment - certainly one possible answer.
        *   [Tim] The idea is that if the seller has 2 buyers bidding in 2 separate currencies, the seller will need to convert in some standard currency (USD) - seller will provide the list of acceptable bidding currencies. Every buyer will submit a bid in any of those acceptable currency. Seller will note the intended currency for each of these buyers, convert them and declare a winner. During reporting, the seller will report a win in the currency that was preferred by the buyer who submitted that winning bid. Seller will also take the second highest bid into the same currency of that of the buyer who submitted the highest bid (winner).
            *   [MK] We do not want to design a system in which there is Event level comms between buyer and seller. Building this seems very contrary to our long term goal of aggregate reporting.
            *   [Martin] Take the highest desirability score… we could offer the seller to run another piece of code to… if you want to win, this is how much you’ll have to bid. Can we maybe male the score ad a multiplier?
                *   Yes, this would make sense to make it a multiplier.
            *   [Joel] Where does the highest order bid come from?
                *   [Paul] https://github.com/WICG/turtledove/blob/main/Proposed\_First\_FLEDGE\_OT\_Details.md#reporting
                *   [MK] Highest order bid is the second price in which you need to run the second price auction.
            *   [Joel] Is it possible that this is the contextual bid?
                *   [MK] We do not have a mechanism for that in FLEDGE right now. If we want the HighestOrderBid to accurately report, then we will need to make the contextual bid a first class citizen in the FLEDGE auction the way that it is not right now. If this is important, then please start a github post.
            *   [Andrew] Opposed to having sellers convert the currency. Buyers need to be responsible for this because it is important for them to understand what they are paying. I am in favor of sellers declaring the currency.
                *   [MK] This is why I advocated for sellers to bid in multiple currencies, the buyers should publish their tables - basically to allow both ways unless there is some privacy leakage which we are not sure about.
                *   [Joel] What we have today is the best we can do for now without any complexity.
                *   Both Joel and Andrew are advocating for the current GAM experiment
            *   [Andrew] If we billed in USD, and we see the currency that the seller was using, then we have the conversation with the seller if it is out of whack. My principle is that the seller is the one conducting the auction and they have the right to declare their currency and it is the buyers responsibility to accept the conversions or not
            *   [Fabian] My understanding is that it is a financial requirement to invoice in Euro if the market is in Euro
                *   [Jonasz] Will take offline and get back to this group
        *   [MK] In summary, we are in the territory of having the ads industry needs to work out. There may be some tweaks that the browser needs to make to accommodate it but it is mostly an issue that needs to be worked out between Buyers and Sellers.

[MK] Final note: Later today, there is a call about Private Aggregation API use cases, e.g. Reach Measurement — see https://github.com/patcg-individual-drafts/private-aggregation-api/issues/19 for details

See you in two weeks.
