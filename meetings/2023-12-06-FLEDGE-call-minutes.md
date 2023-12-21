# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 4pm UTC (during winter)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Dec 6, 2023


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Brian May (dstillery)
3. Itay sharfi (Google Privacy Sandbox)
4. Tim Hsieh (Google Ad Manager)
5. Lixing Dong (Google Ad Manager)
6. Russ Hamilton (Google Privacy Sandbox)
7. Orr Bernstein (Google Privacy Sandbox)
8. Paul Jensen (Google Privacy Sandbox)
9. Matt Menke (Google Chrome)
10. Nick Llerandi (Triplelift)
11. Kevin Lee (Google Privacy Sandbox)
12. Youssef Bourouphael (Google Privacy Sandbox)
13. Joel Meyer (OpenX)
14. Laurentiu Badea (OpenX)
15. Brian Schmidt (OpenX)
16. Isaac Foster (MSFT Ads)
17. Roni Gordon (Index Exchange)
18. David Dabbs (Epsilon)
19. Harshad Mane (PubMatic)
20. Sid Sahoo (Google Privacy Sandbox)
21. Caleb Raitto (Google Chrome)
22. Garrett McGrath (Magnite)
23. Abishai Gray (Google Chrome)
24. Marco Lugo (NextRoll)
25. Tamara Yaeger (BidSwitch)
26. Phil Lee (Google Privacy Sandbox)
27. Matt Davies (Bidswitch)
28. Stan Belov (Google Ads)
29. Jeff Wieland (Magnite)
30. David Tam (Relay42)


## Note taker: Isaac Foster


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


## Suggest agenda items here:



*   Isaac:
    *   Sec-cookie-label for no label: https://github.com/GoogleChromeLabs/privacy-sandbox-dev-support/issues/153 
    *   Buyer/Seller Reporting Questions: https://github.com/WICG/turtledove/issues/682#issuecomment-1710965068
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussi  loop on and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Roni Gordon
    *   Sensitive signals - https://github.com/WICG/turtledove/issues/824
*   Tim Hsieh	
    *   Seller Rendering Server (for native and video) - https://github.com/WICG/turtledove/issues/265
*   Itay sharfi
    *   New WICG to focus on trusted servers. Starting Dec 13
    *   See: https://github.com/WICG/protected-auction-services-discussion/issues/27 
*   Harshad Mane
    *   https://github.com/WICG/turtledove/issues/937 Need of a debug tool in Chrome for Protected Audience like Professor Prebid Chrome Extension developed by Prebid community 
*   David Dabbs
    *   Non-official chrome [testing labels](https://developers.google.com/privacy-sandbox/setup/web/chrome-facilitated-testing) seen in the wild (e.g. “preperiod”). Are these somehow related to this Chrome code (thanks to Przemysław Iwańczak for this src reference)**:** \
https://github.com/chromium/chromium/blob/c0fcffe3bbcf87e4946dae228bd62d740568a429/components/safe\_browsing/core/browser/user\_population.cc#L91 \
`bool is_preperiod = group.find("Preperiod") != std::string::npos;`


# Notes

Isaac = note taker

MK: Itay, new WICG for trusted servers

https://github.com/WICG/protected-auction-services-discussion/issues


## New WICG meeting to focus on trusted servers. Starting Dec 13

Itay: Hi! Starting the WICG group for TEE based, December 13th at 9 AM PST/Noon EST, github and other things are there.

Cross Talk: verify TZ, it’s the hour right after this one but bi-weekly currently

DD: new WICG meeting, does that mean new repo and such

Itay: yes

DD: cool, I’ll start watching

MK: calendar invite, good question, hmm…twixy, need to get chairs to do that, problematic from admin perspective

BMay: Charlie (Harrison) did something kewl (for ARA), ask him about that.

MK: would be great I’ll ask.

Phillip Lee: I’m working on this new group, I’ll be sending out stuff, copying as much process as possible from here.  

MK: issue of where this will all be, let’s put a pointer into issue 88 (main this thing issue) so these folks can easily follow.

Full logistics details, added after the fact: https://github.com/WICG/protected-auction-services-discussion/issues/27

MK: let’s move on, Tim, issue 265


## Tim Hsieh: Seller Rendering Server (for native and video) - https://github.com/WICG/turtledove/issues/265 

Tim (TH): Currently Protected Audience API supports HTML ads. The question is how do we support native and videos? Challenge is that the publisher or the seller needs to provide additional code within the rendering frame. One proposal is to use a Seller Rendering Server. The renderUrl consists of both Seller Rendering Server as the base URL and buyer ad asset server URL as a query parameter. During rendering, the browser calls the Seller Rendering Server. Seller Rendering Server calls buyers ad asset server to fetch ad assets. Seller Rendering Server could construct the final ad asset. This could be done today if the buyer constructs the renderUrl by combining Seller Rendering Server with Buyer Ad Asset Server. The request to Chrome is have Chrome combine the renderUrl.

MK: Should have done this with Shivani around. I will give some initial reactions to the question of “how can we get the buyer and seller involved in the rendering process instead of just the buyer”. Need right privacy properties, key part of way PAA works is stuff inside of rendered creative shouldn’t know users identity, not even on the page being rendered, that needs preservation, so how can we let the seller influence it with the browser still being sure the “seller contribution to the rendering” doesn’t include user info.

TH: yes makes sense, seller contribution shouldn’t contain user ID or some identifier, what I’m trying to understand is what the concern is if the seller can provide arbitrary code?

MK: yes we have K-Anon protection, that is relatively weak, we’d actually like to provide the user with stronger protection in the future than we have today, today with rendering in iframes we do allow a join, which does allow an over time profile-building, fingerprinting risk, etc…FencedFrame approach will rid us of that issue…Event Level Reporting is the other vector of leaks for joins, that is only available to parties who attested, we’d need to extend the attestation to FencedFrames and really any provider of a pixel in the thing rendered, we’d increase the number of parties involved and attesting, what happens if one party is not attested, etc. So, for all those reasons, do want to solve the “cooperation issue” for rendering, but need to do so w/o letting arbitrary info flow from “more private” to “less private” via rendering.

TH: I need to digest.

Isaac Foster: Need to reread proposal, but is the only thing you're aksing for being able to send the buyer's renderURL to the seller's rendering server, or are you asking for more freedom of information than that, like info from seller context?

TH: There are two proposals.  In one, as the seller we provide a seller render URL, and the buyer provides their own, and Chrome combines the two into a single render URL that must be k-anon.

Isaac: So today in Native there are more degrees of freedom, but in Video we have a video asset win, achieves k-anonymity, then the seller has an endpoint specific to that creative?  or seller-endpoint/asset=buyer\_render\_server.html that then decorates it?  or unique per asset?

TH: Seller would be providing seller.ssp.com/render\_video and buyer provides buyer.com/video1, and chrome combines the two into one URL. URL provided by the seller should be common across all videos.  

Isaac: Seller auction config has a single render URL, Chrome would recognize that it's not just doing k-anon on each buyer URL but rather on the concatenation of the two strings, and then the SSP would be able to download and decorate, and get no new information outside the buyer URL?

TH: I don't think there would be any macros. Buyer provides one URL per ad, combination by Chrome.

Harshad: Seller would create a template of overall layout — so would the seller need different code for every publisher?

TH: If the ad slot is native, then the URL to Seller Rendering Server would contain a template ID. 

MK: I just heard two different things. In response to Isaac I heard “seller has one renderURL for all of native one for video”, but in response to Harshad I heard “different per publisher site”, which is way different from a K-anon perspective…just to be clear, passing the K-anon threshold on every pub site that it has to render on seems way worse K-wise. 

TH: When I was responding to Isaac, I was responding to the video. In the video case,  there’d be only one (or a small number) of Seller Rendering Server URLs for videos. In the native case, there could be one for each template. I agree that it would be harder clear K threshold.

MK: native is the more interesting one here, K may not be the right tool here given the per-pub needs. The essence of what we need is some way to know that the seller contribution is not user specific but site specific.

BMay: K requirement + seller side component will, by design, be a detriment to any small sellers. We should exclude sell side K components.

Roni: what about the cases where the seller isn’t rendering, for instance video (out stream?).(Adding link to Index’s PBJS docs re: [outstrream video](https://docs.prebid.org/dev-docs/bidders/ix.html#indexs-outstream-video-player) after discussion for reference with publisher’s rendering capabilities)

MK: yes agree need to figure that out, multi SSP stuff

Roni: native is different, video has lots of flexibility

Paul Jensen: agree with separating out native and video, very different needs. Fenced Frame read only mode does allow for some of what native wants, believe the way it works is it loads stuff from network related to creative, and then the FF shifts into read only mode where it can read info but no more network requests are allowed.

MK: oh yeah I like that, but concern that something “font loading” won’t work.

Paul/MK Cross Talk: might work, well maybe, eh I worry and don’t feel safe

PJ: not sure exactly how this works

MK: something something shared storage, could extend that.

Stan Belov: what’s the risk you’re seeing exactly?

MK: fingerprinting risk; 

SB: isn’t seller attested?

MK: yes, but once information is in the rendering URL, it's not just the seller that sees it, it's every server involved in rendering in any way, even a single pixel inside the creative.

SB: isn’t that true for buyers too?

MK: only matters if you’re combining info across contexts.

Matt Menke: if auction config had a seller URL, could load that seller URL in the FF, after network access cut off pull down the buyer URL, then you don’t have access to x-site and network at same time.

MK: interesting, need to chew.

PJ: yeah this gets tricky, think fonts, lots of stuff to load.

MK: jah interesting, must to think.

Lixing, work with Matt/Tim: what Matt proposed is an option…some sub-optimalities there

TH: two options proposed, one is seller rendering server, one is more client side, maybe seller provides generateHTML option, would that client side solution allow the mixing?

MK: my read is it would allow that…

TH: I will think more

BMay: clarify, are there two URLs and network calls, one from sell one from buy?

MK: yes

BMay: so back channel with timestamps.

MK: for sure.

Cross Talk: timing attacks are hard to deal with, maybe we just don’t allow it

BMay: Preloading assets is very expensive, suggesting that limiting ourselves to a single call as much as possible would be best.

Isaac: Regarding rendering and fonts and native, two questions.  (1) Where does variation show up in Native?  Is it just based on top-level site and styling variation?  can it be limited to some kind of browser signal where the browser has more control over what gets passed through?  (2) On fonts, I wonder if the user agent having a bit of advertising layer on top of what it already does could pre-fetch some sorts of common things needed for rendering

MK: I haven’t thought through the full set of risks.  Needs more thought.  Needs more design work on the privacy side and coordination side between multiple SSPs.  .

Tims issue, 265, been around a while, let’s keep working on this there, needs more design, two solutions are presented in a lot of detail, might need to take a step back from those implementation and think at a larger scope, security, data flows, etc, but it’s a good place to start and ideate.

MK: 5 minutes left

Isaac: The Chrome label for no-group.  Is this the right place to talk about that? Namely https://github.com/GoogleChromeLabs/privacy-sandbox-dev-support/issues/153 

MK: Right people not in the room.  Initial set of population labels did not include a none-of-the-above.  Maybe can be added later.

PJ: I talked to Josh about this and will push him to answer on bug issue.

MK: See you Dec 13th.

BM: End of year meetings?

MK: Will we meet Dec 20th?  We should not meet on 27th.  Tentatively will meet Dec 20th.  Note that protected server meeting will happen after this meeting next week, the 13th.
