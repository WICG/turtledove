**The Protecte Audience API is deprecated and planned for removal from Chrome.  As a result, this repository will be archived and will no longer be updated.**

See our [Update on Plans for Privacy Sandbox Technologies](https://privacysandbox.com/news/update-on-plans-for-privacy-sandbox-technologies/).

[Privacy Sandbox feature status](https://privacysandbox.google.com/overview/status) provides more information about the status of individual APIs and platform features.

# TURTLEDOVE

Some online advertising has been based on showing an ad to a potentially-interested person who has previously interacted with the advertiser or ad network.
Historically this has worked by the advertiser recognizing a specific person as they browse across web sites, a core privacy concern with today's web.

The TURTLEDOVE effort is about offering a new API to address this use case while offering some key privacy advances:

*   The browser, not the advertiser, holds the information about what the advertiser thinks a person is interested in.
*   Advertisers can serve ads based on an interest, but cannot combine that interest with other information about the person — in particular, with who they are or what page they are visiting.
*   Web sites the person visits, and the ad networks those sites use, cannot learn about their visitors' ad interests.

**Chrome has been running [a FLEDGE Origin Trial since milestone 101 (March 2022)](https://groups.google.com/a/chromium.org/g/blink-dev/c/0VmMSsDWsFg/m/_0T5qleqCgAJ).  For details of the current design, see [the FLEDGE explainer](FLEDGE.md) or [the in progress FLEDGE specification](https://wicg.github.io/turtledove/).**

The FLEDGE design draws on many discussions and proposals published during 2020, most notably:
*  The [original TURTLEDOVE](Original-TURTLEDOVE.md) from Chrome.
*  [SPARROW](https://github.com/WICG/sparrow) from Criteo, which entered [WICG incubation jointly](https://discourse.wicg.io/t/advertising-to-interest-groups-without-tracking/4565) with TURTLEDOVE.
*  [Outcome-based TURTLEDOVE](OUTCOME_BASED.md) and [Product-level TURTLEDOVE](PRODUCT_LEVEL.md) from RTB House.
*  [Dovekey](https://github.com/google/ads-privacy/tree/master/proposals/dovekey) from Google Ads.
*  [PARRROT](https://github.com/prebid/identity-gatekeeper/blob/master/proposals/PARRROT.md) from Magnite.
*  [TERN](TERN.md) from NextRoll.

Many additional contributions came from Issues opened in this repo, and from discussion in the [W3C Web Advertising Business Group](https://github.com/w3c/web-advertising).
