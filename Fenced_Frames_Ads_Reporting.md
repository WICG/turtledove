# Objective
  
As part of FLEDGE, interest based ads are [rendered in fenced frames](https://github.com/WICG/turtledove/blob/main/FLEDGE.md#4-browsers-render-the-winning-ad) which are special embedded frames that do not have any communication channels with the publisher page, unlike iframes. Since fenced frames do not have any contextual information, part of the reporting that ads require needs to be done via separate JS contexts called [reporting worklets](https://github.com/WICG/turtledove/blob/main/FLEDGE.md#5-event-level-reporting-for-now). The other part of the reporting which is based on user behavior in relation to the rendered ad comes from the fenced frame. There is thus a requirement from ads infrastructure to be able to correlate these two parts of reporting. Note that this correlation is required only for [event-level reporting](https://github.com/WICG/turtledove/blob/main/FLEDGE.md#5-event-level-reporting-for-now).

For invalid ad traffic detection, signals are collected about the publisher, the browser and the user behavior on the ad. Out of these three categories, signals about the publisher are available in the reporting worklet while those about the browser and user behavior are available inside the fenced frame. We therefore need a channel for signals within the fenced frame to be communicated to the reporting worklet.

Seller ad-tech reports the auction result via [reportResult()](https://github.com/WICG/turtledove/blob/main/FLEDGE.md#51-seller-reporting-on-render) and the buyer reports it via [reportWin()](https://github.com/WICG/turtledove/blob/main/FLEDGE.md#52-buyer-reporting-on-render-and-ad-events). These are implemented in the seller’s and buyer’s reporting worklets, respectively. These are already correlated with the contextual ad request but additionally, they need to be correlated with the events related to the fenced frame e.g. impressions, interactions and clicks. We therefore need a channel for events within the fenced frame to be correlated to the information in the reporting worklet and sent out on the network to support event-level reporting. In the long-term, these events could only be exfiltrated using an aggregate report. 

An ID provided to runAdAuction to identify a contextual query/ad cannot be passed on to the fenced frame because the reporting worklet has publisher information and that could be encoded and passed to the fenced frame defeating the [fenced frame’s privacy guarantees](https://github.com/shivanigithub/fenced-frame#goals). Therefore the higher level idea is for the browser to take the signals provided by the fenced frame and correlate it with the ID passed to it by the reporting worklet and send out the correlated data to the ad-tech server.

From a privacy perspective, it is also important to note that the additional information (from what the worklet already knows) that the fenced frame is sending to the browser and eventually to the ad-tech server, cannot contribute to identifying the user since the fenced frame does not have access to cookies/unpartitioned storage.


# Design

The following summarizes the sequence of events for the buyer and seller. Distinguishing these flows here, since in principle, one should be able to report without the help of the other.

![high level diagram](assets/fenced_frames_reporting.png)


## Seller (SSP) flow of events



1. SSP will generate an event identifier, say sellerEventId, at contextual ad request time.
2. SSP will provide the sellerEventId to the auction via auctionConfig, which will be available in the reporting worklet for the SSP.
3. In reportResult(), the sellerEventId, along with any other contextual response fields relevant for reporting, will be available so that the result can be joined to the corresponding contextual query.
4. Once the winning ad is rendered in the fenced frame, the fenced frame can also communicate other events to the browser e.g. what user interaction happened. Since the fenced frames run the buyer’s scripts, the buyer can also decide what information to be volunteered to the seller via the reportEvent API.
5. The solution proposed in this document allows the seller’s reporting worklet to register a url to which data should be sent for events reported by the fenced frame.

## Buyer (DSP) flow of events



1. If the DSP participates in the contextual ad request, it will generate an event identifier, say buyerEventId, at contextual ad request/response time which is returned to the SSP as part of the perBuyerSignals, which the SSP in turn would specify in navigator.runAdAuction call. For DSPs that do not participate, buyerEventId could just be an ID that the worklet creates.
2. Browser will provide the buyerEventId to the reporting worklet for the DSP via reportWin() as part of the perBuyerSignals. This enables the result to be joined with the corresponding contextual query. 
3. Fenced frame can also communicate other events to the browser e.g. a click happened along with click coordinates.  
4. The solution proposed in this document allows the buyer’s reporting worklet to register a url in reportWin(), to which data should be sent, when events happen in the fenced frame.


# APIs   

The following new APIs will be added for achieving this.


## reportEvent

Fenced frame can invoke this API for the browser to be able to send a beacon with the data in this API, to the URL registered by the worklet in registerAdBeacon (see below). Depending on the ‘destination type’ present here, the beacon will be sent to either the buyer’s or the seller’s registered URL. Examples of such events are mouse hovers, clicks that may or may not lead to navigation e.g. video player control element clicks etc.

Browser will process this similar to how the existing [navigator.sendBeacon](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/sendBeacon) works via sending an HTTP POST request.


### Parameters

**Event type and data:** Includes the event type and data associated with an event. When an event type e.g. click matches to the event type registered in registerAdBeacon, the data will be used by the browser as the data sent in the beacon sent to the registered URL.

**Destination type:** List of values to determine whether this event needs to be reported to both buyer and seller, only buyer or only seller.


### Example


```
navigator.reportEvent({
  'eventType': 'click',
  'eventData': {'clickX': '123', 'clickY': '456'},
  'destination':['buyer', 'seller']
});
```

## registerAdBeacon

A similar API was initially discussed here: https://github.com/WICG/turtledove/issues/99 for reporting clicks. The idea is that the buyer and seller side worklets are able to register a URL with the browser in their reportWin and reportResult APIs. A beacon will be sent to the registered URL when events are reported by the fenced frame via reportEvent.


### Parameters

**Event type:** E.g. click. This corresponds to the event type in reportEvent. This is for enabling buyers/ sellers to register distinct beacon URLs for different event types.

**URL:** The URL to which a beacon will be sent by the browser on receiving events from the fenced frame via reportEvent
The worklet is able to add the buyerEventId/sellerEventId and any other relevant information via this API.


### Example


```
navigator.registerAdBeacon({
 'click': {'url': 'https://adtech.example/click?buyer_event_id=123'},
});
```


