# TURTLEDOVE

   * [Introduction](#introduction)
   * [Motivating Use Case](#motivating-use-case)
   * [API Example Flow](#api-example-flow)
   * [Design Elements](#design-elements)
     * [Browsers Joining Interest Groups](#browsers-joining-interest-groups)
     * [Two Uncorrelated Requests](#two-uncorrelated-requests)
     * [On-Device Auction](#on-device-auction)
     * [Rendering the Winning Ad](#rendering-the-winning-ad)
     * [Frequency Capping, Budgets, Metrics, and Reporting](#frequency-capping-budgets-metrics-and-reporting)
     * [User Interface Controls](#user-interface-controls)
   * [Incremental Adoption Path](#incremental-adoption-path)
   * [Suggested enhancements](#suggested-enhancements)
   * [Alternatives Considered](#alternatives-considered)
     * [PIGIN](#pigin)
     * [FLoC](#floc)

## Introduction

Some online advertising has been based on showing an ad to a potentially-interested person who has previously interacted with the advertiser or ad network.  Historically this has worked by the advertiser recognizing a specific person as they browse across web sites, a core privacy concern with today's web.

We propose a new API to address this use case while offering some key privacy advances:



*   The browser, not the advertiser, holds the information about what the advertiser thinks a person is interested in.
*   Advertisers can serve ads based on an interest, but cannot combine that interest with other information about the person — in particular, with who they are or what page they are visiting.
*   Web sites the person visits, and the ad networks those sites use, cannot learn about their visitors' ad interests.

This Explainer describes a mechanism to achieve this ad serving goal based on issuing Two Uncorrelated Requests, Then Locally-Executed Decision On Victory ("TURTLEDOVE").  At a high level, to show ads based on an advertiser-identified interest:



1.  The browser sends multiple uncorrelated ad requests:
    1.  a contextual ad request, which is allowed to contain the URL of the page where the ad will appear, and
    1.  a separate request indicating an advertiser-identified interest, but which cannot be tied to any browsing behavior, and which can happen at a time other than while loading the web page where the ad will appear.
1.  The ad network must make the final decision of which ad "wins" via code which executes locally within the browser, and which cannot send information off-device.  That is, we allow the ad network to run an on-device auction to choose among previously-served ads.

Shifting the interest data and the final ad decision browser-side instead of server-side lets us offer many advantages: strong privacy guarantees, as well as time limits on group membership, transparency into how the advertiser interest groups are built and used, and granular or global controls over this type of ad targeting.

At the same time, we are acutely aware that any form of on-device ad auction represents a substantial change from how the ad ecosystem works today.  Therefore this Explainer also describes a sequence of incremental steps which lead to this privacy goal in the end, with an eye towards being more easily adopted and digested one at a time.

The type of ad targeting we propose supporting can be of great value to people browsing the web, who often prefer ads for things they are interested in, and to advertisers, who wish to show their ads to people more likely to be interested in them.  That added value is a significant part of why publishers [earn much more money today when cookies are available](https://www.blog.google/products/ads/next-steps-transparency-choice-control/) for ad targeting ([more details](https://services.google.com/fh/files/misc/disabling_third-party_cookies_publisher_revenue.pdf)).  Therefore an API that supports this practice while offering strong privacy guarantees is an important part of creating a web in which publishers can flourish [without cross-site tracking](https://blog.chromium.org/2020/01/building-more-private-web-path-towards.html).


## Motivating Use Case

An "interest group" is a collection of people whom an advertiser or their ad network believes will be interested in seeing some type of ad. As people browse and interact with a web site, the site operator can add people to a number of interest groups. Advertisers then use those interest groups for ad targeting, especially to entice past visitors to return to their site. The advertising industry today uses a variety of terms to refer to variations on the idea of what we're calling an interest group, including "user list", "remarketing list", "custom audience", and "behavioral market segment".

As a concrete example, consider retailer WeReallyLikeShoes.com who sells shoes on the web. When a visitor first shows up at the web site, the visitor will be associated with a "WeReallyLikeShoes-shopper" interest group. As they view different sections of the site, they will be associated with the "WeReallyLikeShoes-athletic-shoes" or "WeReallyLikeShoes-dress-shoes" interest groups. When they view particular shoes, they will be associated with the "WeReallyLikeShoes-item-00123-viewer" interest group. Finally, purchasing the shoe or leaving the website will associate them with either the "WeReallyLikeShoes-buyer" group or "WeReallyLikeShoes-cart-abandoner" group.

Later, WeReallyLikeShoes.com wants to run an ad campaign for potential shoe buyers. They could use their interest groups for more precise targeting, such as offering a discount to members of their "cart-abandoner" group or advertising to their "athletic-shoes" group at the beginning of summer.

The goal of the API is to support this use case with the following outcomes:



*   People who like ads that remind them of sites they're interested in can keep seeing those sorts of ads.
*   People who don't like these types of ads can avoid seeing them.
*   People who wonder "how the ad knew" what they were interested in can get a clear, accurate answer.
*   People who wish to sever their association with the interest group can do so, and can expect to stop seeing ads targeting the group.
*   Advertisers cannot learn the browsing habits of any specific people, even ones who have joined multiple interest groups.
*   Web sites cannot learn the interest groups of any specific people who visit them.

All details of the UI would, of course, be up to the browser. The API provides the infrastructure that enables this ad campaign targeting capability while enforcing privacy rules that make privacy, transparency, and control possible.


## API Example Flow

This section describes one way the API might work end-to-end.  There are many open design choices; see the Design Elements section for explanations and discussion of the various steps.  The example here is a long-term goal, but see the Incremental Adoption Path section for possible milestones we could hit along the way.

Suppose I am a regular reader of myLocalNewspaper.com, which gets their ads from first-ad-network.com.

I visit WeReallyLikeShoes.com and spend some time looking at running shoes.  The advertiser web page decides to add me to an ad interest group for a month:


```
const myGroup = {'owner': 'www.wereallylikeshoes.com',
                 'name': 'athletic-shoes',
                 'readers': ['first-ad-network.com',
                             'second-ad-network.com']
                };
navigator.joinAdInterestGroup(myGroup, 30 * kSecsPerDay);
```


Since a relevant ad network is used by a web site I visit regularly, a while later my browser contacts first-ad-network.com and requests ads targeted at this interest group:


```
GET https://first-ad-network.com/.well-known/fetch-ads?interest_group=www.wereallylikeshoes.com_athletic-shoes
```


The response points at a [Web Bundle](https://wicg.github.io/webpackage/draft-yasskin-wpack-bundled-exchanges.html) holding an ad, along with some browser-intelligible metadata about when that ad can appear, and some ad network logic and data for later decision-making.


```
[{'group-owner': 'www.wereallylikeshoes.com',
  'group-name': 'athletic-shoes',
  'ad-cbor-url': 'https://first-ad-network.com/ads/ad-123456789.wbn',
  'cache-lifetime-secs': 21600,
  'max-times-per-minute': 1,
  'max-times-per-hour': 6,
  'auction-signals': {'atf_value': 250, 'btf_value': 25},
  'decision-logic-url': 'https://first-ad-network.com/js/on-device-bid.js'}]
```


The browser fetches the on-device bidding JS and the Web Bundle.  The bidding JS is very simple:


```
function(adSignals, contextualSignals) {
  return contextualSignals.is_above_the_fold ? 
           adSignals.atf_value : adSignals.btf_value;
}
```


A while later I visit myLocalNewspaper.com.  An ad network script on the page requests ads from first-ad-network.com.  The response requests an in-browser auction to pick between a contextually-targeted ad and the previously-fetched interest-group-targeted ad:


```
const contextualBid = 107;
const metadata = {'network': 'first-ad-network.com',
                  'auction-signals': {'is_above_the_fold': true}};
const rendered = navigator.renderInterestGroupAd(contextualBid, metadata);
if (!rendered) {
  ...render the contextually-targeted ad...
}
```


The ad network's on-device auction logic runs, and since the network believed the ad would render above the fold, it returns the atf-value 250.  The browser compares that to the contextual ad's bid of 107, finds that the contextual bid is lower, and renders the previously-downloaded web bundle.  The myLocalNewspaper.com page shows an ad for running shoes.


## Design Elements


### Browsers Joining Interest Groups

There is a straightforward JS API for an advertiser asking a browser to join a particular interest group for some amount of time.  TURTLEDOVE will then offer advertisers a way to show ads to the people in their interest groups without learning anything new about who the people are or what they are doing.


```
const myGroup = {'owner': 'www.wereallylikeshoes.com',
                 'name': 'athletic-shoes',
                 'readers': ['first-ad-network.com',
                             'second-ad-network.com']
                };
window.navigator.joinAdInterestGroup(myGroup, 30 * kSecsPerDay);
```


The API must be called from a window (top-level or iframe) whose origin matches the `owner`.  This could be on WeReallyLikeShoes.com, or could be a cross-domain iframe — maybe RunningShoeReviews.com writes articles about shoes sold by WeReallyLikeShoes.com, and the review site has an agreement which lets the retailer add people to an interest group with `'name': 'reads-reviews'`.  It should also be possible for a site owner to include a cross-domain iframe _without_ giving it this capability.

The list of `reader` domains indicates the ad networks that WeReallyLikeShoes uses to run their interest-group-targeted ads.  Those ad networks may work with publishers directly, or they may buy ad space from other publisher-affiliated ad platforms.  In the latter case, a URL like `https://first-ad-network.com/.well-known/ad-partners.txt` can list the domain names of other ad networks that first-ad-network buys space from, and a public key that the browser can use to encrypt the interest group information while it is passing through other ad networks.  _(Probably this should be a part of the [IAB ads.txt spec](https://iabtechlab.com/ads-txt/), instead of a new .well-known file; it's similar to their "authorized sellers" — and they can come up with a better name than "ad-partners" for the relationship.)_

There is a complementary API `window.navigator.leaveAdInterestGroup(myGroup)` which does what it says on the tin — WeReallyLikeShoes.com probably wants to take you out of its "cart-abandoner" group if you come back and buy the stuff.  Note that there is no way to ask what groups a person is in, so the semantics of `leaveAdInterestGroup()` are "If you're in the group, please leave; if you're already not in the group, do nothing," and there is no return value to indicate which of these just happened.  The value of `readers` should probably be ignored here.

Browsers could also choose to prevent micro-targeting.  This could be done by disallowing interest groups that are too small, e.g. communicating with some server-side privacy infrastructure to find out whether a group is large enough.  Or it could be done by disallowing ads that would be shown to too few people; this alternative is explored in [Outcome-based TURTLEDOVE](OUTCOME_BASED.md)   (There are also browser-local approaches to this problem, e.g. by limiting the permissible (owner, name) pairs, but it seems hard to make such approaches equitable across different scales of advertisers.)  Browsers and the industry should reach some consensus on whether there should be a minimum threshold for ad audiences, and on what size any such threshold should be.

Some formats of ads are commonly constructed out of smaller pieces, e.g. a carousel that displays a set of products.  This use case could be accomodated by the call to `joinAdInterestGroup` also including some metadata about the intended smaller pieces, which could then each be subject to whatever micro-targeting protection is in place.  This refinement is explored in [Product-level TURTLEDOVE](PRODUCT_LEVEL.md)

### Two Uncorrelated Requests

The TURTLEDOVE mechanism is triggered when the browser is in an interest group and loads a web page that gets its ads from a publisher ad network that is one of the group's `reader` domains or one of their `ad-partners`.  Its goal is to fetch ads targeted at the interest group, without leaking any information about the person doing the browsing.

Ads are fetched via two requests:



*   _A contextual request_: The web page's normal ad request to the publisher's ad network is constructed and sent as usual.  It can contain information about the context in which the ad will appear (e.g. the page URL, ad size and location, etc) and any first-party targeting information.  It is not affected by the browser's interest group memberships.

*   _An interest-group request_: An additional ad request, of a new and different type, is constructed by the browser and sent to the same publisher ad network.  This is an interest-groups-only ad request: it does not contain any information about any web page or about the person visiting it.  Instead it contains information about a small collection of interest-group (owner, name) pairs to target.  If the publisher's ad network is not a reader for the interest group but is an ad-partner of a reader, then the browser encrypts the (owner, name) pairs with the reader's public key, and only includes the reader domain's name in the clear.

It is the browser's responsibility to keep these two ad requests independent and uncorrelated — that is, to not let any ad network know that these two requests are for the same person.

To prevent timing attacks, the interest-group request can be sent at a different time, and the interest-group-targeted ads in the response can be cached for later use (including use multiple times).  In particular, the interest-group request could be sent in advance of the browser navigating to the web page.  For example, a browser might learn over time which ad networks it regularly encounters, and a few times a day issue any relevant interest-group ad requests to those networks.  Sending requests periodically also offers a chance for advertisers to rotate or update the ad being shown to any interest group.

To prevent correlating requests using IP address, browsers could only allow TURTLEDOVE for ad networks using some IP blinding technique ([Willful IP Blindness](https://github.com/bslassey/ip-blindness), Private Information Retrieval, VPN0, etc).

Including multiple interest groups in a single request introduces another opportunity for micro-targeting (even if each individual interest group is sufficiently large).  This could be prevented by sending multiple single-interest-group requests, but it would be good to explore options for reducing the total traffic.  For example, [batched multi-query PIR](https://eprint.iacr.org/2019/1483.pdf) offers relevant technology.


### On-Device Auction

The "Locally-Executed Decision" of which ad to show must take place inside the browser.  It can combine information sent back with the contextual and interest-group responses.

The simplest possible on-device decision making is: Each of the responses includes an ad and a price; the on-device auction is a single if() statement choosing the higher-priced ad.  This has two basic weaknesses.  First, advertisers may be unwilling to show their ads on certain pages — recall that the interest-group request does not contain any information about the site or page being visited.  Second, the "correct price" for the interest-group-targeted ad may vary based on the surrounding page or on first-party data.

To address these needs, TURTLEDOVE allows for locally-executed decisions that combine logic returned in the interest-group response with signals returned in the contextual response.  Specifically:



1.  The contextual response includes:
    1.  An ad;
    1.  A positive number bid, where larger bid = ad is more likely to show, but whose meaning is otherwise up to the ad network;
    1.  An object containing arbitrary "contextual signals" for the local bidding logic. 
1.  The interest-group response includes:
    1.  An ad;
    1.  Some metadata to control how the ad may be used multiple times, e.g. a cache lifetime (after which new ads could be requested) and a frequency cap;
    1.  An object containing arbitrary "ad signals" for the local bidding logic;
    1.  A JS bidding function (probably cacheable / re-used across ads), which:
        1.  takes two arguments: the ad-specific signals delivered with this response and the context-specific signals delivered with the contextual response;
        1.  runs purely locally (e.g. no access to network or other external state);
        1.  returns a bid that can be compared with the contextual response bid.


This model can support the use case where an advertiser is unwilling to run their ad on pages about a certain topic, for example: the contextual response signals could include the ad network's analysis of the topics on the page; the interest-group response could contain a signal that is a block-list of disallowed topics; and the JS bidding function could include logic to compare those two signals and in case of a match return a negative value, guaranteeing the interest-group-response ad would not show.

At one extreme, if the JS bidding function returns a constant bid, this reduces to the "simplest possible on-device" case above.  Slightly more complex: the contextual signals just indicate the dimensions of the ad slot, and the JS function returns a constant bid if the interest-group ad can render at that size, and -1 if not.  At the other extreme, the JS bidding function could evaluate an arbitrary ML model, e.g. to predict the conversion rate of the interest-group ad, and bid accordingly.  Ad networks will doubtless want to experiment with the in-browser logic, which is well supported in this paradigm: the contextual signals could include a server-selected experiment ID that picks the behavior of the previously-served in-browser logic.

In the case where the interest-group response came from some other ad network that buys through the publisher's ad network, the JS bidding function in the interest-group response will need to consume signals on the page or returned with the contextual ad response from the publisher's ad network.  Therefore, the other ad network must either (1) send a bidding function that consumes publisher ad network signals, or else (2) provide its own signals, communicated to the browser directly or through some server-to-server integration with the publisher or the publisher's ad network.

The contextual request/response need not be implemented as its own HTTP request.  For example, some web pages might have the contextual part of TURTLEDOVE happen server-side, with the winning ad served along with the publisher page.  To allow for this flow, we should provide JS APIs to trigger the in-browser auction and provide the contextual signals object, irrespective of surrounding HTTP traffic.

As a possible extension to this idea, the contextual response might _also_ contain a JS bidding function, rather than a fixed bid.  This may be useful if there are any other user-specific signals that are available in the browser but not in the server, which could then be made available to the various on-device bidding functions without compromising privacy.

_Addendum:_ The above description focused on each ad producing a _bid_, and assumed a simple auction in which the winner was the largest bid.  This is probably too simplistic, and instead we will need a mechanism in which (1) each ad can run on-device JS to produce a bid, and then (2) some on-device JS provided by the publisher or their ad network chooses the winner.  This idea is explored more in the [PARROT](https://github.com/prebid/identity-gatekeeper/blob/master/proposals/PARRROT.md) and [TERN](https://github.com/WICG/turtledove/blob/master/TERN.md#6-the-auction) proposals.


### Rendering the Winning Ad

Once the winning ad is chosen, it needs to render in the browser.  If the winning ad is targeted at an interest group, it implicitly knows about the browser's group membership, and so the ad should be rendered in a privacy-preserving way to avoid leaking information.

There is room for a lot of variation in the approach here, and what seems reasonable depends on your bad-actor threat model.

One possible answer: If the winning ad is interest-group targeted, then the browser renders it inside some sort of new environment, an "opaque iframe", which does not allow information exchange with the surrounding page: no `postMessage`, no way to crawl the window tree using `window.parent` or `window.frames[]`, etc.  This does still let the surrounding publisher page learn that _some_ interest-group-targeted ad won.  To avoid leaking even that bit, we could render even the contextual ad in this opaque manner as well, if a cost-benefit discussion shows this is worthwhile.

Aside from communication with the publisher page, we may need to worry about network communication with the outside world, since a timing attack could attempt to join the contextual ad request with the subsequent interest-group ad render.  This could be solved by requiring the interest-group ad (or both ads) to be a Web Bundle that renders entirely locally, without any additional network activity (until the ad is clicked).


### Frequency Capping, Budgets, Metrics, and Reporting

Repeated use of an ad, and the temporal delay between ad serving and the local decision, both pose challenges for ad networks' pacing and budgeting operations.  These and the "opaque iframe" blind rendering ideas also present challenges for the measurement and reporting operations of the ads ecosystem — including the essential operation of knowing how much money each advertiser pays and each publisher receives.

The proposed [Aggregate Reporting API](https://github.com/csharrison/aggregate-reporting-api) is the solution to many of these use cases.  This API would allow an ad network to learn aggregated information about all impressions from a particular advertiser, or about all impressions on a particular publisher's site, without revealing information about any particular impression.

We understand that this will not be an easy adjustment.  In particular, the differential privacy goals of aggregate reporting mean that the API's results will include some noise, so metrics that today can be measured exactly will be replaced with aggregate estimators that include error bars.

Similarly, budgeting precision for interest-group-targeted ad campaigns will suffer when the interest-group requests happen only a few times per day.  Since interest-group-targeted ads tend to be relatively valuable to advertisers, we expect this loss of budget precision will be a cost worth paying.


### User Interface Controls

In this model, the browser is in control of interest group membership, and has full insight into what interest group a particular ad targeted.  This API means browsers can offer people a UI that provides insight into why they saw those ads, what interest groups they are on, and how they got there, as well as control over both past and future group memberships.  Some ideas for controls that a browser might choose to offer include:



*   "What targeted ads have I seen, and why?" — Show pictures of all ads that have appeared on screen and were targeted at interest groups, with the ability to dig into why.
*   "What interest groups am I in?" — Show the owners, names, and expiration dates of interest groups, along with pictures of the ads for any that have appeared in the browser.
*   "How did I get on this list?" — The display of any interest group should include the advertiser's domain name, and also the day and domain of the top-level page the browser was visiting when it joined that group.
*   "Remove me from this list" — Tell the browser to leave any or all interest groups.
*   "Block this advertiser" or "Block this site" — The browser could leave all interest groups owned by WeReallyLikeShoes.com or all interest groups joined while on RunningShoeReviews.com, and disallow such list additions in the future.
*   "Disable Ad Interest Groups" — This control is analogous to disabling 3rd-party cookies today, but without the disadvantage of breaking other unrelated use cases.


## Incremental Adoption Path

Pieces of the full design can be rolled out over time, with incremental benefits along the way.  For example:



1.  Minimum Viable Product: Browser maintains interest group memberships, issues two TURTLEDOVE requests simultaneously, and runs the in-browser logic to decide which of the two responses to render.  It renders the winning ad with no new protections. \
\
There are still lots of tracking work-arounds at this stage: Ad networks can still correlate requests using timing attacks, and the publisher's ad network can learn about which interest group the winning ad was targeted at if the advertiser's ad network cooperates with them.  But budgets and reporting don't have to change, and we can already support interest group targeting [without the need for third-party cookies](https://blog.chromium.org/2020/01/building-more-private-web-path-towards.html).

1.  Require support for caching and ad re-use for the interest-group ad request.  Ad networks need to learn to handle an ad served once and rendered multiple times, but still get event-level reporting.

1.  Browsers predictively issue interest-group ad requests before the pageload.  Nothing new required from the ad networks.  Breaks timing-based attacks on ad requests.  Still possible to learn about a person's interest group membership, but only when an ad targeting that interest group wins the auction and renders in the browser.

1.  Blind rendering in opaque iframes.  This is hard because it requires ads that can render without network access, and also requires the switch to aggregate reporting.  We could ease this by placing limits on the format of interest-group-targeted ads at the outset — e.g. require ads that can be automatically converted to Web Bundles (e.g. AMPHTML maybe?). \
\
But adapting to aggregate reporting will require a big change from the ad industry, no two ways about it.

An even more gentle start could begin with a "step 0" where the browser issues the two TURTLEDOVE request at pageload time, one after the other: first send the browser-constructed interest-group request, then run the in-browser auction among the interest-group responses, and then _send the highest bid along with the ad-network-constructed contextual ad request_.  This serial variant carries more latency cost, and the in-browser auction cannot receive contextual signals from the server.  But in exchange it allows a server-side contextual auction to remain certain that the ad it picks "really is the winner".  We should have a conversation with the advertising community about this or any other ways to make the adoption path more accessible.

## Suggested Enhancements
- [Product-level TURTLEDOVE](PRODUCT_LEVEL.md)
- [Outcome-based TURTLEDOVE](PRODUCT_LEVEL.md)
- [TERN](TERN.md)


## Alternatives Considered


### PIGIN

A previous Explainer proposed the [PIGIN API](https://github.com/michaelkleber/pigin) to meet this use case, and indeed some of that explainer is re-used here.  In that proposal, the browser was in control of interest group membership (as in this one), but a small amount of interest group information was sent along with the regular contextual ad request (and there was no in-browser auction).

Feedback from the W3C Web-Advertising Business Group and Privacy Interest Group and from the browser and privacy research communities highlighted weaknesses in that design.  The ability to associate an advertiser's interest group assignment with a person's first-party identity on a publisher's site was considered too high of a privacy leak even with that Explainer's proposed mitigations.  Additionally, analysis of real-world userlist memberships indicated that even the proposed "small amount of interest group information" would consume too much of a page's [Privacy Budget](https://github.com/bslassey/privacy-budget).

TURTLEDOVE therefore replaces the PIGIN proposal.  The addition of an in-browser auction is a way to address both of these privacy weaknesses, and discussions with participants in the ads ecosystem have made it seem more plausible that an in-browser auction component could be adopted.


### FLoC

The [FLoC proposal](https://github.com/jkarlin/floc) offers a different approach to ad targeting, focused on people's general interests rather than on specific actions.

In the FLoC model, advertisers and ad networks have no control over what groups people are in.  Instead the browser itself is responsible for somehow grouping together flocks of "similar people", with wide latitude in how it comes up with its notion of similarity.  The browser can then take steps to prevent its grouping from allowing tracking or revealing sensitive characteristics.

The current FLoC proposal involves sending the browser's flock assignment to a server and letting it influence the server-side ad auction.  In a world with a TURTLEDOVE in-browser auction, one could imagine making FLoC an in-browser-only signal as well, fetching ads for the flock in yet a third uncorrelated request ("ThURTLEDOVE"?).  Given how hard even the incremental adoption path in this path is already, keeping FLoC as an independent effort for now seems prudent.
