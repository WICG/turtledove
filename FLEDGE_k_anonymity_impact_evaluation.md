# Evaluating k-anonymity impact

# Purpose

This document aims to explain the method for evaluating the impact of k-anonymity on utility. The impact of k-anonymity on latency and cost is not considered in this document.

# The Challenge

K-anonymity requires that a minimum number of devices (`k`) have reported a request to show a particular ad (more precisely, a render URL) within a time window (`w`) before that ad is eligible to be shown. This helps to protect the privacy of individuals by reducing the risk that an ad impression can be linked to them specifically. In a nutshell, estimating the impact of the K-anonymity Server requires computing the fraction of revenue that comes from ads that have been shown to &lt; k users in the window of time<sup>1</sup>.

Although some ads may not pass the k-anonymity check, we do not expect their potential revenue to be fully lost. This is because the ads can be replaced by k-anonymous ads that may have bid slightly less. The Chrome Protected Audience API process is to have a second auction if the winning ad was not k-anonymous. In the second auction, only k-anonymous ads participate, and another remarketing ad may win, perhaps older or more generic with more wins, or a contextual ad.  So this analysis, in which we treat each ad that isn't k-anonymous as if it yields zero revenue, is surely far too pessimistic.

It is difficult to directly measure the impact of the k-anonymity requirement using a standard A/B testing methodology, because experiments on just a small fraction of the eventual traffic will result in just a small fraction of the true number of ad impression opportunities.

The following are three key factors outside the direct control of Protected Audience that reduce the likelihood of a particular ad winning a programmatic auction.

*   Traffic where third-party cookies are still available
*   3P-cookieless traffic where the publisher's SSP chooses not to run Protected Audience auction
*   Protected Audience auctions where the ad's DSP chooses not to bid

Notice that, by definition, for traffic that is not routed to the Protected Audience, the K-anonymity Server can’t receive a reported `Join` for a given ad. Hence, from the server output, it is only possible to know whether the fraction of traffic currently routed to the system exceeds the threshold in the given period of time. This selection is made by the browser and therefore correlated with the number of devices that can report the ad to the K-anonymity Server. In addition, SSPs may choose not to call Protected Audience on all cookieless traffic, which will further reduce the amount of traffic that has reported `Join`s. This reduction may not be proportional, as it can be done based on the query and not per-browser or per first-party identifier. 

Each of these factors, not all under the control of the Privacy Sandbox, affects the final fraction of traffic that can report a `Join` to the K-anonymity server.

Unfortunately, if any combination of these factors result in a small fraction of traffic routed to Protected Audience, it is difficult to obtain a statistically sound estimate of the impact of the k-anonymity check using only the output of the server. 

Since this situation is expected to last during the ramp-up of the Privacy Sandbox, this document describes an alternative approach to estimate the utility impact of the k-anonymity check. This approach has been used at Google, and we suggest all adtechs adopt it while the traffic to the Protect Audience API is limited. 


# Suggested method for utility impact analysis

During the period in which the fraction of traffic to the K-anonymity Server is limited, we suggest using historical data (outside of the experiment traffic). In this section, we describe how this can be done. 

In our methodology, we used internal ads logs, containing records with a user id, an ad id, a timestamp, and revenue for the ad.

For a given ad, we extract an ad id to be used as a proxy for the `renderURL` that will be used in the Protected Audience API. For each impression, we check whether at least `k` distinct users have reported that ad id in the preceding 168 hours (7 days) before the impression. If at least `k` distinct users have reported the ad id, we consider the revenue of the event preserved (and otherwise we consider that revenue lost). Finally, we measure what fraction of ad revenue was preserved over a period of 7 days.

Notice that this analysis neglects the effect of the [AboveThreshold With Period Restarts](https://github.com/WICG/turtledove/blob/main/FLEDGE_k_anonymity_differential_privacy.md) differential privacy (DP) algorithm, which introduces noise in the threshold (`k`) of users needed to pass the anonymity bar. We observe, however, that the impact of the DP algorithm is negligible in terms of revenue preservation (a simulation of the effect of DP noise is reported [here](https://github.com/WICG/turtledove/blob/main/FLEDGE_k_anonymity_server.md)). 

### Methodology shortcomings:

1. This estimate is based on a specific period of data (e.g., one week). Numbers may vary in different periods of time, depending for instance on seasonality.
2. This analysis ignores the effect of delays in reporting a `Join` to the server, or for receiving the k-anonymous status of the `renderURL` from the server. We don’t expect this impact to be significant for advertising campaigns lasting multiple days.
3. The proxy for the future Protected Audience API’s `renderURLs` used in the analysis is imperfect,  For instance, `renderURLs` could be optimized (e.g., combining small audiences) to limit the utility impact. We predict that this type of optimization will result in k-anonymity causing less loss.
4. We do not distinguish between segments of traffic in the analysis. It is possible that some segments that are highly personalized (e.g., dynamic programmatic ads) may have higher utility impact . This is expected given the purpose of the K-anonymity Server to prevent ads that can lead to tracking.

# Privacy impact

Privacy impact is harder to estimate with a single number. The privacy goals of the Protected Audience API are multifaceted and affected by the overall architecture. We observe that there are two main privacy knobs (excluding the [DP parameters](https://github.com/WICG/turtledove/blob/main/FLEDGE_k_anonymity_differential_privacy.md), which are not covered in this document): the `k` anonymity parameter and `w` window length parameter. 

The `k` parameter has a stronger impact on privacy as it is the main protection offered by the K-anonymity Server. Lower `k` makes attacking the server with k-stuffing easier (showing an ad to `k` devices, with 1 device is targeted, and `k-1` devices are shown an ad only to stuff k-anonymity). In fact, observe that halving the `k` parameter means that an attacker needs to use half as many fake `Join` reports from distinct identities to achieve k-anonymity of the URL. The need to use fewer spam identities to attack a given URL increases the likelihood of abuse and the difficulty of detecting it.

On the other hand, relaxing the `w` parameter mostly affects the amount of time that a k-stuffed URL is considered k-anonymous but does not reduce the number of entities needed to compromise the k-anonymity of the URL.

<hr/>
<sup>1</sup> 

The DP [algorithm](https://github.com/WICG/turtledove/blob/main/FLEDGE_k_anonymity_differential_privacy.md) introduces noise and a delayed reset of the K-anonymity status,  however in this document, for simplicity, we ignore the impact of these complications, as we observe that the DP Algorithm differs marginally in utility estimations from the ground truth algorithm (i.e., using the exact counts).
