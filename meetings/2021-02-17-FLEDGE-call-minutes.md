# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 4pm UTC.

That's 11am Boston = 8am California = 5pm Paris time (the winner of the [doodle poll](https://doodle.com/poll/eht2imrtvec69hxx?utm_source=poll&utm_medium=link)).

(This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit) will be editable by everyone, during the meeting — if you can only comment, hit reload)


# Next meeting: Wednesday Feb 17 2021


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Nicole Lesko (Meredith)
3. Allen Fung (ShareThis)
4. Michael Lysak (Carbon)
5. Brendan Riordan-Butterworth (IAB Tech Lab)
6. Brendan Riordan-Butterworth (eyeo GmbH)
7. Dinesh Kandhari (PubMatic)
8. Jonasz Pamuła (RTB House)
9. Erik Anderson (Microsoft)
10. Ryan Avecilla (Neustar)
11. Basile Leparmentier (Criteo)
12. Erik Taubeneck (FB)
13. Andrew Pascoe (NextRoll)
14. Raj Belur (amazon)
15. Mike Smith (Hearst Magazines)
16. Paul Marcilhacy (Criteo)
17. Greg Bougeard (Teads)
18. John-Marcus Phillips(Nielsen)
19. Jeff Wieland (Magnite)
20. Scott Both (Hearst)
21. Phil Eligio (EPC)
22. Don Marti (CafeMedia)
23. Grzegorz Lyczba (OpenX)
24. Benny Lin (Bombora)
25. Christa Dickson (Meredith)
26. Viraj Awati (Amazon)
27. Théophile du Besset (Scibids)
28. Kelda Anderson (Microsoft)
29. Shivani Sharma (Google Chrome)
30. Brain May (dstillery)
31. Greg Truchetet (Teads)
32. Nick Sanchez (Hearst Magazines)
33. Paul Selden (OpenX)
34. Brad Rodriguez (Magnite)


## Note taker:

Brendan Riordan-Butterworth to take notes.  

MK: If you speak up during the meeting, take a look at the notes and feel free to correct.  

MK: Having been a scribe earlier for the Privacy CG call, it’s hard, and takes a moment of distraction to miss intent.  

MK: It’s a collective job, Brendan is primary, but all of us need to be on the ball.  

MK: Also - reload if you’re in “Suggest” Mode.  


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


[<img src="https://i.insider.com/5fc55f5350e71a00115580e8?width=700" alt="Screenshot showing location of the Raise My Hand button">](https://www.businessinsider.com/how-to-raise-hand-on-google-meet)

MK: If you want to talk, use “Raise my hand” feature on Google Meet.  

If you look at the list of participants, at the top of the list will be the people who have raised their hands, and their order.  


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


MK: All the way at the top of the agenda - this call is happening under the WICG.  Pronounced “Why CG” or “Wick G”.  Participating in this group include making promises under W3C IP agreement. Join the group, so that you or your employer has made the appropriate agreement.  This governs making changes, like pull requests and etc.  

MK: We’re 5 minutes in and the join rate has slowed.  


### Open Issues, or Issues that Need Opening

https://github.com/WICG/turtledove/issues?q=is%3Aopen+is%3Aissue+label%3AFLEDGE

MK: No topics on the agenda other than talking about issues.  As of last time I checked, MK was “winning” on issues.  More of them have me rather than anyone else as last commenter.  Hope that we can close issues, rather than going back and forth on them.  Let’s drive towards making changes on the Fledge doc to resolve the open issues.  

 MK: Hopefully to make clear or change due to inadequacy.    And make Fledge better.  

MK: Some issues are closed due to Merge and Pull requests.  Some Fledge labels are missing, fixing that.  .  

MK: Expectation - discussion existing issues.  Once that dies down, I will poke the fire where there are not yet issues but predicted to be contention, to generate a cascade of issues to discuss (offline|here|elsewhere)

MK: Hands up. 

Erik Tauberneck: Issue 97, seeking clarification.  Intersection of 2 interest groups, might identity.  If the interest groups are read-only to the owner, then you wound’t be able to read it.  Is showing the ad the issue on the threat model?

MK: 2 different threats to think about separately.  



1. The possibility of targeting an ad at an audience where the membership of that audience is some cross-site.  FLEDGE is about single site, but limited to that one site.  If you’re allowed BOOL, then it allows you to target at an audience where membership is derived from 2 diff sites.  
    1. Details about how to build cross-site audience (like SWAN) are worth evaluating.  
        1. How can we expand joining interest groups, with multiple sites worth of activity.  
    2. Balance privacy / is there some extent about cross-device (or cross site) profiles?  
    3. The one bit of “am I part of an interest group” is leakable to the party who delivered an ad to you.  This allows copying a bit from one domain to another.  This is a lesser risk than manufacturing new bit based on a bunch of cross-site.  
    4. We should have a lot of freedom, but getting the bit of information only after a success is probably acceptable.  
2. The one that is actually probably relevant to issue 97.  The FLEDGE explainer tries to do a careful job about assigning ownership of “user A visited site A” to site A.  This is restricted to the site A, and only available elsewhere if delegated.  
1. If NYT doesn’t want to allow anyone to create a NYT segment, so ads can’t track that they were delivered onto NYT, to prevent the creation of this.  

ET: ClarificationClarifcation - are interest groups read-only to the owner, or can a site know what interest groups a user is in?  

MK: No, the site can’t ask for that disclosure.  

ET: Frequency capping is important.  Metadata on the interest group - is that violating the (1) from above?  

MK: Seems fine for me?  

Leparmentier Basil: What actually can be done.  I’m asking a question, need to get clear.  On the billing, in FLEDGE, there’s only one book?  Are both of the API in sync, is it all Chromium?  Who does accurate bidding happen?  When a website shows an ad on a website, today we pay SSP (with whom we have a contract), in FLEDGE who has information about who owes what to whom?  

MK: Trying to answer.  My intention in FLEDGE is that after an ad renders, there are opportunities to log on behalf of both the SSP and the DSP.  Sec 5.1 “seller has opportunity to do reporting”, while they know everything (incl clearing price of auction), and sellers belief about payout.  At the outset, we expect event level, future better job preserving privacy.  Second, there is buyer’s opportunity to do logging (At the outset, we expect event level, future better job preserving privacy.  ).  Buyer has all info they have all the buying time info, seller info, plus browser info.  If the buyer trusts the seller, then all cool.  

LB: Clarification.  On the longer run, you will have server that will or will not be the same, that will run the aggregation.  Today we have contracts with SSPs - we don’t run ads on pubs that there's no contract.  How does this work in the future, with Private Reporting, with only 1 or 2 books (uncertain), if it’s private it will be harder to reconcile!  Who is going to have information once the Aggregate Reporting API exists?  

MK: There is a lot of discussion to still have about the Aggregate Reporting API.  Won’t be in place by the time 3rtd party cookies go away.  Probably after 2022 before solving the problems with this.  I think that both buyer and seller should have opps. To get aggregate data, and reconciling the two books is something that shouldn’t involve the browser - contracts between 2 parties who both use the browser.  If those two parties want to coordinate for 1 canonical data source, that should be fine.  Not the browser’s choice, but the contract between the parties.  

LB: The problem is that both happen at the same time, what happens if there is a problem with the browser?  Only the special frame has access to it - if the browser / reporting API fails.  Even if there are 2 reports, the browser holding onto and making private the reporting events.  Fenced Frame is a single point of failure.  POtentially a big issue, in terms of contractual obligation!

MK: But the browser is already a single point of failure.  Even though server side decisioning happens, the browser is still responsible for 

LB: Who pays what to whom?  Is it right to hear that it should still be SSP and DSP to have contracts and work it all out?  The problem with FLEDGE is uncertainty about whether an ad will run outside of where the contracts exist?  

MK: Buyer knows what the SSP is before making a bid.  SO it is feasible to run buys only in the case where the SSP is contract existing.  

Jonasz Pamula: Also tied to reporting.  The question of how to optimize models in the interim reporting phase.  Happy to hear that in the long term there will be a function to record based on all the information.  Can there be something available (even basic) in the interim phase?  If this is no, then we will need a completely separate system for interim phase (with degraded performance) and then a new system for the final form.  **Issue 93.**

MK: Charlie Harrison should be part of this discussion, but is in Texas without electricity or heat.  

JP: Should this be covered on the later call, conversion measurement call?  

MK: Maybe?  It’s also just about understanding what happens at bidding time.  You might have a PCTR model that you want to train, so it’s fine to talk about this issue at either place.  

MK: Should we be able to support…  We definitely do want to support the kind of custom reporting token that is described in that issue, we should talk to the other people in Chrome to make sure we have all the infrastructure in place, esp. Charlie.  

MK: Recap on full issue.  Getting reporting on all the inbrowser signals, that aren’t the contextual signals, like per-user metadata, plus recency data about clicks and etc.  Getting information about this, as long as it comes through the right anonymity filters (k-anon), should be possible to get even in the interim / early days.  

JP: It would be great to have clarity about the dependency cascade.  Would this block 3rd party cookie deprecating?  We would prefer that it block.  

MK: I can’t imagine Chrome deprecating cookies before some form of aggregate reporting exist

ET: Is it that Chrome would push all information through Agg Reporting and not event level?

MK: Not the same question.  

ET: Won’t jump the queue then. 

Michael Lysak: The publisher is important to consider.  The current system is that the ad news to show up in the browser.  ONe important thing is fraud checking, prices occurred as expected.  Is there some way that the publisher would have access to information about the auction in the fenced frame.  

LB: No-one has access to the Fenced Frame, in the long run at least.  

MK: The stuff that happens in the FF is stuff that people should have access to in aggregate, but not event level.  Including the publisher.  Information in the FF is sensitive, cross-site data.  The privacy guarantee is that the information about a particular user isn’t shared with the pub even if the user shows up on the pub’s site.  Knowing that **users** saw ads from a specific advertiser is fine, knowing that a single **user **did isn’t.  

ML: Example: Tissue Box decided that they want to deliver ads.  Is the pub aware that Tissue Box made the bid?  

MK: The pub should not be aware on an event-by-event basis.  It should be possible to learn in aggregate.  FLEDGE imagines that this is through the SSP, since the SSP is chosen by the publisher, and would report this information in aggregate.  

ML: Is it’s through the SSP, 2 points.  (1) How to know whether an interest group is working?  No way to test / debug.  (2) The proposal is effectively mandating that the SSP will have total control, eliminate whether the pub can detect fraud on the part of SSP.  

MK: (1) User should be able to test themselves, because user controls the browser.  And additional flags for Chrome for integration tests.  (2) Fraud questions are good questions, and we should ensure that companies who do anti-fraud can have the access they need to put that info to the reporting systems.  Cross-checking is good, providing it’s coming from the locked down environment that preserves privacy.  

Remi Lemonnier: **Issue 96** it’s closed and split.  About acquisition campaigns.  Interest group bid requests are probably mostly about retargeting?  I should probably use contextual if we’re thinking about large-scale.  There are 2 use cases that might not be covered.  (1) Freq Cap / Freq/Rec optimization.  There is a buyer signal about it in profile, but is it available in Contextual (2) A/B Testing, as simple as sending one bit, but is it available in Contextual, does that have privacy implications.  

MK: Let me start with saying that one of the design choices made during FLEDGE for easy implementation (not that it’s easy, but less hard), the result of the FLEDGE on-device auction might be that the contextual ad wins, and that leaks 1 bit of information.  That is a known leak of data in FLEDGE, since that might be a tracking vector with abuse.  

Knowing which ad wins in profile ads isn’t shared, it’s opaque.  The type of question about “is it possible to do cross-site A or B”, as long as the answers remain opaque to the outside page, then that’s probably OK.  

For contextual ads, the ads are allowed to render freely, so there’s no way to provide cross-site signals.  Every bit of information that would be made available would be supporting cross-site tracking.  

We could create a new API, tradeoff the free rendering for cross-site tracking.  But this doesn’t seem to be the strongest interest case.

RL: On that last point, what would be the specifics?  Understanding that this isn’t top of the list, what would be the needed input to put it on the roadmap?  

MK: Right, not top.  For A/B testing, yeah, fine, in a fenced frame, but timeline?  Not sure.  Probably in a similar timeline as FLEDGE, it reuses a bunch of tech.  Cross-site freq capping is harder, since there’s more bits, due to the complexity of the fallback.  This would affect ads that want to compete against ads that want to use the features - all ads would need to be OK rendering in fenced frame.  

RL: Last question.  Assuming we have some sort of functionality to support these scenarios.  A DSP won’t be able to solve the auction themselves, all bidding functions to the campaign. A given user is going to receive… the DSP will have to send a bunch of information?  Is there a scaling issue here?  

MK: Yes, sending many many ads to the browser, when only 1 will render, yes, that’ll be hard.  

John-Marcus Philips: My question concerns the notion of 3rd parties.  Entities that are well covered in the design proposal.  They are Publisher / Seller / Buyer, in addition to their other classification.  There are others, like data partners, that could be taken into account.  

J-MP: More interest about assigning / joining the interest group.  Data companies need to talk to each pub to get permission to assign their users to an interest group?  

MK: Let’s separate into 2 pieces.  (1) Browsers joining interest groups, (2) Reporting about interest group targeted ads that end up showing.  

For (1), you’re right, FLEDGE introduces the requirement that the ability to add a person to an interest group (or that the user did action that should add them) has additional protection.  There is implicit and explicit control by the owner of the website.  The draft right now says “anyone who shows up on a publisher’s site has the ability to call joinaddinterestgroup”, but “anyone on a cross-domain iframe doesn’t”.  The pub has the ability to grant or revoke this permission. Also there’s a delegation mechanism, allowing an SSP to take responsibility for allowing other parties to have joinaddinterestgroup.  

Your question about needing to make a deal is indeed arduous, but that’s why delegation feature.  There’s a small pool (at the moment) of parties that could be core to chain of trust for that function.  

For (2), about reporting, we can afford to be much more permissive about reporting as long as it goes through aggregate reporting mechanisms.  IF there are ways to do agg reporting that you wish were possible but don’t seem possible, then yes, file an issue.  

The intent with reporting is to make the needed scenarios possible, then document that need!

George London: This may be bigger than 4 minutes.  Want to better understand threat model about event level reporting coming out of fenced frame.  It’s not immediately relevant as to why this is absolutely needed to be replaced by agg reporting?  

GL: The pub can’t see inside, and the fenced frame content can’t see outside, so what’s actually leaked?  Some anonymous person got an ad?

MK: The context about event level reporting has been training ML to decide what ad to show in what context.  Event level info about something (which ad auction), plus k-anonymized data.  This is enough info to discern what you (specific visitor) are interested in.  

MK: It should be possible to do a seriously locked down, but better in the Agg Reporting API

ET: Will it be only the Agg Reporting API, or will the Event Level continue to be available?  

MK: Let’s talk about it in 103.  

Don Marti: Gross Ad question.  Pub gets notified by user about problem creative.  How does pub verify that a particular ad won’t continue to show, after taking steps to block a problem ad? 

MK: Let’s open an issue or bring it back on a future call.  
