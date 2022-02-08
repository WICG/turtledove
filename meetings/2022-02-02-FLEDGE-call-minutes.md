# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 4pm UTC (during winter).

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Feb 2 2022
(There was no meeting 2 weeks ago, Jan 19, due to lack of topics)


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Brian May (dstillery)
3. Russ Hamilton (Google Chrome)
4. Paul Jensen (Google Chrome)
5. Don Marti (CafeMedia)
6. Christa Dickson (Dotdash Meredith)
7. Michael Coward (Arcspire)
8. Chris Evans (NextRoll)
9. Caleb Raitto (Google Chrome)
10. Bartosz Marcinkowski (RTB House)
11. Joel Meyer (OpenX)
12. Tamara Yaeger (Iponweb)
13. Fred Bastello (Index Exchange)
14. Peiwen Hu (Google Chrome)
15. Phil Eligio (EPC, axel Springer)
16. Rotem Dar (eyeo)
17. Christina Ilvento (Google Chrome)
18. Erik Anderson (Microsoft Edge)
19. Wendell Baker (Yahoo)
20. Shivani Sharma (Google Chrome)
21. Angelina Eng (IAB / IAB Tech Lab)
22. Matt Wilons (NextRoll)
23. Benjamin Dick (IAB Tech Lab)
24. Jeff Kaufman (Google Ads)
25. Garima Dhingra (Google Ads)
26. Andrew Pascoe (NextRoll)
27. Martin Gruau (Captify)
28. Sergey Tumakha (Microsoft Ads)
29. Jonasz Pamuła (RTB House)
30. Stan Belov (Google Ads)
31. Kevin Graney (Google Chrome)
32. Matt Menke (Google Chrome)


## Note taker: Michael Kleber


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

 		


## [Suggest agenda items here]



*   [[shivanisha@google.com](mailto:shivanisha@google.com)]: https://github.com/WICG/turtledove/blob/main/Fenced\_Frames\_Ads\_Reporting.md
*   [[pauljensen@google.com](mailto:pauljensen@google.com)]: https://groups.google.com/a/chromium.org/g/fledge-api-announce
*   [[pauljensen@google.com](mailto:pauljensen@google.com)]: https://github.com/WICG/turtledove/blob/main/Proposed\_First\_FLEDGE\_OT\_Details.md
*   [[pauljensen@google.com](mailto:pauljensen@google.com)]: Discuss post-auction signals, in particular:
    *   What signals do losing bidders and sellers need to know?  Initial proposed list:
        *   winning bid price
        *   Was the winning interest group owner the same as my interest group owner?
        *   2nd highest bid price
        *   Was the 2nd highest bid's interest group owner the same as my interest group owner?
    *   What signals do winning bidders and sellers need to know? 2nd highest bid price?



## Notes



*   [[shivanisha@google.com](mailto:shivanisha@google.com)]: https://github.com/WICG/turtledove/blob/main/Fenced\_Frames\_Ads\_Reporting.md

Shivani: This was published a few months back, about how events get reported.  Now that we're moving towards FLEDGE OT, we want to be sure it meets people's needs.  We're trying to carve out event-level reporting in the short term, for events that happen inside the Fenced Frame (rendered winning ad).  The linked document is a proposal for how both the seller and the winning buyer have an opportunity to register what events they are interested in, from inside the winning ad … and once those events happen, where they want to send a ping.

…: Inside the FF, they can notice that an event might be interesting, call a new API to indicate the event happened.  Then the browser can send beacons to the seller, buyer, or both, to report on the event having happened. 

…: Don't take the API shape as a guarantee, we can update spec as we nail down details, but this is the idea of the workflow.  Wanted to understand if this meets your needs

George London (Upwave formerly Servata): We hope to run surveys on people who have seen ads.  We often run on publishers for "rewarded inventory" — if people complete the survey, they get a reward.  That means we need to know if the people actually complete the survey, rather than enter gibberish or just click away.  Any way to transfer that information?

Shivani: We don't have any information flowing from the Fenced Frame to the publisher inside the browser.

George: Who is the seller?

Shivani: The owner of the worklet calling reportResult

Paul Jansen: in FLEDGE terms, the person who is judging what shows up, could be the publisher or an SSP the pub has engaged to do it for them.

George: Publisher could communicate with the SSP to get info back from them.  So can the FF get information to the seller, immediately?

Shivani: There isn't any delay or aggregation, this is to support event-level reporting.  We're figuring out details, it's similar to sendBeacon today.

George: The latency is pretty important, even waiting a few seconds is bad, if they are being rewarded for completing the survey.  If it's slow, I wonder if there is any way to pass just a very small amount of information in a different way — just the one bit that the thing inside the FF has completed.

Paul: Hitting the "submit" in the survey seems similar to a user navigation of an ad.

George: Publishers wouldn't be happy if it required users leaving their site, it would be pretty disruptive.  Not trying to pass back the contents of what happened, just one or a couple of bits.

Paul: This might require a more in-depth look of why to use FLEDGE for this.  Maybe move this discussion to a GitHiub issue

George: There is a write-up on the original web-adv BG repository, I'll try to bring it over to FLEDGE or Fenced Frame repositories.

Shivani: We'll look.

Brian May: Just wanted to mention that the diagram in the FF doc is hard to read in Dark Mode — might need a white background, rather than transparent.

Jeff Kaufman: I like this proposal, very useful for the creative to be able to include something to send in reporting.  Proposal for Component Sellers is out — now there is a low-level seller and a final seller.  In this proposal you can target your message to the buyer or seller, but now we maybe need to refine it to target either kind of seller.

Shivani: Not following the Component Seller work, but seems reasonable to expand in that way

Jeff: Also, will this reportEvent API be available in the first origin trial?

Paul: For stuff that involves happening in a Fenced Frame, we haven't announced when a FF OT, and the first FLEDGE OT isn't going to require them.

Jeff: So are you not planning to even support FF's in the first FLEDGE OT?

Paul: Haven't committed to any specific timeline for various OT's yet.



*   [[pauljensen@google.com](mailto:pauljensen@google.com)]: https://groups.google.com/a/chromium.org/g/fledge-api-announce

Paul: There's a mailing list!  Sam Dutton (Google DevRel) created it, so we can point out newly uploaded things, publications elsewhere, announcements on other mailing lists, etc.  The intent is not to use this for general discussion; GitHub issues have been good for that.  But this is a public list where we can get people's attention for things related to FLEDGE.  Please sign up!



*   [[pauljensen@google.com](mailto:pauljensen@google.com)]: [Proposed First FLEDGE Origin Trial Details - WICG/turtledove](https://github.com/WICG/turtledove/blob/main/Proposed_First_FLEDGE_OT_Details.md)

Paul: A few publications went out last Thursday, about the first FLEDGE OT.  Not about changes to FLEDGE, just a few yes/no details about what specific things could be included right at the start.  No timeline yet.

…: Any OT, we're trying to get feedback on new proposals, get feedback on the semantics and mechanics of the API, does it perform well enough, can you store sufficient interest groups, etc.  We'll be collecting metrics as we do with all new features, so that we can monitor performance, know about it if some corner case becomes popular and needs more work.

…: OT is meant to be a first mechanism where people can try things out, measure what happens, see if it meets their needs.  Having accurate measurement, so that people can judge their answers to that, is essential to it doing this job.  For this reason we made choices to not require aggregated measurement, but to allow event-level measurement using existing mechanisms, so that people can figure out how well it's working in a head-to-head comparison.

…: Other things are still evolving, and they will become available at different times [like FF's as just discussed], and we are not tying things together in terms of timeline.

…: Other things still work in progress in the browser, like better debugging, setting breakpoints inside worklets, etc.  Give us feedback.

…: Also Thursday, Sam Dutton published various DevRel articles, please take a look.  https://developer.chrome.com/docs/privacy-sandbox/fledge/ and https://developer.chrome.com/blog/fledge-api/ 



*   [[pauljensen@google.com](mailto:pauljensen@google.com)]: Discuss post-auction signals, in particular:
    *   What signals do losing bidders and sellers need to know?  Initial proposed list:
        *   winning bid price 
        *   Was the winning interest group owner the same as my interest group owner?
        *   2nd highest bid price
        *   Was the 2nd highest bid's interest group owner the same as my interest group owner?
    *   What signals do winning bidders and sellers need to know? 2nd highest bid price?

Paul: FLEDGE talks about post-auction reporting, and we want to nail down the details.  Right now there is a reportLoss mechanism described that involves registering a URL to send a report to if a bid loses, but since it's registered at bid time, can't include information about other stuff that happens in the auction.  There are ways we can get that information into losing bid reports, by templating or parameterizing or something else like that.  Want to have a discussion here about what values you all want to be available for that reporting.

…: Stuff we thought of already: winning bid price, boolean indicating whether the winning bid was from the same or a different owner, then also the second-highest bid price and a similar boolean.

…: Do folks think those four different values are useful?  Sufficient?  What else about auction outcomes do losing bidders want?

Jeff: I don't know about those specific values, but I think macro-ing things into the URL is a great approach to solving the problem, without needing to incur the cost of running arbitrary JS inside a worklet

Paul: Yup, that resource concern is why we're proposing this kind of mechanism, arbitrary-JS would indeed be costly.  Something declarative is very appealing, but we need to figure out details.

Stan Belov: Those values seem interesting.  Things that seem like they might be useful: some way to know _why_ I lost an auction — some kind of maybe standardized namespace for loss reasons, as discussed on a previous call.  Also some way to identify the ad.

Paul: Interesting — trying to see whether we want the seller's feedback on the loss reason to go through this mechanism or a different one.  We could parameterize $SELLER\_LOSS\_REASON into the loss report URL.  There are different considerations though: we propose this being an API you can call from the bidder worklet, e.g. say your bidding script didn't load from the network, or JS threw an exception, or ran too long.  Then it might not be possible to report on those via a JS API, because the JS might never get a chance to run!  So we could offer reporting via the buyer setting an attribute on the Interest Group itself.

…: Or the information from seller to bidder could come via a different channel, could be a richer signal than just an enum — maybe seller wants to tell a lot of information about why the bid was scored the way it was, might be too much and want its own channel, something even aggregated info about why bids were scored like they were.

Stan: I understand that sometimes the loss can be caused by reasons in the browser by FLEDGE implementations failing, due to infrastructural reasons, definitely a different use case.  I agree that a richer informational channel for loss information sounds useful, I would be interested to understand how it fits into the FLEDGE privacy model.  That's a longer-term question, but if it fits into the origin trial, would be interesting — longer term, need to nail down privacy implications.

Paul: So do you think an enumerated value is enough to pass along loss reasons?

Stan: it seems clearly useful, other people on this call might have other ideas.

Jonasz Pamula: I want to add a +1 to the idea of a macro for a reason why we lost.  Beyond reasons supplied by the seller, reasons like "you crashed' or "you timed out" is important.  Also the ability to call the API multiple times inside generateBid(), so that if we time out, we could register as much information as we've computed so far, in the hopes that we get useful information despite a timeout.

Paul: That seems like useful semantics

Jonasz: That works nicely with the idea of URL being pre-registered.

Stan: Seller could return some sort of reason as the scoreAd function returns, which could be passed to a buyer via macro substitution.  But question: FLEDGE OT #1, the sendReportOnLoss API, would it allow you to construct any sort of a URL for reporting losses?  Is this discussion we're having right now talking about OT #1, or about a subsequent stage?

Paul: We haven't announced the time for OT #1, and the way they operate, they are tied to a particular milestone, so it's the state of the API, however much we've gotten to at the time the code is branched for that milestone.  So anything implemented in Chrome prior to that branch point will be in the OT.

Stan: Specifically in the doc on GitHub for loss reporting, is our discussion about that?  Or about something else later?

Paul: For now, it's referring to what's in the document up on GitHub.

Stan: So the debugReportLoss API may be subject to change based on what we discuss now, what we highlight as useful here?

Paul: Right — implementation in Chrome is evolving.  Implementation right now, in tonight's canary, is pretty close to that document.  If we all think we should add that post-auction stuff to the loss reporting, we can do it.  And if it's done before the branch point of Chrome where we start the OT, then it would be in there.

Brain May: with respect to reporting,  might be helpful to segment and categorize it in different ways — runtime is different from "your bid wasn't accepted by the seller".  Should categorize it.  Useful to know whether a report is OK to send with rich information because it doesn't contain identifying user information.  And we could give people relatively quickly feedback that is low-information but useful, vs information that is richer but comes more slowly.  I want to know right away that my JS failed to compile!

Paul: Good point.  Things we were thinking about included stuff like "should the winning bid be rounded to two decimal places?"

Brian: I was thinking like "if you want immediate feedback we could give you a range of prices, if you can accept slower feedback, we can give you a price to two decimal places."

Paul: OK, maybe will distill some of this into a GitHub issue (not sure if new or existing).

Paul: Discussion so far has been on losing bid signals.  Should more of these go into winning bid reporting?  Do they want to know the second-highest bid price?  ("was the winning bid submitted by the same owner as the second-highest bid?")  Should we think about reportWin similarly?

Brian: Easy answer is "yes"!

Jonasz: I also think this is useful longer-term — we would use it for optimization if available.  More important longer-term in FLEDGE lifetime, even if we might not use it right at the beginning of the OT.



*   Paul: End of agenda.  Any other topics to bring up in the last 10 minutes?

Matt Menke: In multi-SSP auctions, do we want the intermediate seller to be able to update the bid price seen by the final seller?

Stan: Some seller may have a relationship with a  publisher.  One way for a seller to get their bid into the final auction would be to let component auction runner pass along seller dollars net of their fees.

Joel Meyer: Likewise need for second-price auction, where some people advocate for

Stan: Trickier in second-price auction.

Paul: Can we do that just by passing along metadata?

Stan:  Haven't thought about whether the component seller is comfortable with the top-most seller seeing the bid price from the original bidder.  There is an information-sharing question here.

Joel: Something in the metadata seems less straightforward

Paul: Had a hard time, looking back at notes from prev discussions, telling whether you think it's OK for component seller to able to modify price

Joel: I like Stan's proposal, it's in line with what happens today

Paul: Anyone else feel strongly?  Matt, does this answer the question?

Matt: Yes, I'll make it adjustable.

Paul: Yay, in-person decision, that's nice sometimes.

Any other topics?  Meeting adjourned!
