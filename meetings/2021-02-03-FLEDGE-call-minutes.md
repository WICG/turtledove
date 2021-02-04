# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 4pm UTC.

That's 11am Boston = 8am California = 5pm Paris time (the winner of the [doodle poll](https://doodle.com/poll/eht2imrtvec69hxx?utm_source=poll&utm_medium=link)).

(This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit) will be editable by everyone, during the meeting)


# Wednesday Feb 3 2021


## Attendees: please sign yourself in!



1. Michael Kleber (Google Chrome)
2. Michael L (Carbon)
3. Pedro Alvarado (Resonate)
4. Abdellah Lamrani Alaoui (Scibids)
5. Kate Dye (district m)
6. Theophile du Besset (Scibids)
7. Brendan Riordan-Butterworth (eyeo GmbH)
8. Brendan Riordan-Butterworth (IAB Tech Lab)
9. Erik Taubeneck (FB)
10. Allen Fung (ShareThis)
11. Jonasz Pamuła (RTB House)
12. Josh Karlin (Google Chrome)
13. Valentino Volonghi (NextRoll)
14. Chris Needham (BBC)
15. Hong Cai (BBC)
16. Axel Boidin (Teads) 
17. Amelia Waddington (Captify)
18. Jeff Wieland (Magnite)
19. Bartłomiej Romański (RTB House)
20. Marçal Serrate (Hybrid Theory)
21. François-Xavier Solvar (Scibids)
22. Nicole Lesko (Meredith)
23. Martin Gruau (Captify)
24. Brian May (dstillery)
25. Don Marti (CafeMedia)
26. Paul Bannister (CafeMedia)
27. Larry Price  (OpenX)
28. Joel Meyer (OpenX)
29. Brad Rodriguez (Magnite)
30. Wendell Baker (Verizon Media)
31. Erik Anderson (Microsoft)
32. Arnaud Blanchard (Criteo)
33. Nicolas Chrysanthos (Criteo)
34. Phil Eligio (EPC)
35. Max Gendler (NYTimes)
36. Ryan Arnold (P&G)
37. Alex Cone  (IAB Tech Lab)
38. Sam Macbeth (DuckDuckGo)
39. Achim Schlosser (EnID)
40. Basile Leparmentier (Criteo)
41. Airey Baringer (Triplelift)
42. Kristen Chapman (Salesforce)
43. Benjamin Case (Facebook)
44. Abhinav Sinha (PubMatic)
45. Greg Truchetet (Teads))
46. Ryan Avecilla (Neustar)
47. Kelda Anderson (Microsoft)
48. Matt Zambelli (Neustar)
49. Shivani Sharma (Google Chrome)
50. Julie Karasik (NAI)
51. Christa Dickson (Meredith)
52. Andrew Pascoe (NextRoll)
53. Anthony Hitchings (FT)
54. Viraj Awati (Amazon Advertising)
55. Benny Lin (Bombora)
56. Aram Zucker-Scharff (The Washington Post)
57. Grzegorz Lyczba (OpenX)
58. Jonathan Kingston (DuckDuckGo)


## Note taker: Josh Karlin


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.

[<img src="https://i.insider.com/5fc55f5350e71a00115580e8?width=700" alt="Screenshot showing location of the Raise My Hand button">](https://www.businessinsider.com/how-to-raise-hand-on-google-meet)


# Agenda


### Introductions



*   Michael Kleber: Welcome. Let’s get started at 11:03. There are 55 of us. There is a notes doc ([this doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit)) attached to the meet link. The doc is editable by everyone. There is a raise hand button, please use that as your queueing mechanism to talk. There are too many people in the call to do introductions today, so I’ll introduce myself. My name is Michael Kleber, I’ve been interacting with many of you for the last year in the web advertising business group to help figure out the future of advertising in the web, in particular in a web in which there aren’t third-party cookies. One of the proposals that we talked about was called Turtledove, which I published a year ago (Jan. 2020). Folks in this call published counter-proposals (such as folks from Criteo). Turtledove and Sparrow moved into WICG together half a year ago. There have been several proposals since to bring together variations for how advertising on-device auctions might work. I tried to tie everything together in a proposal called FLEDGE a couple of weeks ago, and that’s what we’re discussing here. 


### Process


#### Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/



*   Michael Kleber: If you’ve been in other W3C meetings you shouldn’t have a problem joining the WICG, but please do join the WICG. Any IP that you contribute here (pull requests against the repository) are covered by the IP protections of the W3C.


#### Comments on recording future meetings



*   MK: There has been some discussion about recording meetings in the W3C in the last couple of months. The official W3C process right now is that meetings can be recorded in a few situations, but it requires unanimous consent from everybody in the meeting. The reason to record meetings is to have better notes afterwards. The W3C standard is not to have a literal transcript but to have a scribe take notes by hand while the meeting is in progress. There has been a request to record the meeting. If anyone objects, please do so. You can raise your hand now or send me email directly. Note that “WHY CG” is one of many variants of how one would pronounce WICG. I apologize for the ambiguity. To talk about recording in general, please see [this issue](https://github.com/w3c/w3process/issues/334) in the W3C’s process CG. We will not be recording today’s meeting but could in the future if people support and nobody objects.
*   Brian May: Why don’t we just ask if anybody objects publicly here and now, feel free to do so.
*   MK: Sure. Also, when you speak please let us know your affiliation. Any objections to the recording of meetings?
*   Andrew Pascoe: I would accept a recording but generally against. We tend to talk about sensitive issues, I don’t want a flood of comments from all over the place. I’m more fine with recording for the purposes of note taking only if the recordings aren’t publicly available.
*   Michael Lysak: Michael from Carbon. Are we talking about recording under the auspices of WICG? Or anyone on the call being able to show their coworkers later to let them know what happened.
*   MK: They would be under the auspices and rules of WICG for clearly stated purpose (e.g., to make available to all WICG members after the fact, or expressly for the purpose of note taking after the fact).
*   Wendell Baker: Vote no on recordings. Recordings for purpose of clarifying the transcript are fine, but I don’t relish having my comms team having to respond to conversations in this area. 
*   MK: I’ve heard the same sentiment expressed in the past from participants from a number of large companies. So we’ll continue with the model of not recording for any playback later purposes. If people want to talk about recording to improve the transcript only, please do so. But let’s move on.


#### What this meeting is / is not



*   MK: The process may be surprising and confusing. This meeting is not a place where we’re going to make decisions about what the future of the web is going to look like. Decision making in the W3C standards process happens at a later stage. The proposal that we’re talking about here is in the incubation stage, which is a stage in which we’re trying to figure out whether an idea is well suited to its purpose. We’re trying to get broad participation to figure out if there are capabilities that we need to add/subtract before we turn something into a proposed standard. This is an attempt to get wide participation in the design process. Nothing we do here is set in stone. The standard work model for the W3C is that even the hashing out of issues in the incubation process, generally happens in issues on github repositories or other open forums so everyone can see after-the-fact how we reached our decisions, and allows for the involvement of others that aren’t able to attend the meetings. The good is that people that aren’t here can provide input and see what took place. The downside is that if you thought your argument here was going to win over skeptics, you’ll have to use written form to persuade people in the forums. Repeat, this is not a decision making forum. The most important things we can accomplish here are: 1) open github issues and 2) close github issues. Issues are the place to raise questions. The back-and-forth discussion in the issue is how we figure out how to improve the design, and then we submit a pull request to improve the document. The FLEDGE document is something that we intend to implement and we want to update it to reflect what we actually should implement, Opening issues is the way we figure out what needs to change. Closing issues is how we agree that a need has been met and a PR addresses the issue properly. Goal of meeting is to figure out what issues we’re concerned about and aren’t yet reflected by an issue in the repo, or to close an issue by saying that we agree that the PR satisfies the needs of the issue. Any questions?
*   Brian May: There is a third use for the meeting, to raise awareness about specific issues being discussed.
*   MK: Bringing attention to issues, the best way to do that is to include a reference to an issue in the agenda for the next meeting that we’re having. Adding a particular issue to the agenda or notes doc should be a good way to get people’s eyes on it. I agree, this is an appropriate forum for getting more engagement on issues.
*   Brian May: Is there something that describes what the process around pull requests is?
*   MK: I don’t know. Anyone? &lt;crickets>
*   Jeffrey WIeland: This meeting has enough on its plate. Going back and doing github basics shouldn’t be something we spend our time on.
*   MK: You can do everything via the github interface and not use git itself. The review of PRs will happen inside of github. Discussing inside of the github issue is a fine way to do things.
*   BM: Sounds good. Just to be clear, I wasn’t interested in the mechanics but how the reviews would work.


### References

[FLEDGE](https://github.com/WICG/turtledove/blob/master/FLEDGE.md)


### Open Issues, or Issues that Need Opening

https://github.com/WICG/turtledove/issues?q=is%3Aopen+is%3Aissue+label%3AFLEDGE



*   MK: Are there specific issues that folks would like to add to the agenda for today?
*   Leparmentier Basile: There are some things that are unclear to me. First question: There is a requirement on the number of impressions being above a threshold. Can you clarify how that is done? When we talk about creatives, what are we talking about exactly? How much does it help giving flexibility for small players who want group targeting for specific products?
*   MK: I’ll add two relevant issues for these questions. The k-anonymity discussion should be a PR. For what a creative is, as used in the explainer, I’d be happy to do more discussion here. I describe a creative as, philosophically, as the url that ends up being rendered inside of a fenced frame. It’s a URL that meets some k-anonymity threshold, and the URL shows up inside of a fenced frame (an environment where it can’t communicate with the surrounding page) and the URL renders looking the same way every time (perhaps that’s too ambitious). It might be more dynamic than that, but the goal is for the creative rendering url turn into something that appears on the screen. In the future, the rendering should happen without leaking information, in the early stages it is allowed to use the network.
*   LB: Trying to have more fine-grained capability will be harder due to the k-anonymity threshold. Which may be fine, but I’m asking this question because it affects the usefulness of the tool for this specific category.
*   MK: I agree this is a trade-off, and it seems necessary to me due to concerns of micro-targeting and you’re right we should discuss that.
*   Erik Taubeneck: I wanted to clarify if the k-anonymity is on the interest group and the url, or just on the url. Could you create an interest group of size 1, but show the url enough such that you reach the threshold.
*   MK: In the fledge doc there are two thresholds. One is a threshold on the size of the interest group, but you’re allowed to have an IG smaller than the threshold so long as it just hangs around in the browser and doesn’t touch the outside world. IGs have a periodic update capability (talk to server for updates once per day). Smaller IGs can still be on the browser and show ads, but they can’t do the periodic updates and touch the network. This is to prevent tracking users on IGs of size one. They can still bid, so long as the creative has been seen by enough other people to avoid micro-targeting. 
*   ET: On the trusted-server component, that’s meant to be uncredentialed, but if you had a small IG that would effectively be credentialed, is that deliberate?
*   MK: The IP geo-tracking issue goes away with the trusted server. One expectation of the trusted server is that it won’t track users.
*   Brendan Butterworth: I’m with IAB tech lab. I have slight discomfort with linking creative with URL. There might be more privacy compliant ways that retain a clear origin (ownership) without being stuck on URLs.
*   MK: I completely agree. The TD spec described the creative as a web bundle that doesn't touch the network. I don’t think this proposal wants to take a position. We need a way for the platform to send an ad to the browser to display.
*   Alex Cone: On creative rendering, is the idea that no params can be passed into the fenced frame? I’m thinking specifically about having 100s of thousands of products you want to preregister, can you pass back to code an image url if a parameter could be passed to the FF to indicate the IG so that the FF can dynamically render. I worked in creative registration for some time, taking in hundreds of thousands of creatives per day. By not allowing for dynamic rendering you’re creating a sea change.
*   MK: Check out the FLEDGE section 3.4, discussing ads composed of multiple pieces. The intended way to support your use case is, “When you add a person to an IG, you can say here is a collection of ads to hold onto, but those ads can be a whole creative or a piece of a creative. So when you pick what ad should render in the auction, the buyer’s javascript could glue together the creative that will render out of multiple pieces. Each piece will need to exceed a k-anom threshold. 
*   AC: Thanks for the clear explanation
*   Kris Chapman: I’m wondering about the joining of the ad delivery and the interest groups. There are folks measuring audience level but not involved in the bidding. Is there a way for IG membership and bidding to be better understood?
*   MK: Intention was to support this use case. There is one time at which you add somebody to an IG by calling joinAdInterestGroup. This proposal doesn’t make assumptions about the logic that precedes calling joinAdInterestGroup. How an IG bids in the auction is in the hands of the party (the owner) of the interest group. Please take a look at the flow int he doc and suggest places we could make it more clear. Also happy to talk in a github issue.
*   Aram Zucker-Scharff: I want to make sure issue 90 has the needed clarity. It’s about pub seting timeouts per bidder. It’s a big concern from my perspecitive. Is this allowed or not?
*   MK: There are easier and harder answers. The easy is at the time the buyer’s decision making logic is running, we could let the buyer know how long each bidder took to submit their bid and let the buyer drop slow bids. Preferably we’d not wait for slow bidders, and go on with auction anyway. It seems like the process will need to support that at some point. It seems natural for that to be something the browser ensures. But should the timeouts be under the contrrol of the publisher? That seems harder than a one-line change in a doc. Happy to discuss, but not sure if per-buyer resource allocations is really something we need in first version.
*   AZS: Kicking this into a V2 bucket makes sense, but maybe we can start with a simple solution for the first version. In regards to trusted servers in section 3.2 and 3.1, I’m not super clear exactly how privacy is protected there. They might be too trusted, too much potential to leak information. Can you add clarity?
*   MK: Q about what information goes back and forth between trusted servers, I can clarify that. How the browser or anyone else establishes trust in a server, is not addressed in the doc and needs to be figured out. I expect vigorous discussion on that topic.
*   Brad Rodriguez: If a IG of a small size exists in the browser, can it still talk to trusted server? 
*   MK: Yes. It can talk to the TS. The key trust in the trusted server is that it isn’t creating any logs of who talks to it.
*   AZS: This seems problematic. We should have a size limit. The system having some trust vs zero trust is a delicate decision.
*   MK: The information that goes to the TS is where we need to limit. The question is, “Do you want to provide any real-time signals for interest group X?” Doesn’t say anything about what the user is doing at that moment. Assuming no logging, we can get a lot of benefit. But if we’re worried about the TS misusing information, we need to give it less identifying information. There are technological solutions for mechanisms called private information retrieval, that lets a client query a database on a remote server where the database never learns what the query is. This can be done with multi-party computation for instance. An IP address attached to an encrypted query is then less of a concern.
*   LB: I want to remind everyone that legal guarantees are also an option (not just engineering solutions). I published [certifying SPARROW](https://github.com/WICG/sparrow/blob/master/gatekeeper_certification.md), which is quite different from FLEDGE. I feel that we should put more trust in the server.
*   MK: There will be more conversation about establishing trust.
*   Jonasz Pamula: I’m from RTB house. Q about reporting mechanism, in the light of model optimization. The reporting mechanism described in FLEDGE is said to be temporary. It limits abilities to optimize models. If the mechanism is temporary, what is the end goal and what is the timeline. 
*   MK: The end goal is something that meets the privacy properties that were spelled out as the goals of the TD proposal. A system in which the advertiser doesn’t learn what sites you personally visit. And the publisher doesn’t learn what interest groups visitors are in. Along with no cross-site tracking. Questions about how to achieve those goals are open. Questions about how temporary event-level reporting is, needs an answer. Even if EL reporting is still around when 3p cookies go away, it’s still a major step forward for privacy. Incremental progress for privacy.
*   MK: We’re down to last question or two. Feel free to open github issues, including clarification questions as issues.
*   Erik Taubeneck: Will reporting work with conversion measurement API. Do you envision changing interest grroups in the reporting?
*   Kris Chapman: Process Q. Do you want use cases in individual issues or consolidated?
*   MK: Use cases doc created by Web Adv business grroup. Too large for me to measure everything there against current proposal. So it’d have to be a distributed effort. The Web Adv BG is the place to bring that up
*   Aram ZS: Should have section on how to collect telemetry in more detail and centralized.
*   MK: Next meeting in 2 weeks.

TODOs:



*   AI: Open Issue to clarify k-anonymity infrastructure (how used and what API it exposes)
*   AI: Open Issue on what exactly a "creative" is, as used in the explainer
*   AI: Put issue #90 in a v.2 bucket, but add a line about wanting to address it to the doc
