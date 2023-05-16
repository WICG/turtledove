# Protected Audience (formerly FLEDGE) WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday May 10, 2023


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Sven May (Google Privacy Sandbox)
3. Paul Jensen (Google Chrome)
4. Caleb Raitto (Google Chrome)
5. Orr Bernstein (Google Chrome)
6. David Dabbs (Epsilon)
7. Kevin Lee (Google Privacy Sandbox)
8. Fabian Höring (Criteo)
9. Harshad Mane (PubMatic)
10. Jonasz Pamula (RTB House)
11. Andrew Pascoe (NextRoll)
12. Alex Cone (Google Privacy Sandbox)
13. Joel Meyer (OpenX)
14. Andrew Aikens (TripleLift)
15. Marco Lugo (NextRoll)
16. Supraja Sekhar (Google Ads)
17. Abishai Gray (Google Privacy Sandbox)
18. David Tam (Relay42)
19. Russ Hamilton (Google Chrome)
20. Alonso Velasquez (Google Chrome)


## Note taker: Orr Bernstein


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you  the WICG: https://www.w3.org[join/community/wicg/](https://www.w3.org/community/wicg/) 


## [Suggest agenda items here — no agenda no meeting!]



*   [Sven May] (asking in the name of “Yahoo! Japan”): How should publisher/SSPs handle user's ad opt-out preference? See https://github.com/WICG/turtledove/issues/573 for Y!J’s proposal.
*   


# Notes



*   Sven May
    *   Works for Google, issue brought up from Yahoo! Japan. They asked specifically if Sven can ask how other SSPs can implement user’s ad opt-out preference.
    *   They opened https://github.com/WICG/turtledove/issues/573
    *   They also linked to a PDF document with slides: https://github.com/WICG/turtledove/files/11427505/fledge\_proposal.pdf
    *   User lands in an InterestGroup, is shown an ad, clicks to show that they don’t want to see this ad.
    *   You want to maintain the user’s membership in the InterestGroup while still not showing the user that ad again.
    *   It’s also possible for the same ad to be shown in other InterestGroups. User might be concerned that they clicked the button to not be shown that ad, but it’s still shown from the other InterestGroup.
    *   How do other SSPs deal with this situation? Is this a problem for others? Are there obvious solutions?
*   Alex Cone
    *   AdChoices (and its derivatives) now serves several purposes. Was originally designed for opt out of third-party tech doing behavioral advertising (not 1st party). The ad preference specs for AdChoices are broadly supported. And the original opt out itself doesn’t necessarily result in getting dropped from the segment(s) that were in target for the ad (for lots of reasons).
    *   Other than plugging into the DMPs for first party publishers, a lot of audience data used for targeting is on the DSP side, so perhaps the more important question is, what are DSPs doing to support the relatively new ad preference portion of AdChoices?
*   Michael Kleber
    *   Two different pieces of this problem:
        *   One thing someone might ask is, when people are unhappy in some way with the ads they’re seeing, what controls does the browser offer them that allows them to affect their ad experience? What can we the browser or designers of the Protected Audience API do to give them agency and make them happier with the experience?
        *   What is possible for DSPs and SSPs to do to interact with the user, and how can they receive information from the user and make use of that information in making better choices later on.
    *   Privacy Sandbox is relevant in both cases. We need to worry that the protections we’re putting in place on the flow of information might make it harder for DSPs and SSPs to act on the signals coming from users on what ads they want to be shown.
*   Alonso Velasquez
    *   In this forum, we’re focused on the second use case (what is possible for DSPs/SSPs to do) more than the first one
    *   When it comes to DSPs being the one that controls, there’s opt-op of a particular ad, it’s different than opt-out of an advertiser. Those are the two levers. Maybe another lever somewhere in the middle.
    *   Advertising could be very broad or very narrow. Could be as broad as, somebody was on my website, or as narrow as, this specific behavior. The way these are defined is sometimes not understandable by consumers.
    *   Very curious to understand if any DSPs have been able to successfully set an expectation or give a control to users, given them a function to opt out of this audience.
*   David Dabbs
    *   Can’t really address Alonso’s “middle level” scenario (opting out of individual creatives or all adverts from a particular advertiser), but can speak to DSP-level control. Our DSP provides users with a notice and choice disclosure accessible via an AdChoices icon on the creative. There is one choice, turn off all Interest-based advertising from our specific IBA platform. When delivering via PAAPI we will not be able to offer the same choice that we make in this current disclosure. We will use many IGs and PAAPI provides just _leaveAdInterestGroup_. Adtechs participating in industry self-regulatory systems ([NAI](https://thenai.org/accountability/code-enforcement/), [DAA](https://digitaladvertisingalliance.org/principles), EDAA) are obliged to offer a user a means to have that organisation no longer collect & target that user via IBA. I do not see a means to honor that even scoping the notice and choice to the PAAPI mechanism. \
[DDabbs note after the fact: Notice and choice is important, but it is IMO lower on our post cookie deprecation Maslow’s hierarchy of needs. We need to get advertising utility out that works and scales.] \

*   Michael Kleber
    *   When we - Chrome - try to give the user some agency and allow them to exert some control, we want to make sure that we offer them options where we can deliver. We want to avoid promising them something we can’t actually do. With advertising, that can be extremely difficult. For example, can’t honor, “don’t show any more ads from this advertiser.” There are many ways to show ads from a given advertiser, some through this API and some not through this API.
    *   The promise the browser is in a better position to make is, something about the user of the user’s data. When using the PAAPI, there’s basically one rock solid thing that the browser knows, which is that, at some point in the past month, the user was on a given site - site X - and was added into an InterestGroup - IG Y - and then went to site Z and saw it. If the user isn’t happy that they’re seeing an ad based on the event of having visited site X, we can trim that particular branch, and have the browser forget everything associated with that site visit.
    *   That might be too drastic. You’re going to get rid of ads from potentially many advertisers. Not even just IGs from a particular DSP, but IGs from lots of DSPs. Covering that case is probably a good thing - it doesn’t matter which buying path the ad came through, so we won’t show the ad regardless of buying path.
    *   Some limitations of this. It only affects past behavior. If you go back to that site later on, you could be added into InterestGroups again, as the Y!J issue points out. Might be too heavy handed to handle that by stopping that site from adding the user to InterestGroups again.
*   Harshad Mane
    *   The particular ad shown to the user, the user doesn’t want to see that ad, but may still be interested in other ads from that InterestGroup. If you want to keep track of which particular creatives the user doesn’t want to see, can keep that information in the browser and send those to SSPs and DSPs.
*   Michael Kleber
    *   One bad experience is that user has expressed that they don’t want to see a particular ad, and yet they keep seeing that ad or visually similar ad through different channels, for example through multiple DSPs that don’t coordinate with each other which ads they show, or multiple SSPs, or one advertiser where they honor not showing a user an ad with some given ID, but have similar ads with different IDs. Runs the risk of the user getting frustrated by trying to say that they don’t want to see an ad, but then to still be shown that ad.
*   David Dabbs
    *   To the point of, “I don’t want to see this ad”, it’s “not this ad by any channel”; it’s always going to be specific to who it is that’s making the promise. The user saying, “whoever you are, I don’t want you to target me with these ads” - the browser and PAAPI would need some sticky data that we couldn’t query, but PAAPI would say, this entity that owns a bunch of InterestGroups has basically said, don’t add this user to any more InterestGroups. Is that something that you would be amenable to discussing? If not, perhaps we can look at a special purpose of storage that you use to block an InterestGroup registration? \

*   Michael Kleber
    *   A little skeptical of a user that’s exercising a level of control like that particularly wants to address a particular DSP. A person browsing the web is much more likely that they don’t want to see an ad from an advertiser, versus a particular link in the chain between the advertiser and the user.
*   David Dabbs
    *   An adtech who’s making a statement to give a user some choice cannot make that promise on behalf of any other adtech party.
*   Michael Kleber
    *   Sort of true, but if we’re talking about, can we expand the API to offer more to the user, then we should think about the full range of possibilities. For example, we could request that every ad is stored along with a piece of metadata that is, who is the advertiser?  The browser doesn’t necessarily have a way to verify who the advertiser is, but if the DSP offering the ad can provide it - maybe even in a way that’s consistent across DSPs - then the browser would have enough information to take it upon themselves to ensure that this advertiser can never win an auction through this API.
    *   But admittedly this is pretty drastic.  Welcome comments on why labeling who the advertiser is for a given creative is a bad idea.
*   Paul Jensen
    *   Anything that’s cross-site has to be done by the browser, and is really hard for the browser to do, and becomes a fingerprinting mechanism. Something less bad than other things - removing InterestGroups or preventing future joins of InterestGroups - can be less bad than adding InterestGroups. If we try to get rid of one InterestGroup.. You could figure out what part of a given ad a user disagrees with … Michael’s idea of knowing the advertiser, something that buyers might want.
    *   Looks like a user wants to drop all ads that were joined on this page, but could be something someone uses.
    *   At some point, might be something the browser should be doing.
*   Michael Kleber
    *   On the one specific feature request that David Dabbs made, should we make it possible for a particular DSP to say, this person has told me that they never want me - a DSP - to show them ads through PAAPI. Is it possible for them to say, "browser, don’t add this user to any of my InterestGroups in the future"?
*   Paul Jensen
    *   Could still be a tracking vector.
*   Michael Kleber
    *   Could see if there’s a way we could add enough protections around it, if this is valuable enough. Chrome putting effort into a way that lets people opt out of a specific DSP's use of the API, but still preserves privacy, might make sense if this offers enough benefit to users.  But do normal people really care about opting out of one specific delivery channel?
*   Sven May
    *   From Yahoo! Japan’s perspective, the bare minimum use case, the user wants to tell the publisher, don’t show me this ad. What if the user clicks on the icon, then the browser notices the creative of the first group. When there’s a FLEDGE auction, don’t show this ad. Can we solve this use case?
*   Michael Kleber
    *   “Don’t show this ad _on this site”_ seems like a problem we can solve. But the ad is inside a FencedFrame, and the user communicating that they don’t want to see this ad breaks some of the limited information flow.
*   Paul Jensen
    *   Yahoo! Japan’s example showed the user going to the advertiser site and expressing this.
*   Michael Kleber
    *   But not easy to do, because the advertiser may not have the chain back to how the ad was shown. User might have been added to the IG on a different site. On the publisher site, can communicate this back to the advertiser. We could hope to tell the publisher or tell the advertiser. But if we let the user navigate the user to the advertiser, the user can control what the user sees on their site, but less so on other sites.
    *   On the publisher page, could pierce the Fenced Frame barrier, end up communicating back to the publisher, who should be able to control all the ways that ads come to their page.
*   Joel Meyer
    *   You already do tracking of creatives to do frequency capped. If you treat it as always blocked or frequency capped, the important thing is that the user never sees that ad again. As far as I know, SSPs don’t block specific creatives for specific users. For David Dobb’s case, never opt them into eligible buyers and then garbage college their InterestGroups.
*   Michael Kleber
    *   Disagree that we just solve the simplest case. First of all, need to have some new browser UI. Some sort of visual indication. Then help to ensure that the user isn’t shown the same ad with a slightly different URL. The browser should satisfy the user’s simplest possible need.
*   Joel Meyer
    *   What choices does AdChoices make today? Are you going for parity or perfection?
*   Michael Kleber
    *   What AdChoices makes today is in the hands in the ad tech community, and Chrome doesn’t want to implement exactly that if that’s not right for the user.
*   David Dabbs
    *   We’ve all been talking about AdChoices as if there’s an agency there. There’s a standard for the icon, if you’re going to use that, you have to use certain language, etc. Its industry self-regulatory bodies, if you’re a member of those, that require you to offer users to opt out of all Interest-based advertising (by your org). To be members in good standing and use the AdChoices icon, need to offer the ability to opt out. All of these regimes were built into the bad old cookie world when there wasn’t an alternate privacy-centric IBA mechanism like PAAPI.  Won’t be able to use the same creative in the PAAPI type delivery as on different mechanisms. Probably a different disclosure mechanism or standard. As a DSP, we can’t promise generically to never do _any _Interest-based advertising, only via the on device mechanism offered by this browser (or whatever language is blessed by the self-reg groups.) \

*   Michael Kleber
    *   Quite plausible that a user wants to deliver that message to one DSP and not another DSP, but it’s also possible the user wants this for all DSPs, in which case we can turn off Interest-based advertising for everyone.
*   Harshad Mane
    *   If you want to block a particular creative, then that information needs to be passed on to SSPs or DSPs, because that creative should not win the auction again. Should be a clear signal that this particular creation should not be a part of the auction.
    *   When user says that they don’t want to see this ad, it should work for both contextual as well as FLEDGE. Solution shouldn’t be partial.
*   Michael Kleber
    *   Great goal, and if it’s possible to offer, then would certainly endorse the browser trying to offer it. Don’t want the browser to tell the user, yep, we’ll keep you from seeing this ad, and then showing them the ad again. If an ad were the same as a renderURL, then it would be easy, but it’s not.
*   David Dobbs (in chat)
    *   I don't see how we can make a disclosure from a FLEDGE delivered creative that makes any statement about behavior in the traditional, identifier/identified programmatic world.
*   David Dobbs
    *   You have the OS/Web issue here too. Can you get closure on all the mechanisms? All the channels and all the surfaces.
*   Michael Kleber
    *   Could the ad have a piece of metadata that said, “This is this particular ad”, and everyone in the ecosystem agreed to support this, could solve this problem. It’s not something the browser could invent; it would have to come from the ad industry. If the ad industry wanted to invent that sort of label - the level at which things should be blocked - could work across multiple channels, or could be a tracking vector. Hard problem no matter what.
*   Paul Jensen
    *   How does AdChoices enable you to opt out of Interest-based advertising?
*   Michael Kleber
    *   One ad tech at a time. Sets a 3p cookie on that ad tech that indicates that for that user.
*   David Dabbs
    *   There’s a page that allows you to express this for hundreds of different advertiser domains. YourAdChoices, NAI Opt Out portal, &c.
*   Marco Lugo (on chat)
    *   the thing is if we don't get this right, ad block will again be the go-to solution and the industry as a whole loses so even if this is a hard problem it's certainly a worthy one
