# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California = 5pm Paris time = 3pm UTC (during summer)

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday May 29, 2024 

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 


## Attendees: please sign yourself in!

1. Michael Kleber (Google Privacy Sandbox)
1. Aymeric Le Corre (Lucead)
1. Brian May (Dstillery)
1. Harshad Mane (PubMatic)
1. David Eilertsen (Remerge)
1. Matt Kendall (Index Exchange)
1. Kevin Lee (Google Privacy Sandbox)
1. Laurentiu Badea (OpenX)
1. Jacob Goldman (Google Ad Manager)
1. Anthony Yam (Flashtalking)
1. Leeron Israel (Google Privacy Sandbox)
1. Sven May (Google Privacy Sandbox)
1. David Dabbs (Epsilon)
1. Alexandre Nderagakura (Independent / consulting)
1. Rickey Davis (Flashtalking)
1. Alex Cone (Google Privacy Sandbox)
1. Yanay Zimran (start.io)
1. Garrett McGrath (Magnite)
1. Omri Ariav (Taboola)
1. Abishai Gray (Google Privacy Sandbox)
1. Scott Myers (Google Privacy Sandbox)
1. Paul Jensen (Google Privacy Sandbox)
1. JASON LYDON (MO)
1. Becky Hatley (Flashtalking)
1. Manny Isu (Google Privacy Sandbox)
1. Tim Taylor (Flashtalking)
1. Luckey Harpley (Remerge)
1. Courtney Johnson (Privacy Sandbox)
1. Orr Bernstein (Google Privacy Sandbox)
1. Paul Spadaccini (Flashtalking)
1. Sarah Harris (Flashtalking) 
1. Isaac Foster (MSFT Ads)
1. Fabian Höring (Criteo)
1. Matt Davies (Criteo | Bidswitch)
1. Caleb Raitto (Google Chrome)
1. Felipe Gutierrez (Microsoft)
1. Tamara Yaeger (Criteo | BidSwitch)
1. Shivani Sharma (Privacy Sandbox)
1. Matthew Atkinson (Samsung)
1. Brian Schneider (Privacy Sandbox)
1. Alonso Velasquez (Privacy Sandbox)
1. Elmostapha Beljebbar (Lucead)
1. Sathish Manickam (Google Privacy Sandbox)
1. Shafir Uddin (Raptive)
1. Warren Fernandes (Media.net)
1. Tal Bar Zvi (Taboola)
1. Kevin Nolan (NextRoll)
1. Andrew Pascoe (NextRoll)
1. Koji Ota(CyberAgent)


## Note taker: Manny Isu

# Agenda
## Process reminder: Join WICG
If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which can be found here: https://www.w3.org/community/about/process/cla/ 

## Suggest agenda items here:

* Leeron Israel:
  * Deals support in Protected Audience: https://github.com/WICG/turtledove/issues/873#issuecomment-1994888034


* Isaac Foster:
  * Patch’y Updates When on Joining Origin: https://github.com/WICG/turtledove/issues/1162
  * Attestation Process  https://github.com/privacysandbox/attestation/issues/53 
  * Brief revisit the “coarse information sharing” thing, we had talked about setting up time but never did, all got too busy…can even answer here
  * Multi Tag Support via “Mixed Ranking”: (really, this + multi tag + bit leak discussion and how we can be creative) https://github.com/WICG/turtledove/issues/846
  * Optional decouple bidding/reporting function urls to allow smaller k tuple: https://github.com/WICG/turtledove/issues/679#issuecomment-1703973736

* Omri Ariav:
  * Follow up on main blockers for native advertising (one, two, three) 
  * Follow up on easing domain restrictions (https://github.com/WICG/turtledove/issues/956) 

  * Warren Fernandes
    * Follow up on the proposal to support an analytics entity (https://github.com/WICG/turtledove/issues/1115)
   

# Announcements
The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

# Notes
## (Leeron Israel) Deals support in Protected Audience: https://github.com/WICG/turtledove/issues/873#issuecomment-1994888034
* This is a proposal to support deals in PA API understanding that it is an important use case for all of you. We propose to use an existing field - buyer and seller reporting ID to store deal IDs to allow the buyer in generatebid to report in score ad and reportwin. This will satisfy the vast majority of use cases. Is anyone familiar with it or have any thoughts to share with the group?
* [Isaac] Is there a change to the API?
* [Leeron] Yes, there are a few changes we plan to make, 1) Modify buyer and seller reporting ID to accept an array 2.) Enable generatebid to select specific value from that array 3.) Buyer and seller reporting id will be made available to scoread 4.) K anon - If ad does not pass the k anon threshold, the bid will be withdrawn. 
* [Isaac] So this will make it easier to bid on - but this will not allow a situation where deal eligibility can be determined based on private signals, correct?
* [Leeron] My understanding is that when it comes to deals, there are 2 categories 1. Targeting 2. Commercial. Commercial can be pre agreed upon… but there might be cross site seller provided information. We are not making any changes to Targeting to accommodate in this design. The reason why it is not included is to keep the scope tight.
* [Isaac] Would like to think through this, but I see some of it to be valuable. I think it’s a positive 
* [Kleber] It is worth pointing out that the attribute that says this is an essential piece of information is already available today by putting it in the URL; subject to K anon…
* [Isaac] Understood. To the extent that we can reduce the number of places we have to persist things…
* [David] So the source of the list of deal structure has to come from the attributes of the ads, correct?
* [Leeron] Yes
* [David] So my sense of deals today is that I am going to get a bag of deal IDs specific to me that will change from… not grocking how to embed all the various deals. Maybe I am missing something
* [Leeron] Today, deals are created on the SSP side… someone on the buy side needs to setup targeting on a campaign for a specific deal. The change here is that the association needs to be stored in the IG (on device)
* [Isaac] Agree with David but guess you may be calling out a challenge with the proposal or a challenge with predeclaration of your targeting in IG
* [David] Yes, precisely. 
* [Leeron] One thing to call out - Deals in contextual flow will continue to work as they do today. But of the buyer wants to bring some IG information with deals to target an ad, it may reduce the subset of deals perhaps. But what is the specific technical issue?
* [David] The deals I have to put on my creatives may be unwieldy… need to read this some more
* [Kleber] Would you feel differently if the list of possible deal ids were an attribute of the entire IG?
* [David] Might be but need to get over the conceptual mismatch from associating deals from a seller
* [Isaac] You choose to target a deal using your campaign and it gives you access to private marketing or pricing. The effect of the deal id on your evaluation will be in scoread and generatebid. The targeting will be accomplished by grabbing your object graph and dumping it into the IG
* [Brian] So will you put your entire catalog of deals into the IG?
* [David] Not sure. Today we receive the deals on a bid request from the seller. Need to look at this and check in with my folks on it. Maybe the sue case I am thinking of does not apply here
* [Warren] Can a seller inject deals on a per buyer basis via the auction config perhaps?
* [Leeron] Technically that is possible today. The list of deals will be available in scoread but the key piece of information missing is the association between the deal and the bid. Our solution addresses that because we can do the k anon check in advance to make sure it can egress from the auction.
* [Warren] What we put into the per buyer signals tend to be what we… 
* [David] We have an industry group working to fill in… one of the things is an opt in mechanism for the buyer to address exactly what you are describing. 
* [Kleber] On the subject of unlocking inventory, it is worth pointing out that if the fundamental thing you are trying to do is for the seller to annotate the auction; if you use Deal ID 1 2 3 then you can unlock the inventory. If this is the problem you are trying to solve then a way of looking at the solution is… From the k anon pov, a single response for taking the seller up on their offer could be used on different pages. The notion that IG carries the deal id around with it might be useful in some situations. From the browser pov, we the browser can check the k anon properties before the auction starts. We cannot allow bids on arbitrary deal id is that there will be some privacy leakage. 
* [Miguel Morales] We looked at Deal ID and did not realize that it is subject to k anon?
* [Leeron] Do not believe that any value passed into per buyer signals is subject to k anon check
* [Miguel] But the ad metadata is not subject to k anon
* [Leeron] Yes but it is also does not egress from the auction
* [Kleber] If you are willing to use PAgg to find out how often a deal got used, then that is also good. This is specifically about getting deal id into event level reports
* Note: Please review GH post 873 and if there is anything else, please post on that GH



## Omri Ariav: Follow up on main blockers for native advertising (one, two, three) 
* [Omri] Taboola is a leading native advertising vendor with code on page on many publishers websites. We show sponsored and organic recommendations, usually in the form of an endless feed below the article. . We have some challenges with implementation. Is there any plan for a dedicated loop with Native?
* [Kleber] The focus to integrate information from publisher pages has been focused on the video use case. A lot of the answers being developed for video may be useful for native rendering. No primary focus recently, so you are right in that regard
* [Omri] Can we expect a bolder discussion of the native use cases in the foreseeable future?
* [Kleber] The look and feel is best suited for reusing some of the video discussions happening.
* [Alonso] Do you really mean targeting in bgeneral or do you mean that there is a specific targeting capability for the native use case?
* [Omri] It is the first one… having it working in the context of native (O.A: in native the amount of ad slots can be endless comparing to display)
* [Kleber] The negative targeting GH is Issue 1096. It sounds like the core thing you are looking for is… suppose that same buyer is going to show  a bunch of different ads, you don’t run a single auction, you run a series of auction, one for each ad slot, and when you return a result, then you are dealing with having the second ad slot on the page to be aware of the first ad slot so you can skip that one and take the next one based on priority. Is that correct?
* [Omri] That is correct.
* [Kleber] It seems like this is something that we should be able to help with. Do you feel like frequency capping can help solve your issue?
* [Omri] Yes, we can look into it. Let’s work together to solve those use cases
* [Fabian] It is an interesting proposal. So the idea is to take out stuff that has already been displayed?
* [Kleber] Yes, that is correct. 
* [Fabian] But then what about atomic …?
* [Kleber] Need to spend some time talking to the Chrome engineers working on the auctions to figure out how to add appropriate atomicity to make this work. We will need some time to think about this one
* [Paul] Which one of the issues is the highest priority?
* [Omri] Issue 1096 (Negative Targeting) and then Issue 1074 (Return Multiple Results)
* [Paul] Are those ads all from the same IG?
* [Omri] Not necessarily. As long as we keep it siloed per auction, if we will keep it one auction per slot it will result performance issues, many worklets running, and high infra costs to native advertising vendors
* [Omri] Ads may come from multiple advertisers as well as multiple IGs..
* [Kleber] I think this is an area where we can design something better to support your use cases
* [Omri] we are willing to collaborate and help you in the design 
* Follow up on easing domain restrictions (https://github.com/WICG/turtledove/issues/956) in the next meeting
