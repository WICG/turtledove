# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday July 6, 2022


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Martin Gruau (Captify)
3. Brian May (dstillery)
4. Benjamin Dick (IAB Tech Lab)
5. Matt Menke (Google Chrome)
6. Paul Jensen (Google Chrome)
7. Fabian Höring (Criteo)
8. Andrew Aikens (TripleLift)
9. Marco Lugo (NextRoll)
10. Bartosz Marcinkowski (RTB House)
11. Alexandru Daicu (eyeo)
12. Supraja Sekhar Google Ads)
13. Ryan Arnold (P&G)
14. Sven May (Google Chrome)
15. Tamara Yaeger (IPONWEB)
16. Sid Sahoo (Google Chrome)
17. Fred Bastello (Index Exchange)
18. Harneet Sidhana (Microsoft Edge)
19. Jonasz Pamuła (RTB House)
20. Andrew Pascoe (NextRoll)
21. Chris Starr (Hearst Autos)
22. Angelina Eng (IAB)
23. John Mooring (Microsoft)
24. Zheng Wei (Google Ads)


## Note taker: Michael Kleber


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here]



*   “Abandoned” auctions and reporting
*   Support for publisher- or seller-rendered formats (video, native) \
https://github.com/WICG/turtledove/issues/265 


## Notes


### “Abandoned” auctions and reporting



*   https://github.com/WICG/turtledove/issues/318
*   Zheng Wei: Current implementation of runAdAuction() will trigger reporting at the end of the auction, not the same as what's mentioned in the spec, that reporting only happens when winning url is handed to Fenced Frame to render.  If the caller runs an auction and decides to abandon it because it took too long and should time out, the auction will run to the finish and will send a report, even though the caller no longer cares about the result.
    *   This lead to conflicting pings: tag will send a ping saying the auction timed out and we fell back on rendering the contextual ad, and auction sends a ping saying there is a winner.  This conflict causes an issue in reporting and billing
    *   Suggestion: only send reports once the ad is rendered in a Fenced Frame
*   Paul: Posted a response an hour ago, hot off the presses.
*   Zheng:	
    *   Option 2 works, but relies on creative to trigger reporting event API from inside the FF.  That requires cooperation and participation from all buyers, which is tricky.
    *   Option 3, extending API to include global timeout, so that nobody wins if the auction times out
    *   Option 4, cancellable API, so caller can decide to end auction, so that nobody wins if the caller cancels
    *   Option 5, only send reports when ad rendered in Fenced Frame.
*   Paul: Option 3 is nice: simplest to implement, doesn't waste resources letting the auction keep running if it's not going to render.
*   Zheng: Like number 4, so that caller has more control over when to handle the auction.  Imagine a situation where ad scrolls off the top of the screen — then in Option 4 you can cancel the auction, even if you didn't have a way to know beforehand when the timeout should be
*   Vineet Kahlon: 4 is more generic than 3 where Chrome has the control.
*   Brain May: Are 3 and 4 exclusive?  Could you do 4 and then have 3 as a fallback?
*   Paul: Can probably do 4 and have 3 as a fallback.  
*   Brian: Chrome could impose a timeout even if the SSP doesn't do it
*   Matt Menke: One problem with explicit cancellation (4) is that the time it takes an auction to run is a cross-site data leak.  If we don't expose to the page, you could imagine a variant of FLEDGE in which we return a URN immediately, and you hand it to the FF, and then the URN only turns into a winning ad later.  This would be a future API change, but if we have a Chrome-run timeout it would remain possible.  The explicit cancellation API wouldn't work, in a world where we've closed the leak of how long the auction takes.
*   Paul: Either way, with a timeout or a cancellable promise, we could mitigate that link with a backup URL, like a contextual ad — which we render if no winner.  Then could still have either a timeout or an explicit cancel
*   Matt: So "cancel" means "cancel and return this URL, and in case the auction is still running it might return the URL, and if the auction has already ended then it doesn't do anything."
*   Paul: RIght, or could pass in the backup URL as part of the auction config.
*   Matt: only works if the contextual auction is completed before the FLEDGE auction begins, which seems like it would slow things down
*   Paul: If the contextual signals are passed into the FLEDGE auction, then we've already lost that use case.
*   Vineet: SSP tag can already measure how long the auction took, why is this a concern?
*   Matt: Suppose spy.com creates 32 IGs from 32 origins.  Another site can run 32 auctions restricted to each of the 32 origins, and now timing auctions can reveal the 32-bit ID.  We can limit auctions-per-page to make this more difficult, but it is still a leak, just one that takes longer
*   Vineet: that is all possible right now, right?
*   Matt: We're talking about a variant where FLEDGE instantly returns a URN, even if the auction isn't completed.  This is a discussion about how to close that leak.  Also the network requests needed to render leak information, but not to the publisher page
*   Zheng: Is this planned for the long term?  If that's the case then the proposal of having a backup URL, then cancelling and rendering the backup URL should work.
*   Matt: Yes, this isn't for implementing immediately,  but in the long term we want to cut down the leaks, so we want to avoid breakage when we do so
*   Brian: How does backup URL work?  What's the difference between backup ad and primary ad?
*   Paul: Primary ads coming from IGs that are bidding and stored in the browser.  Backup ad comes from the contextual auction — if the on-device auction times out, then render this one.
*   Brian: How does that work on the part of the buyer?
*   Paul: It's not the winner of the FLEDGE auction that submits the backup ad, it would be a separate contextual auction that outputs a backup ad.  If someone is in no IGs, for example, then this is the ad that would render.  Or if an auction takes too long.
*   Brain: Does that mean I'm always opted into buying a contextual impression if the on-device auction takes too long?  Feels like we're asking buyers to make a commitment that they don't want to
*   Matt: Nobody can be forced to show an ad.  Based on contextual signals, someone wants to show an ad.  Then we run a FLEDGE auction, and the publisher passes a reserve price into the on-device auction, or not.  Then if the auction times out, or no ad beats threshold, or no IGs, then the contextual bidder wins.  Then reporting goes to everyone who needs to know.  Contextual bidder has to bid in the first place — not like people are forced to bid.
*   Brian: I don't want to take up more time if people don't see the same issue.
*   Paul: Sounds like your vote is for Option 4, cancellable auction.
    *   1: sellers are responsible for which reports to ignore
    *   2: use FF ad reporting API to trigger an event from inside the rendered ad, if the ad cooperates
    *   3: Overall timeout set by seller, enforced by Chrome
    *   4: allow seller to cancel auction
    *   5: delay sending report until FF renders the ad
*   Zheng: We believe any of 3/4/5 could solve the problem, but 4 gives the most flexibility.
*   [from chat:] Chris Starr: "From pub view, I assume that we would be able to report on the number/frequency of cancels in 4?"
*   Paul: Yes.  As in issue #299, sellers want to understand how much resources / time the buyer is using; working on a response to that.
*   Zheng: Expect timeline for implementing this?
*   Paul: My preference is for #3, where implementation is much simpler.  Cancellation through the promise may be more complicated; will need to consult with Promise experts, will take more work to pipe the signal all over.
*   Zheng: Maybe do 3 first, then do 4 later as a superset / feature enhancement?  Would timeline for #3 be better?  How complicated would it be?
*   Paul: I think we've discussed this global timeout in other issues.  It's probably one of the simplest timeout options — simpler to specify, parse, pass around, etc.  Probably something we can try implementing, see how far we get how fast.  Zheng, can you summarize your responses as a comment on the github issue?
*   Zheng: will do
*   [from chat:] Fabian Höring — Can you elaborate why we could not just use the fenced frame reporting instead of reportWin ? As this is implemented now in chrome ? https://github.com/WICG/turtledove/blob/main/Fenced\_Frames\_Ads\_Reporting.md
*   Paul: This is option 2.  But this only works if the winning bidder cooperates and participates in this process.  Triggering the event itself happens from inside the FF; cannot necessarily guarantee that the people who want to register the event and who might want to call the API want to cooperate with each other — if I don't want to pay for an impression, just don't call the reporting API.  
*   [from chat:] Lisa Trongprakob "for sellside/SSP, we don't have a full control what get called, by DSP who renders ads/ creatives, from Fenced Frame."
*   Paul: Right, using the FF API is already implemented, so it could work today, definitely a pro, but requires that cooperation.


### Support for publisher- or seller-rendered formats (video, native)


### https://github.com/WICG/turtledove/issues/265  \




*   John Mooring:I didn't put this on the agenda, but I'm happy to talk it through! 
    *   The big change is that Fenced Frames would be output-gated.  In this mode, FFs navigation URL would have to be specified in advance.  This would allow for native placements, which have higher CTR, make up a lot of supply.
*   Paul: Right now FFs do a pretty good job of prevently publisher from leaking info into the FF.  Native ads would compromise that.  Your solution is to prevent info from leaking _out_ of the FF.  That seems like a pretty limiting constraint
*   John: The hope is to make that an additional _mode_ for FFs — so it's an additional constraint if you want to display Native Ads,. but not for other types.
*   Paul: FF Ads Reporting API talks about some of this.  People might want to know whether ad was viewable, for example.  How could we constrain viewability reporting, for example, if the FF has info about the publisher?  I can see how to constrain the output URL, but I don't know how to tackle impression reporting, like viewability, etc
*   John: Are we talking about a need to restrict FF Event Reporting API in the context of a native ad?
*   Paul: right.  If someone is paying by impression, how would they know when their ad was shown?  Maybe we could spec some limits on what could be reported, but there's a lot of work to think about
*   Angelina Eng; There are different viewability req's by different platforms — display ads 1sec 50%px viewable, video ads 2sec.  As we develop new attention metrics, talk to different buyers right now there is no standards.  That would include things like rollovers, expandable ads, viewability, hover time, video ad duration, etc.  Viewability + fraud + attention metrics are all important.  Highly recommend looking at MRC website for guidelines  we've established and have been in place for a while.
*   Paul: How much information from the ad does it take to report the ad?  Just trigger after a certain time and viewability, or a lot of information from the ad?
*   Angelina: each ad verification have their own methodologies, I'd recommend looking into.,  MOAT uses screen resolution, cpu, device type, pixels on screen, all that stuff.  making sure that the tab is active.  Each of the viewability verification vendors use things.  Look at the standards.  Vendors disclose in a very general sense, don't fully expose details.  Some use cookies, some don't, but I don't see how.  They also look ad frequency, viewability, fraud, IP address, URLs, all the way down to the full URL not just the domain.  Also the content of the page, &lt;meta> tags on the page, AI to evaluate sentiment.  That is the minimum requirements from the buy side.
*   Paul: That's a lot of information
*   Angelina: there are billions of dollars being spent on ads that aren't seen, as well as fraud.  Signals are to protect the publisher and the buyer.  If there is too much fraud, CPAs and conversion rates are going to be too low for a publisher, so buyer optimizes out of that publisher, pub loses money.  Don't' want bad guys to get any dollars, and want ads to be seen.  We've had studies where 50% of ads are fraud, or publishers have 30% viewability because people don't scroll down the page
*   Paul: John, I wonder if there is a different way of approaching this?  For Native Ads, how many different templates are there?  If we could limit the stock of templates, then could let the FF know the template and it wouldn't be that identifying.  Any sense of how common templates are?
*   John: On one particular publisher, very small number of templates.  So a very large k-anonymity bar would be feasible.  Problem is when you look at reusing across publishers, there are variations of fonts, font size, image size, etc.  Per-pub you could really limit it, reusing across publishers doesn't seem feasible without some limited information flow into frame. If it is per-publisher, could have a really high bar for k-anonymity of a template. Would prevent decorating an outgoing click url with a user id, would make it possible to decorate with a publisher domain.
*   Paul: Would be nice to have some k-limit on number of publishers, that's at odds with what you're saying.  Publisher is the sort of thing we don't want it getting crossed with.  I think there was quite a bit of talk about Native Ads in the Parakeet call a few weeks ago
*   John: What appetite and time horizon do you see for moving to aggregate reporting in FLEDGE?  That's when it matters that we can do native ads reporting that isn't viable at event reporting time
*   Paul: I don't think we've shared a timeline for that.
*   John: OK, let's not go into it.
*   Paul: Are you suggesting that aggregate reporting might solve this problem?  If the reporting from a native ad is gated by aggregate then it would mitigate?
*   John: I'm wondering, not necessarily proposing
*   Angelina: Group M has a policy of getting to 100% viewability.  Rest of industry is at 70%, should timeline be longer?  Most viewability vendors are 1 pixel, click tracker, but I could be wrong.  With audio/video/display, there are different companies with different requirements.  If we're going to move to aggregate reporting, then in some cases may want 100% viewability or another campaign at 70% or depends on deals with publishers.  Buyers do evaluate or have different agreements on pub-by-pub or campaign-by-campaign.  Aggregate reporting will need flexibility in allowing companies to set their requirements at whatever levels or tiers.  That's what ad verification companies are doing now — they get the raw data, process it into dashboards they reveal back to publishers and buyers.
*   Paul: Thanks.  One last thing: Next meeting date?  Some people are out of office in two weeks

Quick poll got thumbs-ups to skip the Wednesday in two weeks.


### **Next Meeting in 4 weeks, Wednesday August 3**
