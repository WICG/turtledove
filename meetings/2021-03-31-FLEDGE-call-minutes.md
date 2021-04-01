# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 4pm UTC.

That's 11am Boston = 8am California = 5pm Paris time (the winner of the [doodle poll](https://doodle.com/poll/eht2imrtvec69hxx?utm_source=poll&utm_medium=link)).

(This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload)


# Next meeting: Wednesday March 31 2021


## Attendees: please sign yourself in!

1. Michael Kleber (Google Chrome)
2. Brian May (dstillery)
3. Brendan Riordan-Butterworth (IAB & eyeo GmbH)
4. Allen Fung (ShareThis)
5. Jeff Wieland (Magnite)
6. Newton (Magnite)
7. Wendell Baker (Verizon Media)
8. Basile Leparmentier (Criteo)
9. Ryan Arnold (P&G)
10. Abdellah Lamrani Alaoui (Scibids)
11. Andrew Pascoe (NextRoll)
12. Kale Smith (Nielsen)
13. Christa Dickson (Meredith)
14. Jonathan Sullivan (Nielsen)
15. Robert Ries (Nielsen)
16. Luke Whittington (Glimpse)
17. Jonasz Pamuła (RTB House)
18. Przemysław Iwańczak (RTB House)
19. Don Marti (CafeMedia)
20. Paul Selden (OpenX)
21. Phil Lee (Google)
22. Raj Belur (Amazon)
23. Paul Marcilhacy (Criteo)
24. Sean Bedford (Facebook)
25. Erik Taubeneck (Facebook)
26. Grzegorz Lyczba (OpenX)
27. Jeff Kaufman (Google Ads)
28. Remi Lemonnier (Scibids)
29. Larry Price (OpenX)
30. Gwen Gillingham (Nielsen)
31. Alasdair Macdonald (Glimpse)
32. Phil Eligio (EPC)
33. Matt Wilson (NextRoll)
34. Nathan Jia (Nielsen)
35. Abhinav Sinha (PubMatic)
36. Marie Aniorté (Scibids)
37. Vedant Seta (Media.net)
38. Viraj Awati (Amazon)
39. Brad Rodriguez (Magnite)
40. Tamara Yaeger (IPONWEB)
41. Chris Evans (NextRoll)
42. Kelda Anderson (Microsoft)


## Note taker: Brendan Riordan-Butterworth

## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.



[<img src="https://i.insider.com/5fc55f5350e71a00115580e8?width=700" alt="Screenshot showing location of the Raise My Hand button">](https://www.businessinsider.com/how-to-raise-hand-on-google-meet)



# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


### Open Issues, or Issues that Need Opening

https://github.com/WICG/turtledove/issues?q=is%3Aopen+is%3Aissue+label%3AFLEDGE

MK: Bad news first.  Very little personally done on FLEDGE over the last week.  More issues filed, last 2 weeks have had little change.  

MK: Bunch of work done on FLoC origin trial.  Apologies for not a lot has happened.  

MK: This might be little happening!  But the floor is open.  

Jonasz Pamula: I have noticed that first change that has implementing FLEDGE in Chromium are merged or in review.  These align to the current snapshot of the FLEDGE explainer.  How can we have continued input, stay engaged?

MK: Paul might be best to answer.  Yes, you are correct there is an initial round of trying to write code to implement what’s described.  The code is first implementation of the initial version of FLEDGE that was described in the doc as it stands.  This is standard expectation for spec development work.  It is expected to be iterative, as the spec is worked on through collaborative process.  It is somewhat chaotic, but the way to implement standards, often times.  Browsermakers are used to this process.  

MK: Once Chrome engineers have something end-to-end but rudimentary, not all features but at least basic structure as to how to do this, there’s lots of room for features, ordering, tweaking.  It won’t be just me talking about it, there will be engineering team probably in GitHub and these meetings, probably.  We expect to be doing lots of iterative once we have the foundation implemented, that is the snapshot.  

MK: Changes will happen out of sync.  

JP: Continue talking in GitHub?

MK: Yes.  

JP: It seems that there will be some changes necessary, there are issues like multiple products being handled.  What will be best place for custom reporting tokens discussion (issue #93)?

MK: GitHub probably the best place.  That was for Aggregated Measurement, not Event Level.  Yes, it seems like there are 2 parties particularly interested in the product-level turtledove approach.  RTB House and Google Ads have both opened items.  

JP: Yes, and other companies.

MK: Maybe get everyone interested in the use case.  Instead of 3 different issues, get interested parties together and develop consensus, instead of leaving it up to the Chrome team to decide.  

MK: Other thing - there are some things, that will necessitate changes to the current code, yes.  Everyone writing code now expects that their code may need to change.  

JP: Last quick question: Spec for Custom Reporting Token - have any thoughts? 

MK: Not right now.  Not in my brain, been full of FLoC.  

Newton Koumantzelis: Trusted Server for sellers, debating what will be returned on that.  “Value” vs “Render URL Key”, String or Object?  

MK: Clarifying.  One of the changes that has been made in the last 2 weeks.  The trusted server that provides input into the auction has 2 potential consumers.  Not only the bidders (initial proposal, just a string, or list of strings).  Since the last time, we’ve added another user of the same mechanism, which is the seller.  The seller can now retrieve in real time, but the key is the render URL of the particular ad, accompanied.  

NK: Both return an object?  

MK: Yes, both will be expected to 

NK: Where the bidder can provide a series of keys, the seller can provide just the render URL.  

MK: That’s the proposal from issue #108.  There are no alternatives proposed yet.  

NK: You’re expecting it to have same thing, URL structure plus hostname?  

MK: Yes, seems to make sense.  

MK: Any other of…  

Remi Lemonnier: We have a few open issues.  Question today, issue #101.  About reporting, aggregate reporting for training bidding models.  If we say that there is going to be an aggregate reporting that enforces k-anonymity but has the goal of providing enough info for training.  Some anonymixation procedures ware going to delete relevant information, handicapping ML process.  We wanted to propose some methods, but wanted to understand if there was going to be a trusted server or MPC framework?  What is the constraint on the anonymization procedure?  What is short-term vs long term?  Short is trusted server, long is MPC in place?

MK: The situation you described at the end, Short is trusted server, long is MPC in place, sounds entirely plausible, but no public discussion on the trust model.  

MK: Looking at issue #101, it seems like it’s open-ended.  Picking a way of collapsing things down is TBD.  Charlie Harrison is probably very interested in seeing a specific proposal.  In generally, no matter the agg machinery, goal is that we come up with a model that means we don’t need to say “yes, some server gets to see all the data”.  We dont’ want to bake into whatever choices we make that someone gets to see full cross-site user data.  MPC is a model for aggregation that doesn’t allow anyone to do this.  

MK: We do want to be able to have a more secure way of doing things, pushing towards in the long term.  

RL: Even if there is a trusted server that works within Chrome trusted envelope, that’s not goal.

MK: Our proposals haven’t used a trusted server.  PARAKEET does seem to work under this model, I think.  That’s a possible direction to go, but there are obvious tradeoffs.  

Brian May: Parakeet has suggested that there is a proxy, not necessarily an independent system.  It could run on the client. 

MK: The proxy could have global insight?

BM: Yes.

RL: Follow-up: The constraint for future method proposals is that there shouldn’t be a dependency on global oversight.  

MK: Additional questions?

NK: JP was mentioning Chromium pull requests?  Can you link them?

Grzegorz Lyczba: https://chromium-review.googlesource.com/q/status:open++fledge

MK: OK, any additional questions?  

Kelda Anderson: I don’t want to speak too much without full team, but we do want to invite next week at this time to dive into Brand Safety concept more on the PARAKEET topic.  

MK: Issue on the GitHub repo of the Web-Adv BG, issue 104, of all the related meetings. https://github.com/w3c/web-advertising/issues/104 Mentions FLoC one-off, PARAKEET periodic, this FLEDGE call.  Join as you see fit!
