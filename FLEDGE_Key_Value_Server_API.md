# FLEDGE Key/Value Server APIs Explainer

## Summary

[FLEDGE](https://github.com/WICG/turtledove/blob/main/FLEDGE.md) provides a
privacy advancing API to facilitate interest group based advertising. Trusted
servers in FLEDGE are used to add real-time signals into ad selection for both
buyers and sellers. The FLEDGE proposal specifies that these trusted servers
should provide basic key-value lookups to facilitate fetching these signals but
do no event-level logging or have other side effects.

This explainer proposes APIs for trusted servers. Future explainers will cover
trusted servers in more detail, including threat models, trust models, and
system design.

The APIs presented here are a proposal for the initial version “v1” of these
servers.

## Concepts

### Namespace

Keys may exist in different namespaces. The namespaces help the server identify
keys needed from request query strings and prevent potential key collision, even
though the keys may be unique across namespaces in today’s use cases.

*   For a DSP, there is only one namespace: `keys`.
*   For an SSP, there are `renderUrls` and `adComponentRenderUrls`.

The
[FLEDGE explainer](https://github.com/WICG/turtledove/blob/main/FLEDGE.md#31-fetching-real-time-data-from-a-trusted-server)
provides more context about these namespaces.

### Mode

The server can be configured to run in slightly different modes depending on
whether it is serving the DSP use case or the SSP use case.

### Subkey

For a given key, a subkey may be used to further specify a dedicated value
override.

During the query, the browser sets the hostname as the subkey value. When a
query to a particular subkey does not match any existing entry, the server
system can automatically fallback to a default value for the key, specified by
not setting the subkey during data updates.

## Read API

### Description

This is the mechanism for the browser client to fetch real-time bidding signals.
The API is called during the ad auction process, as described in the
[FLEDGE explainer](https://github.com/WICG/turtledove/blob/main/FLEDGE.md#31-fetching-real-time-data-from-a-trusted-server).

The returned values are purely dependent on the keys (namespace + key + subkey),
except for advanced use cases explicitly agreed upon between browsers and ad
tech platforms. A potential advanced use case being discussed is how to provide
country-level IPGeo information to the bidders. The API provides read-only
access to the key/value data.

As mentioned in the Mutating API section below, possible data staleness may
occur. Different values may be returned for the same keys if reads happen during
data updates, due to the distributed nature of the system. But if the data is
stable, requests are deterministic.

### Form

GET `https://www.kv-server.example/v1/getvalues`

### Examples

```
https://www.dsp-kv-server.example/v1/getvalues?subkey=publisher.com&keys=key1,key2
https://www.ssp-kv-server.example/v1/getvalues?renderUrls=url1,url2&adComponentRenderUrls=url3,url4
```

### Query parameters

<table>
  <tr>
   <td style="background-color: #cfe2f3">Field
   </td>
   <td style="background-color: #cfe2f3">Value Description
   </td>
   <td style="background-color: #cfe2f3">Mode
   </td>
   <td style="background-color: #cfe2f3">Original <a href="https://github.com/WICG/turtledove/blob/main/FLEDGE.md#31-fetching-real-time-data-from-a-trusted-server">FLEDGE explainer</a> description
   </td>
   <td style="background-color: #cfe2f3">Required
   </td>
   <td style="background-color: #cfe2f3">Type
   </td>
  </tr>
  <tr>
   <td>keys
   </td>
   <td>List of keys to query values for, under the namespace <code>keys</code>.
   </td>
   <td>DSP
   </td>
   <td><em>“keys” is a list of trustedBiddingSignalsKeys strings, perhaps coalesced (for efficiency) across any number of interest groups that share a trustedBiddingSignalsUrl.</em>
   </td>
   <td>Required
   </td>
   <td>List of strings
   </td>
  </tr>
  <tr>
   <td>renderUrls
   </td>
   <td>List of keys to query values for, under the namespace <code>renderUrls</code>.
   </td>
   <td>SSP
   </td>
   <td rowspan="2"><em>Similarly, sellers may want to fetch information about a specific creative, e.g. the results of some out-of-band ad scanning system. This works in much the same way, with the base URL coming from the trustedScoringSignalsUrl property of the seller's auction configuration object. However, it has two sets of keys: "renderUrls=url1,url2,..." and "adComponentRenderUrls=url1,url2,..." for the main and adComponent renderUrls bids offered in the auction.</em>
   </td>
   <td>Required
   </td>
   <td>List of strings
   </td>
  </tr>
  <tr>
   <td>adComponentRenderUrls
   </td>
   <td>List of keys to query values for, under the namespace <code>adComponentRenderUrls</code>.
   </td>
   <td>SSP
   </td>
   <td>Optional
   </td>
   <td>List of strings
   </td>
  </tr>
  <tr>
   <td>subkey
   </td>
   <td>The browser sets the hostname of the publisher page to be the value.
<p>
If no specific value is available in the system for this subkey, a default value will be returned. The default value corresponds to the key when the subkey is not set.
   </td>
   <td>DSP
   </td>
   <td><em>the hostname of the top-level webpage where the ad will appear. The hostname is provided by the browser.</em>
   </td>
   <td>Required
   </td>
   <td>String
   </td>
  </tr>
</table>

### JSON response

<table>
  <tr>
   <td style="background-color: #cfe2f3">Parent Field
   </td>
   <td style="background-color: #cfe2f3">Field
   </td>
   <td style="background-color: #cfe2f3">Value Description
   </td>
  </tr>
  <tr>
   <td rowspan="3">namespace1
   </td>
   <td>key1
   </td>
   <td>Value corresponding to the key. Any JSON type
   </td>
  </tr>
  <tr>
   <td>key2
   </td>
   <td>Value corresponding to the key. Any JSON type
   </td>
  </tr>
  <tr>
   <td>…
   </td>
   <td>…
   </td>
  </tr>
  <tr>
   <td rowspan="3">namespace2
   </td>
   <td>key1
   </td>
   <td>Value corresponding to the key. Any JSON type
   </td>
  </tr>
  <tr>
   <td>key2
   </td>
   <td>Value corresponding to the key. Any JSON type
   </td>
  </tr>
  <tr>
   <td>…
   </td>
   <td>…
   </td>
  </tr>
</table>

### Example response

```
{
  "renderUrls": {
      "https://cdn.com/render_url_of_some_bid": <arbitrary_json>,
      "https://cdn.com/render_url_of_some_other_bid": <arbitrary_json>,
      ...},
  "adComponentRenderUrls": {
      "https://cdn.com/ad_component_of_a_bid": <arbitrary_json>,
      "https://cdn.com/another_ad_component_of_a_bid": <arbitrary_json>,
      ...}
}
```

Under each namespace, a list of key-value pairs is returned where the keys match
those specified in query parameters. If a key is not found by the server, it
will not be set in the response.

### Data-Version response header

As mentioned in the FLEDGE explainer, a `data-version` header may be optionally
returned. All key-value pairs updated during a particular period of time belong
to a version assigned by the system during the update, i.e, the version slowly
increments.

If the header is returned, all key-value pairs in the response are guaranteed to
belong to that version and the values will always be deterministic for a
version. This version cannot be specified in a request. The server makes the
decision on a best-effort basis to use versions as new as possible.

The server does not guarantee that the version will always be returned. It’s
much more likely when the number of keys in a request is small.

## Mutating API

### Description

This is an internal API with ACLs controlled by the ad tech operator of the
system, for only the operator (and their designated parties) to use.

This is the only API that can write data to the system. Details will be defined
in the future on the mechanism to bootstrap the system state.

The ad tech platform will need to decide when to invoke the Mutating API for
data updates. The API supports writing key-value objects in batches but it is
not required to group all updates in one call.

The API is synchronous. When it returns successfully, the update data is
guaranteed to be persisted but might not necessarily be served immediately.

A small amount of staleness (*O(minutes*)) may be expected before updated data
can be returned by the server for queries (“becomes live”). As mentioned in the
[data version](#data-version-response-header) section, data newly written will
be assigned a version. Repeated updates on the same namespace + key + subkey in
a short amount of time before the data is live will result in only one of the
updates being assigned a version. The others will be overwritten and discarded.

### Form

```
[endpoint internal to the ad tech operator]/v1/setvalues
```

There will be configuration options for setting up the endpoint. The
configuration is not considered API.

### Protocol

For usability, HTTPS POST will be the default protocol. Streaming protocols
could be evaluated for support based on demand.

### Body JSON schema

A list of JSON objects where each object contains the following:

<table>
  <tr>
   <td style="background-color: #cfe2f3">Field
   </td>
   <td style="background-color: #cfe2f3">Value Description
   </td>
   <td style="background-color: #cfe2f3">Mode
   </td>
   <td style="background-color: #cfe2f3">Required
   </td>
   <td style="background-color: #cfe2f3">Type
   </td>
  </tr>
  <tr>
   <td>namespace
   </td>
   <td>See the <a href="#namespace">namespace</a> concept.
   </td>
   <td><code>keys</code> is available for the DSP and <code>renderUrls</code> and <code>adComponentRenderUrls</code> are for the SSP.
   </td>
   <td>Required
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>key
   </td>
   <td>Value of the key itself.
   </td>
   <td>DSP, SSP
   </td>
   <td>Required
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>subkey
   </td>
   <td>If set, the value being updated will only be returned when the Read API parameters include this subkey. If unset, the value will become the default value for the key. When a lookup of a particular subkey returns no result, the default value will serve as the result.
   </td>
   <td>DSP, SSP
   </td>
   <td>Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>update
   </td>
   <td>If set, it indicates that this entry should be created/updated with given information.
   </td>
   <td>DSP, SSP
   </td>
   <td>Optional
   </td>
   <td><a href="#update-json-schema">JSON object</a>
   </td>
  </tr>
  <tr>
   <td>delete
   </td>
   <td>If set, it indicates that this key should be deleted immediately.
   </td>
   <td>DSP, SSP
   </td>
   <td>Optional
   </td>
   <td>Bool
   </td>
  </tr>
</table>

Note:`update` and `delete` are mutually exclusive. Only one must be set.

### Update JSON schema

<table>
  <tr>
   <td style="background-color: #cfe2f3">Field
   </td>
   <td style="background-color: #cfe2f3">Value Description
   </td>
   <td style="background-color: #cfe2f3">Required
   </td>
   <td style="background-color: #cfe2f3">Type
   </td>
  </tr>
  <tr>
   <td>value
   </td>
   <td>Actual value corresponding to the key. If set, the current value will be updated to this new value.
   </td>
   <td>Optional
   </td>
   <td>Any JSON type
   </td>
  </tr>
  <tr>
   <td>expiration
   </td>
   <td>Specification of the key expiration configuration. If not set, the key will not be garbage collected.
   </td>
   <td>Optional
   </td>
   <td><a href="#expiration-json-schema">JSON object</a>
   </td>
  </tr>
</table>

### Expiration JSON schema

The fields are mutually exclusive. Only one can be set.

<table>
  <tr>
   <td style="background-color: #cfe2f3">Field
   </td>
   <td style="background-color: #cfe2f3">Value Description
   </td>
   <td style="background-color: #cfe2f3">Required
   </td>
   <td style="background-color: #cfe2f3">Type
   </td>
  </tr>
  <tr>
   <td>time
   </td>
   <td>Absolute time after which the key will be expired.
   </td>
   <td>Optional
   </td>
   <td>String. In RFC 3339 time format
   </td>
  </tr>
  <tr>
   <td>hours
   </td>
   <td>Hours since the current server time as of processing of this request.
   </td>
   <td>Optional
   </td>
   <td>Number (integer)
   </td>
  </tr>
</table>

### Example request body

```
[
  {
    "namespace": "renderUrls",
    "key": "https://cdn.com/render_url_of_some_bid",
    "update": {
      "value": "foo",
      "expiration": {"hours": 10}
    }
  },
  {
    "namespace": "adComponentRenderUrls",
    "key": "https://cdn.com/ad_component_of_a_bid",
    "update": {
      "value": "bar",
      "expiration": {"time": "2039-12-31T22:33:44Z"}
    }
  }
]
```

### Response

If the operation succeeds, an HTTP 200 response is returned. If the server
fails, an appropriate error code (e.g., HTTP 500) will be returned.

The request body is a list of JSON objects where each object represents a change
to be applied. When a request includes more than one change (such as multiple
updates) and one of the changes encounters an error during processing (such as
having an invalid expiration format), the other changes in the same request may
or may not be applied on the server side.

If the protocol is bi-directional streaming, the server will return a status for
each request.
