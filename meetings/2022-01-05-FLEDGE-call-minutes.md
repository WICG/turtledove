# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 4pm UTC (during winter).

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Jan 5 2022


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Caleb Raitto (Google Chrome)
3. Paul Jensen (Google Chrome)
4. Brian May (dstillery)
5. Russ Hamilton (Google Chrome)
6. Sven May (Google Partnerships)
7. Wendell Baler (Yahoo)
8. David Tam (Relay42)
9. Stan Belov (Google Ads)
10. Jeff Kaufman (Google Ads)
11. Jonasz Pamuła (RTB House)
12. Michael Coward (Arcspire)
13. Ryan Arnold (P&G)
14. Supraja Sekhar (Google Ads)
15. Christa Dickson (Dotdash Meredith)
16. Angelina Eng (IAB / IAB Tech Lab)
17. Bartosz Łoś (RTB House)
18. Matt Menke (Google Chrome)
19. Don Marti (CafeMedia)
20. Brad Rodriguez (Magnite)
21. Fred Bastello (Index Exchange)
22. Bartosz Marcinkowski (RTB House)
23. Alex Cone (IAB Tech Lab)
24. Joel Meyer (OpenX)
25. Yuval Tanny (Oracle Advertising)
26. Przemyslaw Iwanczak (RTB House)
27. Basile Leparmentier (Criteo)
28. Aleksei Gorbushin (Walmart)
29. Helene Maestripieri (Google Chrome)
30. Anthony Katsur (IAB Tech Lab)


## Note taker: Joel Meyer


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

 		


## [Suggest agenda items here]



*   Supraja Sekhar: Pull request for seller losing bids reporting
*   Supraja Sekhar: Process question regarding time period for updating meeting notes
*   


## Notes

MK: Two items on the suggested agenda list, both from Supraja.

SS: These are follow-ups from previous discussions about ways for sellers to get reporting for losing bids. Is the team open to a PR on what the losing bid reporting may look like?

There would be a JS function that would be exposed taking an ad render reason and a rejection reason sent by the seller. Both would be subject to a k-anon thresholds that would be used to ensure privacy is preserved. Other part up for discussion is how we prevent network latency from being too high as the number of calls made by this API could be high. Couple ideas: network calls could be batched and/or delayed. Building on this idea, there seems to be two use cases 1) counting the number of losing bids and reason, 2) ad components that lost in losing bid. In last mtg brainstormed about an ad component and ad render url to know what component lost. At render URL level could understand # of losing bids. Could also have an additional function for learning about losing ad component URL

MK: Regarding PR: discussion on a GH issue would be a better idea to make sure all use cases from diff parties are incorporated. PR is good once we know what change to make, at reqs stage an issue is better. Paul - any thoughts from the Chrome side?

Paul Jensen: We’ve been thinking about how to do loss reporting in the Fledge API. One pertinent thing is that losing bids >> winning bids. If we use a reportLoss function/worklet, then after auction we have to reload all worklets again and kick off JS interpreters on each, etc - this is a fair bit of work (on the browser side and slow things down). Perhaps we could delay to later to not slow down page load. Is there a declarative loss reporting API that could be defined earlier in the process (as when adding a user to an IG, or during bidding) to say that if the bid loses a URL gets fetched to report loss. Predeclaring saves computation time later on (due to overhead of loading worklet and executing function). Explainer discusses how to aggregate, but it’s not clear how we can have an aggregated and declarative approach.

MK: Some hands raised.

BL: This is a very important topic - in the case of First Price bids, it’s very hard to know how to bid efficiently if you don’t know the losing bid value. Perhaps we don’t need it all the time, but we do need at least a subset to be able to know what our win rate is and if there are bugs (eg win rate of zero may indicate bug). In a non-SecondPrice auction we need losing bids reporting to tune bidding strategy and campaign.

MK: Thank you, Basille. A couple clarifying questions. Supraja - you requested a rejection reason as information desired in the loss notification. The seller would provide this signal. Are you imagining that every seller comes up with the list of rejection reasons, or is it a predefined/standardized list that could be defined in Fledge?

SS; Ideally the latter - a common set across sellers. But consensus among sellers could be hard. The reason for the rejection reason is for seller troubleshooting - not for buyers. Used for sellers to understand how many bids lost due to reasons: eg - non scanned creative, etc.

MK: Thanks. Basille: you mentioned the need for some sort of bid landscape information so losing bids… I assume the implied use case is to understand how much higher a losing bid would need to be in order to be the winner.

BL: You don’t need that granularity actually. A binary is fine: win/loss tells me how to update my bidding. Today I don’t get the winning bid level in 99% of the cases. I’m okay with my bid, I just need to know if I won or not and I need to know what my bid was. This is interesting for the buyer to know as well - perhaps the buyer is showing the wrong inventory. Buyers need to troubleshoot as well.

MK: Got it. If we have an explanation of why the bid lost and it distinguishes outbid vs other seller logic, then that goes a long way toward addressing what BL is looking for. Next hand is Brian May.

BM: I think we’d do well to write up the use cases we’re talking about. Closer examination will probably show that there are some pretty significant problems with feedback mechanisms. For any use case like bidding optimization, I can’t imagine how I will optimize it based on win/loss given the little information I have access to.

BL: The big issue with what you just said is that if you are FirstPrice you have a huge CPM cost. Not knowing the clearing price causes a lot of effects, like Lemon effect. You can just bid high, but it’s expensive. You can bid low but you just lose a lot. There are probably some dumb strategies, but there will be a huge impact due to the opaqueness of the market. At least in SecondPrice you could be your true value and hope it wasn’t too expensive. FirstPrice you really need to know the competition.

BM: Yup. We need to describe the use cases and implications of this very different context.

MK: Sounds like a good discussion. Some of it we just talked about, but also pivoted here to information that the winner gets (not the loser, as the original topic was). Do agree, though, that the winner would really like to know some information to inform their strategy.

SB: Wanted to react to the declarative approach that Paul mentioned. It seems plausible to me that if the problem the seller is trying to learn about some limited set of filtering/rejection reasons then returning that info from the scoring function (about the loss reason) that might be sufficient for the browser to invoke some sort or declarative reporting URL (w/templating, perhaps). You don’t need arbitrary JS to do that.

MK: Yes, that sounds very like what the Chrome team has been thinking about.

SB: Re; standardizing rejection reasons, if it helps from the privacy standpoint to limit that subset, then maybe it’s feasible.

JoelPM: Quick question: OpenRTB has Bid Loss Reasons (“[Loss Reason Codes](https://www.iab.com/wp-content/uploads/2016/03/OpenRTB-API-Specification-Version-2-5-FINAL.pdf)”), would that be a template we could use?

SB: Could be! Don't know how many exchanges use something like that.

JoelPM: Should push in that direction if there's precedent.

SB: I think there was some meaningful distinction between the use case for buyers and sellers. For buyers, one of the major pieces of info is how I understand what the bid landscape looks like. For that, the contextual information seems like it would be very useful - for a pub, or for a slot, or for a geo, potentially). What Supraja was describing for seller loss reporting, that was just about what ads are losing - a smaller context/subset of the broader case.

BL: It’s possible that knowing the page URL at the losing time would be enough information to know the bid landscape, at least for the buyer use case. I would gladly take the URL over nothing. I’d prefer more info, like size, etc. but URL is a starting point. Could the browser give a generic report to everyone on the losing side?

MK: Thanks, good discussion. All things Chrome has been considering and looking forward to ongoing discussion. Any other thoughts before moving on?

Bartosz Łoś: I agree with Basille that it’s very important to know when buyers lose their bids, but as I understand he focused mainly on logical reasons - would it be important information from a performance standpoint? Eg - we lose bids due to timeouts, etc? Would be helpful to understand performance by device type, operating system etc. as well - to help troubleshoot based on env and optimize accordingly.

MK: Good use case.

BM: Would be great to have an issue started so we can all discuss it there.

MK: Agreed - issue is the right place. Supraja? Issue [#218](https://github.com/WICG/turtledove/issues/218) created (for seller reporting). We can create a new one for buyer loss or expand.

JP: Already have issue for buyer loss - [#239](https://github.com/WICG/turtledove/issues/239).

MK: Topic #2 on suggested agenda item, also from Supraja:

SS: What’s the time period that you want notes speakers to go back and update?

MK: I take the notes and dump them into Markdown the next day. Any adjustments the same day will be automatically incorporated. After that, requires PR to fix. No real time limit.

MK: That’s all the pre-populated agenda topics. Other topics to discuss? Hopes, aspirations, New Year’s resolutions? Or we could end early…

WB: First, thanks. All looking forward to a success plan… which is… when? We’ve been waiting since Q2 of last year and this is a big commitment of time and it would be very helpful to know when this will land.

MK: We are eager to get an announcement out as well, but we need to be ready to make an announcement before we can say anything. I appreciate the request and can’t give a useful answer today.

BM: Would it be coming in the next couple weeks?

MK: I can point out that the Privacy Sandbox site still lists… well, we are in the quarter that the timeline indicates we ought to be giving you something. So… nothing new to give. The monthly updates to privacysandbox.com/timeline is the official channel for timeline communications. That is where you should look.

BM: In terms of your question about hopes, one of mine is that we’ll get traction and be able to test some things.

MK: Couldn’t agree more.

JK: I want to point out that you can test things using command line args in Chromium. You should definitely do that. Google Ads already has integration tests that utilize this. Worth doing.

MK: Great point - lots of implementation work going on that’s available in Chrome Beta, Chrome Dev channel. Not something for outside users, but for your own development it’s a good resource.

MK: Okay, anything else before we wrap up? … Great to see you - wish you a healthy 2022 and see you in a couple weeks!
