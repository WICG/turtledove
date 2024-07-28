# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday July 24, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!	 



1. Michael Kleber (Google Privacy Sandbox)
2. Brian May (Dstillery)
3. Eyal Segal (Taboola)
4. Roni Gordon (Index Exchange)
5. David Dabbs (Epsilon)
6. Isaac Foster (MSFT Ads | The Avengers)
7. Konstantin Stepanov (MSFT Ads)
8. Felipe Gutierrez (MSFT Ads)
9. Patrick McCann (Raptive)
10. Sven May (Google Privacy Sandbox)
11. Harshad Mane (PubMatic)
12. Marco Lugo (NextRoll)
13. Matt Kendall (Index Exchange)
14. Elmostapha BEL JEBBAR (Lucead)
15. Matt Davies (Bidswitch | Criteo)
16. Tamara Yaeger (BidSwitch)
17. Ivan Staritskii (Bidswitch | Criteo)
18. B. McLeod Sims (Media.net)
19. Owen Ridolfi (Flashtalking)
20. Anthony Yam (Flashtalking)
21. Harry Stevens (Bidswitch | Criteo)
22. Becky Hatley (Flashtalking)
23. Jason Lydon (Flashtalking …)
24. Arthur Coleman (IDPrivacy)
25. Alex Cone (Google Privacy Sandbox)
26. Jeroune Rhodes (Privacy Sandbox) 
27. Alonso Velasquez (Google Privacy Sandbox)
28. Kevin Lee (Google Privacy Sandbox)
29. Ashley Irving (Google Privacy Sandbox)
30. Sid Sahoo (Google Chrome)
31. Orr Bernstein (Google Privacy Sandbox)
32. Sathish Manickam (Google Privacy Sandbox)
33. Fabian Höring (Criteo)
34. Laura Morinigo (Samsung)
35. Paul Jensen (Google Privacy Sandbox)
36. Matt Menke (Google Chrome)
37. Andrew Pascoe (NextRoll)
38. Antoine Niek (Optable)
39. Alex Peckham (Flashtalking)
40. Guillaume Polaert (Pubstack)
41. Sarah Harris (Flashtalking) 
42. Taranjit Singh (Jivox)
43. Tim Taylor (Flashtalking)
44. Viacheslav Levshukov (Microsoft)
45. Josh Singh (Microsoft)
46. Miguel Morales (IAB TechLab)
47. Achim Schlosser (Bertelsmann)
48. Paul Spadaccini (Flashtalking)
49. Brian Schneider (Google Privacy Sandbox)
50. Matthew Atkinson (Samsung)
51. Jeremy Bao (Google Privacy Sandbox)
52. Aymeric Le Corre (Lucead)
53. Pierre Perez (Azerion)
54. Shafir Uddin (Raptive)
55. Premkumar Srinivasan (Microsoft Ads)
56. Daniel Rojas (Google Privacy Sandbox)
57. Courtney Johnson (Privacy Sandbox)
58. Manny Isu (Google Privacy Sandbox)
59. Amitava Ray Chaudhuri (Adobe Adcloud)
60. Koji Ota(CyberAgent)
61. Laurentiu Badea (OpenX)
62. Suresh Chahal (Microsoft)
63. Nick Colletti (Raptive)
64. Abishai Gray (Google Privacy Sandbox)
65. Veronica Kim (Raptive)
66. Jinhwa Rustand (Nativo)
67. Siddharth VP (Jivox)
68. Warren Fernandes(Media.net)
69. David Tam (Relay42)
70. Rickey Davis (Flashtalking)
71. Hillary Slattery (IAB Tech Lab) SORRY KLEBER!
72. Arian Senior (IAB France)
73. Maybelline Boon (Google Privacy Sandbox)
74. Caleb Raitto (Google Chrome)
75. Hari Krishna Bikmal (Google Ads)


## Note taker: Manny Isu, Alonso Velasquez	


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here:



*   Discussion of Monday's "A new path for Privacy Sandbox on the web" news https://privacysandbox.com/news/privacy-sandbox-update/   

*   Isaac Foster:
    *   Brief revisit the “coarse information sharing” thing, we had talked about setting up time but never did, all got too busy…can even answer here
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736  

*   Warren: 
    *   [Addition of an analytics/reporting entity to enable centralised reporting · Issue #1115 · WICG/turtledove · GitHub](https://github.com/WICG/turtledove/issues/1115)  

*   David Dabbs
    *   Low entropy client hints on TBS requests ([#1031](https://github.com/WICG/turtledove/issues/1031))
        *   Interest also in updateURL and real-time reporting postbacks.  

    *   Request for `updateURL` processing to support the leaving/joining of negative interest groups via the [nascent header feature](https://github.com/WICG/turtledove/issues/896)  
(entered as issue [#1228](https://github.com/WICG/turtledove/issues/1228))

        In the “shell IG” pattern a buyer joins an IG with a minimal, updateable attribute set. This centralizes and defers page latency inducing processing (computing and rendering IG content) to updateURL time. (Proposed) negative targeting adds a wrinkle. Scenario: a buyer intends to negatively target a positive interest group. The Chrome-suggested approach has the buyer pair the excluded positive IG with a targetable ‘negative shadow.’ At site visit time, the buyer joins the browser to “shells” of “EXCLUDED_POSITIVE,” the excluded IG, “EXCLUDED_NEGATIVE,” its negative ‘shadow,’ and “EXCLUDER,” an IG that seeks to negatively target the other group. EXCLUDER’s update will configure EXCLUDED_NEGATIVE in `negativeInterestGroups[]`. If EXCLUDED_POSITIVE’s update determines that IG membership condition doesn’t hold (or the update doesn’t run &c.), the buyer has a partial, incorrect IG set.   

        The buyer’s ability to join or leave EXCLUDED_NEGATIVE during EXCLUDED_POSITIVE’s update would smooth this wrinkle. The primary can ensure its ‘shadow’ is always appropriately joined when it is processed. 

    *   Request for nascent header join/leave capability to support <code>[fetchLater API](https://developer.chrome.com/blog/fetch-later-api-origin-trial)</code>  
(entered as [comment](https://github.com/WICG/turtledove/issues/896#issuecomment-2233667864) on #896)  
  
        Chrome is [migrating](https://issues.chromium.org/issues/40236167) keep-alive request handling from the “renderer” (front-end) to the “browser” process (back-end). [Explainer](https://docs.google.com/document/d/1ZzxMMBvpqn8VZBZKnb7Go8TWjnrGcXuLS_USwVVRUvY/edit#). Attribution Reporting API (ARA) supports event and trigger header registrations on background requests, and this will move to the browser. ARA team has [extended that hook](https://issues.chromium.org/issues/40242339#comment48) so that <code>fetchLater</code> API requests will also be able to set ARA headers. Requesting that Protected Audience header processing also support <code>fetchLater</code>.   

    *   K-Anonymity: According to the k-anon [doc](https://developers.google.com/privacy-sandbox/relevance/protected-audience-api/k-anonymity): 
        *   _In Q1 2024, for up to 20% of Chrome Stable traffic, excluding Mode A and Mode B experimental traffic, we will begin to check k-anonymity with the same parameters._  
  
            _In Q3 2024, when the third-party cookie deprecation (3PCD) ramp-up process is planned to begin, k-anonymity will be checked for 100% of Chrome Stable traffic with the same parameters?_  

    *   Will the experimental groups continue to be excluded?  
  
        (You are discussing the path forward with CMA, but any clarity on k-anon, which has been and I assume still is the next major PA privacy enforcement change to drop, will be welcome when you can provide it.)


# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!! :) 


# Notes 


## Discussion of Monday's "A new path for Privacy Sandbox on the web" news https://privacysandbox.com/news/privacy-sandbox-update/ 



*   [Kleber] Alex Cone will be discussing this blog post
*   [Alex Cone] This is a new experience for users over 3PC - giving the users a choice. Acknowledge there are a ton of questions. But for now, we do not have all the answers to the questions but we are taking this very seriously, and we are working through them internally to make sure we provide answers to things like timeline, UX, etc… and will share with you all when we have them
*   [Alex Cone] This team stays here, the APIs don’t go anywhere
*   [Brian May] Will the community invite to participate in the interaction around user choice and will there be a WICG for it?
    *   [Alex Cone] We do not have plans for that yet, and we are still in discussion around it with regulators. We do not have designs yet, and generally you can expect we continue seeking feedback
    *   [Kleber] The kind of things discussed in WICG calls or other W3C groups are cross browser standards. That does not include the UI for a browser and has never included the UI of a browser - user settings are not part of a WICG call. These are things that any browser reasonably calls the shots on their own and there is no reason for any interoperability. I do not expect anything about settings UI to show up here or any W3C forum
*   [Isaac] Curious to any extent you can comment - do you anticipate continuing to do some type of ramp to get stronger evidence of  its performance (where it needs to get better)? I’ll decouple from 3PCD.
    *   [Alex Cone] Do you mean a ramp of how choice would work or…?
    *   I could see Chrome making a statement - the current 1% stays the way it is but rather than ramping to say 5% with no option to go back, mode B will become 5% with default 3PC off?
    *   [Alex Cone] We do not have a plan to announce that, but acknowledge that having more traffic to test with will be helpful
*   [From in-call chat: Anthony Yam]: is the 1% rolling back?
    *   [Alex Cone] You can expect to hear from us later. We do not have an answer right now
*   [Brian May] Can you give us any rough idea on how things will progress from here? Timeline?
    *   [Alex Cone] We are moving with haste on that, but a piece is the discussion with regulators. But I cannot give you a timeline for the timeline just yet.
*   [Pat MacCann] Will the gnatcatcher IP privatization work now be tied to this new commitment date? Should I read everything that was tied to deprecation tied to UX release
    *   [Alex Cone] Still working through those details right now
*   [B. McLeod Sims] My understanding is that work will continue to move forward on these APIs, is that correct?
    *   [Alex Cone] We are continuing to invest in the APIs both on the privacy and utility side
    *   [Kleber] These APIs are being added to the web platform. They are available today for folks who have 3PC turned on or have 3PC turned off. If we decide to make any change as to the availability of the APIs, you will know about it long before it happens. But for now, there is no reason to expect any change with respect to availability
*   [Luckey Harpley] One goal of PS was to unify Chrome and Android. Any comment to this?
    *   [Kleber] No announcement as to changes for Android
*   [Brian] Is the PS timeline going to be kept updated and used to communicate to the ecosystem?
    *   [Alex] In terms of how we continue to use ecosystem update, I cannot speak to that right now
*   [Isaac] Curious on Chrome’s commitment to making additional web standard changes to ARA?
    *   [Kleber] Every change we make to the web platform in Chrome is designed to be a change to the Web in general. Our goal is for everything we are working on to become part of web standards across browsers. Those who can remember the days of different browsers behaving differently can understand we don’t want to go back to a similar state.
*   [Harshad Mane] When will UX be presented to user and what will be the default position?
    *   [Alex Cone] This is something we are working through on what the experience will be
    *   [Harshad Mane] Will it be global or in stages during release?
    *   [Alex Cone] No plans to share at this point
*   [From in-call chat: Warren Fernandes]: is IP obfuscation still on the cards?
    *   [Kleber] The linked news release does mention something about IP… we are going to be introducing IP protection in Chrome incognito mode. No statements to make as to what will happen to IP protection in the future. There is an ongoing discussion about IP addresses and privacy in every browser globally.  This W3C meeting, and W3C in general, is not the forum for it because IP address stuff operates at a different protocol layer: W3C is about stuff happening at the HTTP layer, while IP address discussions are happening at the IETF, where network protocols live. Not a whole lot to say about it here.
*   [David Dabbs]: Suggest that folks who have further questions on the announcement add their questions to this doc to be further addressed
    *   [Kleber]: given the nature of this doc, intended to be a log of the live discussion vs a discoverable document for people outside the call, may not be the most appropriate place for that.
*   [David Tam]: What can you say about the announcement and the Shared Storage API?
    *   [Kleber]: Recall the Shared Storage API is an API that allows cross-site writing of data, with specific output gates for the secure and private consumption of it like the Private Aggregation API. There is no effect on that API based on this announcement
*   [David Tam]: Currently, when a cookie sync call is made it is a redirect that calls a third party domain.  If the cookie sits in shared storage then how do you read the cookie. Correction, I should have referred to CHIPS.
    *   [Kleber]: the behavior of cookies has been the same for the last 25 years. If the browser has 3rd-party cookies on, then they will continue to work the same way they have before. If the browser has 3p cookies off, that's also a setting that browsers have had for a long time.
*   [From in-call chat: Kevin Lee]: Hello folks, please post your questions about User Choice in the PS Dev Support repo:[ https://github.com/privacysandbox/privacy-sandbox-dev-support/issues](https://github.com/privacysandbox/privacy-sandbox-dev-support/issues). A new issue label "user-choice" has been created.
    *   [Kleber] If you have questions about the whole new path user choice cookie thing, there is a right place for them in the Privacy Sandbox developer support GitHub repository that Kevin linked to.


## Discussion of: [Addition of an analytics/reporting entity to enable centralised reporting · Issue #1115 · WICG/turtledove · GitHub](https://github.com/WICG/turtledove/issues/1115)



*   [Warren F] Have you had a chance to go through the issue?
    *   [Paul] We discussed a few weeks ago and I had a chance to speak to the Shared Storage folks. Previously, we talked about who will be in charge of giving the information - we decided it will be the seller. Also, for header mechanism to facilitate that, it looks like you added that to the document. I wanted to talk to Josh (Shared Storage), and wondering the timing of it, how will it get permission? We envision it will get written to SS and how do we envision the SS worklet, say how will the publisher know when to publish and what timeframe?
    *   [Warren F] I think it is something the browser will do periodically. It is in section C of the document
    *   [Paul] Not an expert in SS. The browser does not do a whole lot of things periodically - it is hard for it to do so
    *   [Warren F] But it could be a similar mechanism as aggregate with some arbitrary delay. I think it will work fine no matter what the cadence is, but as long as it is not super infrequent
    *   [Patrick McCann] Publishers can call this, and on page, they can do a clean up and see if there are any events that they haven't called yet… the requirement is that some other entity will be able to generate this report. 
    *   [Paul] I think SS is still the right proposal; the issue is the when - that is, when to run it
    *   [Kleber] I suspect the answer might run into a coordination problem with the publisher. But I think we should be able to find time to run it.
*   [Brian] It sounds like they may be a parallel with the trigger attribution call in ARA
    *   [Kleber] Not sure about that call
    *   [Paul] I think at the time Patrick was describing, not sure there is a network fetch for a header response. We generally do not make a request at page unload time
*   [Patrick McCann] There is [Event level reporting](https://github.com/WICG/shared-storage?tab=readme-ov-file#event-level-reporting) out of SS currently documented for 2026 - will that be moved forward? Should we still plan to build tooling on it?
    *   [Kleber] The flow we should build: Write to Shared Storage - Use existing output gate to do reporting. I do not think event-level reporting coming out of SS is a good fit for what you are trying to do - that's part of the selectURL output gate, and the event-level reporting is about which of the 8 possible URLs was selected, not about the contents of Shared Storage. The private aggregation is much more appropriate
    *   [Patrick] Makes sense. That answers it.
*   [Warren F] How do we move forward on this one since everyone is broadly okay with it?
    *   [Paul] We should formalize the proposal a bit more; explicitly stating names of the header and names of the APIs and make sure we are all onboard. If everyone is interested, we can then consider moving forward
    *   [Warren F] You can pick whatever you want as it’s just a name
    *   [Patrick M] I agree with Warren. Just make the decisions and preserve the momentum of this effort.
    *   [Paul] I think we may be at the point where we can propose something more formal given my preliminary review. The next step is to convert this into an Explainer and then we can make it a PR to the GH issue. I will either do this or find somebody on the Chrome side to do this one. Note that this is pretty complicated, so it might take me a while.

[Kevin Lee]



*   Hello folks, please post your questions about User Choice in the PS Dev Support repo: https://github.com/privacysandbox/privacy-sandbox-dev-support/issues 
*   A new issue label "user-choice" has been created.
