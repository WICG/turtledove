# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California and 5pm Paris time.

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday April 16, 2025

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!



1. Michael Kleber (Google Privacy Sandbox)
2. Trenton Starkey (Google Privacy Sandbox)
3. Kevin Lee (Google Privacy Sandbox)
4. Jeremy Bao (Google Privacy Sandbox)
5. Paul Jensen (Google Privacy Sandbox)
6. Shafir Uddin (Raptive)
7. Phil Acker (Amazon)
8. David Dabbs (Epsilon)
9. Alexander Tretyakov (Google Privacy Sandbox)
10. Garrett McGrath (Magnite)
11. Laurentiu Badea (OpenX)
12. Hill Slatts (IAB Tech Lab)
13. Orr Bernstein (Google Privacy Sandbox)
14. Kristina Baron (Dun & Bradstreet / Eyeota)
15. Andrew Pascoe (NextRoll)
16. Abishai Gray (Google Privacy Sandbox)
17. Sid Sahoo (Google Privacy Sandbox)
18. Jun Kang Chin (Google Privacy Sandbox)
19. Shruti Agarwal (Google Privacy Sandbox)


## Note taker: Orr Bernstein


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which  can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here (meetings with no items may be canceled):



*   [From Jeremy, Privacy Sandbox] Deals
    *   Topic 1: understand real-world DSP deal ID usage and the potential load to the K-anonymity server
    *   Topic 2: deal ID as trustedScoringSignalKeys - make sure I get sufficient feedback from this group (Github: https://github.com/WICG/turtledove/issues/873#issuecomment-2652255187)
*   [From Jeremy, Privacy Sandbox] Negative Targeting for PA bids
    *   Github ticket: https://github.com/WICG/turtledove/issues/896#issuecomment-2174598427
    *   IG level vs ad level association, especially with first-party-cookie style IG creation; understand what will be valuable for ecosystem partners
*   [From Hill, Tech Lab] Security Model
    *   Initial discussions around some kind of security model design. If Tech Lab creates some proposal, what would the next steps be? 


# Announcements

Privacy Sandbox folks are holding every-second-Wednesday meetings in the hour after this meeting to discuss the trusted server elements of Protected Audience work. For more information see https://github.com/WICG/protected-auction-services-discussion/issues/27.

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!


# Notes 


## Deals: understand real-world DSP deal ID usage and the potential load to the K-anonymity server

[From Jeremy, Privacy Sandbox]



*   Jeremy Bao
    *   We have a solution for deals & deal reporting - launched at 10% on Chrome stable.
    *   Need to ensure before we continue launch that k-anonymity server can handle additional load.
    *   To ensure that deal IDs are k-anonymous, the browser needs to fetch the k-anon status from k-anonymity server for each deal.
    *   If you review your OpenRTB logs, how many deal IDs do you have per ad, ad campaign? What does the distribution look like - P50, P90, P95?
*   Michael Kleber
    *   We've had a discussion about this in this forum before. We'd heard a few hundred deal IDs as the high number from the Index Exchange folks. Don't appear to be here today, though.
*   Hillary Slattery
    *   Two different numbers - max numbers of deal IDs or max number of combinations of deal ID and creative size?
*   David Dabbs
    *   +1.
*   Hillary Slattery
    *   There is some research (maybe from Jounce?) on the average number of deal IDs in the bid stream.
*   Paul
    *   Jeremy is specifically interested in the load on k-anonymity server, which is directly impacted by the number of selectableBuyerAndSellerReportingIds.
    *   For other use cases, we've talked about using private aggregation, which doesn't have to go through k-anonymity.
    *   In the bid stream, there may be a lot of deal IDs, but here we want to know how many the buyers want to put into selectableBuyerAndSellerReportingIds.
*   David Dabbs
    *   If we look at how many deals we use, there may be 500 or 1000 we use today in existing programmatic trading.
    *   Deal IDs are not in some global list - I have a different set of deal IDs with every seller - some with Roni, some with the folks at Google Ads. 
*   Gerrett McGrath
    *   When you ask the question specifically about the DSP side, are you asking for rough number of deal IDs that would appear in a response from a deal ID? Contextual auction we do today, you're asking about quantity of deal IDs from a DSP perspective? For a particular auction?
*   David Dabbs
    *   You can only provide one per bid on a creative. But for Protected Audience, we need to know a priori every single deal ID in advance that we might want to use with a particular creative.
*   Garrett
    *   The matrix of creative and ID is an exponent, there are 10s of thousands.
*   David Dabbs
    *   We're asked for a straight list, but it's not a straight list - there's a different set for every seller.
    *   Because we want all potential identifiers in our reporting, we're going to be declaring always - at the very least - an empty string as a DSP so that we can access all of our reporting IDs.
    *   An empty string won't generate any extra load, right?
*   Orr Bernstein
    *   Unfortunately the empty string will add one more, but this is somewhat different from what Jeremy is asking about where there may be many more
*   Jeremy
    *   A question about how we organize deal IDs from a DSP perspective. You mentioned two questions - how many deal IDs and how many permutations you can have. It's per campaign. The model you have at the forefront of your conception is that — there are some campaigns or line items — to be able to run, the bid can only be placed when a particular deal ID is there. The only way to recognize that it's that kind of inventory. The program that you want for this campaign is to explicitly target a set of deals.
    *   I can't speak for other DSPs, but there are PMPs that are activated with deals where you get access to some type of curated inventory. True across every banner type campaign.
*   Garrett
    *   Same creative for same advertiser through different DSP and different publisher is a new matrix of Deal IDs.
*   David
    *   As we said months ago, same story.
*   Jeremy
    *   When you say 500 or 1000 deals, is that per campaign?
*   David
    *   It depends. I have some number - let's say in the hundreds - of deals of various usage patterns across all of our sellers (25-26 different sellers). If a deal appears, the amount we want to bid is within the floor there, then we would bid with that deal.
*   Jeremy
    *   With all of the sellers we work with, 500 or 1000 deals. You're saying, these deals can apply to any campaign? And for any campaign that comes up, you may pick one?
*   Hillary
    *   When the bid request comes from the publisher, that comes with some number of deals. The decision engine algorithm that those into account and returns a deal with one or more than one deal ID attached.
*   David
    *   Might be more than one bid, hence the more than one deal ID.
*   Hillary
    *   Just because a bid happens doesn't mean that bid wins. When you're talking about what gets returned in the ad markup, can have 10 auction winners with bids.
*   David
    *   For each creative that's been embedded in that interest group — because you don't have a concept of line items — we would have to put 400 or 500 potential deals. Everyone's nodding their heads on the GVC.
    *   Plus, the namespacing to know which partner, but that's a separate issue. So we're going to need however many potential deals are across all sellers.
    *   Deals are not specific to ads. The interest group is potentially used across a lot of sellers.
*   Andrew Pascoe
    *   I agree with everything that David and Gerrett were saying. This is not a scalable solution. I think DV360 said they can use this, but they're kind of a unique DSP - work with really large advertisers, who can command their own deals and go directly to publishers. But a lot of deals apply to pretty much everything. For every interest group, for every creative, we have to duplicate this list because we have no idea what site this bid is going to get triggered on. A lot of us negotiate deals across a wide swath of our client base for campaigns. 
    *   The IG structure itself requires a lot of duplications because it works at the creative level. You'd have to put the same deal IDs across every ad across every IG across our entire inventory. Within an IG, we would require some logic - a lot of them apply contextually based on what the supply is on the page.
*   Paul Jensen
    *   Three times I heard mention of inventory-dependent stuff. The bid request includes deal IDs. And Andrew was saying that deal IDs apply across wide swath of campaigns.
    *   We've talked about how deal IDs can be used to specify an index into a list of deal IDs that's provided in the request.
    *   What percentage of the deals that you use are coming form the seller's bid request?
*   David Dabbs
    *   100% of them. We negotiate a DSP. For us to use a deal, we have to had negotiated it with the seller. That's one bar.
    *   For a buyer to be able to apply a deal and transact with it on some ad opportunity, the seller has to say — in the bid request (today in OpenRTB, and in PA we will be lifting and shifting and reformatting that OpenRTB signaling into the sellerSignals) so that the seller can tell me, this is the range of deals that apply here.
    *   If I want to generate a bid and that meets the floor and all of the other rules associated with the deal, then I will bid with that deal.
*   Paul
    *   What's the cardinality of the deals that the sellers are passing into these auctions?
*   David
    *   I don't know. It depends on the seller and the deals that apply.
*   Michael Kleber
    *   This question is something that came up before. Hillary, you were the one that gave us the best answer, which I think is that something like 90% of the bytes is the list of deal IDs.
*   Hillary
    *   Not 90%, but well over half.
*   Garrett McGrath
    *   The beginning of this conversation was about server capacity at k-anon. That number fluctuates a lot - changes a lot. One thing that would be helpful is knowing the capacity you're working with.
    *   Also, seat ID needs to be there.
*   Jeremy
    *   Can't speak to that specifically.  Only one of the things that affects k-anon.
*   Hillary
    *   No way to know which deals will apply a priori to the request, you're in Schrodinger's Deal ID.
    *   You're adding a lot of pain by putting it where it is, on the creative.
*   Jeremy
    *   If a deal is not specific to any ad, what kind of deals are we talking about?
*   Hillary
    *   Can represent any aggregation or inventory or audience that could possibly exist on the Internet.
*   Jeremy
    *   Or business deals and terms?
    *   several people: Yes, that too
*   David
    *   Are you asking specifically about how they're divided across the sellers, or just why we use them? Are you asking that because you want to segue into how you're asking us to organize them and place them on the IG?
*   Jeremy
    *   More for the latter.
    *   PMPs - there are typically negotiation between a publisher and an advertiser. If you have a pool of deals that could apply to any advertiser and any ad, then it's likely it wouldn't be the first type.
*   Garrett / David
    *   It's all of those things,
*   David
    *   What we have is this one-size-fits-all model - we have this one vector of 0-N deals, and ad tech are free to use this as we want them.
    *   Are you going to make that a richer description? I heard Paul allude to that - wouldn't be a deal ID but rather use an index
*   Andrew Pascoe
    *   To elaborate on something David just said, you shouldn't think about deal IDs as just deals - it's something used to communicate information, setup in the background business arrangement.
    *   In the same way an interest group isn't just about interests, I can use it however I want.
*   Hillary
    *   Andrew's right
*   Andrew
    *   Might be passing information that we can get a preferred rate, or there's some minimum buy we need to achieve. Or it could be just a number that's designed to mean some kind of inventory. What's the fastest way to onboard a new data source. Just don't think about it as just deals.
*   Hillary
    *   You can think about it as an alpha-numeric representation that's some instruction between one or more parties over the wire.
*   Michael
    *   Important distinction, which is what Paul was getting at earlier. Two different fates for deal IDs. Maybe this is a helpful distinction to go from the wide amount of uses of deal IDs and put them in two different classes.
    *   The question is — which of the many uses of deal IDs are things that need to show up in event-level reporting? I'll remind everyone here that the whole paradigm of Protected Audience is that there are two types of ways to get information out about what happened:
        *   There's event level reporting - learn exactly what creative appeared on exactly which page.
        *   Aggregate reporting.
    *   David - I hear you say that all of them are needed for event-level reporting, but hearing all the ways you're describing deal IDs used, it sounds like many of these are well-supported by aggregate reporting. With aggregate reporting, we can be a lot more generous with getting that information after the fact.
    *   Sounds like we're ending at the same place we did last time.
*   David Dabbs
    *   To Laurentiu's point, if we're going to use a deal, we need to reconcile it and the seller needs to reconcile it. We'll need it at row-level reporting. Could there be a way to reengineer things? I don't know, but it doesn't seem to be worth turning our system on its head. If I'm bidding on a deal, both we and the seller need to know that.
*   Jeremy
    *   The more we discuss, the more I'm hearing that different DSPs organize deals very differently. Are there other DSPs that have other organization models for deals that you'd like to share?
*   Garrett
    *   You can ask DV3
*   David
    *   Or Dayo - not present here, but invested in Protected Audience.
    *   One thing to say is — irrespective of the cardinality — we do have to solve, because the deals are subsetted by the seller, and you have it organized as a single list, it makes it difficult (or very awkward) to use. Different facet of the ergonomics from a DSP perspective.
*   Jeremy
    *   Will bring the feedback back. Challenging design because everyone's model is a bit different.


## Security Model [From Hillary Slattery, IAB Tech Lab]

Initial discussions around some kind of security model design. If Tech Lab creates some proposal, what would the next steps be? 



*   Hillary
    *   There was discussion about designing some kind of security model. Only want to send some information to verification vendors. Appointed back to Tech Lab.
    *   If me or Miguels's working groups came up with a security model, doesn't fit in any of the GitHub repos.
*   Michael
    *   Great for this call - the PA WICG call - or the GitHub repository that has turtledove in its name. The idea that some parties might have different information that gives special access to information for anti-fraud vendors or something like that is different from what everybody else in the world gets is not something that's part of the Privacy Sandbox right now. Something we'd like to have a conversation about.
*   Paul
    *   Who are the parties, what's the information you'd like to get to those parties?
*   Hillary
    *   Reach out at hillary@iabtechlab.com for an out-of-band primer.
*   Paul
    *   Last May on the W3C, anti-fraud community group presented all of the different kinds of abuse and fraud someone might want to perpetrate against folks?

No meeting next week, Apr 23, 2025. We'll next convene the following week on Apr 30, 2025.
