# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Aug 17, 2022, but _only if_ there are suggested agenda items (suggest below)



*   There are indeed agenda items for Aug 17


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Aleksei Gorbushin (Walmart)
3. Andrew Aikens (TripleLift)
4. Brian May (dstillery)
5. Shivani Sharma (Google Chrome)
6. Andrew Pascoe (NextRoll)
7. Daniel LaLiberte (Google Web Standards)
8. Russ Hamilton (Google Chrome)
9. Caleb Raitto (Google Chrome)
10. Paul Jensen (Google Chrome)
11. Sid Sahoo (Google Chrome)
12. Matt Menke (Google Chrome)
13. Marco Lugo (NextRoll)
14. Wendell Baker (Yahoo)
15. Eric Trouton (Google Chrome)
16. Bartosz Marcinkowski (RTB House)
17. David Dabbs (Epsilon)
18. Sangharsh Kamble (Walmart)
19. Abhimanyu Singh (Amazon)
20. Georgia Franklin (Google Chrome)
21. Anthony Molinaro (OpenX)
22. Joel Meyer (OpenX)
23. Alex Cone (IAB Tech Lab)
24. Stan Belov (Google Ads)
25. Tamara Yaeger (Iponweb)
26. Daniel Rojas (Google Chrome)
27. John Mooring (Microsoft) 
28. Zheng Wei (Google Ads)
29. Philippe de Lurand Pierre-Paul (Google Chrome)
30. Martin Pál (Google Chrome)


## Note taker: &lt;Sid Sahoo>


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here]



*   [Paul Jensen] FLEDGE Origin Trial updates:
    *   [Extended to M107](https://groups.google.com/a/chromium.org/g/blink-dev/c/SD8Ot2gpz4g/m/Cc0TGPhoAAAJ)
    *   [Rolled out to Stable for desktop](https://groups.google.com/a/chromium.org/g/fledge-api-announce/c/mvPL3Qn6WjI/m/YD-Tsy9BCQAJ) 
    *   [Expanding to Android](https://groups.google.com/a/chromium.org/g/blink-dev/c/0VmMSsDWsFg/m/uWXVt89lAwAJ)
*   [[shivanisha@google.com](mailto:shivanisha@google.com)]: https://github.com/WICG/turtledove/issues/264#issuecomment-1213194145
*   [Aleksei Gorbushin] https://github.com/WICG/turtledove/issues/338 


## Notes



*   Paul Jensen: Origin trial updates
    *   Normally OTs are requested for 3 milestones–we now extended it for 3 more on the FLEDGE OT (from 105-107 now). We sent out an announcement on the group, and extending the OT to Stable. Seeing Stable traffic on desktop.
    *   Please join https://groups.google.com/a/chromium.org/g/fledge-api-announce/ if you're not on it
    *   Made some changes to process model in Android–site isolation. FLEDGE auction latency won’t be as affected by process startup time. Looking to expand OT to Android as well, will start with canary, dev, beta (as usual).
    *   Added links above with more details. \

*   Shivani Sharma: Fenced frames–intersection observer
    *   Current behavior of Intersection Observer in fenced frames is similar to iframes. Embedder trying to tell the frame where you are on the screen. Should not be used as a comm channel bet embedder and the frame.
    *   Jonathan added an issue in github: https://github.com/WICG/turtledove/issues/264. Requesting feedback here. Also look at the [linked doc](https://docs.google.com/document/u/1/d/1qUzcup_BX9LNlv2Bw4QxWJaTo7nbwQEkPiLKfYbYsP0/edit).
    *   Considerations for solution space:
        *   (1) whether the intersection events are coming to the FF or not
        *   (2) whether the events are at aggregate level or event-level
    *   MRC has definitions of the viewability (e.g. "50% of pixels for 1 second")--added this to the doc as well.
    *   Want to understand use-cases: such as a 1-bit information to indicate whether ad was on-screen or viewable. Alternatively, maybe the ad itself does something special when ad scrolls onto the screen.  Please let us know.
*   Aleksei: https://github.com/WICG/turtledove/issues/338
    *   Trying to figure out using FLEDGE for our use-cases; participated in a few existing issues. Will explain what we want to achieve and seek help.
        *   Step for collecting events on the side. … store visits–some visits can be identified (credit cards, app use, etc.)--correlated browser and user behavior to build segments (audiences) for Walmart.
        *   Maybe audiences are uploaded to DSP, and then use campaign manager (1:1 or 1:m for audience:device)
        *   Waterfall in diagram: Pub -> SSP -> DSP (auction). DSP figures out campaigns. In the middle, could modify audiences (sort of retargeting)--some additional events happened (e.g. purchase happened, and so remove user from audience)
        *   Strong separation: Walmart has user-behavior data. DSP implements the auctions, integration with SSP. With FLEDGE these responsibilities need to flip.
        *   With IG, userbiddingSignals can contain additional signals. E.g. no. of times the browser saw shoe pages–multiple brands. And can define custom logic—show ad when this count is more than threshold. No way to update IG if the user doesn't visit your site. FLEDGE tied to publisher; we have both browser-side events and application events (user logged in across app and browser) How do we combine these signals to define effective campaigns?
        *   How do we solve the issue with updates? Daily updates are not meant to be identifiable.
    *   Michael Kleber: 2 different things to talk about here.  Thing 1: relationship between multiple parties (Walmart-adv vs DSP). When we built FLEDGE, we didn’t think of separate jobs–audience building and bidding. We called it the IG owner—really this is the buyer/bidder in auctions. Maybe different terminology would have been better. DSP as the buyer and maintaining IG should be compatible with your use case. In today's world you create audiences and then upload to DSP. This same division of responsibilities should work in FLEDGE. Instead of uploading, you could make your choice about membership and then have DSP code on your pages or ask DSP to add user to IG. FLEDGE has two built-in mechanisms: iframe with permissioning to allow DSP to add to IGs for Walmart, or delegation the other direction where DSP lets Walmart add people to IGs the DSP owns. If these two techniques don't meet your needs (about split of jobs), happy to tweak things, add another way to do delegation, etc.
    *   Aleksei: This applies to browser only. Is PS for Android meant to work in a way to link things.
    *   Michael Kleber (Thing 2): Non-browser events. FLEDGE was not designed considering other events (outside of browser). Did not plan for this. Good use-case for us to talk about PS for Android–what happens on app vs website.  But very different from e.g. if an in-store purchase happens (linked with loyalty card, credit card, etc.)
    *   Aleksei: Need to remove (not add) to IG. Or update counts to indicate the same.
    *   Michael Kleber: Aha, that is great news.  K/V server can help with some things here. K/V server allow IGs to use realtime data–just before the auction. Keys are set in IG, and these keys don't have k-anon constraints. E.g. browser added to shoes IG, add a key for K/v lookup e.g. running-shoes-walmart-user-1238234967. Browser will reach out to k/v server. No logging is done here, cannot find anything out about the user!  But info can always be pushed/updated. The above key can be updated to indicate the purchase event, and IG can consume the data from k/v server to short-circuit auction.
    *   Aleksei: Is this request identifiable?
    *   Michael Kleber: No cookies in the request, but the keys can be as identifiable as you want. Runs in TEE.
    *   Aleksei: Do we have specs for k/v server?
    *   Michael Kleber: Yes, people can drop links for this. \
https://github.com/WICG/turtledove/blob/main/FLEDGE\_Key\_Value\_Server\_API.md
    *   Stan Belov: Other feature: You might to run multiple campaigns for audiences–e.g. audience extension–more brands. Also need to associate ads with IG ahead of time. Assuming these campaigns might change, how can we update ads?
    *   Aleksei: Won’t be easy to formulate groups with overlapping segments, needs to be thoughtfully decided in advance
    *   Stan Belov: 2 moments to ad ads to IGs: when IG is defined and then with daily update. Would that accommodate?
    *   Aleksei: Yes, that would work.
    *   Michael Kleber: Are you talking about a single Walmart IG is being used by multiple DSPs? From Walmart POV, just a single audience. From FLEDGE POV, this needs to be multiple IGs, maybe same membership. Maybe both are activated at the same time, but used by different DSPs. If multiple brands want to use the same DSP and leverage the single walmart IG, DSP logic will need to dictate the campaign identification etc. (puma vs nike)
    *   Brian May: Wondering how anyone knows if K/v store are being actually accessed by browsers? Any feedback loop?
    *   Martin Pál: Fairly early in development; can export aggregate stats. Don’t want to leak personal information, so want to be conservative.
    *   Brian May: Use-case: remove person from IG using K/v server. Want to verify that browser is getting this signal. Maybe consider counting that this updated value was read by ‘a’ browser.
    *   Martin Pál: So want to know whether a key has been queried or not?
    *   Brain May: Don’t know if my signal made it to the browser. So want the k/v updater to receive feedback.
    *   Michael Kleber: Are you more concerned about k/v server serving stale data? If IG participated in auction, generateBid() receives data from k/v server.
    *   Brian May: As an operator of k/v server, I don’t know the updated values reflect on the browser-side. System is like a black-box. Don’t want to leak privacy. Maybe consider ability to query whether the keys were queried.
    *   Michael Kleber: If the k/v server were keeping track of things like this, it’s no longer stateless. Not recording events, requests is an essential part of k/v server. We think reporting from browser can help mitigate some of this. When the IG participates in auctions, we can use reporting to indicate freshness of data retrieved from k/v server, e.g. use a timestamp. Would prefer reporting from browser as opposed to reporting from k/v server.
    *   Aleksei: Entity updating k/v server may be different from IG owner.
    *   Michael Kleber: Maybe FLEDGE should have a different notion of buyer and IG owner, and it can let both get aggregate information. Or maybe this can also part of the deal that allows DSP to buy using Walmart audience—where DSP shares data back.
    *   Alex Cone: Maybe make distinction in docs to describe audience extension implementation. Maybe Walmart coordinates with TTD to control levers for campaigns across multiple brands. (Managed service). How to allow multiple brands to use Walmart IG: maybe add to documentation–define the use-cases, what’s supported vs not.
    *   Aleksei: E.g. last month’s shoe buyers + visitors of cnn.com > we have IGs and IG owners, but no intersection.
    *   Michael Kleber: Feel that these discussions are more headed towards mapping adtech use-cases to FLEDGE world as opposed to changing the design of FLEDGE. Maybe can create ‘FLEDGE cookbook’ with multiple recipes (such as audience extension). Should identify additional use-cases/recipes.
