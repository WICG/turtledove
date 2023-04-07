# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday March 29, 2023


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Russ Hamilton (Google Chrome)
3. Brian May (Dstillery)
4. Sven May (Google Privacy Sandbox)
5. Dominic Farolino (Google Chrome)
6. Paul Jensen (Google Chrome)
7. Orr Bernstein (Google Chrome)
8. Fabian Höring (Criteo)
9. Michal Kalisz (RTB House)
10. Risako Hamano (Yahoo Japan)
11. Kevin Lee (Google Chrome)
12. Matt Menke (Google Chrome)
13. Andrew Aikens (TripleLift)
14. Joel Meyer (OpenX)
15. Marco Lugo (NextRoll)
16. Maciej Zdanowicz (RTB House)
17. David Dabbs (Epsilon)
18. Shivani Sharma (Google Chrome)
19. Andrew Pascoe (NextRoll)
20. Alex Cone (Coir)
21. Shawn Hong (Google DV360)


## Note taker: Orr Bernstein


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here]

Held-over agenda items:



*   Fenced frames: Migrating from URNs to FencedFrameConfigs. Summary doc: [FencedFrameConfig Transition](https://docs.google.com/document/d/1a6DFN3d4EEuWFM8lnk7uCeRmPFwniTGirilG33DjOM8/edit#)
*   Creative Macros proposal (Shawn Hong) https://github.com/WICG/turtledove/issues/477 

New agenda items: [please suggest here]



*   Regression in DevTool Integration with FLEDGE API 
https://github.com/WICG/turtledove/issues/499 


# Notes



*   Two items on the agenda. Starting with FencedFrames, with Dominic Farolino.


## Fenced frames: Migrating from URNs to FencedFrameConfigs. Summary doc: [FencedFrameConfig Transition](https://docs.google.com/document/d/1a6DFN3d4EEuWFM8lnk7uCeRmPFwniTGirilG33DjOM8/edit#)



*   Dominic Farolino
    *   Fenced Frames. Sharing the document linked above [here](https://docs.google.com/document/d/1a6DFN3d4EEuWFM8lnk7uCeRmPFwniTGirilG33DjOM8/edit#)
    *   If you’re using FencedFrames to render an ad, you’re using the src attribute and setting it to the URN. We’re removing the src attribute from FencedFrame element in M114, towards the end of May.
        *   New way will be to set the config attribute to a FencedFrameConfig object. You’ll get a FencedFrameConfig object by running RunAdAuction.
        *   Until 3PCD, we’re defaulting to URNs. However, if you pass {resolveToConfig: true} to runAdAuction, the returned promise will return a FencedFrameConfig.
        *   While we’re rolling this out incrementally, passing {resolveToConfig: true} will still return the URN.
        *   By M114, the default behavior will change to return the FencedFrameConfig object.
    *   If you don’t set dimensions in the dimensions in the config are used.
    *   Making URNs a string is misleading; makes it seem like the lifetime of this is longer than it is. Navigating to a URN outside of the top-level frame in which it’s created will fail. This clarified that the returned promise is only valid while in the same top-level frame.
    *   This also gives us a centralized interface for the context. The FencedFrameConfig object allows you to write the event-level IDs.
*   Brian May
    *   The reasons for doing this make sense, are understandable.
    *   Curious about FencedFrameConfig, how that gets defined/modified over time. Who owns that.
    *   Dominic: Not replicable - can hold onto it the same as you would a string. Its lifetime is the same as a normal garbage-collected JS object. From an ads perspective, it’s a frozen representation of the ad identifier.
    *   Brian: to clarify, some standardization of what’s passed in there. How are the standards going to be managed over time?
    *   Dominic: All taking place in the FencedFrameRepository. Right now, width and height are the only attributes that are getting passed in.
    *   Shivani: Unified view of the ad when talking about FLEDGE. Config parameters that are standardized. Any change to the config would map to a change to the FLEDGE standard, would need to be considered for privacy implications. How do we evolve the parameters over time? By changing the flow.
    *   Brian: Multiple constituencies may want to modify the config. Is there a process for doing so?
    *   Shivani: Through the API, through FLEDGE. Unified flow of what’s coming from FLEDGE to FencedFrame.
    *   Dominic: Each user of the FencedFrame will be responsible for filling out parts of the config.
*   Michael Kleber
    *   A constraint in FLEDGE now is that you can’t learn anything new about who the winner of the auction was. The size must be specified in the FLEDGE auction config, so that it’s conveyed to the FencedFrame config, and so it’s safe to expose those attributes of the config because there’s no new information there.
*   David Dobbs
    *   Often the case today, OpenRTB signaling, you might get a bag where you can put a lot of ad sizes in there. But that’s not possible here; if you’re picking a FLEDGE auction, you have to know the size ahead of time.
    *   Michael Kleber: Yes, that’s right. It’s part of the FLEDGE design.
    *   David: Can’t the size be specified as either the fixed size (pixels) or some percentage? Does this cover both? Is the size setting already fixed?
    *   Michael: Today, FLEDGE supports only fixed width/height in pixels.
    *   Russ: Actually, can support percent sizes. Two ways to specify sizes: pixels, or percent of viewport width or height.
    *   David: The width/height seem like numbers in the IDL, so not sure if it would support both types.
    *   Dominic: Should be a string. We’ll follow up on that.
*   Michal Kalisz
    *   Iframe would be supported until 2026. If we want to create iframe as a publisher, we just specify the iframe in the config?
    *   Dominic: Yes, even simpler. If you don’t pass anything in the default to config, you’ll continue to get URNs, and you can still use those URNs with iframes.
    *   Michal: What about the ad components? The one retrieved by RunAdAuction inside FencedFrame or iframe?
    *   Dominic: There’s another way to get these. It’s in the explainer.
    *   Shivani: getNestedConfigs. The exact name is in the explainer. (https://source.chromium.org/chromium/chromium/src/+/main:third_party/blink/renderer/core/html/fenced_frame/fence.idl;l=13?q=fence.idl&ss=chromium)
*   David Dabbs
    *   The whole behavior is triggered by the top-level seller, right? The config goes in there, and everything magically works? No dependency on particular buyer to be aware of the configuration?
    *   Michael Kleber: Exactly right. Change is completely invisible to buyer. The only change necessary by people using FLEDGE is that the rendering step has to happen a bit differently. Actually, because of the ad component change, the bidder that wants to bid has to write their bid differently based on which environment it is.
    *   Shivani: It’s up to the buyer.
    *   Michael: If you are a frame ad that’s going to render component ads inside of it, the SSP that ran the auction knows whether it intends to render in a FencedFrame or an iframe. But how does the buyer know which one it needs to render in?
    *   Shivani: The buyer could render as if it’s in a FencedFrame, even if it’s an iframe.
    *   Michael: Could use either way to render in either environment?
    *   Shivani: Yes.
    *   Michael: Or could do something hacky to figure out which environment you’re being rendered in?
    *   David: Do component sellers have any agency in that? It’s the top-level seller that’s deciding.
    *   Michael: Yes.
    *   David: Long-story short, anything you can do to clarify this would be useful.
    *   Paul Jensen: What’s the lifetime of the config then?
*   Brian May
    *   If we provide a mechanism to use the legacy strategy, they’ll keep doing so indefinitely.
    *   Michael: The difference between rendering in iframes or FencedFrames is the same as the difference between asking for a URN or asking for a FencedFrameConfig. The migration for both of these will look the same.
    *   Brian: We need to communicate the migration clearly.
    *   Michael: Mostly a top-level SSP responsibility, since they’re the ones doing the rendering step.


## Creative Macros proposal (Shawn Hong) https://github.com/WICG/turtledove/issues/477 



*   Michael Kleber: Second item on the agenda - issue #477. But I don’t see Shawn Hong on the call. Actually, Shawn just joined.
*   Shawn Hong
    *   PM working for DV360 at Google.
    *   Today, advertisers, when they upload creatives, the creatives contain macros. Some macros contain campaigns, or info about the campaign itself.
    *   Third-party tracking information about the DSP also tracks ad events.
    *   FLEDGE can replace things available at rendering time, but loses context needed for tracking ad events.
    *   We put together a proposal for a replacement of the contextual macros while preserving privacy.
*   David Dobbs
    *   Similar to today, when a buyer returns a bid, needs to return a series of macros, and they insert those into the creative. Is that what you’re talking about?
    *   Shawn: Yes, talking about the macros inserted as part of the creative markup.
*   Paul Jensen
    *   Couple of clarifying questions.
    *   There’s a RegisterAdMacro function available in ReportWin. Looked similar to RegisterAdBeacon. Basically inserting information available in ReportWin.
    *   Later in FencedFrame, there’s - that makes that information available.
    *   Not sure how it’s different from RegisterAdBeacon.
    *   Requires less coordination between DSP and third-party tracking vendors. Reducing that complexity is the core purpose of this proposal.
*   David Dobbs 
    *   This is intended to be used while we have event-level reporting, over the next four years?
    *   Michael: The goal should be to think now about how to support these needs in a world in which we have aggregate reporting. The timeline for that is because we realize it’s going to take some time to figure that out. We should limit work/investment on event-level reporting solutions.
*   Michael Kleber
    *   Still trying to figure out this proposal
    *   It seems to be cognizant of the FLEDGE principle of keeping the two types of information separate - contextual information with  
    *   This seems risky, in that it proposes macro values being registered somewhere inside the auction, and it being possible to replace those values at rendering time inside the FencedFrame?
    *   Shivani: Not in the FencedFrame.
    *   Michael: In the example, script tag with macro values substituted into it. This is not what you’re trying to do today, but rather what you’re trying to figure out how to accomplish the same need in the future?
    *   Shawn: This script is part of the tracker. Could be a script or a pixel. Rendered along with the creative
    *   Michael: Any mechanism like that would automatically provide a way for the rendered creative to be a function of all the context information available at auction time, not just the kAnonymous winning ad that it’s supposed to be based on. Seems that this doesn’t have the same privacy properties.
    *   Shawn: Will follow up with that. The intent is that this is only for info that is not specific to the user
    *   Michael: Can’t see how to enforce that on the browser, seems to break the privacy models.
*   Alex Cone
    *   In a lot of macro cases for creatives, done for some sort of reporting. That’s the outcome that’s trying to be preserved here. The outcome of a dimension in reporting that’s based on the publishers context. Seems difficult to solve for, but seems possible to solve that while meeting privacy guarantees.
    *   Michael: Agree with you, and the RegisterAdBeacon is supposed to be that. If there’s a macro substitution mechanism that makes RegisterAdBeacon easier or easier to adapt to, we should talk about that.
    *   Alex: Registering a beacon vs registering something that’s inside the beacon itself that’s part of the publisher’s context, that’s the difference.
    *   Michael: RegisterAdBeacon - we can imagine how it does event-level reporting now, and aggregate reporting in the future, and that’s a desirable property. As we invent new things, would like to keep an eye on that goal. Distinct from the goal of separating contextual information and rendered ad.
    *   Shivani: If it calls ReportEventWithMacros, the script once loaded could send requests to server without it being part of the rendering of the ad.
    *   Shawn: The macros are registered, not expected to affect rendering in any way.
    *   Michael: But then do we need any change in FencedFrames now, because current FencedFrame reporting can use beacon-based reporting mecnahism?
    *   Shivani: Doesn’t specify destination URL
    *   Michael: Some things not known at auction time, but known at rendering time. That’s what this is allowing; new destinations. Would require a little more thinking. Not sure if that preserves the privacy model or not.
*   Michael Kleber
    *   Also noted a “star” designation, where macro is populated for all origins. But this could mean that parties that are not either the SSP or the DSP could be informed. Could be parties only known through chain of script inclusions could be included. Seems bad to me. Currently, we know who the parties are that could receive data, especially the mixing of data. If this as a mechanism gives the full power of event-level win reporting, that’s currently locked down - only the buyer and seller participating in the auction currently receive that. But an open-ended star mechanism seems a more promiscuous sharing of information.
    *   Shawn: Thinking on the spot, maybe a list of domains that can get that information. Could become a long list - hundreds long.
    *   Michael: That's not very appealing either
*   David Dobbs
    *   Seems like what Alex pointed out, of the leakiest part of the information.
    *   Michael: If this were a proposal on how this could work for the aggregate-level reporting system, I would be more enthusiastic. If we tried to make an aggregate reporting API that had all of the information you want, it would be a lot better.


## Regression in DevTool Integration with FLEDGE API
https://github.com/WICG/turtledove/issues/499 



*   David Dobbs
    *   Just put in the next agenda item, on behalf of Zheng Wei, just entered yesterday. Would be wonderful if the Chrome team could take a look at that.
    *   Michael: The people on the FLEDGE team who do devtools integration will take note of that, and will look into that.
