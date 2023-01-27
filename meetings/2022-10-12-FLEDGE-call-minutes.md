# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Oct 12, 2022


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Sven May (Google Chrome)
3. Brian May (dstillery)
4. Paul Jensen (Google Chrome)
5. Andrew Pascoe (NextRoll)
6. Wendell Baker (Yahoo)
7. Fred bastello (Index Exchange)
8. Andrew Aikens (TripleLift)
9. Fabian Höring (Criteo)
10. David Tam (Relay42)
11. George London (Upwave)
12. Joel Pfeiffer (Microsoft)
13. Peiwen Hu (Google Chrome)
14. Philip Lee (Google Chrome)
15. Bartosz Łoś (RTB House)
16. Bartosz Marcinkowski (RTB House)
17. Marco Lugo (NextRoll)
18. Caleb Raitto (Google Chrome)
19. Alex Cone (Coir)
20. Jonasz Pamuła (RTB House)
21. Stan Belov (Google Ads)
22. Anthony Molinaro (OpenX)
23. Joel Meyer (OpenX)
24. Sergey Tumakha (Microsoft Ads)
25. Georgia Franklin (Google Chrome)


## Note takers: kleber


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here]



*   [pauljensen] Trusted update server (stemming from issue [333](https://github.com/WICG/turtledove/issues/333) and [361](https://github.com/WICG/turtledove/issues/361))


## Notes

[pauljensen] Trusted update server (stemming from issue [333](https://github.com/WICG/turtledove/issues/333) and [361](https://github.com/WICG/turtledove/issues/361))



*   Paul Jensen: Common theme across two open issues.  Questions about performance, about how long the auction takes to run. 
    *   Came down to pushing lots of information from K/V server to on-device auction, necessitated loading from K/V at the last minute because there are a lot of ads, products, etc., and the K/V server needs to give the on-device bidding scripts information about a lot of different things.  (Many ads in each IG or many different IGs on device)
    *   Trade-off between putting lots of data into the K/V server vs. Daily Update server.  The K/V server is in the critical path, while Update is not latency-critical.  So would be great to move more data to Update server.
    *   However!  Update server request has a k-anonymity requirement, so it limits what is possible to do there.  Need to send even more data from Update server, if you want to use it for this, and then filter down on-device.  If you have a more-specific IG then it's not k-anonymous; if it's more general then you send too much data.
    *   Solution proposed by RTB House: Having the Update server be Trusted.
    *   Updates themselves are not highly personal sensitive requests: it doesn't contain cross-site data, just data from a single site.  However, it would allow joins using IP address or timing correlation.  K-anonymity is a way of allaying that concern.  Could use a trusted server to remove that risk instead.
    *   Adding a new Trusted Server to FLEDGE is always an exciting prospect :-).  Seems like this could be very similar to the K/V server — in both cases, sending the IG name, getting IG-related info back, to use immediately vs store for later.  The thing that makes it hard to have another server doing this is what the constraints are on the server: How much data would the server need to hold?  If an IG name didn't have a k-anon requirement, a single IG could represent a single person's identity on a single site.  Is that too much data to feasibly serve?
*   Stan Belov: What is it that the Daily Update can solve for, that the Trusted Bidding Signals cannot solve for?  Why any need to extend?
    *   Paul: Two big differences:
        *   Update server: each requests just covers a single-site user identity.  So less privacy-sensitive.
    *   Stan: Functionally, what can Daily Update Server give that the Trusted Server cannot provide?
    *   Paul:
        *   Update server can give back ads, URLs pulled out of thin air, while Trusted K/V server can only help let you pick from ads already stored on device
        *   Update server is not latency-sensitive — OK to spend more time computing, send more data to browser.  Best to keep K/V server for time-sensitive things, stuff changing rapidly, like a campaign running out of budget.  Picking ads and pulling from a bigger data pool would be better in the non-latency-critical server.
    *   Stan: The reason to have the set of ads ahead of time: one big reason for that is the k-anonymity requirement, right?  So that the k-anon state can be pre-loaded?
        *   Paul: Yes
*   Jonasz: You have said it all already!  I'll reiterate that the Trusted Update server came up while we were working on optimizing latency.  One of the benefits in our mind would be that certain information that needs to be present on-device for the auction could be fetched out-of-band.  The functionality of actually updating certain fields in the IG is quite a strong one!  But currently we don't use it for that because of the k-anonymity restrictions.  Some advertisers have a high level of churn in their products, and we don't have a way to update them later because we have no way to do that for a specific user.  Trusted Update Server would solve that, so in our view would be a valuable extension to FLEDGE technology.
    *   Paul: Could deal with it via more frequent or larger updates, including stuff that would not be relevant?
    *   Jonasz: Would only be available if the Trusted Update server is there, otherwise no way of doing that.  Currently we can only do that while respecting the k-anon restrictions, which is much less effective while we respect the choice of products shown to the user.
    *   Paul: In terms of size of data — is this stuff that would otherwise be in the K/V server?
    *   Jonasz: Yes, the set of products is the metadata that is part of the IG and that can change.
    *   Paul: That is definitely the kind of thing we want to keep out of the K/V server if we can
    *   Jonasz: Of course we can try to come up with clever ways of compressing etc.  But the presence of a trusted server, that is already a concept within FLEDGE, seems like it would be the best way we could think of to improve latency.  We could try to jump through a lot of hoops to optimize with K/c server, but Trusted Update would be much better.
*   Brian May: Maybe there could be two distinct modalities, one for people with broader IGs who can work with FLEDGE as it is today, and one for people with tighter IGs who want this Trusted Update thing?  Also, maybe the K/V store could somehow signal to the browser that there is some additional metadata that it should fetch, so that could trigger a request to the Trusted Update?  Also it seems increasingly likely that trusted infrastructure will be available, so we should move in the direction of figuring out how best to use it.
    *   Paul: Re. K/V signaling that there is more to get, was thinking about how to do it without slowing down the auction
    *   Brian: Just have a flag in the K/V store to signal "hey there is data to be picked up later", then the browser does the update once the auction is over and it has time to do so.
    *   Paul: Maybe some IGs want to be updated a lot, maybe others don't.  Had been thinking that the IG could specify which, but having the K/V server decide which might be better.
    *   Brian: Sort of a sidecar in the K/V server, to send data that needs to be processed later but not in the context of the auction
    *   Paul: Interesting possibilities there.  I wonder if they could be the same server in some ways.
    *   Brian: Thinking of Update as a slow, after-the-fact version of the K/V server. There is the possibility of doing a range of small, fast, inline updates in K/V to bigger, slower, background updates in the update server.
*   David Tam: Seems like Trusted Server idea is made for the case where the advertiser has a large product catalog that wants to be updated.  Is that right?
    *   Paul: Comes down to what decisions do we want to make in the background, vs what need to be made at auction time?  If Update server gets a per-site identity (removing the k-anon req), then it can make more kinds of decisions than before.
*   Brian: There is also the possibility of moving some of the auction-time on-device auction to happen on a trusted server.  We could do that without compromising privacy.  That would allow those of us with large catalogs to leverage having more resources.
    *   Stan: If there is such a way to extend the perimeter-of-trust from the browser to a server-side environment, then some of the considerations might not be so critical.  Some of this discussion is about saving bandwidth between client and server, so that at auction time you have most of the stuff ready to go on-device.  If there is a way for privacy experts to be comfortable with extending the privacy boundary to server-side, then need much less computation on device, the computations happen within trusted perimeter on the server.  Could impact the overall design.
    *   Paul: The K/V User Defined Functions (UDFs) proposal allows some computation off-device as well.
    *   Brian: Suggests a model where the trusted environment receives info from the browser, can query the K/V server, can do computations and send back to the browser, and then forget the data it had.
    *   Paul: UDF allows for that, yes.
    *   Brian: Not just bid-price or auction functionality, also brand safety and peripheral concerns could be calculated in some trusted server environment.
*   Jonasz: To comment on what Stan said: If the computation happens on a trusted server, in UDF or other trusted environment, that changes our thinking about optimizing latency.  When it comes to the added functionality that the Trusted Update would bring, that would still be very useful in our perspective.  Paul, is the biggest question when to perform such a trusted update to avoid affecting auction performance?
    *   Paul: The thought has always been that we don't want to do slow things, like the update, in the middle of the auction.  No change there.  It's about segmenting what work we want to do during the auction, and what we can do later, off the critical path.
    *   Jonasz: A natural thing that comes to mind using these pieces is to use the opportunity that is triggered once a day, but instead of asking the Daily Update asking the Trusted Update.  Any problems with that, or is that feasible in your mind?
    *   Paul: Yes, that's exactly what I was proposing, if we think that is reasonable in terms of your needs and server specs.
    *   Jonasz: Would you think that only once a day is a reasonable limitation, or could it happen more often?
    *   Paul: Haven't thought a lot about that — is there a lot of benefit from doing it more often?  Not sure we want to be doing something for each of a lot of interest groups a lot of times.
    *   Jonasz: I think it would be an incremental improvement — calling it 2x or 4x a day would be a little better than 1x a day, but only a marginal improvement.
*   Brian: I think more frequent updates open up possibilities not available with fewer or less-frequent.  There are things that I wouldn't be able to do if I only had a chance once per day.
    *   Paul: Combines well with the previous point of being signaled by the K/V server.  Maybe some kind of prioritization of which IGs want to update more or less often?  If you had the choice to signal which IGs are which.
    *   Brian: Yes — the more opportunities we have to make decisions, the better.
    *   Paul: Wonder if update priority would go hand-in-hand with IG priority for bidding in an auction?
    *   Brian: Not necessarily.

Next meeting time?



*   Two weeks from now is Oct 26, the day in between the PATCG's 2x3hr meeting block.
*   We can meet then _if_ there is some important topic that people want to talk about.  If there is nothing urgent, meet in 4 weeks, Nov 9th.
