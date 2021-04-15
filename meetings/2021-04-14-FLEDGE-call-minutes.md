# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 4pm UTC.

That's 11am Boston = 8am California = 5pm Paris time (the winner of the [doodle poll](https://doodle.com/poll/eht2imrtvec69hxx?utm_source=poll&utm_medium=link)).

(This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload)


# Next meeting: Wednesday April 14 2021


## Attendees: please sign yourself in!

1. Michael Kleber (Google Chrome)
2. Basile Leparmentier (Criteo)
3. Michael Lysak(Carbon)
4. Brian May (dstillery)
5. Brendan Riordan-Butterworth (IAB Tech Lab / eyeo GmbH)
6. Allen Fung (ShareThis)
7. Abdellah Lamrani Alaoui (Scibids)
8. Luke Whittington (Glimpse)
9. Phil Lee (Google Ads)
10. Paul Jensen (Chrome)
11. Joel Meyer (OpenX)
12. Matt Wilson (NextRoll)
13. Nathan Jia (Nielsen)
14. Thomas Mouron (Teads)
15. Michael Coward (Arcspire)
16. Gwen Gillingham (Nielsen)
17. Jonasz Pamuła (RTB House)
18. Przemyslaw Iwanczak (RTB House) 
19. Ryan Arnold (P&G)
20. Erik Anderson (Microsoft)
21. Matt Kendall (Blockthrough)
22. Brad Rodriguez (Magnite)
23. Al Macdonald (Glimpse)
24. John Bryson (Resonate)
25. Kale Smith (Nielsen)
26. Mark Lee (Carbon)
27. Cory Watson (Search Discovery)
28. Cory Underwood (Search Discovery)
29. Vedant Seta (Media.net)
30. Don Marti (CafeMedia)
31. Larry Price (OpenX)
32. Anthony Molinaro (OpenX)
33. Wendell Baker (Verizon Media)
34. Kelda Anderson (Microsoft)
35. Mehul Parsana (Microsoft)
36. Robert Ries (Nielsen)
37. Russell Stringham (Adobe)
38. Jeff Kaufman (Google Ads)
39. Christa Dickson (Meredith)
40. Viraj Awati (Amazon
41. Al McLean (Carbon)
42. Amelia Waddington (Captify)
43. Vincent Grosbois (Criteo)
44. Gang Wang (Google Ads)Newton (Magnite)
45. Sean Bedford (Facebook)
46. Brian Schmidt (OpenX)
47. Phil Eligio (EPC
48. Vishnu Prasad (Nielsen)
49. Andrew Pascoe (NextRoll)
50. Les Cochrane (Audigent)
51. Grzegorz Lyczba (OpenX)


## Note taker: Brendan Riordan-Butterworth

Michael Kleber: Notes are collaborative.  Brendan is taking first draft.  Correct as needed, especially if you speak.  Notes will be posted publicly on GitHub later.  

## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.

[<img src="https://i.insider.com/5fc55f5350e71a00115580e8?width=700" alt="Screenshot showing location of the Raise My Hand button">](https://www.businessinsider.com/how-to-raise-hand-on-google-meet)

### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/



# Agenda

## Limitations on bidding and scoring functions

_For the April 14 call, I would like to discuss [#132](https://github.com/WICG/turtledove/issues/132) "Restrictions on interest group JS processing power/memory usages" and [#154](https://github.com/WICG/turtledove/issues/154) "Edge MPC". And more generally, I'd like us to discuss the type of computations needed for bidding and bid scoring._

_The recent proposals of a [Dovekey auction using secure 2PC](https://github.com/google/ads-privacy/blob/master/proposals/dovekey/dovekey_auction_secure_2pc.md) and the [MaCAW 2PC extension to PARAKEET](https://github.com/WICG/privacy-preserving-ads/blob/main/MACAW.md) are both relevant here: the MPC approach offers substantial wins in both privacy and utility, but only if the functions to be computed fall within the restrictions imposed by their computational model._



MK: Getting onto the Agenda item.  Issue number 88 where the initial call was linked.  

MK: I would like to spend today on the subject of limitations on bidding and scoring functions.  Running start.

Wendell Baker: I would like to express thanks to MK and everyone else who is stuarding this in a professional manner.  This is important, and it’s great that people are taking a professional stance.  

WB: Warning that there are people who are not involved in W3C and trying to insert themselves by inflammatory social media and elsewhere.  

MK: I would like to present my thanks in turn, for everyone who is here, engaging professionally with these proposals.  Making this effort and others possible to move forward over the last year.  Interaction and feedback has made proposals much better.  It is sometimes trying.  There are people scared off of the public process because of downsides, it is rough sometimes, and I appreciate everyone who is involved.  Great value.  

MK: Moving on.  Section “Limitations on Bidding and Scoring Functions” in issue [132](https://github.com/WICG/turtledove/issues/132), [154](https://github.com/WICG/turtledove/issues/154) (Edge computing, not browser), discussion about where the bidding and auction should happen are part of the larger Dovekey / MaCAW (published late last week) / PARAKEET proposals.  This includes the MPC/2PC aspects of these.  

Kelda Anderson: Here! (in context of MaCAW, Edge as a browser)

MK: The original proposal in FLEDGE was to have bidders and the person running the auction both get to run arbitrary JS.  In the Worklet environment.  No network, no access to the surrounding page.  It’s a clean design, in theory.  It has issues with practicality.  Someone with Ad Tech put it: more cost is free to the server, the client can be overloaded.  

MK: There are other potential problems with arbitrary JS, which includes unlimited time!  Some sort of limits on time, CPU usage, and possibly other limits that are valuable, also!

MK: Dovekey and MaCAW both involve moving computation to a server.  There are tradeoffs for utility and privacy in this.  Relying on MPC (for privacy wins) inherently comes with limitations on what the bidding and ranking algorithms can do.  

MK: In Dovekey MPC, the limit is being stuck on a final bidding process on the dot-product of two vectors, IG and Context, with some external info possible.  In MaCAW, there is a generalization of this, something that involves a restricted language to write the computations.  You can write more complicated (like arbitrary tensor-flow), but there is a similar complicated risk.  There is overhead between MPC servers, which limits the depth of complexity in this type of environment.  

MK: Given high level overview, I would like for us to spend time.  What are the operations actually necessary to do bidding and auction functions, and their feasibility in the Dovekey, MaCAW, and browser models.  Would like to understand the tradeoffs between them. 

MK: Queue:

Basile Leparmentier: We are starting to work on this at Criteo.  We have a group for advertiser.  First step: who is going to run for a campaign.  This is going to be quite small.  If we have multiple campaigns for advertisers, we are going to have multiple bidding functions, and this can be quite big in 2 dimensions.  Introduces cost in CPU and also memory!  

BL: One of the hardest things to compress is contextual data.  Reality of the web, context is big!  High memory, and then also high CPU.  

BL: Once you have done bidding and ranking, you have brand safety overlay for advertisers.  This again necessitates very big memory footprint, so that you have choice by domain (and even subdomain).  You will need a potentially quite large list in the client!  Plane crash example, could include a huge number of URLs.  This list can become quite easily quite big if everything is done inside the browser.  

BL: After Brand Safety and Bidding, you have to choose rendering.  One banner per size?  And then the product.  If you need to preload.  If you want to show a product, preload the bundle, it’s going to be not linked to the user, you can poll the server, but this will be a whole lot of stuff over the wire to handle all eventualities.  Criteo holds several TB of creative at any given moment.  

BL: On the pub side, you want to have a blocklist of parties that you don’t want to trade with.  Some products from Walmart might not be acceptable for some publishers, so there might be very large blocklist for keeping these subsets of ad types from occurring.  

BL: Blocking IPs shown to be fraudulent is also an area.  Don’t have a good idea of how to do this, but another risk.  

MK: Pulling some things apart.  

MK: Some of the uses you describe involve lookups in lists.  There are clever compsci options to make lists smaller (but…).  Some things you describe don’t fit into that lookup in the same way, like sending a list of all URLs that match the “plane crash”, seems to cardinal to send.  

MK: FLEDGE: the contextual is purely contextual, some sort of server-side lookup can add labels like “adult” or “plane crash” from a somewhat large (but bounded) list of sensitive labels.  The analysis can be handed back to the browser.  

BL: This was in one place, but not needed anymore in FLEDGE?  

MK: My intention is that, in FLEDGE, there is a purely contextual interaction between the browser and some ad networks.  The object that gets passed in, the “per buyer signals”, that allows signals to be passed in.  

BL: We are reaching into a new issue with TURTLEDOVE.  We need to know the user.  We cannot pre-choose what DSPs are called.  If you are Walmart, you still need to answer to every request in case they have your user.  I fear that the server will be called 20k times for 1 actual impression.  This might be a scaling issue.  Not clear that it’s easy.  

MK: There is a method to add insights, even probably in MaCAW.  Agree that, in any case, not knowing what IGs a user is in could introduce scaling issues.  There are potential caching opportunities.  

MK: Aside from what mentioned to date, you talked about ML engine?  Tensor flow or dot-product.  Would you like to say more about that?

BL: These will need to work on every publisher, so they will need to be big.  Dot-product will not account for user signal.  You need 2 separate models, so that one can say “hey they’re on a travel site and area currently interested in travel!”.  

BL: One of the advantages about deep models is learning about interaction between features, but this is not possible with the scoping of curren system. 

Mehul Parsana: In the MaCAW, we’re not separating the context/user signal, so there’s some integration possible here.  But it’s not presented to this group yet!  

Mehul Parsana: Questions: Too many campaigns from one pub, allow/block list, contextual and behavioral signal, we think these are key questions.  

MP: We can have discussion further in the future, but for now focus on the scalability question? 

MK: You had other items? 

MP: On the restriction of the computation.  In MaCAW, we tried to have 2 different calculations.  (1), user signal by DSP, who doesn’t see context.  (2) generic by the SSP, who is running the auction.  

MP: There is a compiler, to run on your own machine, doesn’t require browser, it has a model for client/server to test what goes wrong.  There are a lot of assumptions about factorizing the compute.  There are 1000s of campaigns, 100s of advertisers, all of this in the model.  So some factorizing has to happen. Would love to get more thoughts on that.  

MK: Yes, apologies for thrusting MaCAW into spotlight ahead of announcement.  

Brian May: This is going to be an arms race to get the resources they need to compete.  Have the browser tell people with quotas, provide a model that’s reproducible server side, so that folks can experiment, figure out what works and they know when they run things in the browser how it will behave.  

BM: Can the contextual signal be used as a way to feed info back to the browser?  It would only be meaningful in campaigns where the advertiser could give a go-no-go response.  Some class of FLEDGE ads that would be able to respond to contextual signal?  

MK: Yes.  

BM: Creatives on the browser stay dormant until a signal from a server says “OK, go ahead”.  

MK: There is potential for declarative rules, like “I will only participate in bidding if these conditions are met”.  If arbitrary JS is allowed, this can be done from scratch, but more restrictive set of allowed language would necessitate the inclusion of more structured function.  

Jonasz Pamula: A couple of comments.  +1 to BM comment.  It is hard to establish early on about the type of model needed.  Iterative process, once experimentation can be done, we can talk about what works and what can be added.  There are lots of details missing, like how costly it is to start the bidding process, is there caching method, and other relevant questions. (Issue #80) When it comes to CPU limits, we want to put … One important thing to note is that the CPU and other limits are useful, but optimizing where computation is going is important as well, issue number 79.  Even in today’s environment, we don’t process all of the campaigns that are eligible to win an auction, we are able to prioritize which campaigns we want.  If FLEDGE can provide an analogous function, it would provide optimization.  

MK: Does each campaign come with a signal that input into the early filtering? 

JP: A very simple model that gives preliminary score, then order the ads, then invest resources into the highly ranked campaigns.  

MK: User / Context - what is input into the simple model?

JP: More data is better, but a very simple model can still be useful.  

BL: You can have some pretty simple to compute assumptions that feed into the simple model, first pass.  You don’t want to send 10 bids, because they’ll compete against each other in silly ways, potentially against themselves.  

MK: Interesting new topic, the way I wrote it in FLEDGE, based on TURTLEDOVE feedback. Buyers get to bid price, but sellers get to order based on their own metrics.  If the seller rank is different, there is a risk that the buyer only submit 1 bid, but then there’s complexity.  

BL: Yes, it’s possible that this could be detrimental.  Seller want to have most suitors, but there’s also downside to bidding against yourself as a buyer.  

JP: One last quick comment: One topic that is very closely related is how to train the models.  We can agree that certain computation models are useful.  But if we can’t train, given the reporting mechanism, we might not be able to keep these models.  (Issue #93)

MP: We are working on this as a next step (MaCAW / PARAKEET)

BM: A resource allocation quota system, that will give us an on-browser ecosystem, use the opportunity wisely!  

Joel Meyer: Blocklist issue that was raised.  As an SSP, DSPs can upload lists of 100s of domains that they don’t want to bid on.  We would have to process this on the browser.  RE2 might be useful, but…  The browser could send this home?  

MK: Any sort of send something home would have to happen for every page, not just because of IG membership, since IG membership trigger would disclose.  Proposal was that a bloom filter would allow a very large list and compute very efficiently whether a particular domain was on a list, making local computation efficient on device.  

MK: Depending on which proposal, the server-side component could solve the “don’t bid on certain domains” without depending on the browser.  Fledge, trusted bidder could load just a bit about bid/don’t bid.  It is feasible to affect the bidding.

MP: With PARAKEET, we are looking to make greater granularity on context available in the server.  This did invluve adding C’.

MP: There is definitely the case of lots of spurious requests, that would allow the reality to be kept from the bidder, to support differential privacy assertion.  

MP: API should be kept generic, so that different scenarios of traffic load can be handled.  Dynamic sending of spurious requests to maintain anonymity, but PARAKEET and MaCAW try to keep the processing away from the client.  

Gang Wang: All the lookups would happen on the DSP servers.  The response goes back to Dovekey Server.  The auction logic can include information from the context bid to filter out some responses.  

MK: So we have a good understanding of the different models.  

BL: If all the things I said could be done in a single gatekeeper (or if it’s achievable / available in a trusted server), I think that the edge computing issue can become a non-issue.  If what it does is sending a request to a server, its easier to do a whole lot of things.  Depending on how much can be sent to the server, the more or less of an issue running quotas.  But there’s balance on how much privacy

BL: Worst case, we have to create different companies to grab a larger scale of quota in the client!  

MK: Thanks for the discussion, lots of information.  

