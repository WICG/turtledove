# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 4pm UTC (during winter)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday Jan 31, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!	



1. Orr Bernstein (Google Privacy Sandbox)
2. Patrick McCann (Raptive)
3. Roni Gordon (Index Exchange)4
4. Brian May (dstillery)
5. Shankar Venkataraman (Jivox)
6. Andrew Pascoe (NextRoll)
7. David Dabbs (Epsilon)
8. Isaac Schechtman (Sovrn)
9. Tim Hsieh (Google Ad Manager) 
10. Laurentiu Badea (OpenX)
11. Sven May (Google Privacy Sandbox)
12. Youssef Bourouphael (Google Privacy Sandbox)
13. Amit Gupta (Jivox)
14. Taranjit Singh (Jivox)
15. Fabian Höring (Criteo)
16. Paul Spadaccini (Flashtalking)
17. Caleb Raitto (Google Chrome)
18. Becky Hatley (Flashtalking)
19. Matt MEnke (Google Chrome)
20. Arthur Coleman (OnlineMatters)
21. Ricardo Bentin (Media.net)
22. Tim Taylor (Flashtalking)
23. Alex Peckham (Flashtalking)
24. Matt davies (Criteo | Bidswitch)
25. Itay sharfi (Google)
26. Pawel Ruchaj (Audigent)
27. Isaac Foster (MSFT Ads)
28. Russ Hamilton (Google Chrome)
29. Rickey Davis (Flashtalking)
30. Don Marti (Raptive)
31. Bram Woolcott (TripleLift)
32. Abishai Gray (Google Privacy Sandbox)
33. Egor Kozmenko (BidSwitch/Criteo)
34. Laszlo Szoboszlai (Audigent)
35. Amitava Ray Chaudhuri (Adobe)
36. Vedant Seta (media.net)
37. Ruturaj Vartak (media.net)
38. Stan Belov (Google Ads)
39. Anthony Yam (Flashtalking)
40. Alonso Velasquez (Google Chrome)
41. Owen Ridolfi (Flashtalking)
42. Alexandre Nderagakura (Not affiliated)
43. Guillaume Polaert (Pubstack)


## Note taker: Matt Davies


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/


## Suggest agenda items here:



*   Orr's Creative Scanning proposal: https://github.com/WICG/turtledove/issues/792 
*   Isaac:
    *   [Revisit Persistent Opt Outs question w/r/t PST](https://github.com/WICG/turtledove/issues/915#issuecomment-1892962819) (see PST specific question [here](https://github.com/WICG/trust-token-api/issues/288))
    *   Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
    *   Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736
*   Tim Hsieh
    *   Updated proposal for Video and Native support https://github.com/WICG/turtledove/issues/265#issuecomment-1914779917 


# Reminder of Upcoming Events:



*   [Announcement] Chrome Facilitated Testing Webinar
    *   The Google Chrome team will be hosting our next external webinar sessions where we will focus on the testing and preparing for the current 1% restriction of third-party cookies in Chrome along with use of Mode A and B available as part of Chrome-facilitated testing.

        The first **Americas friendly session** is happening on**  Feb. 1st 12-1 pm ET**. A second **Japanese language session** is happening **Feb. 6th 3:30-5:30 pm JST**. A third **EMEA friendly session** takes place on** Feb. 8th 12-1 pm GMT**. 

    *   To join, please register below: 
        *   AMER-friendly: [Register Here](https://rsvp.withgoogle.com/events/chrome-facilitated-testing-webinar-amer)
        *   EMEA-friendly: [Register Here](https://rsvp.withgoogle.com/events/chrome-facilitated-testing-webinar-emea)
        *   Japanese language: [Register Here](https://rsvp.withgoogle.com/events/testing-and-preparing-for-third-party-cookie-restrictions-oh)


# Notes


## Orr’s creative scanning proposal API

Slides presented: https://github.com/WICG/turtledove/blob/main/meetings/2024-01-31-FLEDGE-call-slides-creative-scanning.pdf

Document is at https://docs.google.com/document/d/1s0tTN25AiPwl3ocCFYOLqeKhetZCt_YFIYQEQ7wzHqI

Presentation and discussion 

Why do we need this 

Git hub 792 https://github.com/WICG/turtledove/issues/792  - talks about what is challenging 

Sellers need to comply with creative restrictions for ads

Sellers need to scan - which takes time and determines the suitability of the ad for the seller 

Challenge - takes place not at the time of auction - and is separated which makes a challenge 

Event level reporting could be one way - however a chicken egg issue 

Event level reporting just shows the ad that has won, but if creative has not been scanned it can't win 

Also could use debugging - but the problem is the downsampling etc …. So unsuitable 

Goals: 



*   Ensure that ads sent for creative scanning 
*   Don't overload sellers servers with a firehose of ads to scan 
*   Minimize the privacy impact 

Is hard to avoid sending a huge amount of requests but try and minimize the impact 

What info is being sent 

Creative scanning metadata - e.g a domain and seat 

Intentionally distinct from metadata - meant to be small 

Low privacy risk because it's the same data that could have been sent from the buyer directly

Want to send a lot of this metadata to the seller for scanning - the privacy risk is low as the same info the buyer could have sent to the seller 

Same info can be encoded into the URL 

To what endpoint is it sent?

Once you have the need between buyer and seller then you need to agree an endpoint 



*   Safest way - have a well known urI within the platform 
*   E.g. www.example-ssp.com/-well-knowl/interest-group/creative-scanning 

To which seller are ads sent?

Buyer specifically defines which seller the creatives should be sent to at a well-known uri 

Privacy risk of listing a specific listing of sellers 

Buyers have to list which sellers that they work with / would score their ad etc…. 

Can override sellers for an interest group as occasional scenarios occur that can allow this 

If you specify a set of sellers in seller capabilities it takes precedence over the well-known file

LImit traffic to creative scanning entry points 

Brian May - suggests that we don't refer to debugging only 



*   Wants more info around the flow of the creative scanning 

The slides mimic the document and they have certain options so will discuss the other options and will come back to a flow doc

Patrick - Raptive said that they don't trust the sellers for the scanning so will hire some vendors to do this for them such as confiant, geo-edge etc… 

Would be better fit for purpose if the top level seller could identify the creative scanner instead of the component sellers scanning the ads?

Paul Jensen: The wording here says sellers  but there is nothing in the design that specifies who should be the scanner - 

Pat McCann - publisher will have a great relationship with the scanner 

Could have confiant set up within  the TEE and then could use this to run scans and then you can specify to the top level seller that you can specify that the pub use confiant 

Paul Jensen: This is interesting point and doesn't talk about this within the spec 

Pat McCann: Does the spec only allow for component level or top level 

Paul Jensen: It does not specify which, there are certain options within the spec - boikls down to who the buyer specifies that they want to use who the scanner?

Brian May: Does it make sense for the buyer to be specifying this? 

Orr: Question is then how does the browser know this, and determine the sequence of events etc 

Brian May: Was due to this around the flow - as the sequence doesnt quite fit with how it works in the real world 

Orr : there are some options on the spec and will run through this, but is partially a privacy concern 

Buyers are indicating sellers who they trust and then sellers delegate the responsibility  of who that they want to scan 

What is boiling down to is needing a whitelist of sellers they want to work with to know this in advance. 

Orr: The document describes a number of different options of when we send the scan request. 

Most of the options are related to when the IG group is created or updated

Privacy risk is lower than at auction time. 

Some of the options are at the time of auction whilst still remaining privacy compliant but this is a challenge 

Talked about the preferred option which is 2b 

When an interest group is joined and updated the browser considers each of the ads in the interest group. 

Browser then maintains a mapping table to avoid scanning again 

If the user revisits the site again then the creative would not be sent for scanning again

Send only those ads not previously sent from this device for a given joining site

Idea is that each ad is sent to each seller everytime that the IG is joined from a distinct site 

Still means a lot of scans and requests for scanning 



1. 

It will scan the ads at join time, and then scan for new ads at update time, also check to see if the list of sellers have changed it will update then 

Shenkar: 

This assumes that the ad does not change from one use to another. 

What happens if there are multiple versions of the creatives in the same ad etc… 

Orr: Largely this is a discovery problem, trying to solve how to send the info to the browser and seller etc - what it scans is out of scope for now, this is about the transit of request information 

How the scanning happens there is no proposal for this in the spec

David Dabbs: Scanning is in the eye of the scanner as to how / what that they do so long as they can get the details to scan which is part of this spec. 

Orr: Question on component ads - how does this work today 

DD: Component ads are a new thing, there will be some vendors that use this, and you would get some markup blob to scan but you would need to talk to the seller - effectively you are submitting markup 

Anthony Yam: This touches on the core issue around the dsp is defining what the ads are, but is actually controlled by the adserver rather than the dsp. This design presumes that the dsp controls the creative and decides on what is being shown. 

This is not how it works currently, it is the creative adserver that controls this and knows when a new creative is available and needs to be scanned and the dsp does not know what creative needs to be scanned 

DD: The dsp does know that there are some markup that needs to be shown etc… 

Anthony Yam: This is correct but the dsp won't know exactly what the creative is going to be served exactly and there is some utility there, but the design presumes that the dsp is the creative controller when it is usually not. 

Orr: The concern is that as soon as the dsp is joining an IG - the creatives may not be ready at that stage 

Anthony Yam: There maybe something to scan but may not be what is actually shown to the user

Orr: This is likely same as today, how do we do this now? 

Anthony Yam: Is it only at the point of auction for creative scanning 

SV: Has clients that scan at random times during the day 

Scanning often happens at the time of launch of the creatives etc… 

Largely this is a design around the discovery of the ads to be scanned, what the scanner then does with how it carries this out etc the spec has no opinions on it 

Other options considered 



1. Send all ads during ig update / join 
    1. No benefits over fetch requests 
2. Send only ads not previously sent from this device 
    2. Privacy risk from browser memory 
3. Send only those ads as configured by the seller 
    3. Does not scale 
4. Use k-anon as a proxy for which ads the seller has not yet seen 
    4. Ads not sent to sellers who join late 
5. Call trusted scoring signals server during join/update
    5. More expensive
6. Reuse the auction time call to the trusted scoring signals server 
    6. Leeks privacy 
7. Reuse the auction time call but only send ads that are k-anon 
    7. No meta data 

Pat McCann: Can you describe more about 5 - is it more expensive with higher quality? 

Orr: Not nec higher quality? 

Pat McCann: If it is happening in TEE can you then use third party creative scanning service etc

Orr: could use it to triage and then if you want can pass to another server etc…. 

Don't get much more by first talking to the trusted signal server 

Roni: 

Wants to talk option 4 

Specifically option 4 is specifically k-anon so curious about option 2 - leverages UA cache to keep track of what is going on, would this not solve the late sellers 

If it has not met k-anon send 

If it has and has not been scanned before then you can send etc

At an individual browser level it boils down to 2 things it does not know if itself is the cause of the lateness etc - needs more thoughts on this… 

The issue is the significant volume of data - 2 is very large and 4 is better but with caveats around k-anon

Question around 6: 

If the previous statement that there is no new data from the render url - so what is wrong with it

With 6 it is subtle - at the auction time and you conveying the bunch of contextual info and the ad, the issue is not that the trusted scoring signals could be use, but has a channel that could allow for it to send - it allows for a 1 but leak but can be taken advantage of for every ad and could lead to cross-site information. 

The problem at auction time allows this - so experiments carried out adding noise but even that can be overcome. 

Laurentiu: 

Would it be possible to use the k-v server missed key lookup - if the kv server can remember and expose which keys it has missed should solve this problem, no privacy leak as only sending renderUrls it has missed in aggregate. 

Has this been considered? 

Orr: not fully grocked this, please write it up in the github issue so that it can be looked in more details 

LB: The KV server can maintain a cache of the missed lookups - as an internal endpoint that can be queried and then you will know all the queried render urls not known to its store, to queue for scanning. 

Orr: Will look more into this and take it as an action point. 

David dabbs: 

The simplest approach here is the industry solves this and buyers submit to sellers etc… 

Could we find a way for the community to solve this if none of the options work. 

Industry does not have a track record of shared infrastructure - shared spec yes but not often shared infrastructure. 

Stan (gam) : was a discussion from the private aggregation api to use this for the trusted scoring signals - issue was that the map is mapped to a 128bit design which does not know about the creative ids etc…. 

Have you thought about extending the private aggregation api 

In score ad you can make the decision for score ad to report this should go to scan 

The render urls will need to go to count with a weight signal 

Orr: not considered but will look at. 

Link to the slides will be in notes or converted to pdf and uploaded to the github along with the document 


## Chat copied into notes: 

Rickey Davis

4:04 PM

we only need two "aye's" to have a quorum, right?

Matt Menke

4:05 PM

https://docs.google.com/document/d/1Kr0hpfQ\_Q1LX1aN00D5k\_09yV\_a7WE9RSn69nS3nZho/edit?usp=sharing

David Dabbs

4:07 PM

Please ensure your seatbacks and traytables are in their upright and fully-locked position.

You

4:08 PM

If no one will ill volunteer

Brian May

4:09 PM

Thanks

Patrick McCann

4:13 PM

is crid in that eg list?

crd = dsp creative id

Amitava Ray Chaudhuri

4:15 PM

can buyer send a map of universal ID and interest group.

can buyer create a map of universal ID and interest group.

Patrick McCann

4:17 PM

On many many publishers, there is a third party (eg confiant, geoedge, the media trust, adlightning, or clean.io) that does this creative scanning. Could the top level seller identify one of these vendors for creative scanning in some way, even if the component seller does not?

Arthur Coleman

4:20 PM

Agree with Patrick - was one of my thoughts

Laurentiu Badea

4:28 PM

Also if I want to know all the sellers you are working with...

Shankar Venkataraman

4:31 PM

If the ads are dynamically assembled  at ad serve time,  as in a DCO ad server, then this model fails?

Isaac Foster

4:32 PM

fwends i must drop, this is great and look forward to engaging more on this...hopefully we can talk the PST opt out thing next week? :)

Isaac Schechtman

4:32 PM

or would you have to scan each component and somehow allow all variations depending on the DCO

David Dabbs

4:33 PM

THat's no different that today.

Sellers see adm in the bid stream and sample iot.

Swati Vartak

4:39 PM

What about “I dont like this Ad”, like in Ad choices button

David Dabbs

4:40 PM

As you say in the doc, the job here is \_discovery\_.

Roni Gordon

4:42 PM

I'd like to dig more into Options 4 & 6 -- as per my comments on the github issue

Anthony Yam

4:43 PM

Creative scanning isn't our primary concern. We're just continuing to point out that the PA overall design is overly DSP-centric on the buy side.

Shankar Venkataraman

4:43 PM

Fully agree, Anthony

Patrick McCann

4:44 PM

it seems publishers might be willing to pay for option 5 if it provides better service

more expensive and higher quality doesnt seem like a worse option

Shankar Venkataraman

4:45 PM

My contention is that the DSP is the least qualified in the PA case to have any control other than bid logic. The core data is owned by the advertiser (site activity) and the ad server who serves the ad based on the activity.

David Dabbs

4:45 PM

And only 15 minutes

Laurentiu Badea

4:46 PM

if seller KV server can expose missed key lookups via an endpoint

no extra traffic, precise lookup, minimal effort

David Dabbs

4:48 PM

>if seller KV server can expose missed key lookups via an endpoint

Laurentiu, you're referring to a new affordance in the future TEE-based K/V setup, yes?

Laurentiu Badea

4:49 PM

yes

Amitava Ray Chaudhuri

4:50 PM

Can we get the link of the presentation

Patrick McCann

4:50 PM

x2: would love to share this presentation with my creative scanning vendor

Brian May

4:51 PM

Another 1 bit leak!

Isaac Schechtman

4:51 PM

yes please a copy of this presentation would be appreciated

Arthur Coleman

4:55 PM

please include link in the notes

Roni Gordon

4:56 PM

ideally attached to the wicg meeting notes and available on the repo, like we have for other such preseentations

Patrick McCann

4:56 PM

could someone paste that linl

Brian May

4:56 PM

It does seem like something that we should create interop standards for.

Patrick McCann

4:57 PM

how can we access the document linked on the screen without screenshotting it and typing it

Roni Gordon

4:58 PM

it's here -[ https://github.com/WICG/turtledove/issues/792#issuecomment-1900643025](https://github.com/WICG/turtledove/issues/792#issuecomment-1900643025)

Patrick McCann

4:58 PM

ty

Brian May

4:58 PM

ty

Arthur Coleman

4:59 PM

I like Stans idea a lot - in fact a buyer'

Laurentiu Badea

4:59 PM

Link to Joel's comment on KV missed key:[ https://github.com/WICG/turtledove/issues/792#issuecomment-1763536769](https://github.com/WICG/turtledove/issues/792#issuecomment-1763536769)

Arthur Coleman

4:59 PM

sa buyers ad quality score could be added

Amitava Ray Chaudhuri

4:59 PM

Thanks for sharing
