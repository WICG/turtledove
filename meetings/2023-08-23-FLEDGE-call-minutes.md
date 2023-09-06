
# Protected Audience (formerly FLEDGE) WICG Calls: Agenda & Notes

**_Frequency update: Note that meetings now happen every week!_**

Calls take place on ~~some~~ Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Aug 23, 2023


## Attendees: please sign yourself in!	


1. Paul Jensen (Google Chrome)
2. Bosko Milekic (Optable)
3. Priyanka Chatterjee (Google Privacy Sandbox)
4. Manny Isu (Google Chrome)
5. Matt Menke (Google Chrome)
6. Matt Menke (Google Chrome)
7. Orr Bernstein (Google Chrome)
8. Sid Sahoo (Google Chrome)
9. Isaac Foster (MSFT Ads)
10. Paul Farrow (Microsoft Ads)
11. Kevin Lee (Google Chrome)
12. Marco Lugo (NextRoll)
13. Xavier Capaldi (Optable)
14. Antoine Niek (Optable)
15. David Dabbs (Epsilon)
16. Andrew Pascoe (NextRoll)
17. Jonasz Pamuła (RTB House)
18. Caleb Raitto (Google Chrome)
19. Alex Cone (Google Privacy Sandbox)


## Note taker: Manny Isu


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here — no agenda no meeting!]



*   Isaac (MSFT) Issues (don’t all have to be mine, so grouping so we can easily filter)
    *   <span style="text-decoration:underline;">BA: Jan 1st 1% vs Feb Scaled Testing https://github.com/privacysandbox/fledge-docs/issues/55</span>
    *   <span style="text-decoration:underline;">User IG view/delete interaction w/r/t BA payload optimization https://github.com/privacysandbox/fledge-docs/issues/56</span>
    *   <span style="text-decoration:underline;">Multiple Bids Per IG (checkin, was discussed a while back) https://github.com/WICG/turtledove/issues/595</span>
    *   <span style="text-decoration:underline;">PA Deals Phase: https://github.com/WICG/turtledove/issues/686</span>
    *   <span style="text-decoration:underline;">Cross Device https://github.com/WICG/turtledove/issues/607</span>
    *   <span style="text-decoration:underline;">Production Operations: </span>

        <span style="text-decoration:underline;">https://github.com/WICG/turtledove/issues/728 \
https://github.com/WICG/turtledove/issues/620</span>

*   Nick (Triplelift)
    *   Clarifying how the attestations file lookup/check works (Seller perspective); is the “Seller” origin checked prior to every auction?
*   Bosko (Optable)
    *   GAM throttling
    *   https://github.com/WICG/turtledove/issues/760 


# Notes

**Isaac (MSFT) Issues**



*   <span style="text-decoration:underline;">BA: Jan 1st 1% vs Feb Scaled Testing https://github.com/privacysandbox/fledge-docs/issues/55</span>
    *   [Isaac] Want to have a discussion on Google’s recommendations for B&A - Can we expect to use it in production? Just trying to understand the guidance
    *   [Paul] The 1% trial is the mode B testing. privacysandbox.com/timeline will provide overall timeline for 3PCD. The 1% cookie deprecation is different. A number of folks wanted a way to test the protected audience API where it’s not competing against folks using it. Essentially, it is an interim way to test what happens when we do away with 3PCD. https://developer.chrome.com/docs/privacy-sandbox/chrome-testing/
    *   [Isaac] Is it the case that the 1% of Chrome browsers that have opten in will no longer be able to communicate with the iFrame… will we have to use the tools that are provided by Chrome for that 1% traffic?
        *   [Paul] Yes, that sounds right
    *   [Isaac] For adtechs planning to use the server side variant, what is the recommendation?
        *   [Alex] There is nothing we can do to force you to use the APIs. We want you to use them. For those thinking about the server side, it is great. It’s just important to know that on-device is GA and B&A is not. Re: 1% - It is more difficult to evaluate performance of the APIs if you’re being outbid by those using 3PCD and that is why the 1% trial is helpful. 
        *   [Isaac] For adtech planning to use B&A and would like to participate in 1% mode B trial, what will be recommended by the B&A folks?
    *   [David] The mudiness is that the CMA imposed mode B is where uo make it possible for adtechs to have a clear path… but it will be muddied because the ondevice model will be rolled out and be able to be tested. However the real set of wheels (B&A) won’t be fully tested, which is the one everyone wants to be on.
        *   [Isaac] Agree
    *   [Paul] Remember that B&A is an optional service, which has now incorporated features from the ondevice. By testing ondevice, you’re in a way testing features that will be available on B&A. Targeting capabilities, building IGs and audiences - all of these - can be done ondevice today from a targeting perspective.
    *   [David] For migration path, is it yes and or either or - is there a path where they can coexist?
        *   [Paul] From the design, it allows them to coexist.
    *   [David] Is the OnDevice a superset right now, and you’re working expeditiously to get all the features in B&A?
        *   [Paul] Some parts are in B&A today and there are other things that will be different at times
        *   [Arun] It is a superset right now but we are working towards adding feature parity.
    *   [Isaac] It is tricky to do a 1% test when a lot of the industry that believes they need to be on the server side version cannot test most of the features
        *   [Priyanka] We are keeping the uber explainer upto date: ALpha, Beta, OT and along the way. B&A is not in scope for CMA testing because of the deadline. My recommendation is that for any adtechs looking at B&A should participate in the testing OnDevice and work towards B&A
    *   [Isaac] Can I request that you make this clear publicly? The challenge I see here is that a lot of the readiness is indexed on the logical side… the operational side of this doesn’t work?? for the industry
    *   [Paul] OnDevice makes it easier to adopt in some ways
    *   [Arun] We are recommending to start on device and if they do like, they can move on to B&A servers. We are making sure that the transition will be seamless.
    *   [David] It is not clear right now of how many SSPs and Publishers will allow this… it might be working but how well deployed is it going to be? Also, is my understanding that the providence of B&A was the Android side?
        *   [Paul] We have been running PA auctions on Android devices for over a year now. The devices are much more capable than they were a few years ago. There are some upsides like JSON parsing - happens a lot faster.
    *   [Bosko] What is the recommendation on how we can scale and getting value as soon as possible?
        *   [Paul] I will not be the best person to answer that. We will need somebody from GAM.
        *   [Bosko] I understand that there are good reasons to throttle but I have some reservations about it. I will hold my comment until next week when we have a GAM representative.
            *   Related comment in google/ads-privacy here: https://github.com/google/ads-privacy/issues/77#issuecomment-1690872192 

https://github.com/WICG/turtledove/issues/760 

*   [Bosko] The updateURL does not allow refreshing/updating the user bidding signals. Is this intentional or is it a bug? If intentional, then we should make that clear in the documentation.
    *   [Paul] It seems to me that we should be able to but will defer to Caleb
    *   [Caleb] No we do not support that update
    *   [Bosko] If there is no reason to forbid updating the signals, then we should perhaps look into it
    *   [Paul] We can dig into it some more
