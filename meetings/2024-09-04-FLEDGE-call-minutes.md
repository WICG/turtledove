# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Sept 10, 2024


To be added to a Google Calendar invitation for this meeting, join the Google Group <https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/> 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!  

1. Michael Kleber (Google Privacy Sandbox)
2. Sven May (Google Privacy Sandbox)
3. Roni Gordon (Index Exchange)
4. Brian May (Dstillery)
5. Isaac Foster (MSFT Ads)
6. Luckey Harpley (Remerge.io)
7. Matt Davies (Bidswitch | Criteo) 
8. Arthur Coleman (IDPrivacy)
9. Courtney Johnson (Google Privacy Sandbox)
10. Orr Bernstein (Google Privacy Sandbox)
11. Matt Kendall (Index Exchange)
12. Paul Jensen (Google Privacy Sandbox)
13. Kevin Lee (Google Privacy Sandbox)
14. Fabian Höring (Criteo)
15. Harshad Mane (PubMatic)
16. Ivan Staritskii (Bidswitch | Criteo)
17. Tamara Yaeger (BidSwitch)
18. Laura Morinigo (Samsung)
19. Laurentiu Badea (OpenX)
20. Alexandre Nderagakura (Consulting | Not affiliated)
21. Bharat Rathi (Google, Privacy Sandbox)
22. Jeremy Bao (Google Privacy Sandbox)
23. Andrew Chasin (Google Privacy Sandbox)
24. Koji Ota(CyberAgent)
25. Andrew Pascoe (NextRoll)
26. David Tam (paapi.ai)
27. Suresh Chahal ( MSFT)
28. Shivani Sharma (Google Privacy Sandbox)
29. David Dabbs (Epsilon)

## Note taker: Tamara Yaeger

# Agenda

## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https\://www\.w3.org/community/wicg/

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https\://www\.w3.org/community/about/process/cla/ 


## Suggest agenda items here (meetings with no items may be canceled)

- Matt Davies: <https://github.com/WICG/turtledove/issues/1220>

  - Quick update on the plans?

- Fabian Höring <https://github.com/WICG/turtledove/issues/529> 

- Matt Kendall - review of deals explainer <https://github.com/WICG/turtledove/pull/1237/files>

-


# Announcements

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See <https://github.com/WICG/privacy-preserving-ads/issues/50> for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!! :) 


# Notes 

## Matt Davies: <https://github.com/WICG/turtledove/issues/1220>: Quick update on the plans?

Michael Kleber (Google Privacy Sandbox)\
Looking for update on <https://github.com/WICG/turtledove/issues/1220>. 

Paul Jensen (Google Privacy Sandbox)\
Centered around how BidSwitch can report on bids passed through them. 

Matt Davies (BidSwitch)\
Are there any further questions, any developments?\
\
Paul\
Came down to adding another party to get through a port. Sounded like an automatic event would be adequate.\
\
Michael\
My recollection is that there was a problem of knowing successful bid; when a particular DSP is buying in component auction run by particular component SSP. Even in that case, was BidSwitch involved in what buyer was in the auction.\
\
Ivan Staritskii (BidSwitch)\
It’s not a problem for DSP to understand whether BidSwitch was in contextual bid; we need to support to send report to function executed twice. Currently SSP function for their own impression tracking, we ask them to send report to BidSwitch. Need to be able to call it 2x. We asked SSPs; not a problem to understand whether they need to send impression multiplication to BidSwitch. They have this in auction config.\
\
Paul\
Proposed during contextual call theywould give list of DSPs they would like in the auction, SSP would include it in their list of buyers. Meant to convey that BidSwtich was the reason they were brought in.\
\
Michael\
So SSP has responsibility when they do component auction config; identifying buyers. At the time that SSP report result is running, they know who winning buyer is, whether integrated directly or through BidSwitch (upon request of accepted buyers), at that moment in report result, the ask is for component SSP to say in addition to send report, to send “win” report to myself, also send to additional party involved in the auction. Should it be a duplicate report? Or are there some diff URLs to send to different partners?\
\
Matt\
What we need is, did an impression occur, what was the spend?\
\
Ivan\
We provide instruction to SSP on what they need to do in worklet to construct our impression URL.

Paul\
We have limits on 10 additional origins you can report to.\
\
Michael\
It’s a DSP concept, but this is really an SSP? Maybe halfway in between, seems this is more about SSP choosing to send info to additional party. Maybe we should have limits? From info POV, no different than if DSP got bid on their own server, for ergonomics it would be better to go directly.\
\
Fabian Höring (Criteo)\
It doesn’t need DSP support to fire the event, one idea would be to introduce a reserved event for displays, then you could register endless fenced frame reporting events for displays.

Michael\
Better thought of as a SSP side event, for what you’re talking about ; this ad wants to display x reporting to y collection of parties. Whereas this is focused more on structure of auction, not something knowable at time of ad construction.

Fabian\
Don’t agree, if SSP could handle and display FF reporting event w/out any creative data; it would be a way to provide same features calling sendReportTo multiple times

Paul / Michael\
It is triggered by render\
\
Fabian\
If you have same event, you don’t need cooperation to send this out\
\
Paul\
I think there is an auto event–\
\
Fabian\
Only for clicks / redirect. There’s no auto event for displays / wins.\
\
Michael\
When we added auto to deal w a few things like clicks, we did it to bring it up to level of what’s supported for render reporting.\
\
Matt\
Important distinction, it would need be win + render.\
\
Michael\
That’s how it works, the send report to things are only triggered by 

Fabian\
The SSP cannot register 2 URLs, if it could it would solve this issue.\
\
Michael\
Sounds like we have same feature request to send report to additional party upon render of winning ad; only diff between Matt’s and Fabian’s proposal is about whether it’s SSP’s report result code or DSP report win code that does that invokes to send report twice, is it that correct?\
\
Fabian\
If it can be handled by different FF reporting event we would have flexibility to choose whether to do it on DSP or on SSP side.\
\
Matt\
Could be useful for triangulation of reporting\
\
Michael\
Good for investigating discrepancies.\
\
Ivan\
Just want to highlight for us it’s most important to use this for SSP more than once.\
\
Michael\
To send things to multi parties POV is already something well established on the buy side of things, we don’t have equivalent on sell side, makes sense. Goal is to have either DSP or SSP or both have additional parties using upon reporting using same reporting to send back to themselves.\
\
Ivan\
For us better for SSP side, the auction is to provide this ability to SSP side.\
\
Roni Gordon (Index Exchange)\
Just want to better understand what happens to the SSP AV call and reporting result w respect that URL is being represented by buyer origin thru BidSwitch? We would need to know that kb time, at report results time.\
\
Michael\
At RR time, the discussion we had earlier was that at RR time, the SSP logic has all info it needs to determine whether this bid came thru direct or via BidSwitch integration. Passing in as part of auction config, the SSP passing list of buyers they are integrated w directly vs w intermediary, is enough info at win time to know how to categorize it.\
\
Roni\
Assumes you don’t have the same origin running through both, so you can never have both in this proposal\
\
Michael\
Any particular buyer would be buying in only 1 of 2 ways, this had been worrying me from last July, assured that it was agreed was not necessary\
\
Roni\
It just means you have to make a decision as an SSP. The other point, from the SSP KV call, how to know which entity is representing this buyer origin? It’s just a render URL out of thin air. 

Michael\
Including the buyer w the render URL in the SSP KV call seems reasonable. Would it solve this problem or others for SSPs? Should branch off the discussion.\
\
Paul\
Roni is wondering about whether bid came from DSP or another party. The seller will know at score ad time\
\
Michael\
Based on auction config, would only be available if SSP pushed info into their own KV server.\
\
Roni\
All speaks to relationship between URL origin and buyer origin and how much overlap there is.\
\
Michael\
Obvious answer is to do 2x as much work, but we won’t do it that way.\
\
David Tam (Paapi . ai)\
In the auto beacon we put an impression event, what is it?\
\
Michael\
It is render\
\
Paul\
You can make it anything, viewability constraints, when they’re met\
\
David\
How would you know in a fenced frame when those constraints are met?\
\
Paul\
Like if they’ve been rendered 5 secs\
\
David\
Is it on the DSP side? Can it be placed in FF?\
\
Paul\
Yes\
\
Matt\
Do we need to indicate in the flow that BidSwitch was involved in the KV logic? For example, we receive auction config from DSP, could we add intermediary URL and SSP would know the OpenRTB came through BidSwitch? Does that all merge together from SSP POV?\
\
Roni\
We would know on the OpenRTB side that it came from BidSwitch and your provider signals are for you, but when dropped on page it’s lost because it goes back to render URLs and origins.\
\
Matt\
That’s what I was worried about getting lost on page when auction config is created.\
\
Roni\
It exists in auction config, but APIs don’t pass it around. It’s more about if buyer origin is shared and URLs are shared and could be uniquely identified as BidSwitch.\
\
Matt\
It’s rare to have a connection via BidSwitch and also via direct. Rare if that’s the case. Any existing moments where that is the case. There could be a 3P operator like Magnite. In this instance it’s likely that it would be 2 separate signals entirely.\
\
Ivan\
It’s up to SSP to decide whether to put BidSwitch in the config.\
\
Paul\
Seller could put signals for score ad, could remark to buyer\
\
Michael\
That means BidSwitch acting as buyer is action that is computable at every point where it’s relevant in the auction, and reportable by both DSPs and SSPs. the one gap, it’s not something automatically noble from KV server; not from info sent to trusted server. It might be possible to re-compute server side, there would probably be gaps and outstanding corrections on synchron. Aside from the Roni SSP trusted server availability question, we just need to make call to send report w multi destination domains. Well understood on DSP side, covers FF reporting events, maybe should cover send report as well. Not supported on SSP report results. Is that a good summary?\
\
Shivani Sharma (company?)\
We do need to send report for diff destinations, but why do we need additional support in FF?\
\
Michael\
This was the point, how do we get it triggered by render event of the frame, as opposed to later event. DSP can get it directly from existence of report send URL in report win function, but can’t be called multi times. We can extend it to allow send result to be called multi times, there would be queue for render event to happen. Or we can use the same FF mechanism and have new reserved event for render.\
\
Fabian\
If I outsource creative rendering I need them to fire the event, I only care about the event. I only want to take this event for billing, I don’t care about event data inside creative, so it would be the most flexible solution.\
\
Shivani\
There’s also a destination which is seller or buyer; either can register for new event type. Is the suggestion to add new destination type? It’s either seller or buyer worklet\
\
Fabian\
I don’t need dest type, I register as buyer or seller and event fires off automatically\

## Fabian – <https://github.com/WICG/turtledove/issues/529>

To recap from last year, there is a limit of number of IGs per owner. Now it’s 2000 (10MB)  increased from 1000. What happens when same owner joins IG in same browser and quota is exceeded?

Right now we get rid of IGs that are going to expire the soonest. Not FIFO because diff IGs can have diff expiration times. Some IGs can stick around for 3 days, some 30. This is question if we can prioritize IGs so that higher priority IGs stick around if we run into the limit? You can already use variety of mechanisms to have owner of IG choose which IGs bid if there are too many for auction. 

My read on this is that it was discussed in April, someone was asking for it in May but has sat untouched for the last year and a quarter. Would there be utility benefit in this mechanism?\
\
Fabian\
This comes up because today we have cases of ad components where we get over 10MB limit and have no feedback from Chrome on how it behaves and how to control it. It would be a useful feature to take out less important IGs from our side. It would improve performance if we can control the most important IGs.\
\
Michael\
Hitting limits and having control in managing API better helps prioritize things, it’s a reasonable ask to allocate resources.\
\
Brian May (distillery)\
It is common in retargeting use cases to have segments, or IGs, that have a short lifetime for things like trying to encourage someone to return to an abandoned cart; however, if they don’t respond in a couple days they probably won’t, so you’d want to free up the slot for other ads. In these cases, the short lifetime segments tend to be high value, so not having them deleted because they’re expiring soon would be important. I’m not sure priority is enough, maybe need to have a means within priority of determining how things should be expired out. More to come.\
\
Michael\
Will think about how to engineer and prioritize this.\
\
Fabian\
We’ll add to Github\

## Matt - <https://github.com/WICG/turtledove/pull/1237/files>

Matt Kendall (Index Exchange)\
I’ve reviewed it and go the gist, had a question about use cases for buyer reporting ID, understanding intention versus selectable buyer / seller reporting IDs.\
\
Paul\
Note buyer and seller IDs existed before deals support implementation. Could be useful as conveying seat ID, but in that case there might still be use case where buyer wants to convey reporting ID only from report win, not report result. 

Kevin Lee (Google Privacy Sandbox)\
There is a deals developers guide coming out very soon to clear this up

Michael\
I will be on vacation next week. This call had no agenda items until call happened; next week we can have a call w Paul running things. Cancel a day beforehand if there are no agenda items. (Check Tuesday)
