
# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday June 22, 2022


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Fabian Höring (Criteo)
3. Antoine Rouzaud (Criteo)
4. Russ Hamilton (Google Chrome)
5. Paul Jensen (Google Chrome)
6. Angelina Eng (IAB / Tech Lab)
7. Brian May (dstillery)
8. Bartosz Marcinkowski (RTB House)
9. Jonasz Pamuła (RTB House)
10. Caleb Raitto (Google Chrome)
11. Philippe de Lurand Pierre-Paul (Google Chrome) 
12. Joel Meyer (OpenX)
13. Sid Sahoo (Google Chrome)
14. Andrew Pascoe (NextRoll)
15. Matt Menke (Google Chrome)
16. Andrew Aikens (TripleLift)
17. Katherine Wei (Zeta Global)
18. Stan Belov (Google Ads)
19. Tim Hsieh (Google Ads)
20. Michael Burns (Google Ads)
21. Marco Lugo (NextRoll)
22. Zheng Wei (Google Ads)
23. Sangram C (Neustar)
24. Yuval Tanny (Oracle Advertising)
25. Aleksei Gorbushin (Walmart)
26. Sangharsh Kamble (Walmart)
27. Mike Pugh (IPONWEB)
28. Peiwen Hu (Google Chrome)
29. Aditya Desai (Amazon)
30. Alex Cone (IAB Tech Lab)
31. Sergey Tumakha (Microsoft Ads)
32. Brad Rodriguez (Magnite)


## Note taker: JoelPM


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here]



*   Assess the true performance of FLEDGE - https://github.com/WICG/turtledove/issues/317
*   “Abandoned” auctions and reporting
*   Support for publisher- or seller-rendered formats (video, native) \
https://github.com/WICG/turtledove/issues/265 


## Notes

Michael Kleber (MK): Please sign-in. This meeting takes place under the auspices of the WICG, it is important that attendees are w3c members and part of the WICG as that provides the IP protections necessary for development of standards. Please be a member if you are going to contribute. There are several suggested items, please suggest more or express preference now. [See list above.]

MK: No objections so far, we will address them in the order listed. Starting with #317, assessing the true performance of Fledge - by Criteo.



*   Assess the true performance of FLEDGE - https://github.com/WICG/turtledove/issues/317

Fabian Höring (FH): Can we start with some discussion of how the OT is going? We are having some small issues, but should be resolved soon and we’ll be getting some traffic in partnership with Google Ads. Opened this ticket, not expecting immediate response due to the complexity. This is not about the runtime/CPU performance, it’s about the ML performance - our ability to make money and assure good rev for the pub and good outcome for advertisers. I can go through the ticket. The idea is to discuss and hopefully get agreement that this is important for Fledge in general. Not expecting a solution, but just awareness/agreement of importance. Let me share my screen…

FH: The basic idea is to remove the 3p cookie, and that means we don’t have as good information as before to link pub features with adv features. We won’t get this anymore. So we implemented a new system, and we don’t have full traffic yet, but it would be a big surprise if the ML performance would be as good as before. We are kind of expecting that for ‘good’ opportunities we will lose against existing bidder, and for ‘bad’ opportunities we will win because others know it’s a ‘bad’ opportunity. It would be good to run the test in a fully independent manner so we’re getting a true assessment of performance. It would be helpful if we could do true A/B testing. Would GAM be able to enable this some way by removing the cookie, but handling freq capping and some of the other complexities?

MK: Good question. The answer needs to come from different parties. Chrome, as a browser, is very interested in helping facilitate the type of testing you’re talking about. But there are probably some ways in which this kind of testing (absence of 3p cookies) needs to be based on behavior changes by other ad-tech companies that also want to experiment. It is only possible to this kind of real test by having all ad-tech companies participating to voluntarily agree to do this testing on their own - it’s difficult to force it on people. As long as you have some willing to participate and some not, it doesn’t seem feasible for 3p cookie removal to create the type of test you’re looking for. We need to collaborate across browser and ad-tech participants.

Stan Belov (SB): I will echo MK’s feedback that there is a very large part here related to ad-tech participants being willing/ready to do this kind of testing. This is broader than Fledge. There are many use cases that ad-tech participants rely on 3p cookies for (eg freq capping, interest advertising, audience modeling, conversion attribution) and just saying 3p cookies are gone on this test slice of traffic, means a lot of companies would need to invest time in being ready for that state if your objective is to measure the future state of no 3p cookies. I think it will take a lot of time and incremental effort from many of us to get to that state, and it would be a very complex setup. If possible, I will try to brainstorm how we could test fledge in isolation.

FH: That’s what I’m speaking about - I’m interested in Fledge. I’m interested in agreement that this need is the ultimate goal.

MK: If the goal is really to focus on Fledge - let’s look at Fledge and how it performs relative to existing state - that seems like an excellent sort of experiment to do and I completely agree with you. As a browser, if we look to fully isolate to Fledge, it can’t happen by Chrome disabling 3p cookies for a certain population of users or sites because of the other use cases for 3p cookies. The disruption would be too large and would overwhelm any benefit. It seems like it would have to be ad-tech companies agreeing that for some slice of users they would test w/o 3p cookies. The question of how to organize that collaboration seems like a good topic of discussion.

Brian May (BM): I’m going to echo SB and MK - I think that trying to isolate a context w/in the internet would be really hard. At best we can carve out little pockets for a controlled env. The best hope to get anything meaningful is to be very specific about what it is we’re testing for and learn and take a close look at what possible confounding variables there are. To suggest that we can somehow turn off 3p cookies for some slice of internet is not possible.

FH: Not asking for the internet - just Fledge. The OT Is kind of doing that - it enables a set of APIs on some set of users of the Chrome Browser.

BM: Agree, but if you want to do a test you have to isolate that and there’s no way to disable cookies. I’m not against experimenting.

MK: The focus of the discussion needs to be how exactly it is possible for us to organize that sort of experiment, even acknowledging that it will be tricky to get the variables right.

Angelina Eng (AE): The IAB, me and the programmatic & data center, is partnering with Prohaska consulting to aggregate and collect data. We’re looking for participants to provide historical data or run a campaign. And we want to do an industry analysis of all the different targeting use cases. If you’re interested in participating, please shoot me an email at [angelina@iabtechlab.com](mailto:angelina@iabtechlab.com). The idea is that until Fledge/Topics are out of incubation/testing, and until it gets to a phase of actual experimentation, we can do a comparison to all the different targeting tactics and how that’s affecting CPM rates, CTR, conversion rates, post-view conversions, CPA ROI/ROAS, etc. If interested, send me an email. The idea is to get an industry supported analysis. The broader the input and analysis, the better. Looking for an independent and impartial analysis.

SB: I wanted to brainstorm for a bit on FH’s issue. One potential avenue the testing could take is that some ad-tech companies volutnarily express a desire to participate in these tests and say “for this test slice of traffic, we are only going to use Fledge for remarketing.” Any sort of additional limitations on 3p cookies might have unintended consequences. Some sort of voluntary mission to not rely on 3p cookies might satisfy what Fabian describes in the issue. The other thing to think through is, how can we all agree that this is the same slice of traffic. We would want some sort of coordinated experiment running. That would include diff supply paths, like header bidding and OB. Ideally we’d want some way for this traffic to behave consistently across all of these supply paths. Perhaps the browser’s help might be needed for that. It’s not clear - maybe somehow it could be done on the page via prebid? But perhaps the browser could be of help.

MK: I think that’s exactly right. As long as everyone involved in that page agrees to behave the same, and there’s also agreements on the use cases that can continue to happen in the normal way, that combination of agreement among a bunch of parties seems like the necessary ingredients. I’m not sure what the browser contribution here out to be, though the question of how to consistently identify and audience across a consistent set of people seems like something that could be brainstormed. I’d like to point out one other thing before next on queue: there is one tricky part that hasn’t been mentioned yet. Certainly, not all ad-tech parties will be ready to engage or willing to engage. Figuring out how to handle that in the experiment and control will be very important. If you involve this set of companies in either group, it will invalidate your analysis.

Aleksei Gorbushin (AG): Not sure I understand the question. I just want to say that we thought about an A/B test that can help us to figure out what is the perf of Fledge compared with original targeting. We tried to implement this with Parakeet, but right now it’s postponed. The idea, i believe, can also apply to Fledge. The idea is that we can figure out for the visitors on our site, do they have Fledge activated or not? After that we can split this evenly. Some of them we can use the 1p cookie and also use the associated 3p cookie with some existing partner we have. One part we will target traditionally (‘control’) and another part we will add to an IG. Later we can come to a DSP and setup a campaign to target each party and see how it performs based on CTR and conversions. That experiment should work and show the difference between the two approaches. What do you think? Will it work or is it too small?

SB: I think it might not take into account other participants and their way of using Fledge or relying on 3p cookies and the shifts in pricing of impressions.

AG: When you say another participant, who do you mean?

SB: Another buyer.

AG: With Parakeet we wanted to do it with MSFT and have them function as supplier and DSP to limit this. We talked to Google Ads to see if they would be interested, but didn’t get the same response.

Joel Meyer (JM): Is this feasible as a whole?  If you have one buyer who keeps targeting based on cookies, do we have any hope of getting any measurable result?

MK: Anybody have thoughts on that?

BM: One possibility is to have everybody participating in the experiment indicate that they are contributing, mark it as such and try and analyze that.

Alex Cone (AC): It seems like one potential (not perfect) path is to do what Prebid can do today using existing controls over IDs. If Prebid had a config to say that we’re going to run a certain amount of imps where we don’t send an ID, and there’s a flag that says why (to distinguish the reason for no ID) - then you’d need some way of saying that DSP who want to buy on ID vs those who don’t, (Joel to summarize this a little later.)

JM: To some extent yes — DSPs that have an adapter themselves will naturally get a 3p cookie in headers, can't limit that, so you  would need to get publishers to turn off those DSPs, which would be hard, would lose money

AC: Who all have prebid adapters as DPS?  Criteo does but they want to test, as evidenced here.

JM: RTB House has one

BM: Prebid is a loosely-affiliated group, getting them to converge is difficult

AC: Difficult but not impossible

AE: I've been using the Browser-OS Taks FOrce as a forum for these conversations.  More on the business side.  I'd love to create a round-table for an hour or two to develop a framework, separate from the Projaska thing.  bring in some data analysis to agree to a standard framework, plus enegineers who understand the pipes and signals.  July, maybe?  Let me know

MK: Okay. That sounds great. I did put myself on the queue to say I think the thing we’re describing here might be somewhat difficult while Fledge is still an OT, because in the case of an OT there’s a lot of work to turn on the availability of Fledge on different pages. From the browser point of view, the next stage is for Fledge to go GA, so you don’t need an OT token to activate it, but only on a small population. For example, on 5% of Chrome stable users, Fledge exists (and not on the other 95%). In that world, the browser is helping with part of the task that you’re talking about - deciding on which pages it would be a good idea to run this experiment. Unfortunately it doesn’t help you distinguish between control and experiment groups. Listening to the experiment description that Alexei described earlier seems like a plausible outcome in that world. On each individual page or site, the goal is to flip a coin and put that 5% of people into either the A or B group. Then it turns into the question discussed earlier - how is it possible to get all the sellers on the page to agree to pass along the group information in a coordinated way and get people to appropriately ignore cookies for the right group.

FH: I think, if this is the case, we wouldn’t even need a control/experiment group. The 5% would be the experiment and the 95% would be the control.

MK: Perhaps the control isn’t necessary - I’m not 100% sure, though. There are enough bumps I’ve encountered by trying to run an experiment w/o control group in the past that I’m not confident in that. Particularly with the potential for ad-tech companies not willing to participate in the experiment. The control would potentially include only companies willing to participate.

BM: I think that if we want to be good scientists we have to figure out what it is we want to measure for and what confounding variables might exist for that measurement. When I think about the Fledge model compared to other existing models, we’re doing something very different. We need the browser to end up on a website where IGs are set, then we need to have enough IG density, then we need to have bids, and when I think about the 5% number, it seems like a challenge to get the intersection between IGs and people bidding on them. People are not set up to develop these IGs. Until then, anything we do based on the bidding on them will be a poor representation of what a Fledge enabled internet would look like.

AC [copied from chat:] There's a bit of a catch 22 I'm seeing here. Market participants asking for 3pc's to be turned off in order to run tests for CMA commitments which itself says 3pc's cannot be shut off until satisfactory results are in place. I wonder if we could show CMA there is consensus to try this test path and therefore get an approval to turn off 3pc's for the % of stable with FLEDGE/ARA.

AC [aloud]: My suggestion is that, if there is consensus coming out of OT into GA, if there was also, for that percentage, a shutting off of cookies, I’m wondering if there are enough people from the groups focused on testing, saying to the CMA, we want a way to be able to test, which requires disabling for a percentage. That seems like a reasonable ask if there are actual industry participants asking for it.

MK: The question, can Chrome turn off 3p cookies for 1% of people right now (or soon) for testing purposes, has a lot of complications and I’m not ready to comment on whether that’s feasible or not. If the result of the collaborative brainstorming is that a consensus w/o 3p cookies would dramatically help the ability to test Fledge, please bring that info and let us know. But that is a pretty highly encumbered suggestion.

Antoine Rouzaud (AR): Just wanted to jump back to Fledge being in GA and waiting for that to test actual performance. In that case I’m not really sure what we’ll be able to accomplish beyond testing the pipes. That seems a bit limited.

MK: Sure, I would say the difference between an API in OT vs in GA, even for a fraction of population, is that during OT the expectation is that the only parties using the API are particularly adventurous people willing to try something new that might change and there is a bunch of cost to investing in trying the API out, but it would not be appropriate for plenty of people until things are more stable. An API in GA, however, is at a state where it’s more reasonable to expect anyone who wants to use the API to start using it, even if not widely available or in use. For cases where avail, it is ready for general use. If we’re talking about an API where one person/dev can do a good job of understanding if that API works for them, then OTs are a good time to try it. Because of the end-to-end testing that Fabian is describing requires so much coordination, I think there is some testing with a smaller number of parties that can happen during the OT stage, but only after GA can we hope that enough people will be working on it to accomplish the large-scale testing between many parties.

BM: I wanted to also bring up the fact that when Fledge is fully in operation we’ll have a very different way of measuring things, which is another problem with trying to do any type of performance assessment prior to that environment. I don’t think it’s very meaningful to test Fledge during the OT because the type of measurement they can do will change.

AE: I agree, but at the same time we have been touting that Fledge is a replacement for remarketing. So when you talk to an agency or remarketer they are going to want to compare the effectiveness of the tactic they spend 40% of their budget on that is going away (3p cookie retargeting). I love this conversation and would like to appeal to you that I’d like to get some data, analytics people, remarketers, in a room to actually talk about what are the available signals and how we go about, if not testing, at least assessing the diff between current retargeting strategies and future strategies. That’s why we’re working with Prohaska - they have the ability. We don’t have storage assets, nor do we have a team of analysts, other than me and someone else.

MK: With that, the queue is empty and we’re pretty much at the end. Thank you Fabian for opening the issue and triggering a very interesting conversation. I look forward to following up. Angelina, do you think browser participation would be useful? If so, please let Chrome know.

AE: Challenging to schedule this in July, but may try. Would like the GAM team to participate. Take your vacay in July and we’ll talk in August?
