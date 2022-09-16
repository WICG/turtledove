# FLEDGE Key/Value Server APIs Explainer

## Summary

[FLEDGE](https://github.com/WICG/turtledove/blob/main/FLEDGE.md) is a privacy-preserving API that facilitates interest group based advertising. Trusted
servers in FLEDGE are used to add real-time signals into ad selection for both
buyers and sellers. The FLEDGE proposal specifies that these trusted servers
should provide basic key-value lookups to facilitate fetching these signals but
do no event-level logging or have other side effects.

This explainer proposes APIs for trusted servers. Future explainers will cover
trusted servers in more detail, including threat models, trust models, and
system design.

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

## Query API Version 1

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

## Server Internal APIs
The system would provide multiple private APIs, primarily for loading data and user-defined functions whose model is described in detail in the [Key/Value server design explainer](https://github.com/privacysandbox/fledge-docs/blob/main/key_value_service_trust_model.md#support-for-user-defined-functions-udfs), and possibly more in the future.

These all are private processes with ACLs controlled by the ad tech operator of the system, for only the operator (and their designated parties) to use.

The key/value server code is available on its [Privacy Sandbox github repo](https://github.com/privacysandbox/fledge-key-value-service) which reflects the most recent API and processes. As an example, the [data loading guide](https://github.com/privacysandbox/fledge-key-value-service/blob/main/docs/loading_data.md) has specific instructions on how to integrate with the data ingestion process. The process may be based on the actual storage medium of the dataset, e.g., the server can read data from data files in a prespecified location.

As the [FLEDGE API](https://github.com/WICG/turtledove/blob/main/FLEDGE.md) describes the client side flow, the APIs and processes related to the server system design and implementation will have the specification posted and updated in the key/value server’s [github repo](https://github.com/privacysandbox/fledge-key-value-service).
