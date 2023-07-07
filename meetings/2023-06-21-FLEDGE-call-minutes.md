# Protected Audience (formerly FLEDGE) WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday June 21, 2023


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Youssef Bourouphael (Google Privacy Sandbox)
3. Nick Llerandi (Triplelift)
4. Andrew Aikens (TripleLift)
5. Sven May (Google Privacy Sandbox)
6. David Dabbs (Epsilon)
7. Orr Bernstein (Google Chrome)
8. Paul Jensen (Google Chrome)
9. Joel Meyer (OpenX)
10. Maciej Zdanowicz (RTB House)
11. Fabian Höring (Criteo)
12. Sid Sahoo (Google Chrome)
13. Michal Kalisz (RTB House)
14. Jonasz Pamula (RTB House)
15. Abishai Gray (Google Privacy Sandbox)
16. Manny Isu (Google Chrome)
17. Kevin Lee (Google Chrome)
18. Marco Lugo (NextRoll)
19. Tamara Yaeger (BidSwitch)
20. Don Marti (Raptive) (the company that used to be CafeMedia)
21. Caleb Raitto (Google Chrome)
22. Risako Hamano (Yahoo Japan) 
23. Martin Pal (Google Privacy Sandbox)
24. Andrew Pascoe (NextRoll)
25. Stan Belov (Google Ads)


## Note taker: Joel Meyer


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here — no agenda no meeting!]



*   [Leave all IGs for a given domain #475](https://github.com/WICG/turtledove/issues/475) [Gianni Campion]
*   [Click related data in browserSignals #579](https://github.com/WICG/turtledove/issues/579) [Maciek Zdanowicz]
*   "[Mode a](https://developer.chrome.com/en/blog/shipping-privacy-sandbox/#mode-a)" plans, timeline [Jonasz Pamuła]


# Notes

Michael Kleber: Please sign in. If you speak, please take a moment to verify that the notes reflect what you intended to say - scribes are only humans. Three items on the agenda, before we get there, a few notes on schedule: the meeting two weeks from now falls on July 5th, which will likely have a lot of people on vacation, including myself and Paul Jensen. So we will follow up in a month on July 19th, when Paul Jensen will lead the meeting. (David Dabbs notes this will be right after Chrome M115). While there are a lot of APIs intended to ship in 115, that does not automatically mean they will be enabled. As with many APIs in Chrome, the API will generally ramp up over the course of the month between M115 and the next release, as traffic fractions ramp up around the world. Exciting times ahead.



*   [Leave all IGs for a given domain #475](https://github.com/WICG/turtledove/issues/475) [Gianni Campion]

Michael Kleber: First item is from Gianni - is he on? [Silence.] Moving on and may come back to it if Gianni comes back. This was a holdover from last time.



### *   [Click related data in browserSignals #579](https://github.com/WICG/turtledove/issues/579) [Maciek Zdanowicz]

Maciej Zdanowicz: I work at RTBHouse and this issue is about click related data included in the browser signals. In the browser signals we have some data related to previous events related to the IG. In this case this is previous wins. We would be very glad and have found that it would be useful if previous clicks were also included. [PJ asks what this means in Chat]. This would be previous clicks on the FF for the same ad. Question from Alsonso?

Alonso Vasquez: Can you please help us understand a little bit more about the use case you’re considering? My hope is that this is related to the testing happening in Private Aggregation. Are you talking about troubleshooting and op monitoring or bidding optimisation? A little more color would help.

Maciej Zdanowicz: Sure - I’m a machine learning researcher at RTBHouse and this would be for the bidding optimization scenario. It would be helpful input to the optimization algorithms.

Michael Kleber: Sometimes when people talk about clicks they mean navigation events. Other times it can be other mouse related interactions. Are you specifically talking about navigation or more general prior interactions?

Maciej Zdanowicz: I believe navigation events would be sufficient for our purposes.

David Dabbs:Yes, what would sync up with the FF private aggregation reporting.

Michael Kleber: For FF we tried to make it possible to define other types of interactions that may line up with MRC Viewability, for example. However, the more complex the use case you support, the more risk there is for abuse… eg, insert a captcha that can be used to join info on another site down the road. But for a navigable click, there doesn’t seem to be as much concern. Paul - what do you think?

Paul Jensen: Well, this is cross-site information of a sort.

Michael Kleber: We already have this in the case of previous wins, the simplest case I can think of would be adding a boolean to this that tracks if the ad was also clicked on.

Paul Jensen: Okay, in the issue there’s discussion of a separate structure to track this information.

David Dabbs: Seems like MK Is making a counter-proposal that differs from the issue.

Michael Kleber: Yes, I proposed a simpler solution.

Jonasz Pamula: Yeah, the boolean would work.

Paul Jensen: From a privacy perspective that doesn’t seem too bad since there’s fewer clicks.

Michael Kleber: Once you’re already recording wins, I don’t think there’s any incremental privacy loss(risk?) from recording if it was clicked on.

Paul Jensen: If you’re recording clicks separately then it seems like it could be more prone to privacy loss, especially if that can be joined with other signals advertisers get.

Michael Kleber: Yeah, but if it’s just a signal on an existing win, I think it’s less of an issue.

David Dabbs: Would this information be subject to DP noise?

Paul Jensen: I think we’re talking about bidding inputs, which are generally un-noised. The reporting data is typically what gets noised. And worth noting that we recently switched to milliseconds vs seconds for some data, because that’s the standard jS timestamp.

Michael Kleber: Confirming no noise involved here. As a feature request, seems like there is no problem with this but we can’t promise anything regarding delivery timeline. But in principle there is no problem and we’ll just have to see when we get around to implementing.

Maciej Zdanowicz: Great, thank you!

Paul Jensen: Will this break people who use the existing API? It’s a vector of two items right now, and we’ll be adding a third.

Michael Kleber: Hmmm, it’s a list of two element lists, not a struct…. Hmm, who designed that? (spoiler alert: MK)

Paul Jensen: Anyone who counts on the length of the list being two could be impacted.

David Dabbs: I have a meta-question about Maciej’s (brief explanation of Polish names). There was a feature request, some dialogue, and then agreement. Where do we memorialize that this type of thing happened and track it so that more than just those who are present know about it?

Alonso Vasquez: As far as the intake mechanism, our current method seems to work. We have the calls, GH, and we have deployed resources through our partnerships team. Whenever we have these meetings that result in good feedback, we have a mechanism for reconciling that with GH and our internal process. So the TL;DR: is that we have a process and it’ll all be tracked. In a nutshell, that’s my job.

Michael Kleber: The important takeaway from David’s question is that it would be nice if there was some publicly available list of backlog feature requests that have been accepted. Some way that doesn’t require people to troll through all the notes, etc.

Michael Kleber: Is Gianni here? [Nope.] Moving on.



### *   “[Mode A](https://developer.chrome.com/en/blog/shipping-privacy-sandbox/#mode-a)” Plans/Timeline

Michael Kleber: Jonasz put this here?

Jonasz Pamula:Yes, thank you. I have a comment and a question. The comment: as RTBHouse we would like to signal our readiness and willingness to participate in Mode A and Mode B. We would like to start small. One of the proposed labels was 0.25% - that would work for us. 0.1% would work as well. The question: Is there a reason to start it in Q4, or could we start it earlier? If we could start earlier, we would like to do so. We imagine it will take some time to integrate with these labels and pass them from the sell side to the buy side, which reduces the time for actual testing.

Michael Kleber: Let me provide some context first: there are two different types of testing of the Privacy Sandbox APIs. If you attend here, we’ve talked about it before, and there was a difference of opinion both here and elsewhere on which way testing should be done. As a result, we chose to do both. Perhaps everyone will be happy, or no one will be happy.  Reference material is https://developer.chrome.com/docs/privacy-sandbox/chrome-testing/ 



*   In Mode A, there are some branches of traffic at small/varying percentages that are labeled as “this is a 0.1% branch” indicating that 0.1% of people are in this experiment. And there’s another branch that indicates this is the control group. This lets people who voluntarily want to ignore 3p cookies and try the PS APIs for one group of people to have an A/B comparison. The labeling allows all participating to agree on the experiment and control groups.
*   This is contrasted with the Mode B approach, which is happening in Q1 of 2024 when some slice of traffic will have 3rd party cookies turned off. In this case, the loss of cookies is not voluntary.

Mode A is scheduled to happen earlier, and the question is how soon can it start? The answer is that we don’t have the right people in the room to talk about the start date and the prereqs that must happen beforehand. So I don’t actually know if we can answer that question right now. I understand the desire to enable Mode A as soon as possible and I will pass along your comments to the people who are running the experiments across all of Privacy Sandbox and we will update our timelines as soon as they are nailed down. We definitely appreciate the feedback on traffic fraction sizes is helpful and would be best put in GitHub where it was asked for. Putting the timeline comment there is also helpful.

Jonasz Pamula: Thanks, we’ve already put comments in the GH issue at https://github.com/GoogleChromeLabs/privacy-sandbox-dev-support/issues/112 and we are happy to start.

Michael Kleber: Fabian - did you have a question?

FH: I have a question related to Mode B. Can you explain how you will expire the cookies from one day to another? And how does this relate to CHiPS?

Michael Kleber: I don’t know exactly when Mode B traffic will start, but the way you should think about it is that for the people who are in the Mode B branch, it will be as if that user had toggled the Chrome setting to disable 3P cookies. So all the 3P cookies previously stored will stop being sent/visible/available in any way and new cookies won’t be set. If you want to experiment, feel free to check the “remove 3P cookies” box and see how things behave. I don’t believe that CHIPS should be impacted at all. The partitioned cookies that are a part of CHIPS should be carried through the way they are today. David?

David Dabbs: Going back to the question earlier about where we’ll discuss these changes, I asked Rowan Merewood and he said there would be some forum created where it could be discussed. I would strongly encourage that to happen soon. My question about Mode B - you didn’t say if you would label Mode B, but I think you need a good before/after measure, so will you label Mode B in advance so you can see the change in performance before/after the removal of cookies?

Michael Kleber: I’m not sure - that’s a good question. Asking on GH seems to be the right place, which you have done.

David Dabbs: Related to that, it would be useful to everyone if someone who is in that cohort has all the PS features enabled.

Michael Kleber: Yes, I think what you’re describing is the intention. The Mode B happens when all the PS features have reached GA and are available. That should be the same for people in any of the branches of experiment. Well, mostly — I will mention that there is a sub-branch in Mode B in which the APIs are not available just as a hold-back for further measurement.

David Dabbs: You described Mode B as someone who elected to turn off cookies - is that the same as the state after 3P cookies are deprecated?

Michael Kleber: I believe there is no difference between those states. If something comes up then I will have to revisit that answer, but I believe these are the same states.

David Dabbs: [Some question about canary and a feature that allows sites to use 3P cookies.]

Michael Kleber: Unclear on what you’re talking about. There are user controls for a lot of things. I don’t know if there is a user control along the lines of what you’re discussing.

Sid Sahoo: [Questions in chat.]

Michael Kleber: CHIPS is entirely separate from the discussion. Everything else is unpartitioned, where CHIPS is partitioned by site. Anything that is unpartitioned will go away.

Alonso Vasquez: You were asking about user experience and changes to user controls…

David Dabbs: No, not really. I thought there was some control where users could enable 3P cookies on some sites. I could be wrong. But if that goes away, then it doesn’t matter.

Michael Kleber: Right now in Chrome Stable settings there is a switch to allow 3P cookies or block 3P cookies. But there is not a site specific control to enable 3P cookies. Wait.. might be a setting to “always allow cookies”? Hmm, I don’t know the answer. Very frequently there are obscure controls that won’t get used outside Enterprise Policy settings. EG - to enable some intranet sites to work. The mere fact that there is a UI with a list of sites that make an exception doesn’t guarantee it will stay or go, but I can’t tell you exactly if it will stay or go in the post 3P cookie era.



### *   [Leave all IGs for a given domain #475](https://github.com/WICG/turtledove/issues/475) [Gianni Campion]

Michael Kleber: Based on reading Gianni’s original request I think I know the answer to Paul’s question. This looks like a request for the ability to leave all the IGs that a user was added to by a particular ad-tech on a particular site. That seems reasonable to enable.

Paul Jensen: Yeah, if they all have the same owner and were on the same site, it seems reasonable to enable them to leave.

Michael Kleber: Yeah, this is something you could’ve done with a 1P cookie, so seems reasonable. This is another example of a feature request we would be happy to support and just need to put on the backlog.

David Dabbs: You could use the clear-site data header or something similar.

Michael Kleber: I think we’d prefer to take requests from people who actually made the request, so I don’t feel like I want to make a header and attributes without the use case in hand.

Paul Jensen: I’d like to learn more about Gianni’s motivation. He mentioned two things, and i’m curious if other people have similar sort of needs: 1) Resets the state of device IGs to ensure it’s in sync with the server. This would let you know that they can be in new IGs without being in old ones.

Michael Kleber: Sounds like it’s worth discussing so I’ll carry it over to the call from a month from now, or we can talk internally, unless other people have input and similar requests - we can engage over GH or a future call.

Michael Kleber: David - did you want to add something?

David Dabbs:I think there’s a document that someone maintains call FOT (First Origin Trial) release notes. One of those documents contains the list of what I call asterisks in this feature and also other ones. I guess, specific to PAAPI, since we’re just about to be beyond the first OT, it would be really handy to have a “here’s the known knowns” that are a part of the product not intended to last forever so we can reason about the tech debt that’s built into the product that we’ll have to address over time. Right now it feels like it’s in the spec, in the explainer, in diff places.

Michael Kleber: There is a blog post from Tris that provides the info you’re looking for. There it is, it’s on the developer site: [https://developer.chrome.com/en/docs/privacy-sandbox/fledge-api/feature-status/]. The page indicates feature statuses, including things that are there now and will go away sometime in the future. I’ll make sure that link is in the notes also.

David Dabbs:[Question regarding other headers, etc that might change.]

Paul Jensen: I published the intent to ship to the email list yesterday, so we’re currently in a transition period. But once we’re rolled out to everyone we’ll be unable to willy-nilly make changes to the API as it’s shipped. Going forward, after we deploy to stable branch, we’ll be subject to Chrome’s general release process for changes we make. This is called the blink-release process for visible changes. Beyond 115 any change will result in publishing an intent, eg intent to prototype, intent to experiment, intent to ship, etc. That will be the way to find out about what’s changing. Each of those should keep the spec up-to-date on where it’s at. So we’ll move away from updating release notes and into the blink-release process.

Michael Kleber: In the same way that the future central repository for requests is a good idea, I think a central repository for coming changes would be a good idea. That seems like a reasonable request.

Paul Jensen: The Chrome Status page offers something like that.

Michael Kleber: That’s right, so the Chrome Status for Privacy Sandbox will have that info. But it would be nice if we had something better than that.

Michael Kleber: Great, thank you all for the discussion. Much of it focused on better sources of information, which is great feedback. And I think we’re set. As noted at the start, no call in two weeks (July 5th). Hope all of you have a good start to the month. Our next call is in four weeks on July 19th when I’ll be on vacation - have a good time without me! And Paul will run the meeting IF there is an agenda.
