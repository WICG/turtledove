# Protected Audience WICG Calls: Agenda & Notes

Calls take place on most Wednesdays, at 11am US Eastern time; check [#88](https://github.com/WICG/turtledove/issues/88) for exceptions.

That's 8am California and 5pm Paris time.

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next video-call meeting: Wednesday April 9, 2025

To be added to a Google Calendar invitation for this meeting, join the Google Group https://groups.google.com/a/chromium.org/g/protected-audience-api-meetings/ 

If the meeting disappears from your calendar: try leaving and re-joining that group


## Attendees: please sign yourself in!



1. Michael Kleber (Google Privacy Sandbox)
2. Orr Bernstein (Google Privacy Sandbox)
3. Brian May (unaffiliated)
4. Paul Jensen (Google Privacy Sandbox)
5. Matt Kendall (Index Exchange)
6. Matt Davies (Bidswitch | Criteo)
7. Alonso Velasquez (Google Privacy Sandbox)
8. Yanush Piskevich(Microsoft)
9. Hillary Slattery (IAB Tech Lab)
10. Garrett McGrath (Magnite)
11. Andrew Verge (Google Privacy Sandbox)
12. Bharat Rathi (Google Privacy Sandbox)
13. Shivani Sharma (Google Privacy Sandbox)
14. Kenneth Kharma (OpenX)
15. Sathish Manickam (Google Privacy Sandbox)
16. Fabian Höring (Criteo)
17. Isaac Foster (MSFT Ads)
18. Phil Acker (Amazon)


## Note taker: Paul Jensen


# Agenda


## Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 

Contributions to this work are subject to W3C Community Contributor License Agreement (CLA) which  can be found here: https://www.w3.org/community/about/process/cla/ 


## Suggest agenda items here (meetings with no items may be canceled):



*   [Andrew Verge](mailto:averge@chromium.org) https://github.com/WICG/fenced-frame/issues/230 


# Announcements

Privacy Sandbox folks are holding every-second-Wednesday meetings in the hour after this meeting to discuss the trusted server elements of Protected Audience work. For more information see https://github.com/WICG/protected-auction-services-discussion/issues/27.

The Microsoft Edge folks are holding every-second-Thursday meetings at this same hour to discuss their very similar "Ad Selection API" work.  See https://github.com/WICG/privacy-preserving-ads/issues/50 for logistics.

Everyone is responsible for checking the accuracy of the notes doc, especially the part with their own comments - so go back later and make sure the notes are accurate.  You can even fix them on Github later!


# Notes 


## Click coordinates inside Fenced Frames: https://github.com/WICG/fenced-frame/issues/230

Andrew Verge: Hi, I work on Privacy Sandbox and Fenced Frames.  Question for folks about Fenced Frames.  The well-lit path today for reporting from FF is the FFAR API.  Adding Fenced Storage Read API, useful for things like payment buttons and rendering last four of credit card number.  Adding NotifyEvent API.  Communicates from FF to embedding frame doc about click events.  Does not include information, only includes event happened information.  Discussing amongst themselves if PA FF usage needs to know X,Y coordinates of click.  Wanted to ask others if use cases where X,Y click in FF is useful.

Michael Kleber: when a PA ad shown in FF, then it’s already the case that the X,Y coordinates of the click might get included in the URL that is navigated to.  Andrew is asking if the surrounding publisher page needs to know the coordinates.

AV: Correct.  FFAR can include X,Y coordinates in network request.  This question relates to just the publisher page needing to know the coordinates.

MK: I don’t remember folks raising the need for 

Matt Davies: which coordinates?

MK: the coordinates of the click relative to the FF.  I don’t remember hearing a need for the coordinates.

Brian May: Possible need: A/B testing and fraud detection and user behavior analysis and interactive ads.  I’m not sure if any of those need to go to publisher page.

MK: Agreed on use cases for coordinators, but these all seem like cases where adtech or adtech working on behalf of advertiser, need to know coordinates, not publisher.

BM: Do adtechs get access or do they need help from publisher to get access?

MK: Adtechs should get direct access to click as they’re the one rendering ad.  From the PA point of view, the FF ad embedded on a publisher page is one site’s worth of information, while the publisher page is another site’s worth of information.  Publisher page and FF both don’t need to generally know about what goes on in the other side.  As long as everything to do with the ad interaction is ad-side information, then publisher probably doesn’t need to know.

BM: Can’t think of a use beyond Publisher wanting to know if click happened.

MK:  We don’t assume attendees represent everyone, but is a good place to provide feedback.  Please feel free to add feedback to linked issue.  Shivani or Andrew, might be worth creating issue on PA Github repo surfacing the FF issue, to give it more attention.  We encourage folks to chat on Github if they think of use case for click coordinates in publisher page.

All done for today.
