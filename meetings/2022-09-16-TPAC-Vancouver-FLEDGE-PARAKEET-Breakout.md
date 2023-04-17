**Notes from the FLEDGE + PARAKEET breakout session at TPAC 2022**

**kleber:** [slide deck] : https://docs.google.com/presentation/d/1QQgrm4oaRRRBr1gfvKj7D8rS2EW8kRgRUHPscvR8BNo/edit#slide=id.g15545e7b627\_0\_127

… I would like to talk through what is same and different between FLEDGE and PARAKEET

… shared goal: support ad targeting via multiparty process

… ad targeting based on contextual, ad-specific, user-specific information


    <npd> "just like the world we live in today" is a strange goal statement

**kleber:** no ability to recognize user across multiple websites.

… user-specific info all stored on-device

… use case of app-ads is one of our goals, so may be stored "by OS"

… shared JS API to support these goals


    <AramZS> Custom Audience is def the standard term for the ad tech/buyer side understanding of this if we're up for a name change.

**kleber:** willingness to support Privacy-enhancing technologies

… mechanism for rendering chosen ads (fenced frames)

… mechanism for reporting auction outcomes

… (Aggregate Reporting API)

… questions before I go on?

… so, what's different:

… 1) what "servers equipped with Privacy-Enhancing Tech" can do

… 2) How much computation needs to be done on-device

… 3) what scope of user-specific info can be used in ad selection

… these 3 things are heavily interrelated; could look at this number differently

… let's go through each of these three things during this talk.

… 1) what PET servers can do. both proposals have changed over time; we once thought this could be entirely on-device

… adtech folks pointed out realtime data is actually key (e,g., if ad budget has run out!)

… or keeping ads from running on sites they find objectionable

… if a browser is going to touch a server, how do we protect privacy?

… in OG FLEDGE, servers could only be implemented in privacy-preserving ways


    <npd> I'm not convinced that real time data is essential, but I can see that there is a preference for it. we could work on both budget and safety questions through other means if there was interest

**kleber:** OG PARAKEET relied on browser-run servers to see but not store data; would proxy a noisy version to adtech servers

**joel:** yes. proxy would sufficiently anonymize

**kleber:** right. Neither of these stories turned out to be realistic.

… at scale

… where we are today: on FLEDGE, browsers can contact servers but only on verifiable builds in Trusted exec environment, still can't store data or user profile, but okay because verifiable.


    <dmarti> npd, it has to be "near" real time (for the unsafe ad placement problem, probably a few hours would work)

**kleber:** on PARAKEET, noisy ranking, must have substantial noising and response caching

… both rely on privacy enhanching technology servers.

… on how much computation needs to be done on-device: P has always said little is done on-device. F has changed a lot more; used to be very on-device-heavy.


    <npd> dmarti, I'm glad to hear there is renewed industry interest in reducing the harms of ads on unsafe sites. it feels a little suspicious that this problem has newly come up in the context of why we can't adopt a privacy-friendly system.

**kleber:** now have sandboxed code execution inside server.

… maybe: DSP bidding and SSP auction may move to TEEs?

… this would help on devices such as phones


    <npd> we could try to explore other mechanisms for blocking dangerous ads or blocking ads from dangerous sites. safebrowsing, e.g. has update mechanisms

**kleber:** finally, gap #3: scope of user-specific information in ad selection

… P: on-device ad targeting profile is based on web-wide data

… F: bidding of each interest group (custom audience) is based only on data from a single site

… so ad-targeting/machine-learning models cannot learn new information.

… this is fundamentally more conservative, and certainly more limited that what goes on today.

… there's a fundamental question here: is it ok to build a cross-site model?


    <npd> it sounds like both are using cross-site data, but Parakeet would be combining data from multiple sites, whereas Fledge would be crossing just from one site to another

**eriktaubeneck:** (clarification)


    <dmarti> npd, thank you -- we have rapidly pausing advertising in general as an advertiser requirement but this needs to be clearer to include rapidly pulling ads from a specific set of problem sites https://github.com/w3c/web-advertising/blob/bdd3224672cde1bb8543ddec798a6ca69ac61a4a/support\_for\_advertising\_use\_cases.md#pausing-advertising

**aram:** wondering on ? of DSP operations: has there been a lot of feedbakc from DSPs and SSPs? Seems like they might be unwilling to move to a different domain they have less control over.

**kleber:** we have heard that. I will say: you're right, the bidding operation may contain their company's secret sauce, so they might not want it running in-browser, where i might be reverse-engineered. Not everyone agrees about this.

… some privacy advocates say putting in the public is better.

**joel:** the big thing adtech doesn't want to do is reveal bids to users, since it would affect the auction.

… the bids themselves can't be revealed in the clear, but lockign them in th TEE is likely okay.

**aram:** the publisher typically plays the bidders against each other. publishers want to monitor the bidding in realtime.

**joel:** I think we can do that in the TEE too.

… you should be able to run a more fair auction.

**aram:** sounds fine, though that doesn't seem to be in the model here.

**kleber:** this is a difference between the transparency necessary when shipping to the device vs in TEE; but the on-device bidding does occur in worklets that are not allowed to leak out to the page.

**aram:** publishers could deploy those workletst?

**kleber:** sure.

… in FLEDGE version of TEEs, there's no difference in capabilities between on-device and shipped to the cloud.

**benjamin:** in isolated worklet, what is the flexibility you have in mind? Originally you could write arbitrary code, but now this is DSL or custom logic that has restrictions?

**kleber:** in FLEDGE, it's arbitrary code execution. This gets to a bunch of issues that makes shipping to TEE in cloud appealing.

… as Martin pointed out in talk on Tuesday, keeping info isolated on a phone can be quite difficult.

… there are tradeoffs in both directions.


    <Zakim> npd, you wanted to ask what happens if users click on ads

**npd:** one privacy concern we might have is about the use of information, particularly when it is off-device


    <AramZS> (worth noting that the on-device process has also faced some trust issues between the bidders, publishers and ad tech vs the device system owners, either the OS or Browser folks)

**npd:** concerned about limiting disclosure to when you click on ads

… I don't think I usually click on ads.

**kleber:** in F, model is every ad that's going to be shown, rendering URL of ad needs to be sent to device.

… cannot be influenced by info stored locally (on-device user profile)

… more than that, subject to a k-minimum check

… explicitly to try to prevent info leakage at time of click

… essentially, there's no more info transfer at click time than if the user clicked on a link from the site they were on to the ad site.

**npd:** I think you're saying all the info is transferred aback to the ad site?

**kleber:** no, only to k-anonymous levels.

… no user-specific data.

**npd:** has to be selected to be shown, but not clicked?

**kleber:** yes.

**npd:** in both cases, it seems all the ad data is revealed when you click

**kleber:** the fact that you were shown an ad is certainly revealed, yes.

… but thas isn't the same question is "can you build a cross-site model"

**npd:** could simply not share this

**kleber:** that would remove significant utility from this. information disclosure; it's about leakage over time.


    <npd> npd: could mitigate disclosure risks on click by these selected/targeted ads not being clickable


    <Zakim> theowarren, you wanted to ask about potential user control differences between FLEDGE & Parakeet targeting

**theo:** on point 3: the difference between F and P's models. F's model registers you in an interest group. one of failure modes of targeting is when you've already bought something.

… P seems to not have the same kind of affordance.

**joel:** there's no signal that says "Stop showing this".

… parakeet would like to be able to give the signal to the DSP "don't show this ad"

… not in current P spec

**kleber:** but in the P model you don't know who "this user" is - you could only do this to a cluster of users


    <npd> I would love to see the more detailed proposal about the functionality of users choosing not to see this ad any more

**kleber:** takeawy theo points on: in F model, there's an answer to "why did I see this ad", in P model, the mixing of data makes that more difficult.


    <AramZS> npd I think it is likely the ad interaction more common than clicking the ad intentionally lol. Just the current system of ad tech makes it so these processes don't work

**russ:** re: #3 - a publisher knows what the context on the page is, and who the user is directly.

… can't they just show ads directly?

**kleber:** of course, first-party ad placement is totally ok; if you have that, you don't need this mechanism. Although having on-device storage, even for first-party ads, this enables features like cross-site frequency capping might be useful.


    <npd> AramZS (IRC), totally, and I think it's a promising set of functionality to make work for user privacy interests, and an improvement over the status quo (where it does seem like that functionality doesn't work)


    <Zakim> AramZS, you wanted to ask about advantages to moving off device for DAI?

**aram:** the android mention raises an interesting point - dynamic ad insertion - but we probably don't have time to get into this deeply.

… I'm wondering if either of these proposals could support those scenarios - e.g. podcast ads

**kleber:** when there's a cloud-based TEE, yes, seems like it could. still have problem of what fenced frames is trying to solve, preventing site from getting access to user profile

**benjamin:** how would you train an ML model in this case?

**kleber:** if we put all this together, we'll need to figure out this answer.

**joel:** [has an answer the scribe can't capture that quickly :P]


    <Zakim> jyasskin, you wanted to suggest that cross-site on-device profiles are probably not ok because it gets leaked on click.

**jeffrey:** suggestion of an answer to #3: based on nick's observation, a link click would lead to some ad data,

**kleber:** no, just that the ad was selected

**jeffrey:** but some data is revealed. so it seems like there needs to be some limit on the number of ads

**kleber:** yes, though that just slows down the rate of leakage

**jeffrey:** is this documented?'

**kleber:** not yet.


    <npd> it seems like it could be small but very revealing pieces of information. not just that it's uniquely identifying, but the properties themselves (that the user visited plannedparenthood.org etc.)

_link to slides: https://docs.google.com/presentation/d/1QQgrm4oaRRRBr1gfvKj7D8rS2EW8kRgRUHPscvR8BNo/edit#slide=id.g15545e7b627\_0\_127_
