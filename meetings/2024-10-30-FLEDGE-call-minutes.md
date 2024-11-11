# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Oct 30, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!



1. Michael Kleber (Google Privacy Sandbox)
2. Roni Gordon (Index Exchange)
3. Brian May (unaffiliated)
4. Sathish Manickam (Google Privacy Sandbox)
5. Sven May (Google Privacy Sandbox)
6. Paul Jensen (Google Privacy Sandbox)
7. Harshad Mane (PubMatic)
8. Matt Kendall (Index Exchange)
9. Jacob Goldman (Google Ad Manager)
10. Paul Spadaccini (Flashtalking)
11. Hillary Slattery (IAB Tech Lab)
12. Alex Peckham (Flashtalking)
13. Aymeric Le Corre (Lucead)
14. Sarah Harris (Flashtalking) 
15. Becky Hatley (Flashtalking)
16. Brian Schmidt (OpenX)
17. Laurentiu Badea (OpenX)
18. Garrett McGrath (Magnite)
19. Fabian Höring (Criteo)
20. Victor Pena (Google Privacy Sandbox)
21. Kevin Lee (Google Privacy Sandbox)
22. Orr Bernstein (Google Privacy Sandbox)
23. Alexandre Nderagakura (Not affiliated)
24. Abishai Gray (Google Privacy Sandbox)
25. David Dabbs (Epsilon)
26. Suresh Chahal (MSFT)
27. Zach Mastromatto (Google Privacy Sandbox)
28. Koji Ota (CyberAgent)
29. Rickey Davis (Flashtalking)
30. Diana Torres (MLB)
31. Alonso Velasquez (Google Privacy Sandbox)
32. Brian Schneider (Google Privacy Sandbox)
33. Hari Krishna Bikmal (Google Ads)


## Note taker: Sathish Manickam


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here (meetings with no items may be canceled)



*   [Charlie Harrison]: Discuss https://github.com/WICG/turtledove/blob/main/PA_private_model_training.md 
*   [Orr Bernstein]: Response to feedback posted re Creative Scanning in: https://github.com/WICG/turtledove/issues/792#issuecomment-2446707641


# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!


# Notes 


## Charlie Harrison: Private Model Training (https://github.com/WICG/turtledove/blob/main/PA_private_model_training.md) 



*   Quick review: 
    *   The explainer post was published last week. 
    *   The explainer discusses how to better support model training. Models pCTR and pCVR are being supported at the moment. This is about client side; more info on server-side coming soon
    *   Modeling signals get only a max of 12 bits. Goal of this API is to get much more model training quality than is possible with 12 bits, with same or better privacy.
    *   From the perspective of privacy, we are considering allowing unlimited modeling signals (raw bytes instead of enums), as long as these are encrypted and the modeling done / processed within a TEE. 
    *   Some parts of the flow are similar to the Private aggregation API. 
    *   More details in the explainer. 
    *   Models can be used in B&A server or on-device in generateBid.
*   Garrett McGrath: You said earlier, different TEE or something similar. What does it mean? 
    *   Charlie: It can be a new binary, but can run on the same TEE infrastructure. 
*   Isaac Foster: Couple of questions
    *   First question: Adtechs should be able to send encrypted payloads to a TEE, and through a series of hooks, join the data, and train the model. The output would be: Released in plain text?  or sent to a TEE based KV server? 
        *   Charlie: The model will be trained in an inherently private way, so the output will be done in a clear format. The model will be returned to the ad tech that built it, in the cloud path you have included in the config. There will be some noise included, but we hope with the new models the impact of the noise will be minimal compared to the noised to models today. 
    *   Isaac: Ability to do the model will be somewhat limited; 
        *   Charlie: In your reportWin context you have a bunch of things that are already in the clear. We are not proposing to limit those signals in this design. The ability to augment an example, with encrypted signals and potentially some clear signals will be supported. Could be via an offline job as well. It should be fine to have an identifier for the interest group and join as well. 
    *   second question: joining the private training data with other stuff that happens on the page or downstream?
        *   Charlie: On the second question related to post auction signals, the way labels will get outside of a FF ad, is via ARA. ARA currently has an event level output gate, that can specify the auction id, and include cross-site event (such as conversion) can be supported. The caveat is that, with the new private model training proposal, we need to do some more work on ARA to support. Currently local noise is added, but we may be able to support more efficient noise or rich data with encryption. 
*   Brian May: Couple of things - sounds like we are developing a server side architecture. Is there an effort to make it more general purpose, to allow for ARA, model training and other use cases we come up with as well? 
        *   Charlie: There is a bit of tension between allowing for privacy vs Utility when we are considering general architecture vs maximizing utility for the specific use cases. 
    *   Brian: Ok with restriction and prescription, so long as the rules are clear and arrived at through a process that allows general input from the community. 
        *   Charlie: We are soliciting early feedback for this reason. We will also reach out to Adtechs for feedback. We also want to be as flexible as possible to support as many use cases as possible but start with the Protected audiences. But we are open to consider other use cases. 
    *   Brian: So, is the interest in getting feedback on other use cases, and what else can be supported? 
        *   Charlie: Yes, we want to keep the privacy constraints, but happy to include all the use cases, and other utility considerations, 
*   Isaac Foster: 
    *   Agree with Brian’s comment on considering other use cases. Also, to echo Charlie’s point, the technology is still being developed, and it will need to be considered. 
    *   Current approach is Internal restriction on processing, and more flexibility on outputs. Have you considered more distributed processing? 
        *   Charlie: This is also the dream on unlimited general computation on aggregated signals, but limiting the input for privacy. Others have model training that is under research, which includes limits on models / processing based on privacy. 
    *   Isaac: Agree, it is still being researched. Most useful model would be a generic training algorithm / general mode, but with bit output restriction that sends it to a TEE.. 
        *   Charlie: If we can figure out a way to do efficient private model training, it would be interesting. There are even some google libraries (e.g. Beam). We can have this chat more when we have some additional information. 
    *   Isaac: exciting to hear what is being proposed, and we should talk more about it in future. 
*   Brian May: 
    *   For privacy/utility tradeoff suggest well understood, modular components that have been vetted and determined to be acceptable for limited, well defined use cases. This can be used in a given component for a given purpose, is something that can be considered? 
        *   Charlie: We thought about these before. But the attack vectors are privacy sensitive. There were lots of edge cases, the noise is challenging to consider in terms of privacy.
    *   Brian: When you look at removal of noise, have you considered limit output gates that will restrict information. 
        *   Charlie: Rather than adding weights in the model, we can add noise in the output, but that will make it  less useful
*   Charlie: Protected audience repo is a good place to continue to engage in this topic. Please CC me on issues you create about this, as I may not be watching each post. 


## Orr Bernstein: Creative Scanning - https://github.com/WICG/turtledove/issues/792#issuecomment-2446707641 



*   Specific focus on what we want to do right now. The earlier feedback was on the other information needed for creative scanning. We have proposed a way to include those extra pieces of information. Using creative scanning metadata and AdSize and buyerOrigin, can be included. Does this satisfy the need for creative scanning? 
*   Roni: Posted a quick response on GitHub
*   Orr: Haven’t gone through the post yet, but will do so and respond back. Are there other questions? 
*   Roni: My requirement is that, if we can get out of forDebuggingOnly, and if that information is available that would be good. I am curious about the buyside response if this is would be tenable? 
*   David Dabbs: I am from the buyside, is the proposal the one on the post? It would nice if it didn’t have to include in all interest groups have include size, adomain, seatID.. If the seatID is available (referencing, for example Index Exchange), it need not be included in all the interest groups. 
*   Fabian Höring: We would like to inject everything into `adCreativeScanningMetadata`, the number of URLs might be too big to identify creatives, so potentially need a creative code/id for many renderURLs. Not sure notion of creative id should be included in the spec as it can already be handled with metadata
*   David: It would not be the Protected audience that needs to do this though.. It should be the adtechs that do it. 
*   Roni: Some change also need to be done on the API side as well.
