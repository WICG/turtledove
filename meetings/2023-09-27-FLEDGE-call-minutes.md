# Protected Audience (formerly FLEDGE) WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Sept 27, 2023


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Brian May (dstillery)
3. Sven May (Google Privacy Sandbox)
4. Nick Llerandi (Triplelift)
5. Xavier Capaldi (Optable)
6. David Dabbs (Epsilon)
7. Russ Hamilton (Google Chrome)
8. Vitalie Maldur (eyeo)
9. Paul Jensen (Google Privacy Sandbox)
10. Don Marti (Raptive)
11. Harshad Mane (PubMatic)
12. Orr Bernstein (Google Privacy Sandbox)
13. Fabian Höring (Criteo)
14. Laurentiu Badea (OpenX)
15. Tamara Yaeger (BidSwitch)
16. Joel Meyer (OpenX)
17. Shivani Sharma (Google Privacy Sandbox)
18. Andrew Pascoe (NextRoll)
19. Maciej Zdanowicz (RTB House)
20. Kevin Lee (Google Privacy Sandbox)
21. Marco Lugo (NextRoll)
22. Isaac Foster (MSFT Ads)
23. Sergey Fedorenko (MSFT Ads)
24. Jeff Wieland MGNI
25. Matt Menke (Google Chrome)
26. Caleb Raitto (Google Chrome)


## Note taker: Joel Meyer


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


## [Suggest agenda items here — no agenda no meeting!]

_Note: old items were cleared out when we used this doc for the [in-person meeting at TPAC](https://github.com/WICG/turtledove/blob/main/meetings/2023-09-11-Protected-Audience-TPAC-notes.md).  Please re-add if there was something held over that you still want to discuss_



*   Isaac Foster: creative URL pre-registration  https://github.com/WICG/turtledove/issues/729
    *   Roni Gordon: https://github.com/WICG/turtledove/issues/792 is related
*   Roni Gordon (unable to attend Sept 27 meeting)
    *   same origin constraints - https://github.com/WICG/turtledove/issues/813
    *   K/V lookup timeouts - https://github.com/WICG/turtledove/issues/814
    *   Macro substitutions - https://github.com/WICG/turtledove/issues/817
    *   API versioning - https://github.com/WICG/turtledove/issues/823
    *   Sensitive signals - https://github.com/WICG/turtledove/issues/824
    *   Multi-size banners - https://github.com/WICG/turtledove/issues/825
    *   Additional browser event beacons - https://github.com/WICG/turtledove/issues/826
*   Shivani Sharma
    *   https://github.com/WICG/turtledove/issues/822 reserved.top\_navigation (https://github.com/WICG/turtledove/blob/main/Fenced\_Frames\_Ads\_Reporting.md#support-for-attribution-reporting)
*   David Dabbs
    *   Moving beyond FIFO topic management. Issue voting & triage, feature proposal (and triage) so stakeholders have visibility into feature consideration
    *   Potential for advertising application of the promising Fenced Frames output gate proposal Shivani Sharma presented at TPAC. https://github.com/WICG/fenced-frame/issues/15#issuecomment-1721834156


# Notes

Michael Kleber: Please add your name to the attendees list in this GDoc. If you plan to contribute to the conversation please make sure you’re a member of the W3C’s WICG group. The code of conduct applies, as do the IP rules. This is an essential component of protecting our work here.

Michael Kleber: First on the agenda is Isaac.



###  Isaac Foster: creative URL pre-registration  https://github.com/WICG/turtledove/issues/729

Isaac Foster: The goal would be to make it such that creatives don’t have to be pre-registered. Between syncing and allowing clients to make changes. There’s the K-anonymity piece, and there’s the piece of being able to understand how you’re targeted. The pre-registered creative is very challenging. I’d like to at least talk through it a little more. For B&A that’s easier to accommodate.

Michael Kleber: I want to make sure we’re all on the same page, as the word preregistration can mean different things to different people. Isaac, you’re asking about the topic of issue 792, whose title is Dynamic but K-anonymous Creative URLs. At auction time you’d like an IG to be able to offer up a renderURL but you’d prefer that the set of possible renderURLs is not something that has to be set in advance from a limited set - you’d like that the renderURL be able to be set dynamically.

Michael Kleber: I will say that there is another topic referencing pre-registration which is in the context of SSPs wanting to know the renderURLs that DSPs will use so they can collect meta-data about them and enforce brand rules, etc. Today SSPs typically do this by recognizing when a novel creative appears in the auction. That has problems in the PAAPI auction because an SSP can’t send a new renderURL back to their servers for scanning because it’s only available in the sandbox. So the question of pre-registering also relates to the idea of SSPs getting the creatives ahead of time. I have a feeling we’ll be talking about this soon, but it’s not on the agenda for today.

Michael Kleber: Having established our terms, I will let David Dabbs speak.

David Dabbs: You hit exactly what I was wanting to address - term disambiguation.

Michael Kleber: Off the bat, there’s a lot of discussion in #729 already. I think that the question you’re asking does not make much sense for on-device auctions. It’s much more relevant for B&A. When you’re talking about on-device auctions, the IG just won’t have the capability to generate so many different URLs. It seems like it must be limited.

Isaac Foster: I would restate it ever so slightly. I think it’s possible that with the current set of constraints that dynamic creatives could be useful in the auction regardless of where it takes place. I would frame it not just as a server-side discussion.

Michael Kleber: Okay, I'll retract my statement. Let’s talk about what makes it hard to do dynamic renderURLs. First is the K-anonymity constraint. We want to make sure that an ad is not being shown to less than fifty different people. That all happens by the browser checking with the k-anon server ahead of time how many people could see the renderURL.

Sergey Fedorenko: I want to talk a little about pre-attaching creatives to IGs. Doing so ignores all the targeting or filtering that happens at the time of rendering. The only thing that is considered is the IG itself. This substantially expands the set of creatives that must be attached to the IG. It doesn’t take into account size, geo, etc. For any IG that multiple advertisers are interested in, there could be thousands of creatives registered. If you limit it, then during the auction there is no way to know by any DSP which creatives were actually attached to the IG. As a DSP, then, I don’t know which of my line items or creatives I already attached to that IG without building some additional tech.

Michael Kleber: Are there any B&A folks on the call? I think there’s been some work on the B&A server to talk about exactly this risk of not knowing which URLs are pre-registered in this IG. So that the generateBid function can know which urls might be missing by checking what’s there.

Sergey Fedorenko: The thing is that if you limit, and I have to choose 50 from a possible 1000, I don’t know if these 50 will actually be the ones that are targetable for any subsequent auction call based on sizes that the browser presents to me. So I have to basically guess, which limits what I can actually bid.

Michael Kleber: I understand. You are saying that if an IG has the capacity to carry 50 different renderURLs, then a 1/day update isn’t good enough because there are more than 50 ads that could be desired to be shown in the next day. You are saying that having to pick the 50 most desired will result in a substantial loss of opportunity relative to having the entire set present.

Sergey Fedorenko: Yes.

Isaac Foster: To frame it in Fledge theoretic terms, the utility hit here is pretty substantial. Also from an operational standpoint, this goes from being a 1000 node set that needs to be updated to a 1M node set that needs to be updated and that brings substantial challenge to operations. If we’re just looking at the ad-tech utility feature set, a cache miss isn’t the worst thing. it would be better from a tradeoff perspective, and I think arguably be only gains to utility with no loss to privacy, if the system accepted renderUrls that weren’t pre-registered in the ads attribute, and then if there’s a cache miss just issue the increment to the server and for the purpose of the auction just move on to the next creative. If the renderURL returned is not in the local cache and it was a miss, having it bypassed for the local auction but it’s available the next time, that would be better. I think there’s room for some creativity.

Michael Kleber: I think what you just suggested: if the first time a renderURL shows up and isn’t cached / checked for K-anon, then it gets added to the Interest Group cache, seems like a potential way of dealing with the issues brought up in #792. However, for the user standpoint, it doesn’t address the question of what does it mean to be in an IG? One thing an IG means to a user is that I took some action that led to me being added to an IG (eg - visited Nike.com last week). The other thing it represents is a pile of ads that the user might see. This means that a user could be able to interrogate that IG and determine what ads are associated with it. If an IG can represent potentially thousands of ads, or an unlimited set of ads, then the IG loses the ability to represent a set of ads for a user.

Brian May: I was thinking about the potential for impact on the campaigns, particularly with those that do retargeting. If you get an opportunity to show an ad but don’t have a creative, that’s a significant loss of opportunity.

Isaac Foster: On the narrative side I’m going to hair split a little. The narrative about being able to explain why an ad was shown to you in the past, would be maintained. The narrative of what happens in the future does change, but it can already change every 24hrs. I think there is art and regulation around showing a creative library and we could potentially do something. Abstractly, let’s think of this as a distributed system with two types of data, user data and business data. I think there’s an implicit long term goal to make the client the source of truth for user info, but various things implicitly acknowledge we’re (we=tech) not ready for that yet. Even if we could make the client source of truth for user data, at that point we’d still have to deal with the business objects, for which the server would be the source of truth almost for sure. So for right now we’re in a world where the server is still the source of truth for all the data the client is syncing. 

[editor's note: the following two paragraphs were added directly to the notes doc after the call ended —mk]

Given that, right now we only offer full batch updates via the updateUrl; we should offer batch updates **_and_** _incremental_ updates via _something_. One way information, in particular the ads attribute, could be incrementally updated is via the bidding function. This could work by a) keep the updateUrl as the thing that updates the client side cache of information (bidding signals, ads, etc) as a “full refresh” b) specifically for `ad` attribute, let the renderUrls be “incrementally updated” if they come in via the bidding function..._assuming_ the full batch refresh is truly that, with minor incremental updates over the next period, you could always just invoke updateUrl to sync the current source of truth. So, when the user says “show me the IG meaning”, you could invoke the updateUrl and at that point it would have “the most up to date version of what the IG means” 

Now, assuming that updateUrl is faithful as a full refresh, which I think the ad tech has at least some incentive to make it, at that point you’re still giving the fully synced state of “what is this IG”, exactly as before. Yes it can change in the future, but the only difference is that now it can change incrementally, rather than only once every 24 (x) hours. 

Michael Kleber: Okay, thanks. I do want to move on to the next topic. The last response I have is that your proposal does seem like a reasonable thing for us to invest resources in. There are many things to invest in, so if you can publish some data to help quantify the utility gained, then that would help us prioritize it relative to the many other things we can look at.

Paul Jensen: We did discuss the privacy issues a few weeks ago.

Michael Kleber: Anything that came out worth bringing up?

Paul Jensen: We didn’t really talk about privacy today. I do have a question for Sergey - if 50 isn’t enough, is there a number that IS enough?

Sergey Fedorenko: You can’t really guarantee that your campaign is a certain number of creatives. Varies based on sizes, advertisers, geos, langs, etc. If the rendering part starts limiting what demand wants to do, it’s a bad solution. [in other words: no number I can give you that’s better than 50].

Isaac Foster: A lot is done programmatically, so the limits that might apply to human driven changes don’t really apply.

Sergey Fedorenko: What I understand as a user is that my browser has to go to the advertiser’s website for my browser to get assigned to an IG. What about brand awareness campaigns? I’m new: how do I attract people to my website to set an IG if no one knows me.

Michael Kleber: That seems like a completely unrelated question. There’s no requirement to go to an advertiser’s website. You can set an IG on any website and we’ve had discussions in the past on other ways to build your audience. The PAAPI is much more general than just the retargeting use case (which is easy to think about, but far from the only use case).

Michael Kleber: Summing up: it would be very helpful to know how much utility gain there is from addressing this use case. If there are other buyers who care and can provide data, that would be useful as well. Per Paul’s question, if there’s a curve of how many renderURLs you can register vs the revenue retained by that number, that would be super helpful. Or, perhaps, if Sergey’s position that no limit is helpful, that would be good to know as well. Though that seems hard to imagine as it seems like there’s a finite set of creatives you would pick to show to a given user. I would guess that there is some number that would return a large fraction of utility.

Michael Kleber: Roni from Index is not here - anyone who would like to start talking about the issues he raised? I’d prefer to have him here.

[Agreement]

Michael Kleber: On to the next agenda item.  Shivani - please go for it.



###   Shivani Sharma: https://github.com/WICG/turtledove/issues/822 reserved.top\_navigation (https://github.com/WICG/turtledove/blob/main/Fenced\_Frames\_Ads\_Reporting.md#support-for-attribution-reporting)

Shivani Sharma: The issue here is about the reserve.topNavigation beacons. To summarize, this allows the browser to send out a beacon when top navigation happens, it is likely that the number of beacons is less than if reportEvent was called on ‘click’ directly. The most plausible explanation seems to be that the browser sends the beacon when the navigation is committed, whereas the navigation on click can be aborted based on user actions - so the page that was supposed to be the landing page did not actually get navigated to. The current design is based on navigation successfully finished, vs what ad-tech seems to want, which is navigation start.

Joel Meyer: The short answer is, most ad techs would want both.  "start navigation" is like the answer used in the real world today, long-term people would be interested in the "successful navigation" metric as well, probably correlates better with value.

Shivani Sharma: Could change the navigation event today, and/or could add a new event for the other one.

Brian May: I’ll second what Joel said.

Michael Kleber: For those to whom this is a new issue, please feel free to provide feedback on issue #822 if you give it more thought. Please socialize it if you think others would be interested.

Michael Kleber: Okay, that brings us to David Dabbs. It seems like you have a meta-issue about how we organize the agenda



###   David Dabbs: Moving beyond FIFO topic management. Issue voting & triage, feature proposal (and triage) so stakeholders have visibility into feature consideration

David Dabbs: Not so much the agenda, but a virtual water cooler discussion around how these issues get discussed. It feels like it’s one thing to see a list of issues we continue to want to work on, and then how we can queue that to focus our efforts and your efforts. The issue Isaac raised, which needs some follow-up based on data, and then prioritization, raised the issue in my mind.

Michael Kleber: Good point - are any of the PMs for PA on the call? Unfortunately not seeing any. We do try to look at all the incoming feature requests from GitHub, calls, other channels. (PrivacySandbox.com has many ways people can give us feedback.) We do our best to prioritize based on the information we have. You’re right, we could probably do better in making it more transparent on how we prioritize what we work on. If I had more PMs I would let them talk, but they’re not here. So, yes, you’re right - we could do a better job of this.

Kevin Lee: If you go to the Status Page on the developer Chrome blog you can see a table of features that are coming out soon. We are actively updating the table and if you’re subscribed you’ll be notified when things are getting launched.

David Dabbs: That’s at a pretty high level, what we’re looking at tends to be more fine grained and it would be helpful to know at that level what’s being prioritized and worked on. We’ll try to come up with a proposal or something. It’s been helpful when Paul labels different issues.

Michael Kleber: I agree - we can do a better job of communicating through GitHub what things we’re actively working on and considering. This would provide an earlier view of what’s coming, rather than communicating what is launching and when. Kevin, please provide a link to the Chrome Status page.  (Maybe this? https://chromestatus.com/features#protected%20audience)

David Dabbs: I know you all take smaller grains of features and put them on Chrome’s status of features, eg some bundled set of private aggregation stuff.

Michael Kleber: The way I would describe it is that Chrome releases once a month. Every Chrome release contains some new features that we’ve added to PAAPI. The info on Chrome Status is kind of similar to the release notes restricted to PAAPI. This does give you some ability to see into the future, but nevertheless the discussion we had today isn’t reflected there and wouldn’t be until we have a sense of when we can actually do it. I agree that some earlier indication of things we’re pursuing would certainly be helpful.

David Dabbs: [General agreement]

Kevin Lee: You’d like to have more visibility into our prioritization process and provide feedback?

Jeffrey Wieland: Not just visibility, but agency.

Kevin Lee: Okay, let me take it to the PMs and think about what we can implement.

Jeffrey Wieland: To hammer this home, we collectively spend a lot of time in this call and we want to see some fruits of that labor. It’s not free for companies to give us time to do this.

Isaac Foster: This is an opportunity for Google to address a potential complaint about outside entities not having the same visibility as internal Google entities.

Kevin Lee: I will follow up with the PMs.

Michael Kleber: The only other thing we have on the agenda is David’s other item.



###   David Dabbs: Potential for advertising application of the promising Fenced Frames output gate proposal Shivani Sharma presented at TPAC. https://github.com/WICG/fenced-frame/issues/15#issuecomment-1721834156

David Dabbs: I was really intrigued by Shivani’s presentation at TPAC and wanted to see if there was on opportunity to bring that into the discussion.

Shivani Sharma: My follow-up question would be; do you have some use cases in mind on how that would benefit advertising?

David Dabbs: Yeah, same thing you presented - the ability to personalize the creative in some way for a user, similar to the payment processor example. To support the shoe industry here, as we do in PAAPI, to present some brand of shoe to them, to increase the advertisers ROI.

Michael Kleber: Let me add a little context for folks who weren’t at TPAC in Seville ([slides](https://docs.google.com/presentation/d/1TqtFtK4x3TMd96JEvkbApUaYVdIaUz9uz3wNGPTuqdU/edit#slide=id.p), [notes](https://docs.google.com/document/d/1RuXH0DEmqRd4pLM4CSWATkyDDWApkaQXQbrohtvY7L8/edit#heading=h.7qjy84e49sb5)). As many know, the original plan for how to render ads after the PAAPI auction was to do so in a Fenced Frame. These allow you to render something from a URL, but originally it was conceived as a way to lock it down so that there was no means of even touching the network. Any time you can load over the network there’s a potential leakage vector. In a FF that doesn’t have network access, then you can be freer with the information that is available to flow into the FF, as there’s no network escape vector. There’s a proposal based on a feature request, to allow a locked FF to read info from shared storage, eg - last four digits of CC#. “Pay with Shopify using card ending in 1234” where 1234 comes from localstorage. This increases recognition and trust. The mechanism proposed is to let the contents of the FF read from shared storage as long as the thing displayed on the screen cannot affect the network in any way. So we need to be clear that reading a URL from shared storage for loading in the FF won’t work. You also couldn’t use shared storage to influence the destination URL from the creative or any other type of action. If, given the extremely tight set of constraints, you think there is still utility for some ads-related use case, then we can have the conversation about how that would work.

David Dabbs: I’m not a data scientist but I play one on TV. I think the original vision was with bundles. But perhaps it’s more limited than I thought. It needs a little more discussion and something concrete to come back.

Michael Kleber: Our original vision of using bundles and no network access proved to be too hard to use for adtech, so we changed course. But if we were wrong, we can always revisit the idea.

Michael Kleber: Thank you all for the discussion, and we will see you in one week at the usual time.
