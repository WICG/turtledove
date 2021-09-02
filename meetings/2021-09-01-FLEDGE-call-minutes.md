# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC.

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Sept 1 2021


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Russ Hamilton (Google Chrome)
3. Wendell Baker 
4. Brian May (dstillery)
5. Jeff Kaufman (Google Ads)
6. Caleb Raitto (Google Chrome)
7. Chris Evans (NextRoll)
8. Christa Dickson (Meredith)
9. Don Marti (CafeMedia)
10. Fred Bastello (Index Exchange)
11. Matt Menke (Google Chrome)
12. Paul Jensen (Google Chrome)
13. Joel Meyer (OpenX)
14. Newton (magnite)
15. Erik Anderson (Microsoft)
16. Ryan Arnold (P&G)
17. Michael Coward (Arcspire)
18. Bartek Łoś (RTB House)
19. Isaac Schechtman (IPONWEB)
20. Tamara Yaeger (IPONWEB)
21. Brad Rodriguez (Magnite)
22. Martin Gruau (Captify)
23. Brendan Riordan-Butterworth (eyeo GmbH)
24. Jonasz Pamuła (RTB House)
25. Przemyslaw Iwanczak (RTB House)
26. Paul Farrow (Xandr)
27. Basile Leparmentier (Criteo)
28. Vincent Grosbois (Criteo)
29. Julien delhommeau (Xandr)
30. Andrew Pascoe (NextRoll)
31. Bill Landers (Xandr)
32. Aditya Desai (Google)
33. Mehul Parsana (Microsoft)

 \



## Note taker: Joel (at first), Basile (for Multi-SSP discussion)


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

 		


## [Suggest agenda items here]



*   Jonasz Pamuła: https://github.com/WICG/turtledove/issues/215
    *   Framework for FLEDGE tests in Chromium: https://github.com/RTBHOUSE/chromium-fledge-tests 
*   Continue discussion of multiple SSPs / multiple auctions for a single slot

Standing Agenda Item: Open Issues, or Issues that Need Opening

https://github.com/WICG/turtledove/issues?q=is%3Aopen+is%3Aissue+label%3AFLEDGE


## Notes

MK: We’ll start talking about #215, bidding worklet perf raised by Jonasz.

Jonasz Pamula (JP): Comes from Bartek, but I will present. I’d like to start by promoting a tool that we created and have shared with the community. Would be good to give it more visibility. At [RTBHouse we have built a tool](https://github.com/RTBHOUSE/chromium-fledge-tests) that allows you to play with Fledge implementation in Chromium. Allows you to specify the interesting parts like joinAdInterestGroup, etc, then runs Fledge auction using selenium and you can perform tests. If you’re wondering about internals of Chromium implementation, or it’s too hard to determine impl specs from explainer, it might be easier to write a simple unit test and see how it works. If you’d like to see what can be piped through reportWin, then you just write a simple unit test and see how it works. This is our contribution to the community. Available on GitHub (https://github.com/RTBHOUSE/chromium-fledge-tests) if you’re interested. Please contribute if you want to. Please take a look, we’re happy to answer any questions.

JP: Let’s move on to the essence of the issue. We think it’s going to be crucial for the success of fledge to optimize the invocation of the auction and each generateBid call. Using our tool, we’ve made some benchmarks and it seems there is a significant gap between an optimal invocation of a bidding function and the invocation that can happen in Chrome. For the purpose of the benchmarks we have extracted a minimal representative implementation - we stripped everything that is not crucial - only left that which is crucial to our bidding model. When benchmarked in a tight loop we see about a 1ms invocation time. When done via Chrome worklet we about 50ms. We have a number of questions. First one is, how much work do you think will be put into optimizing the worklet execution prior to origin trial?

Paul Jensen (PJ): Thanks for reporting the issue, very thorough. I didn’t have a chance to dig in too deeply yet. I would ask that you put your Chromium patches into the Chromium Gerritt so that it’s easier for us to run them. There definitely is a pretty big discrepancy between optimal invocation vs Chrome invocation. Two things I noticed are that 1) optimal invocation looks to be using Node.js which is different than Chrome and may not be apples-to-apples. A closer comparison would be to use the Chrome JS engine which has to deal with a lot of constraints. 2) in Chrome’s implementation we have turned off some of the JS optimizations that v8 uses (eg JIT). Some of that was done to simplify security considerations. We may look at some of these decisions again. We need to consider the ramifications of things like JIT on security, and the interactions between sandboxing. Sandboxing constrains some of what we can optimize. We’ll take another look, but a more apples-to-apples comp would be helpful. Then we can find areas that we might be able to optimize, or at least understand the impact of our constraints.

JP: Thanks, I have some more questions but I see Basile raised his hand.

BL: I understand why an apples-to-apples comparison is needed, but at the end of the day we need the ability to run our code in less than 50ms. Realistically, at the end of the day, your constraints don’t matter to us. Question for you JP: the size of the matrices you used seemed quite small to us, but in reality how much bigger are your matrices? Rough idea is good enough.

JP: The answer is “I don’t know.” In the end we are going to run models that are as big as possible. We’ll optimize the execution environment and use models that as big as possible.

BL: The models you used, IIRC, are around 40k [floating-point numbers, a 200x200 matrix —MK], but that’s much smaller than what we’re used to.

JP: We’re trying to be realistic in our models that we run in the client. The NN that we included, we think that if we can optimize it, then it will be very beneficial for the execution of the final bidding function. We feel that this is very representative of the kind of models that we want to run. BL: if you have numbers of the amount of computation you’d like to run, then I think that would be a good contribution to the discussion.

BL: We’re not taking this action yet, but I think what you have done is very interesting and important. Like you said, there’s a lot of overhead in the bidding function itself. I don’t think we’ll have numbers soon, but the size of the models on our side is in the millions or even tens of millions. Something like 250MB which we won’t be able to ship into the client, but just a reminder that on the server side we can do really big models and on the client we can’t, which means lower performance for publishers.

Vincent Grosbois (VG): Small question, maybe I missed it. For the performance running on the worklet there is no JIT, but is it impossible to ever add it? In other words: can we add JIT to the bidding worklet.

PJ: We’ll revisit it. Our initial impl had a different process model and we’ve since switched, using more processes to add more security. That may change some of the security constraints. We’ll have to revisit this.

VG: Thanks.

MK: JP something else to add?

JP: Yes, JIT is something we noticed. Another is the fact that the bidding function is downloaded synchronously before the auction which adds to the overall auction latency. I’m wondering how you see the current implementation of Fledge. Is this just a first draft that will be refined, or are we pretty close to the final version? Are there other areas that we could get optimization to speed up the invocation significantly? The question is important because we don’t know if we should optimize in the current environment or if we should wait a bit or be aware of changes that are upcoming.

PJ: I wouldn’t say it’s a first draft, it’s been evolving since May. As I said we’ll continue to revisit and we’re interested in feedback about what sorts of things you’re doing in the worklets that we should focus on for what we can do to improve them, whether that be JITing or other. In terms of synchrnously downloading the worklet, we can look at how that’s cached so that we don’t have to download it. There are various trade-offs here. In V8 you have to compile, parse, JIT, and different steps can be cached along the way. So we have things to look at. Privacy and security are the constraints we need to examine. We will definitely look at it again. Thanks for the benchmarks.

JP: I was wondering, would adding support for web assembly be an option?

PJ: It’s certainly something to consider. WebASM has a lot of different features and those features have different privacy and security consideration that we need to look at. I think knowing what kind of things.. I looked at your function and it seems like it’s mostly doing a matrix multiply. If that’s something that really important we can look at optimizing that.

JP: Yes, that would be helpful. If Matrix Multiplication or NN execution in general can be optimized, we would definitely benefit from that. We see improvement to revenue when we expand our NN. The more matrix multiplications we can perform, the better the results.

BL: Just wanted to add that I would be extremely happy if we can improve everything inside of the worklet. But if we see that’s not going to be enough due to security and privacy limitations, we also have the possibility of giving more info to the trusted server. Please don’t forget that there is optimization area outside of the client. 

MK: Thanks for bringing that up, that’s the point I wanted to mention. I know we have some MS Edge folks on the call. This sort of feedback on the performance of doing things in the browser is extremely helpful for us. I was wondering if you have a comparison with the MS Parakeet proposal where a lot of the computation remains on the servers of the buyers, but there’s a trade-off where you get much lower resolution signals because the ad request itself passed through a layer that obfuscates the info. That allows you to make the model as big as you want, but you have the trade-off of lower res input signals.

JP: Our intuition from the beginning was that precise signals are much more important but it’s really hard to say something definitive without know ing the specific of how the signals will be reduced in Parakeet. If there’s more info on that it would be helpful.

MK: Great answer, makes sense, others have had the same question.

Mehul Parsana (MP): Just wanted to clarify on the Parakeet question - we are sort of doing a two step process. First step is to provide differentially private signal and contains multiple IGs in the request  that allows ad server to reduce number of ads from few thousands to few tens. in the future MPC style work or client side finalize ads, you get all the resolution back and can use calibration models to recalibrate the score you computed on server. So Parakeet shouldn’t be thought of as only a lower resolution approach. We don’t think evaluation of only a few tens or hundred of ads on the browser is sufficient OR adding thousands of ads on client is scalable. 

MK: Helpful but does not answer the question Jonasz raised about the need to know what signals are available as part of the ad request received by the SSP / DSP.

MP: That’s fair. Those signals are determined by the number of requests coming in. We are trying to take a live assessment of the traffic and determine that. With enough traffic the contextual signal improves. With IG group vector, same thing. With enough traffic that gets better. We’re not trying to hide it, it just depends on the amount of traffic coming through.

MK: Makes sense, if there is code that people can look at to determine the trade-off between volume and noise that would be very helpful for us to look at, as well as for folks like Basile and Jonasz. They could look at volumes they get and see what to expect.

MP: Makes sense, but going back to the main question about processing available on the client, we believe that you can do much larger scale processing with a hint (where hint Differentially Private interestgroup vector with k-anon publiser data). Once the candidate set is reduced, then you can follow up with client-side recalibration or MPC computation to leverage precise interestgroup.

MK: Absolutely makes a lot of sense, now that Parakeet has the finalization step that allows the running of on-device compute it makes it possible to break the bidding process up into two stages. A server-side stage with more data and a client-side stage with less data. BUt I don’t know if that two-step process would actually help take some of the MM computation out of the browser, or if it would just lead to a same computation of the same size but for fewer ads.

JP: I won’t be able to give you a final answer on that, but let me point you to an issue that has already been submitted to the TD repo that performs a similar optimization. In parallel to technical optimizations, we could apply a higher-level logical optimization. In the current design, each IG is eval’ed independently. Though if there was a way for a buyer to say “I see there are multiple IGs for this user” and I can already say that this one and that one will not win, this would be a very significant optimization. If we can decide how much compute power to allocate to the IGs that are on the device that would allow us to cut away a significant amount of computation early on. This is issue #79.

MK: Makes a lot of sense and seems very in-line with what Mehul was saying.

MP: To clarify, the reason we had to apply Diff Priv was because of a single request that contains vector of interestgroups. This is somewhat similar to Augury API, no signal is given, just making a prediction of interestgroups to include candidate ads to evaluate on client worklet. Providing Differentially Private Interestgroups and a two step process (server side compute followed by clientside finalizeAds()/MPC) simplifies things. 

JP: I'd have some follow-up questions about Parakeet, but this probably isn’t the place. For now we can move on unless other people have suggestions or questions.

MK: Basile?

BL: just very quick to say that it’s very important to say that in Fledge the data we work on is very important. But there’s also a question about the data we’ll get to report on, which is on the learning side. It’s just not very clear to in Fledge what we will have to learn on.

MP: While precise info is available on client, in reporting it’s not as exact so on the reporting this is an issue so there’s way to accurately use this.

MK: I agree with Mehul here. The ability to move some of the logic to the server side would make sense for the reporting use case, which is why I'm very interested in hearing JP or BL  evaluation of this approach.

BL: I have an instinct that I want the figure to be as exact as possible on reporting but we dn’t have an answer yet, at least on our side. But then the idea of having a prefiltering step so we can have fewer candidates is interesting. 

MK: Thanks to all three of you, I’m happy to hear from other folks who want to contribute. Helpful for us on the Chrome side.

MK: We have 15 mins left. There is one additional topic, which is multi-SSP conversation continuation.

Can we get a new scribe? Basile! Thanks!


### Multiple SSPs / multiple auctions for a single slot

[Slides](https://docs.google.com/presentation/d/1q9bES3x5NrDqR1Qa-luQrPx3LZenx3TqWu6MPrB1gYE/edit#slide=id.ge9f7106d2b_0_0)

Joel: 

All the following sums up what was said to support the slide aboves

Why multiple SSPs are needed. Lot of caveats, but actually there is a good reason.

The main reason is that most campaign are filtered. SSP increase bid density. SSP have different rates. (most is available on the slides).

Overall multiple SSP ads to competition with ads to the total welfare.

Two approaches:



*   A publisher merges all SSPs code to unite it all.
*   Chrome allows for multiple runAdAuction, and the publisher  has a unifying way in the end.
*   Pub logic:
    *   Has access to SSP metadata and auction results and all information, but should be not too complex.
    *   It chooses its logic and then is able to get what he wants from multiple SSPs.

Brad [[slide 23](https://docs.google.com/presentation/d/1q9bES3x5NrDqR1Qa-luQrPx3LZenx3TqWu6MPrB1gYE/edit#slide=id.geb6c4d6a55_1_4)]: We have a section with graphs explaining multiple SSPs (in the slides).

There is a question on how to unify the final auctions. Text is also available in the slide.

What we propose is that the publisher proposes a configuration that is then handled by chrome to choose the final winner.

End of the slides

MK: In the final auctions, where the  browser choose based on the publisher config, what is available for the function? Is it the price and others, and is the competition between the DSPs or actually SSPs?

Brad: SSP are doing transformation to the bid, and a clearing price is then transferred to the publisher function. 

There are a lot of legitimate transformation to be done by the SSP. 

Joel: It should be considered as SSP bid, with associated metadata, given by the SSPs, with information about the dsp, such as brand.

MK: I like the simpler model, it appeals to me. Did the bid density argument relied on one SSP second pricing another SSP. 

Joel: The SSP has the billing relationship, and one SSP bid cannot floor anothers SSPs.

Brad: It would ad complexity and today we are not proposing a decision on what the final price should be.

Joel: This function chooses the ads with a price, is not running an auction.

MK: thank you, just choosing from different SSPS seems approachable. 
