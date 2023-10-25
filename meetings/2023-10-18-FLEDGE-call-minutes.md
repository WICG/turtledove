# Protected Audience (formerly FLEDGE) WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Oct 18, 2023


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Manny Isu (Google Privacy Sandbox)
3. Brian May (dstillery)
4. Paul Jensen (Google Privacy Sandbox)
5. Russ Hamilton (Google Privacy Sandbox)
6. Orr Bernstein (Google Privacy Sandbox)
7. Roni Gordon (Index Exchange)
8. Isaac Foster (Microsoft Ads)
9. Harshad Mane (PubMatic)
10. Laurentiu Badea (OpenX)
11. Brian Schmidt (OpenX)
12. Ali Nouri (Google Ads)
13. Jonasz Pamuła (RTB House)
14. Marco Lugo (NextRoll)
15. Matt Menke (Google Chrome)
16. Alonso Velasquez (Google Privacy Sandbox)
17. Hardik Katyarmal (EngageBuddy)
18. Elias Selman (Criteo)
19. Kevin Lee (Google Privacy Sandbox)
20. David Tam (Relay42)
21. Fabian Höring (Criteo)
22. Mahdi Sadjadi (Yieldmo)
23. Caleb Raitto (Google Chrome)
24. Andrew Pascoe (NextRoll)
25. Leeron Israel (Google Privacy Sandbox)
26. David Dabbs (Epsilon)
27. 


## Note taker: Manny Isu


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


## Suggest agenda items here:



*   Request for additional bits: https://github.com/WICG/turtledove/issues/864 
*   Roni Gordon
    *   same origin constraints - https://github.com/WICG/turtledove/issues/813 +1
    *   Additional browser event beacons - https://github.com/WICG/turtledove/issues/826 
    *   K/V lookup timeouts - https://github.com/WICG/turtledove/issues/814
    *   Macro substitutions - https://github.com/WICG/turtledove/issues/817
    *   API versioning - https://github.com/WICG/turtledove/issues/823
    *   Sensitive signals - https://github.com/WICG/turtledove/issues/824
*   Isaac:
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Temporary surge in discussion opportunities, twice a week for a bit? 3 every 2?
    *   Buyer/Seller Reporting Questions: https://github.com/WICG/turtledove/issues/682#issuecomment-1710965068
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Elias Selman \
Interest Group duration - https://github.com/WICG/turtledove/issues/855


# Notes

## Request for additional bits: https://github.com/WICG/turtledove/issues/864

*   [Ali Nouri] Used to deliver more optimization power and utility to the API. Chrome provided 12 bits. As we race against time for 3PCD, we are not having enough time for exploration and the best use for 12 bits. The proposal is to temporarily provide more bits to explore different ideas and converge on the best use for the 12 bits being proposed
*   [MK] The bits that exist are the modeling signals included in event level reports to the winner of the auction. Intended for training of ML models. That signal data is created inside of generateBid. I want to point out that for now, you can get infinity bits… have a unique ID as part of the auction config, and use the forDebuggingOnly reporting APIs to send whatever information you want along with that unique ID.
*   [Ali] There are two aspects - exploring the signals of the world and that is what you just explained. The other is to experiment with these bits to see how the experiment performs.
*   [MK] First of all, do not wait for these extra bits to start the experimentation that you can already do with forDebugOnly.  Chrome release cycle means we could not possibly deliver this in less than two months, even if we implemented it tomorrow.  So it's much faster to get started now with the tools that are already live.  From the privacy POV, there is no new loss here because forDebuggingOnly already gives away the “keys to the kingdom”. But keep in mind that they cannot last past 3PCD - bottom line, it will be 2024 only for half the year.
*   [Brian May] Can you clarify how you want to use the bits, do you want 4 sets of 12 or one of 48?
    *   [Ali] We are flexible. It will be easier if 12 becomes 48 bits
*   [Jonasz] Two comments: We have to be very clear about what happens after 3PCD if we introduce this feature. #2) What happens in mode B? If we end up having 48 bits in mode B, it might affect the outcome of the experiment.
    *   [MK] For #1, we can solve it using naming - the field would need to be called something like “extraModelingSignalsToBeTakenAwayDuring2024” to make it VERY CLEAR.
*   [Fabian] I agree with comments from Jonasz. One other comment - Removing them from the 1% traffic will not be that easy as it also biases the learned models. It will not be a representative mode.
    *   [MK] Certainly true.
    *   [Fabian] If added it should be added everywhere
*   [Jonasz] My intuition is that Mode B should be similar to the target environment as much as possible, so I think it's best to use modelingSignals with 12 bits available, because I believe that's the plan after 3pcd until 2026.
*   [MK] Okay, then I hear at best mild support and no strong opposition.  Since the next version of Chrome gets cut in a week and a half, we will decide on this one way or the other as soon as possible.

## Same origin constraints - https://github.com/WICG/turtledove/issues/813

*   [Roni] Very strict same origin policy from buyers and sellers - things like scoreAd and generateBid are static in practice, could serve from a CDN. The KV needs to be responsive and look things up.
*   [Paul] What is the exact request here?
    *   [Roni] Requirement that everything comes from the same domain should be TLD+1 instead
    *   [Paul] It is baked into the security model of the web so anything that is not the same origin, we have to be very careful. TLD+1 is not a security boundary
    *   [MK] Config for PA auction happens on the publisher page where the publisher's JS — or even 3p JS on publisher page — might have an opportunity to mess around with the auction config. This means that the SSP or DSP who is running an auction and fetching signals from the KV server does not necessarily know that the KV server is the one that it is supposed to be.  When you write your scoreAd function, are you making it check for malicious misconfiguration? For the script and the KV server to come from different domains, what we would really need is both sides to opt into this sharing of information: script needs to say "I am OK receiving data from other domain X" and KV server needs to say "I am OK having this data visible to a script from domain Y".  In that case, it should be okay from that web security point of view.
*   [David] Are we talking about the bidding script or scoring script?
    *   [MK] It is both
*   [Roni] The intent of the issue is to find a solution for these origins to declare trustworthiness to each other.
    *   [MK] Right, we need to figure out what collection of headers to make readable.  This falls under the heading of CORS, Cross-Origin Resource Sharing, and its relatives.  We want to do it without any extra http round trips to avoid more latency.  Not sure whether there is already a header for both halves of this situation.  But this will require some investigation — we need to talk to the cross origin experts
*   [MK] Roni, is this something that is urgent to resolve before testing or before 3PCD? It seems like we need to do something about it soon
    *   [Brian] If we can get this prioritized, that will be good so we can ramp up testing

## Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846

*   [Isaac] The amount of value it will add for adoption of having ability to do multi tag and multi size will be high
    *   [MK] I haven't thought much about it. But is multi tag an easier or harder thing than multi size? 
    *   [Isaac] MT will wind up being more change to the internal folks
    *   [MK] I suggest we do not dive into #846 in today’s discussion.  Multi-size is hard, not enough time left today for something even harder.
*   [Isaac] Can you summarize your thinking on Issue 211?
    *   [MK] The prospect of taking 1 bit leak and making it into many bit leak is extremely unappealing. Getting rid of the 1 bit leak is a necessary ingredient. This will take time to implement - Maybe yes in the long run, but not anytime soon.
    *   [Brian] Do you mean 2025, 2026?
    *   [MK] No, this is a we need to get to this at some point, but we will require sustained research to figure it out. I cannot associate a timeline to it.

## Temporary surge in discussion opportunities, twice a week for a bit? 3 every 2?

*   [MK] I want to point out that there is the server side of PA that will now be moved into WICG
    *   [Arun] Yes, it will be an hour long but still TBD on weekly vs bi-weekly. We can reach out to get feedback from this group

## Interest Group duration - https://github.com/WICG/turtledove/issues/855

*   [Elias] We made a proposal to extend IG TTL from 30 to 90 days. We think that this should not compromise the privacy principles of Chrome
    *   [MK] You are right that this is not deeply part of leaking a users identity so that is good. However, there is a user experience part of PS - we have said to users that a month after you visit a website, a month after, the information is gone - it is true across all the different APIs. So this is part of a story since 2019 - Changing it will be hard because we have told all the users, told all the regulators in the world. Data retention period is a very delicate thing that involves lawyers and regulators.
    *   [Andrew] I appreciate that there are other parties to communicate with on this. We’ve done some studies that show an exponential dropoff curve that shows engagement as far as 90 days, and in fact, 25% of impressions come from users who haven’t visited the site in more than 30 days.
    *   [Brian] Andrew, could you post the numbers around what you said, the types of campaigns and what kinds of campaigns are affected by the cutoff?
    *   [Andrew] Will chat with my analytics folks.
    *   [MK] Thank you for sharing these numbers — actual data about what fraction of ads you can get with what IG max lifetime is extremely valuable.  Would be great if you could say anything about other durations, like 45 or 60 days?  Also would be great to hear about fraction of revenue vs fraction of impressions.  But I understand it's hard to release that kind of information in public.
    *   [Harshad] (in chat) Can DSP keep increasing the expiry of IG to next 30 days like we extend expiry of cookies?
    *   [MK] (in chat) Yes, any time that same user is back on IG join site, can rejoin and get the next 30 days.
 
