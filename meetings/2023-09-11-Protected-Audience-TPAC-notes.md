# Protected Audience WICG Meeting at TPAC 2023: Notes

Calls take place on most Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# In-Person Protected Audience meeting at TPAC on Monday Sept 11, 5pm Seville time.
Joining information for the WICG session [here](https://github.com/WICG/admin/wiki/TPAC-2023-agenda)


# Next video-call meeting: Wednesday Sept 20, 2023


## Attendees: please sign yourself in!	


1. Michael Kleber (Google Chrome)
2. Erik Anderson (Microsoft Edge)
3. Achim Schlosser (European netID Foundation)
4. Alex Cone (Google Chrome)
5. Alexandru Mihai (eyeo)
6. Aram Zucker-Scharff (The Washington Post)
7. Ari Chivukula (Google Chrome)
8. Ben Wiser (Google Android)
9. Carine Bournez (W3C)
10. Charlie Harrison (Google Chrome)
11. Chris Fredrickson (Google Chrome)
12. Daniel Huigens (Proton)
13. David Dabbs (Epsilon)
14. Elias Selman (Criteo)
15. Erik Taubeneck (Meta)
16. Fabian Höring (Criteo)
17. Jeffrey Yasskin (Google Chrome)
18. Jonathan Kingston (DuckDuckGo)
19. Lionel Basdevant (Criteo)
20. Luke Winstrom (Apple)
21. Marek Blachut (HM Government)
22. Martin Thomson (Mozilla)
23. Matthew Finkel (Apple)
24. Mihai Cirlanaru (Google Android)
25. Mike Taylor (Google Chrome)
26. Nick Doty (CDT)
27. Paul Jensen (Google Chrome)
28. Richa Jain (Meta)
29. Sarah Nogueira (Criteo)
30. Shivani Sharma (Google Chrome)
31. Sid Sahoo (Google Chrome)
32. Tammy Greasby (Anonym)
33. Wojciech Filipek (eyeo)
34. Yoav Weiss (Google Chrome)
35. Filipa Senra (Google Chrome)
36. Nicola Tommasi (Google Chrome)
37. Theodore Olsauskas-Warren (Google Chrome)


## Note taker: Erik Anderson


# Agenda

[Michael Kleber] Presentation of Protected Auction API at a high level, for people not familiar with it, and highlighting of big design issues with an eye towards future adoption by PATCG

Slides: https://github.com/WICG/turtledove/blob/main/meetings/2023-09-11-Protected-Audience-TPAC-slides.pdf


# Notes

Michael Kleber: I work on Chrome Privacy Sandbox. For next hour, we’re going to be talking about Protected Audience API. One of the APIs for Privacy Sandbox that we’re shipping in anticipation of removal 3p cookies. We run calls every week for an hour. An API in intense incubation for a long time. This is not going to be another like the calls we have once a week, targeted at people deeply involved in the incubation for the past several years. Instead, will give a general overview of the API and point at several key design questions which I hope will be interesting things we engage in as we move toward a future where we at least graduate this design space (and maybe this proposal) in the PAT CG which has been on the measurement side up until now but maybe in the future will move to targeting and therefore this proposal or something like it.

Kleber: Starting with the problem we’re trying to solve. The context here is the Chrome Privacy Sandbox, the ongoing effort to remove 3p cookies from Chrome. We are now slated to remove 3p cookies starting next year. Removal of 3p cookies as we have learned would result in significant revenue loss for many web sites, therefore as part of the effort to remove 3p cookies we have an additional goal to give ad tech a way to pick which ads to put on which web pages without offering an ability to recognize the same person across web sites.

… we have generally evolved two plausible approaches for targeting in a post-3p cookie world. Approach 1, the easy way: give people who pick the ads a little bit of ad targeting info. Is it possible for the browser to offer some info which doesn’t allow tracking or make users unhappy but let websites recover a reasonable amount of revenue? We believe, yes. The Topics API is our API in this space. Structure is familiar– call a JS API, get some info, then use the info. Actually, I lied– not easy– lots of hidden complexity. But not the topic for this hour. There is a fundamental problem with that entire approach which is that the info returned can be added to a profile associated with a user’s 1st party identity; even though it's just a profile attached to a single user’s behavior on a single web site, you can still amass info about them over time. We can slow down the rate in which you learn more info, but we can’t eliminate it in any model where we essentially give info out.

…if you don’t like that answer, that leaves us with approach #2: the hard way. In the hard way, our approach is to let people who pick ads use some info without revealing it. The obvious question is, “what? Is that even possible?” Again, the answer is that we think it is. To create a recipe for this type of ad solution– you need a repository of secret info; need a way to use the secret information to choose an ad without revealing the secret info; need a way to render the ad without revealing the secret info; need a way for money to move around without revealing the secret information; need some way for the parties involved to learn what happens after the fact without revealing secret info; need some feedback mechanism for how the choice was made (this is probably a secret code name for ML) without revealing secret info. A lot of needs.

…That extremely lengthy and unpalatable list of things is what we need to do to offer an ad selection thing that uses secret info. Privacy Sandbox has a family of answers to meet those needs. Actually, two families of answers. One that we’re shipping now, by which I mean there’s one collection of answers available in 99% of copies of Chrome in the world, but they still have leaks and don’t deliver on “don’t reveal secret information” if you have an adversary trying to exfiltrate. Even so, a good thing– it’s now _possible_ for an ad tech company to try showing ads without tracking people. It was in some sense impossible before this time. Now a new way to do things that’s more private, but it’s not bullet proof. If you want to track people, you can definitely still do it.

…A second family of answers which will have all of these desired properties. Still trying to get there and will talk about where we are and where we are going.

…The Protected Audience API. The central question of how to pick ads based on secret info. Had a lot of names; used to be FLEDGE; before that TURTLEDOVE. (They don’t let me pick the names for things anymore.) In incubation since 2020. Most of what I will talk about this hour. Not the only piece of the answer. Other proposals in Privacy Sandbox for others of those needs. For rendering, the Fenced Frames effort (1 hour slot in WICG tomorrow for that). That’s how you can render without the surrounding page learning what ad was chosen. For learning what happens after someone wins one of these auctions, Attribution Reporting API also has a 1 hour slot tomorrow and had discussion in PAT CG. Alongside that, there’s the PAA (Private Aggregation API); sometimes known as noisy histograms. Also related to what’s going on in the PAT CG; join us there if interested.

… This is a lot of stuff. Thought it was unreasonable to ask ad tech industry and devs to adopt everything at once. For now, not requiring the use of fenced frames. Can still render it in iframes even if using the private way of selecting it. Also don’t require all reporting come through reporting channels; still allow event-level reporting of ad auctions. Moving money and ML training use cases will need changes in the future. What we have now is one step in a long journey. Yes, it does mean things still leak for now. I am absolutely sure that all of you can figure out how to use the API I describe today for fingerprinting if that’s your goal. I will point out, in the vein of giving out pointers to other places doing work, the privacy sandbox requires a public attestation– a party can only use one of these APIs if they make a public statement that they will not use the data from the API to re-identify people across web sites. There’s a lot of discussion on public attestation happening in Privacy Sandbox and elsewhere; not spending more time on it right now. The idea is not that Chrome or the browser or the user agent will try to look into the souls of all API callers and find out the truth of what they are doing.  Rather, companies in ad tech space have regulators who care about what they’re doing and might get upset if they have a privacy policy that says one thing about their use of data but actually do something different with their use of data. APIs that require public statements about how the data is used from a privacy point of view is part of the ongoing attempts to build bridges between our job here as engineers, as people who can place technical limits on things, and the jobs of colleagues of ours who work on policy and care about governmental regulation. Interplay between these two is a fascinating topic that will be interesting in future years; this is just the beginning.

Erik T: is there a timeline when you will need to use fenced frames?

Kleber: In terms of timelines, goal is to be as reasonable and fair to ecosystem as possible, giving advance notice of what will happen. The current state in which you can render in iframes and get event-level reports is a collection of things that leak a particular piece of private info (what ad was chosen plus ancillary stuff) and they will be available until at least 2026. We have not said anything more than that about timelines; predictions for years ahead are notoriously difficult. Want to reassure people that if they adopt things as they stand now, there will be a bunch of time and collaborative public work to have a discussion about how to replace the current version with the next more private version.

Nick Doty: I think there’s some confusion about attestation. A self-declaration that will be held accountable through public means is one thing. In anti-fraud group it’s been about someone else asserting something about someone’s device that they can’t change the declaration of. Very different things. Can we use different words to avoid confusion? In this case, the declaration is that it won’t be used for fingerprinting; are they saying they won’t collect or reveal secret info for targeting ads?

Kleber: Thanks for pointing out the confusion of terminology. For privacy sandbox, the only declaration we've discussed so far is “will not be used to recognize the same person across contexts.” Moving everything to aggregate measurement instead eventually.

Nick: you don’t have to attest that you’re not going to access, collect, use and sell the information intended to be the secret information.

Kleber: correct.

Kleber: Secret info stored in the browser observed about the user in some context and want to influence the ads the user sees in another context. Five components about the secret info; not exhaustive and lots of other stuff going on too. Picked five things I wanted to talk about that are interesting engineering decisions.

…First: a collection of URLs of ads that an interest group might want to show. Second: the URL for a JS function that can produce a bid for an auction. Third: some locally-stored data, inputs to the JS function. Fourth: some keys to look up remotely-stored real-time data for the JS function. Fifth: a URL to enable period updates from a server, e.g. on a daily basis. Interest group is just one piece of the whole Protected Audience API.

…Number one: in the original design, the way we described this was that, “at the time an entity says a-ha and decides they want to show you some ad later, they put a web bundle into your browser which would be the entire ad you will see later on and would be rendered later without any network at all. That offline rendering step is great for privacy. Any server that sees a resource effectively gets to learn that an ad just won an auction and might conduct a timing attack. Navigable web bundles seem to have less support than when we presented in 2020, so for now we are using good old rendering URLs and are defending against the attack in other ways. Fenced frames are good because it prevents contact with the surrounding page; not required now but will be eventually. With fenced frame rendering, a server can tell what ad got rendered but they don’t know what page it was rendered on. Also using k-anonymity of ad rendered URL; Privacy Sandbox is running a k-anon server that counts how many times an ad URL wins the protected audience auction and only allows the ad to render if it has won enough recently on different browsers. Have to be auction winner on 50 different browsers in the past week in order to be able to render. We haven't turned on that k-anonymity requirement yet but we will.

Aram: the challenge with web bundle support is a lack of support from browsers, developers, ad standard community?

Kleber: complicated answer, but… yes.

Isaac Foster: the web bundles, I haven’t seen timelines on the future. Don’t see a future date.

Kleber: don’t have a clear enough vision between here and there. The web bundles idea was a wish of the past. No current vision for that.

Matt Finkel: Sanitize URLs?

Kleber: no, ad URL can be arbitrary. Just has to be same string that wins on a bunch of different browsers.

Matt Finkel: Could target same user multiple times in a week?

Kleber: no, needs to be the winning ad in 50 _different_ browsers. If an ad tries to win, the first 49 times we count it for k-anon purposes, but we actually show the runner up. Also, the threshold isn't really exactly 50, it's 50 plus or minus an appropriate amount of noise, to ensure differential privacy.

Achim Schlosser: the first 50 impressions are lost? How do you deal with it?

Kleber: first impressions in warmup period, you don’t pay for them. The browser declares it ineligible at the last minute and chalk you up as a would-be winner and take the top ad among contenders that are actually k-anonymous and render that.

Achim Schlosser: the k-anon server, is it behind oblivious HTTP?

Kleber: Behind a proxy that strips IP address.  It’s run by the browser, it’s an example of a server whose code is public and is running inside a trusted execution environment, also by-design the API does not allow anyone to learn things like how many times the URL was loaded. Just returns the DP-noised answer of if the particular ad was over or under thresholds. We have a paper about doing DP in this continuous release setting.

Isaac: is there a reason on the creative k-anon counting you used wins instead of just counting the number of bids? In theory if you’re willing to pay, it would count in the same way.

Kleber: Easy to game that: if I’m trying to show an ad just to you, Isaac, I can make the ad bit one millicent for everyone but you.

Kleber: All of these solutions are things you could circumvent. We are in the market for a better solution for loading and showing an ad without leaking info about which ad was shown to the network. Heard lots of ideas, there are other folks involved in similar problems– let me know if they fit the bill; my best guess is “no” right now but interested in collaborations.

Martin Thomson: Content-addressable is interesting in this context.

Kleber: still easy to have unique URLs.

Kleber: Second thing: JS function that produces a bid– a risky thing to exist; sees both data from interest group which is carried across sites _and _the contextual web site data. Means we need to ensure these functions don’t have any way to exfiltrate information. The original plan was an on-device computation inside an isolated worklet. This has a couple of problems. 1) On device: some ad tech will want to use lots of on-device compute, more than it has. 2) An isolated worklet: can use side channels to try to exfiltrate info to a corresponding receiver script e.g. running on the publisher page. We’ve done a lot to mitigate this but not bullet proof. This on-device isolated worklet concept is what is shipping in Chrome right now. Also in early testing of bidding & auction service (B&A) which pushes execution to a server sandboxed v8 environment in a TEE. Google written, open source code. The ad tech gets to pick the JS that runs inside it and does actual computation– same model as on-device. The TEE must be hosted in trustworthy cloud provider, key management is two remote attesters.

… If you don’t believe in non-exfil properties on device, and also don’t believe in non-exfil properties of TEE in the cloud, then what? We are interested in discussing additional solutions here as well. Something that came up today is a domain-specific language for this kind of thing instead of JS. Seems vaguely plausible. What ad tech wants to use could possibly list all commonly done things. On the other hand, seems hard as well.

Erik T: if you rule out TEEs, is MPC ruled out because of latency?

Kleber: I have seen an attempt at MPC solution, the set of what computation you can do inside of MPC is in theory unbounded. You can take any code at all and compile it into MPC, but in practice it’s preposterous unless you do a relatively small set of things. Essentially, if you try to make an MPC answer and want a new capability, you start hiring crypto Ph.D.s. For on-device/TEE solutions, you write the line of code.

Kleber: Third… bids using some locally stored data. Protected Audience is a purpose-built API. Purpose-limitation is a valuable privacy property in and of itself. This puts the browser in a technical position to be able to act as the user agent and be opinionated about how data can get used. So, here’s some opinions built into Protected Audience. (1) IGs contain data from only a single site from when ad tech invoked the API; in a world where we have removed 3p cookies and cross-site recognition, IGs can only be based on what a user did on a single site, (2) interest groups can only last for 30 days, so with Privacy Sandbox 31 days after the most recent time you visited site X, nothing about what you did on site X can influence the ads you see via this API. There are implications of these opinions and this sort of control for user experience. For example, the user might ask “why did I see this ad?” If the ad comes out of Protected Audience API, the browser knows that came from the fact you visited site X last Thursday and we can tell that info to the user. Very different vs. today’s world of 3p cookies where you ask, “why did I see this ad?” and the answer is, “because of everything you’ve ever done online.” Second implication: suppose you see an ad you don’t like. Because you visited the site last Thursday, the browser could let you delete all IGs from site X and the info is now gone. If you go back to site X you might end up seeing ads again, if you still don't like that, the browser could go even further and once you’ve given some indication, it could remember site X is not allowed to put in IGs at all anymore. Genuinely different than 3p cookie experience of “don’t show me this ad anymore.” Right now that might plausibly disable one delivery channel for an ad, but might see the same ad or another ad using same info from other delivery channels delivered by 3rd-parties. Much better story about control of info after browser controls the flow of data and how it’s used. Not the end of the story, the beginning of how we, the user agent, can use this new position.

Nick Doty: Great potential directions for purpose-built APIs for providing that user experience. We should ask if we can provide those things even without this particular IG concept. I agree, the current ad preference/why I saw this ad system doesn’t work well. Not an inherent property of 3p cookies, but it could tell us why you saw the ad and reveal that to the user. Right?

Kleber: I agree that parties that are involved in ad selection might historically have offered a system like this. If you trust them, you got some useful info. The fact that the browser as an independent entity that something is technically true vs. someone’s representation of their process is different and a change from the status quo.

Nick: something we could try to do beyond IGs. Often people don’t want to see a category of ads. I go to many sites about a topic and don’t want to have to ask not to see each of them. Might want to say I don’t want to see any ads about weight loss.

Kleber: Yes, though that would need ad tech to attach meaningful metadata to ads they show. As a browser engineer, I don’t want to do on-device guessing about which ads are about weight loss. If ad tech community is interested in adding that metadata, it would be great to pluck out those interests to help user prefs. Fenced frame rendering is still important here if we want to avoid turning it into a fingerprinting vector.

Paul Farrow: digital services act, others protocols with metadata getting specified. The trend in Europe is going in that direction. As a user, I think this is great and simpler. But unfortunately it’s not usually that simple. I wonder if that answer about why IGs are restricted to a single site goes further than is needed for a user preference.

Kleber: the "IGs only for a single site" is a deeper question we’ve had some discussion on recently. Gets into deep philosophical questions about what the nature of privacy is. If a ML model discovers some novel attribute that it believes is true about you based on activity across a bunch of sites, is that inherently a thing we need to worry about if the only thing it is used for is picking ads without leaking to the outside world? The point of view that ads you’re seeing can only come from one additional site is the privacy sandbox attempt to pick a conservative answer to this question. Seems like "Putting this ad into your browser now and then having it show up on your screen later" is a more palatable flow for how ad picking logic works vs. an ML model that picks ads from among every ad in the universe. To me, those feel different. I don’t know if in the long term the difference is meaningful to users; it feels real to me right now. Does not seem like a question where privacy regulators and key decision makers of the world have a clear answer right now. Maybe in 5 years the general feeling will be different.

Theo: I wanted to add some context. I think it’s important when we give the user a button to click that it says it does something that we have a defensible answer for what it does. This idea that compartmentalization and linking to things with user history, e.g. you were on this site, you can click something to remove all things tied to that site… less detailed than what ad tech could provide, but concretely implementable in the browser to help the user reason about it. That’s probably 90% of why the user value in knowing why something was shown. Mostly want to know it’s because they went to the site. The more we can tie that to users understanding their behavior, the better. More certainty for users about an operation when asking to stop seeing something.

Paul: performance of ads is an indicator of how interested the user was in the ad. I’m worried we’ll end up with ads that are less performant, that users like less.

Kleber: these opinions are the browser trying to do some balancing act. I don’t have all of the info in the world for the right answer. If there was compelling info that the only one-other-site restriction is a crippling disability in how performant ads can be, I’d love to have it introduced to the conversation as we (all of us collectively, not just those of us in the room) to try to figure out the right balancing act.

Aram: I think that if you increase privacy, less perf is inevitable. This API is intentionally narrow and I think that’s good. The problem with looking to 3p systems for transparency is they have no history of being successful in doing so, so this is preferable. Without this functionality for asking certain ads not to show and understanding where ads come from, users will reject the feature. Some users already are. I think if you want retargeting in the post 3p future you will have to accept that there will be less perf, more transparency. Or you end up with nothing. 

Isaac: what thing are users rejecting?

Aram: Some users are rejecting privacy sandbox.

Kleber: if you follow the right social media this week, lots of people talking about how to turn it off.

Kleber: Thing 4: keys to look up remote real-time data from a server. Ads that are bidding require some kind of real-time data. An obvious example is, “do I have money left to participate in this auction?” No way you could pre-load the answer for an entire day. However, the set of real-time data that a particular browser needs is a fingerprint. Real-time DB is very large, changes rapidly. Private Info Retrieval doesn’t seem up to the task. Falling back on the usual Privacy Sandbox answer: key-value server in a TEE, so we know for sure it cannot log which sets of keys’ values got looked up at the same time as which other ones. TEEs: the Privacy Sandbox swiss army.

Kleber: Thing 5: a URL that an interest group queries periodically as a way to update the info from 1 through 4. You look at a product on a website, a week later it goes on sale– how do they tell you (and 50 other people)? IG contains a URL, once a day hits that URL to update the stored IG data. Since the IG contains data from only one site, doesn’t seem like a lot of leakage here– right? Just gets back the URL, same data it put in your browser. Well, no– two new pieces of info: a time and an IP address. Each one has some risk associated with it. We have some mitigations that involve only updating when the IG participates in an auction and the IP address aspect here. An IP address blinding proxy feels like an extremely likely future thing; IP is too tempting otherwise. Anti-fraud CG discussion on it tomorrow (and in other places). And could use a TEE here too if that other stuff isn’t good enough.

Kleber: If this stuff appeals to you, we’ve been working on this for years. Phone call once a week. These sets of questions are things we need to find answers to. Not just for this proposal, but any proposal for picking ads based on secret info. I hope this discussion leads to discussion in the PAT CG sometime in the foreseeable future once that group frees up capacity to work on new things; this will be waiting in the wings.


