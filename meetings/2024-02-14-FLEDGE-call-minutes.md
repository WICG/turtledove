# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 4pm UTC (during winter)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Feb 14, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Brian May (dstillery)
3. Harshad Mane (PubMatic)
4. Zach Edwards (Victory Medium)
5. Sven May (Google Privacy Sandbox)
6. David Dabbs (Epsilon)
7. Xavier Capaldi (Optable)
8. Russ Hamilton (Google Chrome)
9. Sathish Manickam (Google Privacy Sandbox)
10. Laurentiu Badea (OpenX)
11. Paul Jensen (Google Privacy Sandbox)
12. Shankar Venkataraman (Jivox)
13. Tim Hsieh (Google Ad Manager)
14. Wendell Baker (Yahoo)
15. Fabian Höring (Criteo)
16. Isaac Foster (MSFT Ads)
17. Hillary Slattery (IAB Tech Lab)
18. Miguel Morales (IAB Tech Lab)
19. Patrick McCann (Raptive)
20. Pawel Ruchaj (Audigent)
21. Alexandre Nderagakura (Consultant)
22. Jacob Goldman (Google Ad Manager)
23. Alonso Velasquez (Google Privacy Sandbox)
24. Orr Bernstein (Google Privacy Sandbox)
25. Becky Hatley (Flashtalking)
26. Stan Belov (Google Ad Manager)
27. Taranjit Singh (Jivox)
28. Arthur Coleman (OnlineMatters)
29. Anthony Yam (Flashtalking)
30. Garrett McGrath (Magnite)
31. Garima Mimani(Google Privacy Sandbox)
32. Abishai Gray (Google Privacy Sandbox)
33. David Tam (Relay42)
34. Laszlo Szoboszlai (Audigent)
35. Andrew Pascoe (NextRoll)
36. Warren Fernandes (media.net)
37. Rickey Davis (Flashtalking)
38. Don Marti (Raptive)
39. Tamara Yaeger (BidSwitch)
40. Sarah Harris (Flashtalking)
41. Roni Gordon (Index Exchange)
42. Matt Davies (Criteo | Bidswitch)
43. Kevin Lee (Google Privacy Sandbox)
44. Amitava Ray Chaudhuri (Adobe)
45. Siddharth Purushotham (Jivox)
46. Alex Peckham (Flashtalking)
47. Laura Morinigo (Samsung)


## Note taker: Arthur Coleman


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WITCH: https://www.w3.org/community/wicg/ 


## Suggest agenda items here:



*   Isaac Foster:
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Tim Hsieh
    *   Updated proposal for Video and Native support https://github.com/WICG/turtledove/issues/265#issuecomment-1914779917 
*   David Dabbs
    *   Is `updateAdInterestGroups()` temporary? It is in the spec but unmentioned in the main explainer. Search [here](https://github.com/search?q=repo%3AWICG%2Fturtledove%20updateAdInterestGroups&type=code)
*   Arthur Coleman
    *    Start a subgroup to examine/respond to  the IAB Report on the Sandbox
        *   Heard today in the IAB webinar that Google is working to respond directly but I think that it will be better if the response/technical solution is done by the community on an issue-by-issue basis.  This will give it broader acceptance.  
*   David Dabbs
    *   Size on bids and macros \
https://github.com/WICG/turtledove/issues/312#issuecomment-1942991181


# Announcements


# Notes



*   Standard warning: Under auspices of W3C Community Group - all of the work we discuss here as contributions can make its way into the specs - so IP goes to the group.  Be aware and don’t share anything you need to protect.


## Tim Hsieh: Updated proposal for Video and Native support https://github.com/WICG/turtledove/issues/265#issuecomment-1914779917 



*   Overview of Issue Thread to this Point: https://github.com/WICG/turtledove/issues/265#issuecomment-1914779917 continues a conversation/proposals about how to handle aspects of native advertising using Protected Audience API. Previous proposal had privacy concerns. 
*   Open Discussion
    *   Tim Hsieh: 
        *   Goal of issue is to propose changes to Chrome API to support video and native
        *   We have a new proposal that could address the privacy concern from the last call. The idea is:
            *   Seller will provide a sellerRenderUrl that will return HTML/CSS/JS that contains the seller’s styling.
            *   Buyer will provide a buyerRenderUrl that will return the ad asset. 
            *   During joinAdInterestGroup(), Buyer will use buyerRenderUrl as the creative. 
            *   During runAdAuction(), the seller will pass sellerRenderUrl to the auction.
            *   During rendering, Chrome will pass sellerRenderUrl as the URN. This means that the seller will handle the rendering. Chrome could provide an API from within the rendering frame for the seller to retrieve buyer’s creatives.
            *   To prevent the combination of buyer and seller’s information from leaking out of the rendering frame, both the seller and buyer could declare a list of URLs that they would like to load during rendering. Seller and buyers could declare this allowlist as part of the HTTP response header on their renderUrls.
    *   Michael Kleber:
        *   Let me restate what I think you just said
        *   Two different timeframes you are talking about
        *   Separate from that there's the longer term future and quickly one way to render that is more private and that avoids the mixing of those two pools of information
        *   Fenced frames give you a way to have web page that has content from two different sources that don’t mix with each other
        *   The way we handle this - there is a barrier (fenced frames) that prevents information from the two parties from being mixed.  This prevents information from ‘outside the page from being able to exchange information from an (ad) unit on the page.  Those are the existing fundamental protections.
        *   What you are proposing is a new additional way to have content from multiple different parties to coexist on the same page without the ability to combine them outside of the browser.
        *   In a single frame, we can let content from two different domains load and interact with each other in how things are rendered on the screen, but a combination of those two pools of information cannot leak to the network because each party that is going to put info into the frame right up front and have to declare all the info they are putting in in advance - so no contextual information gets mixed.
        *   Is that a fair assessment?
    *   Tim Hsieh: 
        *   That is exactly correct
    *   Shankar Venkataraman: 
        *   How can this work with VAST? Especially VAST4 and SIMID (VAST 4.1+) 
        *   Everything on the context for the video player is through a payload which provide the context at the time the video is to be rendered
        *   If we have to pre-declare and the seller/publisher cannot see the payload at load time, that breaks how VAST works.
        *   Please explain further 
    *   Tim Hsieh:
        *   My understanding is that the entity that is hosting the VAST would know the list of ad assets. For example, if an ad server is hosting the video ad, the ad server would know the assets for the VAST. When returning VAST, the same entity could also declare and return a list of allowed URLs.
    *   Shankar Venkataraman 
        *   The VAST payload is essentially an XML document, and we have a tracker that is fired when the video is served
        *   The SIMID protocol, can associate additional items like HTML fragments in addition to the video output to be served.  Where is the impression tracker to be fired? How do we record progressive events?  All of that stuff goes into the response today. 
    *   Stan Belov:
        *   The buyers’ rendering server or some rendering endpoint constructs the VAST document and the sellers’ HTML.frame knows how to fetch assets, including all the UX functionality to render that information
        *   The seller is responsible, in this case, to fire trackers in response to various VAST events
        *   Is the question how the buyer endpoint returns the VAST document?  Is the problem that what you are calling out is the lack of contextual information that would make it problematic for a buyer to construct a VAST document and, in the case of the DSP… (they have already received the VAST documents from the publisher?)
    *   Shankar Venkataraman :
        *   If I understand Tim's proposal, the buyer's rendering server or some rendering endpoint returns a VAST document, right? So it constructs that VAST document.
        *   And the seller’s HTML knows how to play that, including fetching assets, including all the UX functionality of play. For example, including firing tracker.  It is still on the seller to fire trackers in reaction to various VAST events.
        *   In this new proposal, are we changing the way the VAST document is constructed?
    *   Stan Belov:
        *   The renderURL can be something that the DSP returns that is a VAST record tag that points to that ad server
        *   One difference from the current mechanic is that there would be limited access to the contextual information to construct the VAST document
        *   Where this approach to the renderurl is supported, you won’t be able to inject contextual information into that VAST request
        *   Longer term with fenced frames, this VAST would need to be constructed without rich contextual information
    *   Shankar Venkataraman :
        *   In the case of VAST, what happens in the case of brand safety partners? The current model is to take the VAST tag from the ad server, and wrap it in a VPAID tag to accomplish the needs of brand safety. Ie. the DSP calls this wrapped tag and then the brand safety partner redirects to the ad server tag if the page is brand safe. 
    *   Tim Hsieh:
        *   What information does the brand safety provider need? 
    *   Michael Kleber:
        *   Seems we have gone beyond the topic into brand safety.  The original question was about combining information from the buyer and the seller in a PA auction.  Brand safety involves trying to solve a very different problem, where we are also combining information about the publisher.
        *   Top line reaction is: we definitely should spend time discussing better ways to handle brand safety in the context of protected audience options
        *   My instinct how to do this that is compatible is for brand safety stuff to happen in some sense inside the auction rather than in the rendering environment
        *   Having the brand safety control inside the protected auction is a better match for brand safety goals than happening in the rendering environment
        *   Historically, there was no way to separate these two (browser reporting and browser rendering) from each other.  It would be great if we can continue to break these two apart - the auction environment vs the rendering environment.  Separating those two out is a fundamental design goal of the Protected Audience API, for a number of reasons/user cases.  And brand safety is exactly one of the use cases that splitting those two out was designed to deal with.
        *   Second reaction: Tim I feel like, as a person who worked on Chrome, I am in an extremely bad place to understand whether the thing that you are proposing is actually a practical solution for the way that SSPs and DSPs cooperate to get something to show on the screen
        *   Putting aside the privacy question, I don’t have an idea of how good a job this would do to enable the goal of finding some way for SSPs and DSPs to work together.
        *   There is a relatively new Chrome-built demo we put up last week that is  to show how DSPs and SSPs can cooperate in producing a VAST output using POST messages and iFrames
        *   (https://privacy-sandbox-demos.dev/docs/demos/instream-video-ad-multi-seller)
        *   It seems like the big question is: how much of what SSPs and DSPs want to do in cooperation is feasible for them in the current environment
    *   Stan Belov:
        *   I agree that in the longer term it would be possible to enforce brand safety by incorporating into the bidding logic.
        *   There are also issues of how the ecosystem functions in the short-term 
        *   Can we use temporary iframe-based mechanisms in the meantime?
        *   We need to design for the long-term but make sure we find a solution that works today
    *   Roni Gordon:
        *   From the proposal there are three things happening here:
            *   You have the buyer’s VAST document, to use the most common example
            *   You have the video player as the rendering engine - might be the publisher or the seller’s player, conceivably could even be the buyer but generally not the case
            *   You have the events that need to be exfiltrated from the fenced frame to know what is going on in the video player.
            *   Today: sellers add VAST XML elements that they need to be notified about (i.e. impression element, video progress, etc.)
            *   Buyers embed whatever that that is stitched together for what they want to know and track
            *   THEN: all that is fed into the video player according to the spec
            *   In your (Stan’s) example, what I am trying to understand is:
                *   The buyer side is a black box by the endpoint  - that’s easy
                *   Is the video player in your example in the fenced frame?
    *   Tim Hsieh:
        *   In this example, the video player is in the fenced frame. SSPs would be providing the video player.  
        *   Not saying that is the only approach - publishers can also own the player.
        *   But in this case it is assumed the VAST player belongs to the SSP 
    *   Roni Gordon:
        *   Where is the opportunity to insert VAST tracking elements?
        *   Where does this happen in the proposal?
    *   Tim Hsieh:
        *   Seller could call registerAdBeacon() from reportResult().
        *   The ad video player will call reportEvent() on the events. 
    *   Roni Gordon:
        *   That is true for the buyer as well?
    *   Tim Hsieh:
        *   Yes
        *   Also, it is possible to support 3rd party trackers from measurement partners as well. The ad player would need to use the Fenced Frame Reporting API’s destinationUrl mechanism to send the URL.
    *   Roni Gordon:
        *   You recognize no video player does that.  All video players as per the spec these VAST events
        *   These VAST events need to be completely recast in the world of fenced frames and somehow the trackers have some manifest, an “allow list” ?
        *   VAST provides 100 tracking events, if not more
            *   Let’s stipulate_ attested parties_ to make life easier
            *   But all parties need to be notified of the VAST events
            *   But we have to go beyond that it tends to be very tricky
        *   How this works seems exceedingly problematic. 
    *   Tim Hsieh:
        *   To be clear, are we talking about how the buyer or seller receives their events, correct?
    *   Roni Gordon:
        *   Yes
        *   In the simple case, you put a VAST impression element - the buyer and the seller in the auction put one each, other third-party puts one potentially
        *   They both need to know an impression event fired, at the very least
        *   So how would that work in this environment?  
            *   The buyer gets to put theirs in because they are constructing the VAST document.  
            *   How does the seller inject theirs into the fenced frame?  How does it get from the seller somewhere into this fenced frame?
    *   Tim Hsieh:
        *   The idea is to use the Fenced Frame API to do exactly that.
        *   I agree this is a deviation from the way it works today. However, the Protected Audience API is a deviation from how it works today.
    *   Roni Gordon:
        *   Understand that, but trying to understand how to specify that
        *   Where do I inject it?
        *   Today I pass in a URL element and say “call this on an impression element” 
        *   I can pass it another thing that says “fire a report event”
        *   But how am I getting this into the fenced frame?  
        *   Where does the seller say this is the impression?
    *   Donato Borrello:
        *   They can trigger anything 
        *   They would be one-to-one with the seller
    *   Roni Gordon:
        *   The player is generic.  It is not specific to this creative or auction 
    *   Tim Hsieh:
        *   In the current proposal, the seller could call registerAdBeacon() to register a list of URLs and types that they would like to receive.
        *   The list of event types should be the same types that the ad video player defined. 
        *   There needs to be an agreement between the ad video player and the seller/buyer.
    *   Roni Gordon:
        *   Let’s pretend I have a VAST player, one that I use over the entire Internet
        *   The impression callback is different from the request.  
        *   Where do I pass in the URL that is not hard-coded in the player?  Player doesn’t do anything - just operates on a VAST document
    *   Tim Hsieh:
        *   You (buyer or seller) can register the URL within reportResult() and reportWin(). 
    *   Roni Gordon:
        *   You want me to register a URL that gets to fire on an as yet undefined event that doesn’t exist?
        *   It's not clicked and it’s not rendered.  
        *   The proposal is the systems need to replicate, define the full set of VAST events?
    *   Tim Hsieh:
        *   The ad video player should say a list of events that it will call reportEvent() on. The actual URL that will be called is provided by the buyer and the seller in reportWin() and reportResult(). 
    *   Michael Kleber:
        *   I don’t think we are going to figure out how to reimplement VAST in the next 20 minutes
        *   What Tim is proposing may not be well-aligned exactly with VAST, but the pre-ordained list of URLs may not be aligned/compatible with how it is done today
        *   BUT: I am not sure that should be part of the goal.  The predefined list of URLS is on the rendering side,  and all the event reporting - “What has happened?” -  is on the reporting side. They are two separate issues.
        *   This points to a concern with Tim’s proposal where the reporting events that need to be called might be influenced by elements that come from the other domain. So now we are open to the risk of mixing content from multiple domains together.   
        *   We may need to think about how to make VAST work with the new proposal - this is a longer-term item to discuss.  We can pick this up in a later meeting.  
        *   We can talk about the demo for how we think it can work today, but this is a longer term issue and needs significant thinking on the part of lots of people.
    *   Isaac Foster:
        *   Michael Kleber - you mentioned brand safety in this new world, there is the ability to have cross-domain information in an opaque way in the auction and keeping rendering separate
        *   For the long run - in the general future when we have to use fenced frames with enforcement, what would your thoughts be about combining rendering information and logic from two or more parties in the fenced frame specifically?
    *   Michael Kleber:
        *   That is a variant of fenced frames that we already have proposal out for
        *   There already is a proposal that describes a way a fenced frame can load some stuff, then renounce its network access - this is what the URL is going to be - then it can read from shared storage - so we already have something that moves in the direction you are talking about.
            *   https://github.com/WICG/fenced-frame/blob/master/explainer/use\_cases.md#read-only
        *   May not be a fit for what you are thinking but a baseline for moving forward
    *   Isaac Foster:
        *   Just responding to some of the elements in the prior discussion - good to hear and give the floor back
    *   Stan Belov:
        *   Potential proposal: 
        *   What if:
            *   We have someone from this group to verify how it works today to the group, including Chrome folks
            *   Then maybe ad manager people could present a more in-depth proposal for how these functionalities should work
    *   Tim Hsieh:
        *   Great idea. I can prepare a deck to better explain this. 
    *   Brian May:
        *   Sounds to me like the general model is : we provide lots of contextual information to people who want to make a bid
        *   They provide some subset of information back to the browser which gets incorporated into the auction
        *   Once in browser, we don’t want information most critical to doing business to be visible to the other party
        *   We will take the results of the auction, plus some other information needed to put that data into a context and supply them into an isolated context so that information does not leak and then makes it into a frame to be rendered
        *   Correct? (Michael Kleber confirms correct understanding)
        *   If so, in this isolated context::
            *   How do we prevent information from one context being influenced or influencing information from another context?
            *   If we take info from two sources and combine it in a fenced frame with the belief that a fenced frame prevents leakage, is there any way to prevent what happens in that fenced frame from being modified based upon who is inputting the information into it?
    *   Michael Kleber:
        *   Current mechanic
            *   Render a k-anonymous render URL in a fenced frame from only one domain.  Is just one party’s information
            *   In the auction there are things that impact multiple domain’s information
            *   But the render URL itself that is put into the iFrame (or fenced frame) is from a single domain’s worth of information
        *   The proof of concept VAST demo that we published last week doesn’t break that model
        *   The actual selection of the ads render URL is based on one domain’s information
        *   All stuff about combining information about user across multiple domains is much trickier and you have to prevent information from leaving the browser otherwise you risk tracking, and that is hard
        *   Brian - that is what your concern is about - you are worrying about leakage
        *   And I agree YES - which I why I am hesitant to embrace Tim’s proposal because there is still egress through the reporting channel
    *   Brian May:
        *   I would go beyond behaviors that are just reported.
        *   I would also include any kind of behavior that happens in the fenced frame that could be detectable outside the fenced frame
    *   Michael Kleber:
        *   Agreed. Easiest way is to not mix information from different domains at all - whether we can live with that long term is still a question for debate.
    *   Stan Belov:
        *   You mention an information channel for reporting where information might leak: isn’t that the existing channel today in the FLEDGE API.
        *   Today that allows the combination of publisher information with some information about the ad
        *   Tomorrow, when fenced frames and aggregate reporting are implemented, that information channel is closed
    *   Michael Kleber:
        *   One answer is don’t mix two parties’ data
        *   One other answer is once you combine information, the only way to extract it is via aggregate reporting, aggregation server, and TTE to avoid cross-site tracking of users.
        *   We have a lot of work that will involve using those two elements to prevent leakage
          
## VAST and the DEMO: Alex Cone you want to discuss?

*   Alex Cone:
    *   Group: Not enough time.  Awfully short
*   Michael Kleber:
    *   Maybe you can give a brief teaser for what the thing is, let them go look at it, and then talk about in a future call
*   Alex Cone:
    *   Hearing from the ecosystem the core problems we need to solve with VAST we heard:
        *   Re iFrames - we need to get seller events fired by player in addition to buyer VAST events
        *   We need to get that information to the player
        *   We need some way of joining events in the reporting system
    *   What we’ve come up with:
        *   https://privacy-sandbox-demos.dev/docs/demos/instream-video-ad-multi-seller/
        *   A way to use macro insertions in the buyer’s render URL from sellers - whether component or top-level - to have the sellers VAST elements depopulated as a URL in the render uRL at render time so that the buyer’s creative can take that endpoint.
        *   Couple of ways to do this:
            *   We fetch information and construct the wrapper on the page with the iFrame before messaging out or
            *   Pinging that endpoint from the winning seller and having the seller actually construct the VAST wrapper around the buyer’s VAST document and sending that back to the buyer’s iFrame which sends a postMessage to code on the publisher page 
    *   Goal of demo was not to dictate how the ecosystem should work
    *   Was meant to give idea of how VAST could be supported now before fenced frames are implemented
    *   On demo site - but together by Kevin Lee (Yea thank you @Kevin.  Great work!)
    *   Just to get discussion started
    *   Important to come up with some way to make VAST work most efficiently before fenced frames even as we have discussions for fenced frames
*   Kevin Lee:
    *   There are multiple ways to handle VAST
    *   Who handles the VAST processing -we have chosen one way - by seller - but can be done by the buyer - up to the ecosystem to determine who is going to be doing the VAST processing.
*   Michael Kleber:
    *   Two halves to this discussion:
        *   What is technically possible.  Browser folks happy to engage to make more things possible
        *   What are all the needs of the ecosystem to make this happen?  How should SSPs, DSPs and publishers work to make this work in the easiest possible way in the real world?
        *   That is not something Chrome team can answer - that is more about roles in the market and needs input from both Chrome team as well as adATech folks to make work
        *   We’ll be working on this for a while
