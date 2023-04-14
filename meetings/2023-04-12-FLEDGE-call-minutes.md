# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday April 12, 2023


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Caleb Raitto (Google Chrome)
3. Joel Meyer (OpenX)
4. Manny Isu (Google Chrome)
5. Orr Bernstein (Google Chrome)
6. Sven May (Google Privacy Sandbox)
7. Andrew Aikens (TripleLift)
8. Alex Turner (Google Chrome)
9. Asha Menon (Google Chrome)
10. Kevin Lee (Google Chrome)
11. Paul Jensen (Google Chrome)
12. David Eilertsen (Remerge)
13. Kevin Nolan (NextRoll)
14. Risako Hamano (Yahoo Japan)
15. Fabian Höring (Criteo)
16. David Tam (Relay42)
17. Russ Hamilton (Google Chrome)
18. Michał Kalisz (RTB House)
19. Martin Pal (Google Privacy Sandbox)
20. Joey Trotz (Google Privacy Sandbox)
21. Marco Lugo (NextRoll)
22. Connor Doherty (Amazon)
23. Luckey Harpley (Remerge)
24. Tamara Yaeger (Criteo)
25. Zheng Wei (Google Ads)
26. Patrick McCann (Cafemedia)
27. Harshad Mane (PubMatic)
28. Andrew Pascoe (NextRoll)
29. Alonso Velasquez (Google Chrome)
30. David Dabbs (Epsilon)


## Note taker: Joel Meyer


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here]



*   Private Aggregation API [Alex Turner]: FLEDGE extensions announcement ([email](https://groups.google.com/u/1/a/chromium.org/g/fledge-api-announce/c/049BzYmVyoM))
*   Private Aggregation API [Alex Turner]: Contribution bound scope change ([PAA #23](https://github.com/patcg-individual-drafts/private-aggregation-api/issues/23))
*   Private Aggregation API [Alex Turner]: Report verification explainer ([link](https://github.com/patcg-individual-drafts/private-aggregation-api/blob/main/report_verification.md))


# Notes

Michael Kleber: Please sign yourself in. This meeting occurs under the auspices of WICG. To participate, please make sure you are a member of the WICG - this provides the IP protection needed to take the ideas put forward in this meeting. This is an essential part of our collective design process. Still need people to sign in, please. Let’s get started.

We have a number of items related to Private Aggregation API from Alex Turner, related to how do you know what happened after the fact. Let’s get started.


## FLEDGE Extensions Announcement

Alex Turner: Private Aggregation API is a proposal to allow reporting on what happens in FLEDGE worklets. We have some updates to share. The first item is just an announcement. We wanted to highlight that there are extensions to the PAA related to FLEDGE. Those are now live in the OT on Canary and Dev. Testing and feedback would be much appreciated. We hope to graduate them to later versions of Chrome once we have sufficient testing to feel confident about them.

Michael Kleber: What milestone are these in?

Alex Turner: M113 onwards (Canary & Dev only, for now)


## Contribution Bound Scope Change ([PAA #23](https://github.com/patcg-individual-drafts/private-aggregation-api/issues/23))

Alex Turner: [Going to present linked material.] This topic is about the budget. For PAA one of the key things is the ability to limit the amount of contribution a user or site can make in a limited time frame to prevent data leakage. The current bound built in is pretty broad - per origin, per day - separate for Fledge and Shared Storage. We’ve gotten feedback that this bound may be difficult to work with and reason about. If anyone has thoughts on how this bound affects the scope of the use cases, we’d love to hear it. There are a couple proposals on changes we like to make.

Alex Turner: The first change is to make the scope of this bound per site rather than per origin. We’re considering this to mitigate abuse potential. If adtech.example, instead of allowing them to use a.adtech.example and b.adtech.example to have separate budgets, we’d have a single budget at the domain level.

Michael Kleber: Alex, could you please back-up and provide some context on how users of this experience the impact of this budget? Those familiar with the measurement meetings probably understand this, but the Fledge participants may not. Can you provide some user/developer background?

Alex Turner: Sure, how you use this API is to send reports that embed private, potentially cross-site data. Today, that can only be a contribution to a histogram that has a bucket and value. The bucket is the X-axis, the value is how much to increase that spot on the x-axis. You might measure a whole bunch of different things within Fledge, and what we’re trying to limit is the effect that an individual user can have on the overall budget. The goal is that PAA be used to measure the impact of different groups (as opposed to individuals). When you process these reports together later on, the overall histograms will be summed and then noise is added. In order to reduce the ability for one user’s contributions to dominate the measurement, we limit the sum of all the values that a user can contribute within a particular scope. We limit the sum of the values - if you sum all the values and limit it by the origin w/in a 24hr period. That is limited to a certain value. If a dev tries to send a report that exceeds that threshold, the report is dropped to enforce this limit.

Paul: Is that a first or third party origin?

Alex Turner: This is the script origin - the bidder or the seller. That was a little off-the-cuff - does anyone have questions I can elaborate on?

Michael Kleber: To reiterate Paul’s point - this is the origin of the reporting entity. 

Alex Turner: Correct, in the case of the bidder this would be the IG owner origin. For a seller it would be the seller origin.

Paul Jensen: Would a typical adtech just have one reporting origin?

Alex Turner: I think some adtechs might have multiple origins if they want to separate use cases. But with subsequent changes it’s possible they’ll put things onto a single origin to help things. Depends on the use case.

David Dabbs: There was recently a limit put in place to prevent the use of many domains, I believe.

Alex Turner: I think you’re referring to the attribution reporting API? They were considering a limit like that.

Alex Turner: This ability for people to shard their reporting into a number of different origins is what motivated this change. We want to prevent this abuse vector where you could get as much budget as you want by using many subdomains.

Paul Jensen: By site do you mean top-level domain?

Alex Turner: No, site is based on the public suffix list - who owns a domain. See ticket. It gets a little difficult with things like google.com.au. We thought site was a more natural boundary than origin. It’s a little more difficult to mint a new site than to mint a new origin.

Michael Kleber: Just to make sure I understand, the question “Is it possible to send aggregatable reports to a variety of subdomains?” so you can subdivide those reports into different use cases, is a different question from shared budget, correct?

Alex Turner: Yes, I should’ve been more clear. In Fledge it would have to be separate IGs in order to send it to different origins since IG owner is the origin.

Michael Kleber: This is a proposal about having one unified budget across all reporting subdomains. So it’s still possible to have multiple different histograms, they just draw on the same budget.

Alex Turner: Correct.

Alonso Velasquez: Is it a fair characterization to say that this budget is per view? Eg this view, with these aggregations, has one budget while another view, with different aggregations has a different budget? Is that a fair comparison?

Alex Turner: Not sure I follow.

Alonso Velasquez: For a given set of keys, there’s one budget, but for another set of keys, there’s another budget?

Alex Turner: The way we enforce the budget is on the sum of the keys. This is a limit to prevent the worst case information leakage, so there are a number of different measurements that all use the same budget. It is relatively generic in how it is measured.

Alonso Velasquez: Maybe? I can follow up offline. Just trying to illustrate, or give an analogy.

Michael Kleber: For the sake of clarity, is there a specific number for this budget? A number that is the total you’re allowed per reporting origin per day?

Alex Turner: It’s 2^16 currently. This number is essentially an arbitrary number because the scale of the number doesn’t matter as much as it matters that the PAA knows it and can add the noise appropriate to a particular epsilon.

Michael Kleber: Alonso - to answer your question. Regardless of what you’re trying to measure, if you want to measure 64k diff things about what happened on my browser inside Fledge auctions today, then this budget is enough to use a number “1” to contribute to each of those buckets. Then the report you get is the sum total number of “1s” that was contributed across all the different buckets on the different browsers. What that means is that when the PA service runs, there will be a larger amount of noise so that the ability to see individual users is unavailable, but also the ability to see 10 users or a 100 users is prevented. If you want to know with more precision how many people contributed to a given bucket, then instead of using a 1 to measure the contribution, you would use a much higher res count of how many people contributed. Instead of using a 1, use the number 100. Now I can only measure 640 events instead of 64k events. So I have less ability to count different things, but I get less noise for those buckets I do count so I can get better quality of information.

David Tam: Just to follow up on that, is a bucket a site in this case?

Alex Turner: A bucket is just a place in this histogram. What it represents is up to the measurer. Beforehand they would need to choose what each bucket is. They could declare that bucket 1 counts this, bucket 2 counts that. It is entirely up to the measurer to make that space work for them.

David Tam: Is that measurer the seller?

Alex Turner: The buyer or the seller - whoever is using the API. They could split it up by origin if they want to use buckets for different things on different sites. Link from Kevin here.

Alex Turner: The other proposed change is to move from a daily scope to a ten minute scope, as the daily scope can be difficult to reason about. We’re hoping that would allow more flexibility and simplify the budget management. One caveat is that we might still have a daily bound, but it would be a looser backstop (2^20 - see ticket comment). We’d appreciate feedback on whether people think this would help/hurt - are the values too big, too small, etc. Feedback is welcome. Please respond on the GitHub issue.


## Report verification explainer ([link](https://github.com/patcg-individual-drafts/private-aggregation-api/blob/main/report_verification.md))

Alex Turner: The last topic is the proposal for Report Verification, which is a way to prevent invalid traffic on PAA. One concern is how to tell apart valid from invalid traffic (spam, etc). We have a proposal for how to get those trust signals through the API without the user-level data that we expect in today’s systems. I will give a brief, high-level view, but want to emphasize we are not tied to the design.

Alex Turner: The quick overview of one possibility, is that for sellers, when someone calls runAdAuction, they can provide a contextId - some string that represents that context - which would allow the seller to pair together any signals that they have about trust (related to user behavior) and have that string sent in reports later, so they can associate that report with those context. If the user seemed spammy, they could drop the report later if they wanted to. There are some complications to prevent cross-site leakage. We need to make sure that the report is always sent even if the seller doesn’t call the report. For bidders, this is a little more complicated as the existence of bidders is inherently cross-site. It’s not possible to link bidders to the context. Instead, we could use Private State Tokens, which are an encrypted ways of communicating state through. It would allow a “spam” or “not\_spam” signal to be sent through without revealing the full context. It allows one bit of information to be communicated using cryptography. I will leave it at that for now. I encourage people to read the document, post issues, give feedback on the trust model and how you think it would work for you, or if something else is needed.

David Dabbs: Is the proposal here something that’s easily layered on as an addition, or is it pretty invasive and we need to make a decision soon?

Alex Turner: For the context\_ID it’s backwards compatible and can easily be layered on. For the bidder, we’re still considering this as something backwards compatible and wouldn’t fundamentally change things. I do think the lift of adopting it is a little more complicated because you have to set yourself up as a token issuer which is a little more complex. We can layer it on later.

Alex Turner: Other questions?


## Fabian asked about Issue [#428](https://github.com/WICG/turtledove/issues/428).

Michael Kleber: If no other questions, I see that Fabian asked about Issue 428. I invite further discussion on topic 2.

Fabian (FH): Question regarding how PAA handles float bid values and different types of measurements (bid requests, displays, clicks)

Alex Turner: Just to clarify, the reason you want 10k is to have sufficient precision given the inherent granularity?

FH: Yes, let’s say the bid value for CPM is 0 - 100, double-digit precision, maybe I’m getting something wrong, but to me this means I have to map it to int values, multiply by 100, which means values in the 10000s, which takes significant budget. Getting bid value is a very reasonable thing to want to record.

Michael Kleber: The essential answer is that you should expect to report the bid values at much lower precision. We’re talking about aggregate reporting, so what you’re going to be able to find out is some kind of average in value over a large number of different auctions/browsers. Coming up with an avg value doesn’t require knowing the full double precision value for each bid - on that scale you don’t lose much by having less granularity.

FH: Do you agree that for billing I need to have the exact value, though? How does this work for billing?

Michael Kleber: I don’t think our intention is to say that right now PAA is the right way to do billing. We expect that billing is met using event-level reports in the short term and we’re interested in figuring out what the long-term solution is. But as most people know, EL reporting is going to be here for years, so for billing this isn’t something we’re addressing today.

Alex Turner: For things like billing, that is our answer. For other things, you can measure beyond averages by bucketing the value by defining buckets and incrementing them. But there is a limit to the precision you can get through aggregate reporting, particularly given the addition of noise at the end. Even if you were very precise, the noise would erase some of that. Given that the L1 bound is this arbitrary number, we don’t want it’s granularity to feel limiting. One thing we were hoping is that this switch from daily to 10min window would help with this by letting the 2^16 value address a ten minute period rather than 24hr period.

Alex Turner: I also want to address the need for different measurements - eg bid requests, displays, clicks - and not having budget interfere there. We hear that feedback and agree it can be a little difficult to measure budget in the current system. We’re reluctant to have a rigidly defined budget because different adtechs may have different ways of measuring and it doesn’t seem right for us to declare how the budget is allocated. Would it be helpful if we revealed what the remaining budget was so that an adtech could make an informed decision about where to spend the remaining budget.

Harshad Mane: Knowing how much budget is remaining would be really useful. Since you mentioned AR shouldn’t be used for billing, and if EL is going to be deprecated, what is the reliable source for billing?

Alonso Velasquez: We plan to have EL support this use case for now and we agree it would be nice to know what is available after that, but the key point is that right now we are still working on getting that answer. [In other words, we don’t know but we’ll get you an answer by the time you need it. - said much more diplomatically] We’re still working on it, but rest assured this is a priority and we’re considering the different uses for reporting, billing, rev attribution, etc - those needs have different characteristics than fitting a model - so we’re considering the different user cases and tailoring our designs to those needs.

David Tam: From the buyers perspective, they need to know what to bill. From a buyer’s perspective, how do they know how much to budget and spend? What kind of reporting can they expect?

Alonso Velasquez: Are you talking about the realtime budgeting needs or after the fact planning?

David Tam: Both. In the attribution API you can do attribution and plan accordingly. An example of the type of reports that can be available would be useful. But from a buyer’s POV, how would they verify how much they should be paid?



*   Alonso Velasquez: We’re not talking about RT budgeting, so it’s a validation use case. Post auction reconciliation?

David Tam: Yes. Today a DSP provides reporting. But in Fledge how do I verify how much I should be paid?

Alonso Velasquez: Is this available via reportWin? I believe it is (heads nod). We are adding an adCost field (rounded to protect privacy) but that’s going to be available in the event level  reporting and would be used for this. I’ll share the GH issue for this.

Paul Jensen: Yes, we added that support a little while ago (M113?) and should be in the explainer as well.

Michael Kleber: Just to connect this back to Fabian’s question about precision - even in the case of auction clearing price available in EL reporting - it’s still the case that precision is limited. The way we’re looking at precision, the binned value, the mantissa for the number is limited to 8 bits. You can’t report bid values down to billionths of a cent. You can make bids with precision as high as you want, but you can’t report them out at the same precision. If you bid $.37 CPM, but then attempt to use the next 16 digits to encode a user ID to reveal a user, you won’t get that highly detailed precision in reporting.

Michael Kleber: We are nearing the end - any last words? Thank you all for coming, see you in two weeks (4/26) if we have topics.
