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

Fenced frames can invoke the `reportEvent` API to tell the browser to send a beacon with event data to a URL registered by the worklet in `registerAdBeacon` (see below). Depending on the declared `destination`, the beacon is sent to either the buyer's or the seller's registered URL. Examples of such events are mouse hovers, clicks (which may or may not lead to navigation e.g. video player control element clicks), etc.

This API is available from same-origin frames within the initial rendered ad document and across subsequent same-origin navigations, but it's no longer available after cross-origin navigations or in cross-origin subframes. (For this API, for chains of redirects, the requestor is considered same-origin with the target only if it is same-origin with all redirect URLs in the chain.) This way, the ad may redirect itself without losing access to reporting, but other sites can't send spurious reports.

The browser processes the beacon by sending an HTTP POST request, like the existing [navigator.sendBeacon](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/sendBeacon).


### Parameters

**Event type and data:** Includes the event type and data associated with an event. When an event type e.g. click matches to the event type registered in registerAdBeacon, the data will be used by the browser as the data sent in the beacon sent to the registered URL.

**Destination type:** List of values to determine whose registered beacons are reported, can be a combination of 'buyer', 'seller', or 'component-seller'.


### Example


```
window.fence.reportEvent({
  'eventType': 'click',
  'eventData': JSON.stringify({'clickX': '123', 'clickY': '456'}),
  'destination':['buyer', 'seller']
});
```

```
window.fence.reportEvent({
  'eventType': 'click',
  'eventData': 'an example string',
  'destination':['component-seller']
});
```

Note `window.fence` here is a new namepsace for APIs that are only available from within a fenced frame. 

## registerAdBeacon

A similar API was initially discussed here: https://github.com/WICG/turtledove/issues/99 for reporting clicks. The idea is that the buyer and seller side worklets are able to register a URL with the browser in their reportWin and reportResult APIs. A beacon will be sent to the registered URL when events are reported by the fenced frame via reportEvent.


### Parameters

A map from event type to reporting URL, where the event type corresponds to the `eventType` value passed to  `reportEvent()`. The event type enables worklets (for buyer, seller, or component seller) to register distinct reporting URLs for different event types. The reporting URL is the location where a beacon is sent once the fenced frame delivers the corresponding event via `reportEvent()`.

Worklets can add a buyer event identifier, seller event identifier, or any other relevant information as query parameters to this URL.

### Example


```
registerAdBeacon({
 'click': 'https://adtech.example/click?buyer_event_id=123',
});
```

## Support in component ad fenced frames

The issue [here](https://github.com/WICG/turtledove/issues/332) points out a reasonable use case where the user clicks on a component ad and the ad-tech server would want to be notified of that via reportEvent mechanism since a click on a nested component ad is a significant enough event.  

However reportEvent() only works if there is an associated registerAdBeacon() call and that metadata is only associated with the root fenced frame (the parent ad). If we were to also associate that metadata with the nested ads then that would imply that the server can join arbitrary data from the component ads and the parent ad,  which is not a desired FLEDGE information flow (more details below). Note that reportEvent() can be called anytime and with arbitrary data and is not gated on a user click/gesture.


### Related issue : event level attribution reporting

A related issue is how a [click in the component ad can be used for attribution reporting.](https://github.com/WICG/turtledove/issues/281#issuecomment-1225746871)

To allow event level attribution reporting, the mechanism used by ad-tech is that reportEvent() from the parent FF carries a generated id which is also associated with the click based navigation and that helps join the 2 at the server side and attribute the navigation to the specific ad slot (fenced frame). 

This can be fixed if reportEvent() were allowed from the component fenced frame. It can then create its own id and that can also be joined at the server side to the already joined <id-in-registerAdBeacon, id-in-parent-fenced-frame> since this reportEvent will be resulting in a ping to the same reporting url as the parent fenced frame.


### Privacy impact of reportEvent from component ad

registerAdBeacon is only allowed from [reportWin](https://github.com/WICG/turtledove/blob/main/FLEDGE.md#52-buyer-reporting-on-render-and-ad-events) or [reportResult](https://github.com/WICG/turtledove/blob/main/FLEDGE.md#51-seller-reporting-on-render), neither of which have the component ads information, so the combined event level reporting from reportWin, reportResult and FFAR does not allow a join of the main ad and components ads.

If reportEvent() was allowed from a component ad, the combination of main ad and component ads would now be known at the server which goes against the privacy assumption where each of the main and component urls are k-anonymous and their join would not necessarily be k-anonymous.


### Fix and privacy impact

The fix could be to allow reportEvent from the component fenced frame but only if it received a click/transient user activation. To further reduce the amount of information leaked we will restrict it to at most one such reportEvent among all the component ads fenced frames for a given parent fenced frame.

Privacy-wise this can lead to at most 2 k-anonymous urls to be joined (that of the parent and the component that was clicked) but that seems acceptable as this additional information is gated on user activation on that component ad. 

Utility wise it will fix the attribution reporting as well as click reporting issues mentioned above and the max of one event should be sufficient to serve most use cases since a click will likely lead to a navigation.


### Alternatives considered

An alternative could be to propagate the click event from component ad fenced frames to the parent fenced frame. Since it will be gated on user activation and does not carry arbitrary information, it is ok to have this limited  communication channel between the nested component FF and its parent. However see below for concerns with this approach:

**Concerns:**
*   On the web platform click events don’t get bubbled across the fenced frame boundary as well as across cross-origin iframe boundary while such an approach will require to diverge from the web platform norm around cross-origin iframes.
*   The other concern in this approach is possible complexity around the ordering of the parent fenced frame receiving the click event and calling reportEvent while the click might lead to the navigation of the top-level page.

Because of the concerns above, instead of bubbling a click event, another approach could be adding an API which only works for component ads, as below:

window.fence.reportClick()

This API when invoked from the component ad fenced frame will let the browser know to invoke the same functionality as if window.fence.reportEvent(‘click’) was invoked from the parent frame. 

The important difference between this and a reportEvent call is that there is no arbitrary data and arbitrary values of event type allowed. This however does not fix the attribution reporting issue mentioned above because there is no associated id in the reportEvent ping and the navigation resulting from the click. 
