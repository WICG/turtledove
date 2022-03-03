# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 4pm UTC (during winter).

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Feb 16 2022


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Brian May (dstillery)
3. Wendell Baker (Yahoo)
4. Matt Menke (Google Chrome)
5. Chris Evans (NextRoll)
6. Peiwen Hu (Google Chrome)
7. Tamara Yaeger (IPONWEB)
8. Paul Jensen (Google Chrome)
9. Jonasz Pamuła (RTB House)
10. Lukasz Wlodarczyk (RTB House)
11. Przemyslaw Iwanczak (RTB House)
12. Stan Belov (Google Ads)
13. George London (Upwave)
14. Jeff Kaufman (Google Ads)
15. Don Marti (CafeMedia)
16. Joel Meyer (OpenX)
17. Rotem Dar (eyeo)
18. Alexandru Daicu (eyeo)
19. Michael Coward (Arcspire)
20. Sven May (Google Web Platform Team)
21. Brad Rodriguez (Magnite)
22. Anthony Molinaro (OpenX)
23. Alex Cone (IAB Tech Lab)
24. Martin Gruau (Captify)
25. Russ Hamilton (Google Chrome)
26. Sergey Tumakha (Microsoft Ads)
27. Supraja Sekhar (Google Ads)
28. Zach Mastromatto (Chrome Partnerships)
29. Andrew Pascoe (NextRoll)
30. Kevin Graney (Google Chrome)
31. Caleb Raitto (Google Chrome)
32. Tim Hsieh (Google Ads)
33. Daniel Rojas (Google Chrome)
34. Aleksei Gorbushin (Walmart)


## Note taker: JoelPM + Supraja Sekhar


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

 		


## [Suggest agenda items here]



*   Today's news: Privacy Sandbox on Android: https://privacysandbox.com/android/
    *    // FLEDGE on Android - how to stay involved?
*   Component Auctions & Prebid.js Review [[link](https://github.com/JoelPM/prebid-td/blob/main/PrebidFledge.md)]
*   Propagating auction signals from server to turtledove worklet securely: https://github.com/WICG/turtledove/issues/119


## Notes

Michael Kleber: Welcome back to the Fledge WICG call, this takes place under the auspices of the w3c WICG - please join if you haven’t. It’s important for IPR purposes. Please sign in above. Still need a scribe to help for Joel's agenda item #3 Prebid-stuff. (Thank you Supraja.) Please use “Raise My Hand” to join the speaker queue.

Michael Kleber: Item #1 is an announcement this morning that Privacy Sandbox will be coming to Android:

https://privacysandbox.com/android/ Links to other stuff there. The goal is to bring many of the same Privacy Sandbox ideas to Android, including Fledge and Interest Groups, though on Android it will be called Custom Audiences based on the feedback. Very similar to the Fledge API, encourage everyone to go check it out.

Jonasz Pamuła: My question is about Fledge on Android, namely, is there a chance that the design choices will diverge from what was discussed for Chrome. I assume there will be some technical challenges and I’m curious if there will be a forum to stay involved and take part in shaping what is happening on Android. Will that happen here?

Michael Kleber: That’s a great question, I don’t think we have any Android representatives today. The Privacy Sandbox site includes a link to an Android page for Fledge:

https://developer.android.com/design-for-safety/ads/fledge That page includes a "Provide Feedback" link, which would be a good way to provide feedback to the team working on implementing it. The Android folks who aren’t participating in the w3c are used to a different model of interaction, so it’s likely that they will engage differently and it will probably more model the standard Android approach. Android folks are welcome to come to this meeting, though there is nothing specific lined up. As far as divergence, we do expect that it will happen over time simply due to the difference in the platforms themselves (web vs mobile) and how people operate. SDKs vs iframes, etc. So there will certainly be technical differences, but in terms of overall approach I don’t see fundamental reasons why those should differ. We will try to have the two platforms work as similarly as possible except for areas where they need to diverge. Probably not the most satisfying answer, but best that can be said right now.

Jonasz Pamuła: Everything you said makes sense, thanks. If there was a chance to hear from someone on the Android team, that would be great. Also, do you know if it’s possible to test it out behind some flag, perhaps? Or is it too early?

Michael Kleber: I don’t know the answer, but I believe it’s too early. None of the announcements contained info on that.

Brad Rodriguez: I had a question about whether we know if we can expect that data would be cross-device at some point. For example, will Interest Group information span web/mobile? Are there any plans now that would indicate IG information could leave that device?

Michael Kleber: Let me point out that there are two different questions. On one hand, it could be about data on one physical device moving to another physical device, eg Chrome to Chrome. Answer: we have not discussed any plans to implement that kind of cross-device membership for IGs. It’s not impossible, but it’s not developed and we don’t have a definite position on that. Separately, now that Fledge is available on Android, there’s also the question of data sharing on a single device between browser on that device and app on that device -- Chrome Fledge to Android Fledge. And webview is another environment that ads render in. We don’t have an answer to that either. It doesn’t seem implausible that sharing between web and app on a single device could happen, but privacy and security models on the two environments is different so it would take some work to make sure that any sharing is safe. I can’t promise one way or the other.

Brad Rodriguez: I know the work we’re doing within Fledge is in the w3c, and the Chrome team have indicated that anything beyond the web is out of the jurisdiction. How do you see Android interacting then?

Michael Kleber: As noted above, the Android team has their own model. We on the Chrome team will continue to be here. But jurisdiction might be the wrong word - w3c doesn’t tell anyone what to do. This is just a place where we can come together to talk about web standards. Other people that wish to discuss standards can come here if they desire…

Brad Rodriguez: So can we, as this group, ask for features? Ask for the standard to allow for sharing across device/context? Is that appropriate for this venue, if not, then where?

Michael Kleber: Absolutely appropriate. Android folks have made it clear (via button) they are interested in feedback. Feedback from a group like this one (a consensus) should certainly be an appropriate source for feedback back into their development process. I absolutely think that our conversations can be pushed to the Android development side of the house.

Brian May: I think you’ve already answered this, but just to confirm: is the intent for the Chrome and Android teams to work together to figure out compatibilities and incompatibilities in order to ease the burden on those of us using Fledge to have a consistent implementation?

Michael Kleber: The answer is yes, but I don’t think this is a w3c type of question. If you are involved in adtech and working with people who put ads on Android and have some kind of SDK that will be involved in the Android Fledge side, and you’re involved in the Fledge Web side of things. Wanting to do work once that works as much as possible is natural and is likely shared by anyone. Saying that you would like consistency is absolutely valid feedback to give to both teams. 

Brian May: Is your intent to work with the Android folks to support us in understanding how to compare compatibility? If I set up infra to put stuff on a browser, will I be able to reuse that 90%, 50%, … for interacting with Android?

Michael Kleber: Don’t have a good answer right now.

Brian May: Is the intent to work in that direction?

Michael Kleber: Intent is definitely to make it as easy as possible to reuse infra across platforms, but I can't answer how much inescapable differences will impact that.

Michael Kleber: Any other thoughts on that before moving on?

Michael Kleber: On to topic #2.. 

**Topic: Component Auctions & Prebid.js Review**

Joel Meyer: https://github.com/JoelPM/prebid-td/blob/main/PrebidFledge.md

[Reviewed changes to the steps as noted in the document.]  Biggest question is how GAM/GPT will participate. In general, Component Auctions seem to meet our needs.

Jeff Kaufman: I work on GPT in Google Ads. We are not sure what we plan to do yet. Useful to understand what sort of hooks you might be able to work with and the ideal scenario for you. 

Lukasz: Ideal landscape in the perspective of demand partner: We see a need for the whole market to have logic managing top-level auction to be part of open source code managed by a community. For example: prebid. This approach will guarantee a level playing field.  It's important both from buy and sell-side to be able to audit top level auction. It allows to consciously manage advertising cost and bidding strategy from buyside and yield management in sellside.We are aware that in the short term this will produce some overlaps and need deduplication. On the other hand, it will allow us in the longer run to optimize our supply path.

Brad: How to best engage with the GPT team on support for FLEDGE?

Jeff: Probably Github repo of FLEDGE, but someone should jump in and correct me if I'm wrong

Michael Kleber: Probably WICG is not the best forum. Google Ads repo or dedicated meetings better than W3 forums such as the FLEDGE meeting or repository. The topic is not how FLEDGE browser auction works but rather the partners using them will work. If it is about how browser auction works, then it is best to bring back to a FLEDGE forum like this one. 

Stan: Google Ads [repo](https://github.com/google/ads-privacy) might be better. Stan & Jeff will circle back on the best forum.

Michael Kleber: Glad discussion is happening now.

Brian May: This is now the second instance where Fledge is growing beyond the boundaries of this group, is there any possibility of a group where we can explore cross-boundary issues that arise?

Michael Kleber: Interesting thought… I’m not sure I know the answer. Is there some better venue than a w3c group for Fledge as … I almost expect an IAB spokesperson to speak up and suggest that IAB is the right place for this to happen.

Alex Cone: I will acknowledge that in the same way that you are treading gently around the distinction between Chrome and Android, the same thing exists in Adtech - prebid and IAB are similar. There are reasons, but I’m not sure Angie’s group is the place for that.

JoelPM: We're just trying to figure stuff out!  We welcome Alex / IAB being involved.

Alex: Just trying to answer why IAB didn't leap in for this — I assume prebid has a focus on this, don't want to try to steal prebid conversations into TechLab.  Maybe room for a self-organized thing?

Brian May: It was inevitable we’ll get to this point and this isn’t the first time we’ll have this conversation.

Brad Rodriguez: I wonder if the team building Android and the team building Chrome were not linked by any corporate structure, would there already be some type of conversation happening for them to collaborate? Could we pretend that is the case and have those conversations in public?

Wendell Baker: I think we should focus on getting the Google Fledge tech working and operable. We have some serious concerns about whether it even works. But this is a breathtaking vision put forth by Google and taking into account the systems, the science, the economics, I think we need to show it working in one place, then another, and another, etc. Thanks.

Michael Kleber: That all sounds entirely reasonable. Completely agree that we need to walk before we can run, and that’s the right way to function at the stage we are currently at.

Michael Kleber: This does still leave us with an interesting question about where to have the larger conversation. The question about where to work out the Prebid and GPT interacting is a symptom of the fact that there is a lot of adtech industry discussion that will need to happen. And whether that’s the same place that the Web/Android conversation happens is unclear to me. The Android+Web conversation is more plausible to take place in this group. The adtech stuff is probably better off running in industry channels.

Alex Cone: Privacy Enhancing Technologies WG is spinning up in IAB Tech Lab and it may be the right place for this conversation to happen. It’s a technical forum for gathering ideas for the broader web community. But it still doesn’t need to subsume the Prebid/GPT stuff that is best served out of Prebid, maybe organized by PMC. If you have thoughts on that and you would like to use IAB Tech Lab as an organization, please ping me ([alex.cone@iabtechlab.com](mailto:alex.cone@iabtechlab.com)).  (Side note: PMC = Prebid Management Committee)

Michael Kleber: Interesting discussion and not sure where either question will land. I look forward to ongoing discussion. Prebid/GPT best housed in something run by adtech, not a browser conversation. I look forward to seeing those venues develop.

Brad Rodriguez: Would Chrome participate if such a venue was created?

Michael Kleber: Good question, would have to ask the chrome folks responsible for interaction with outside developer community. I expect the answer is ‘yes’ when it is related to how Fledge should work. Questions about GPT/Prebid might be best without Chrome in the room at all.

Brad Rodriguez: Makes sense. Other than the benefit of having a representative who can absorb context would be nice.

Brian May: It was inevitable that we were going to get to this point. I assume that we have these structures in our industry as much for legal/business reasons as for being able to coordinate with each other. It seems like having to set up the legal scaffolding for a new group is a fairly dicey proposition. Maybe the best path forward is a forum for informal discussions. … We will never get anything done if people are second guessing if they’re running afoul of a legal issue.

Michael Kleber: I do agree that Chrome being involved in discussions with adtech vendors in forums that aren’t operating under protections provided by the w3c does get into trickier waters, so Chrome might need to prefer w3c for any discussion that will end up as browser standards. As far as understanding use cases better, the web-adv business group is kind of explicitly made for that purpose. It is not possible for me to speak right now to other places that Chrome could participate.

Brian May: Anyone know if there is precedent for needs to collaborate like this?

Wendell Baker: I would posit that the Web DRM conversation is precedent. The idea that other industries have enforced their will on the web and won is not new ground.

Don Marti: The DRM issue is definitely a concern. I would encourage people to look at some of the papers on the economics of advertising and where there is an incentive for buyers and sellers to share information in the form of economic signal. There are ad systems in which the user is not incentivized to block ads and in which an ad free version of an ad supported service is not considered attractive. As a former reader of Computer Shopper, magazine advertising is an example of that kind of ad system. (Vogue is another high-profile example.) I don’t believe that a properly designed web-adv system would have to impose a DRM-like set of anti-user controls to support advertising. I’m happy to share links, there are some in the web-adv business group.

Michael Kleber: I think that gets us to the end of our discussion, we’re at a good stopping place. We’re not at the end of any of the discussions we've kicked off, but it’s a good place to stop.  I look forward to future conversation.
