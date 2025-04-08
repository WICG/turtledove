# Protected Audience Key/Value Server APIs Explainer

Authors:

* Peiwen Hu
* Russ Hamilton

## Summary

[Protected Audience](https://github.com/WICG/turtledove/blob/main/FLEDGE.md) is a privacy-advancing API that facilitates interest group based advertising. Trusted
servers in Protected Audience are used to add real-time signals into ad selection for both
buyers and sellers. The Protected Audience proposal specifies that these trusted servers
should provide basic key-value lookups to facilitate fetching these signals but
do no event-level logging or have other side effects.

This explainer proposes APIs for trusted servers. [The trust model explainer](https://github.com/privacysandbox/fledge-docs/blob/main/key_value_service_trust_model.md) covers
the service in more detail, including threat models, trust models, and
system design.

### BYOS (Bring Your Own Server) and Trusted server

As mentioned in the main explainer, _"As a temporary mechanism during the First Experiment timeframe, the buyer and seller can fetch these bidding signals from any server, including one they operate themselves (a "Bring Your Own Server" model). However, in the final version after the removal of third-party cookies, the request will only be sent to a trusted key-value-type server."_

Designed on the foundation of the BYOS API, the trusted server API includes a few technical changes to facilitate communication with a key-value server running in a TEE (Trust Execution Environment). From the Trusted Key Value Server development perspective, the BYOS API is considered "Version 1". For the BYOS API, please refer to the [Protected Audience Spec as the source of truth](https://wicg.github.io/turtledove/).

This doc is focused on the Trusted Key Value Server API. The API version starts with version 2. The latest version is version 2.

## Terminology

### Namespace

Keys may exist in different namespaces. The namespaces help the server identify
keys needed from request query strings and prevent potential key collision, even
though the keys may be unique across namespaces in today’s use cases.

*   For a DSP, there are: `keys` and `interestGroupNames`.
*   For an SSP, there are `renderURLs` and `adComponentRenderURLs`.

The
[Protected Audience explainer](https://github.com/WICG/turtledove/blob/main/FLEDGE.md#31-fetching-real-time-data-from-a-trusted-server)
provides more context about these namespaces.

## Query API version 2

__Query versions 2 and beyond are specifically designed for the trusted TEE key/value service as part of the trust model, published here for explanatory purposes. BYOS servers do not need to implement these.__

### Background

In this version we present a protocol that enables trusted communication between Chrome and the trusted key/value service. The protocol assumes a functioning trust model as described in [the key/value service explainer](https://github.com/privacysandbox/protected-auction-services-docs/blob/main/key_value_service_trust_model.md) is in place, primarily that only service implementations recognized by Privacy Sandbox can obtain private decryption keys, and the mechanism for the client and service to obtain cryptographic keys is available but outside the scope of this document.

On a high level, the protocol is similar to the [Bidding & Auction services protocol](https://github.com/WICG/turtledove/blob/main/FLEDGE_browser_bidding_and_auction_API.md), with (bidirectional) [HPKE encryption](https://datatracker.ietf.org/doc/rfc9180/).

*   TLS is used to ensure that the client is talking to the real service operator (identified by the domain). FLEDGE enforces that the origin of the trusted server matches the config owner ([interest group owner](https://wicg.github.io/turtledove/#joining-interest-groups) for the trusted bidding signal server or [auction config’s seller](https://wicg.github.io/turtledove/#running-ad-auctions) for the trusted scoring signal server).
*   HPKE is used to ensure that the message is only visible to the approved versions of services inside the trusted execution environment (TEE).
    *   The reason to use HPKE is that the request must be encrypted and can only be decrypted by the trusted service itself. A notable alternative protocol is TLS which in addition to the domain validation, validates the service identity attestation. However, attestation verification as part of TLS can present performance challenges and is still being evaluated.
    *   The protocol to configure the HPKE is roughly based on the [Oblivious HTTP proposal](https://datatracker.ietf.org/doc/rfc9458/).

For more information on the design, please refer to [the trust model explainer](https://github.com/privacysandbox/protected-auction-services-docs/blob/main/key_value_service_trust_model.md).

### Overview

![V2 API diagram](assets/fledge_kv_server_v2_api.png)

The request contains an outer HTTP layer with an inner [Oblivious HTTP](https://datatracker.ietf.org/doc/draft-ietf-ohai-ohttp/) layer.

### Outer HTTP layer
For the outer HTTP layer:
* HTTPS is used to transport data.
* The HTTP method is `POST`.
* Requests specify Content types via these headers:
   ```
   Content-Type: message/ad-auction-trusted-signals-request
   Accept: message/ad-auction-trusted-signals-response
   ```

### Inner HTTP layer

#### Encryption

We will use [Oblivious HTTP](https://datatracker.ietf.org/doc/draft-ietf-ohai-ohttp/) with the following configuration for encryption:

*   0x0020 DHKEM(X25519, HKDF-SHA256) for KEM (Key encapsulation mechanisms)
*   0x0001 HKDF-SHA256 for KDF (key derivation functions)
*   AES256GCM for AEAD scheme.

Since we are [repurposing the OHTTP encapsulation mechanism, we are required to define new media types](https://www.rfc-editor.org/rfc/rfc9458.html#name-repurposing-the-encapsulati):
* The OHTTP request media type is “message/ad-auction-trusted-signals-request”
* The OHTTP response media type is “message/ad-auction-trusted-signals-response”

Note that these media types are [concatenated with other fields when creating the HPKE encryption context](https://www.rfc-editor.org/rfc/rfc9458.html#name-encapsulation-of-requests), and are not HTTP content or media types.

Inside the ciphertext, the request/response is framed with a 5 byte header, where the first byte is the format+compression byte, and the following 4 bytes are the length of the request message in network byte order. Then the request is zero padded to a set of pre-configured lengths.

The lower 2 bits are used for compression specification. The higher 6 bits are currently unused.

#### Format+compression byte

- `0x00` - [CBOR](https://www.rfc-editor.org/rfc/rfc8949.html) no compression
- `0x01` - CBOR compressed in brotli
- `0x02` - CBOR compressed in gzip

For request, the byte value is 0x00. For response, the byte value depends on the “acceptCompression” field in the request and the server behavior.

#### Padding

Padding is applied with sizes as multiples of 2^n KBs ranging from 0 to 2MB. So the valid response sizes will be [0, 128B, 256B, 512B, 1KB, 2KB, 4KB, 8KB, 16KB, 32KB, 64KB, 128KB, 256KB, 512KB, 1MB, 2MB].

### Core data

Core request and response data structures are all in [CBOR](https://www.rfc-editor.org/rfc/rfc8949.html).

The schema below is defined following the spec by https://json-schema.org/.

The API is generic, agnostic to DSP or SSP use cases.

#### Request

##### Request version 2.0

Requests are not compressed. Compression could save size but may add latency.  Request size is presumed to be small, so compression may not improve overall performance.  Version 2 will initially not implement compression for the request but more experimentation is necessary to determine if compression is an overall win and should be implemented in the future.

In the request, one major difference from V1/BYOS is that the keys are now grouped. There is a tree-like hierarchy:

*   Each request contains one or more partitions. Each partition is a collection of keys that can be processed together by the service without any potential privacy leakage (For example, if the server uses [User Defined Functions](https://github.com/privacysandbox/protected-auction-services-docs/blob/main/key_value_service_user_defined_functions.md) to process, one UDF call can only process one partition). Keys from one interest group must be in the same partition. Keys from different interest groups with the same joining site may or may not be in the same partition, so the server User Defined Functions should not make any assumptions based on that.
*   Each partition contains one or more key groups. Each key group has its unique attributes among all key groups in the partition. The attributes are represented by a list of “Tags”. Besides tags, the key group contains a list of keys to look up.
*   Each partition has a unique id.
*   Each partition has a compression group field. Results of partitions belonging to the same compression group can be compressed together in the response. Different compression groups must be compressed separately. See more details below. The expected use case by the client is that interest groups from the same joining origin and owner can be in the same compression group.

![request structure](assets/fledge_kv_server_v2_req_structure.jpeg)

In addition, the browser may only pass [contextual signals](https://github.com/WICG/turtledove/blob/main/PA_Key_Value_Server_Contextual_Signals.md) (`sellerTKVSignals` and `buyerTKVSignals`) to a trusted key/value service.

#### Schema of the request

```json
{
  "title": "tkv.request.v2.Request",
  "description": "Key Value Service GetValues request",
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "acceptCompression": {
      "type": "array",
      "items": {
        "type": "string",
        "description": "must contain at least one of none, gzip, brotli"
      },
      "description": "Algorithm accepted by the browser for the response."
    },
    "metadata": {
      "title": "tkv.request.v2.RequestMetadata",
      "description": "metadata",
      "type": "object",
      "additionalProperties": false,
      "properties": {
        "hostname": {
          "description": "The hostname of the top-level frame calling runAdAuction().",
          "type": "string"
        }
      }
    },
    "perPartitionMetadata":{
      "title": "tkv.request.v2.PerPartitionMetadata",
      "description": "Partition-level metadata, batched to avoid duplicating values in the request.",
      "type": "object",
      "additionalProperties": false,
      "properties": {
        "contextualData": {
          "description":"Contextual signals (buyerTKVSignals or sellerTKVSignals) to the trusted Key/Value server.",
          "type": "array",
          "items": {
            "description": "Metadata value configuration object specifying which "
                           "value applies to which partitions. "
                           "Duplicate partition-level metadata specification "
                           "for a single partition will result in an error.",
            "type": "object",
            "properties": {
              "value": {
                "description": "Metadata value to apply to specified partitions in `ids`. "
                               "If `ids` is not present, apply the value to all partitions. "
                               "The metadata value may be a serialized JSON string.",
                "type": "string"
              },
              "ids": {
                "description": "Array of [compression group id, partition id] to uniquely identify a partition. "
                               "The metadata value is applied to all partitions with a matching ID. "
                               "If `ids` is not present, apply the value to all partitions.",
                "type": "array",
                "items": {
                  "description": "Id comprised of [compression group id, partition id] to uniquely identify a partition.",
                  "type": "array",
                  "items": {
                    "type": "integer"
                  },
                  "minItems": 2,
                  "maxItems": 2
                }
              }
            },
            "required": [
              "value"
            ]
          }
        }
      }
    },
    "partitions": {
      "description": "A list of partitions. Each must be processed independently. Accessible by UDF.",
      "type": "array",
      "items": {
        "title": "tkv.request.v2.Partition",
        "description": "Single partition object. A collection of keys that can be processed together",
        "type": "object",
        "additionalProperties": false,
        "properties": {
          "id": {
            "description": "Id of the partition. [compressionGroupId,id] must be unique across the request.",
            "type": "unsigned integer"
          },
          "compressionGroupId": {
            "description": "Unique id of the compression group. Only partitions belonging to the same compression group will be compressed together in the response",
            "type": "unsigned integer"
          },
          "metadata": {
            "title": "tkv.request.v2.PartitionMetadata",
            "description": "metadata",
            "type": "object",
            "additionalProperties": false,
            "properties": {
              "experimentGroupId": {
                "type": "string"
              },
              "slotSize": {
                "description": "Available if trustedBiddingSignalsSlotSizeMode=slot-size. In the form of <width>,<height>",
                "type": "string"
              },
              "allSlotsRequestedSizes": {
                "description": "Available if trustedBiddingSignalsSlotSizeMode=all-slots-requested-sizes. In the form of <width1>,<height1>,<width2>,<height2>,...",
                "type": "string"
              }
            }
          },
          "arguments": {
            "type": "array",
            "items": {
              "title": "tkv.request.v2.Argument",
              "description": "One group of keys and common attributes about them",
              "type": "object",
              "additionalProperties": false,
              "properties": {
                "tags": {
                  "description": "List of tags describing this group's attributes",
                  "type": "array",
                  "items": {
                    "type": "string"
                  }
                },
                "data": {
                  "type": "array",
                  "description": "List of keys to get values for",
                  "items": {
                    "type": "string"
                  }
                }
              }
            }
          }
        },
        "required": [
          "id",
          "compressionGroupId",
          "arguments"
        ]
      }
    }
  },
  "required": [
    "partitions"
  ]
}

```

##### Available Tags

###### Version 2024.04

<table>
  <tr>
   <td>Tag category
   </td>
   <td>Category description
   </td>
   <td>Tag
   </td>
   <td>Description
   </td>
  </tr>
  <tr>
   <td rowspan="4">Namespace
   </td>
   <td rowspan="4">Each key group has exactly one tag from this category.
   </td>
   <td>interestGroupNames
   </td>
   <td>Names of interest groups in the encompassing partition.
   </td>
  </tr>
  <tr>
   <td>keys
   </td>
   <td><em>“keys” is a list of trustedBiddingSignalsKeys strings.</em>
   </td>
  </tr>
  <tr>
   <td>renderURLs
   </td>
   <td rowspan="2"><em>Similarly, sellers may want to fetch information about a specific creative, e.g. the results of some out-of-band ad scanning system. This works in much the same way, with the base URL coming from the trustedScoringSignalsUrl property of the seller's auction configuration object.</em>
   </td>
  </tr>
  <tr>
   <td>adComponentRenderURLs
   </td>
  </tr>
</table>

Example trusted bidding signals request from Chrome:

```json
{
  "acceptCompression": [
    "none",
    "gzip"
  ],
  "metadata": {
    "hostname": "example.com"
  },
  "partitions": [
    {
      "id": 0,
      "compressionGroupId": 0,
      "metadata": {
        "experimentGroupId": "12345",
        "slotSize": "100,200",
      },
      "arguments": [
        {
          "tags": [
            "interestGroupNames"
          ],
          "data": [
            "InterestGroup1"
          ]
        },
        {
          "tags": [
            "keys"
          ],
          "data": [
            "keyAfromInterestGroup1",
            "keyBfromInterestGroup1"
          ]
        }
      ]
    },
    {
      "id": 1,
      "compressionGroupId": 0,
      "arguments": [
        {
          "tags": [
            "interestGroupNames"
          ],
          "data": [
            "InterestGroup2",
            "InterestGroup3"
          ]
        },
        {
          "tags": [
            "keys"
          ],
          "data": [
            "keyMfromInterestGroup2",
            "keyNfromInterestGroup3"
          ]
        }
      ]
    }
  ]
}
```
#### Schema of the Response

##### Response version 2.0

The response is compressed. Due to security and privacy reasons the compression is applied independently to each compression group. That means, The response object mainly contains a list of compressed blobs, each for one compression group. Each blob is for outputs of one or more partitions, sharing the same compressionGroup value as specified in the request.

###### tkv.response.v2.Response

```json
{
  "title": "tkv.response.v2.Response",
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "compressionGroups": {
      "type": "array",
      "items": {
        "title": "tkv.response.v2.CompressedCompressionGroup",
        "type": "object",
        "description": "Object for a compression group, compressed using the algorithm specified in the request",
        "additionalProperties": false,
        "properties": {
          "compressionGroupId": {
            "type": "unsigned integer"
          },
          "ttl_ms": {
            "description": "Adtech-specified TTL for client-side caching. In milliseconds. Unset means no caching.",
            "type": "unsigned integer"
          },
          "content": {
            "description": "compressed CBOR binary string. For details see compressed response content schema: tkv.response.v2.CompressionGroup",
            "type": "byte string"
          }
        }
      }
    }
  }
}
```

###### tkv.response.v2.CompressionGroup

The content of each compressed blob is a CBOR list of partition outputs. This object contains actual key value results for partitions in the corresponding compression group.

```json
{
  "type": "array",
  "items": {
    "title": "tkv.response.v2.PartitionOutput",
    "description": "Output for one partition",
    "type": "object",
    "properties": {
      "id": {
        "description": "Unique id of the partition from the request",
        "type": "unsigned integer"
      },
      "dataVersion" {
        "description": "An optional field to indicate the state of the data that generated this response, which will then be available in bid generation/scoring and reporting.",
        "type": "unsigned integer"
      },
      "keyGroupOutputs": {
        "type": "array",
        "items": {
          "title": "tkv.response.v2.KeyGroupOutput",
          "title": "Output for one key group",
          "type": "object",
          "additionalProperties": false,
          "properties": {
            "tags": {
              "description": "Attributes of this key group.",
              "type": "array",
              "items": {
                "description": "List of tags describing this key group's attributes",
                "type": "string"
              }
            },
            "keyValues": {
              "description": "If a keyValues object exists, it must at least contain one key-value pair. If no key-value pair can be returned, the key group should not be in the response.",
              "type": "object",
              "patternProperties": {
                ".*": {
                  "description": "One value to be returned in response for one key",
                  "type": "object",
                  "additionalProperties": false,
                  "properties": {
                    "value": {
                      "type": [
                        "text string"
                      ]
                    }
                  },
                  "required": [
                    "value"
                  ]
                }
              }
            }
          }
        }
      }
    }
  }
}

```

Example:

```json
[
  {
    "id": 0,
    "dataVersion": 102,
    "keyGroupOutputs": [
      {
        "tags": [
          "interestGroupNames"
        ],
        "keyValues": {
          "InterestGroup1": {
            "value": "{\"priorityVector\":{\"signal1\":1},\"updateIfOlderThanMs\": 10000}"
          }
        }
      },
      {
        "tags": [
          "keys"
        ],
        "keyValues": {
          "keyAfromInterestGroup1": {
            "value": "valueForA"
          },
          "keyBfromInterestGroup1": {
            "value":"[\"value1ForB\",\"value2ForB\"]"
          }
        }
      }
    ]
  }
]
```

#### Structured keys response specification

Structured keys are keys that the browser is aware of and the browser can use the response to do additional processing. The value of these keys must abide by the following schema for the browser to successfully parse them.

Note that they must be serialized to string when stored as the value.

##### tkv.response.v2.InterestGroupResponse

For values for keys from the `interestGroupNames` namespace, they must conform to the following schema, prior to being serialized to string:

```json
{
  "title": "tkv.response.v2.InterestGroupResponse",
  "description": "Format for value of keys in groups tagged 'interestGroupNames'",
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "priorityVector": {
      "type": "object",
      "patternProperties": {
        ".*": {
          "description": "signals",
          "type": "number"
        }
      }
    },
    "updateIfOlderThanMs": {
      "description": "This optional field specifies that the interest group should be updated if the interest group hasn't been joined or updated in a duration of time exceeding `updateIfOlderThanMs` milliseconds. Updates that ended in failure, either parse or network failure, are not considered to increment the last update or join time. An `updateIfOlderThanMs` that's less than 10 minutes will be clamped to 10 minutes.",
      "type": "unsigned integer"
    }
  }
}
```

Example:

```json
{
  "priorityVector": {
    "signal1": 1,
    "signal2": 2
  },
  "updateIfOlderThanMs": 10000
}
```

#### Size limitation

For the K/V service, plaintext response payload before compression must not be more than 8MB. 8MB is a preliminary target and can be changed in the future if there is a need and no significant negative impact to the browser.

#### Error handling

To be supported in the future.

## Server Internal APIs and procedures

The system will provide multiple private APIs and procedures, for loading data and user-defined functions whose model is described in detail in the [Key/Value server design explainer](https://github.com/privacysandbox/protected-auction-services-docs/blob/main/key_value_service_trust_model.md#support-for-user-defined-functions-udfs).

These APIs and procedures are ACLs controlled by the ad tech operator of the system, for only the operator (and their designated parties) to use.

The key/value server code is available on its [Privacy Sandbox github repo](https://github.com/privacysandbox/protected-auction-key-value-service) which reflects the most recent API and procedures. As an example, the [data loading guide](https://github.com/privacysandbox/protected-auction-key-value-service/blob/main/docs/data_loading/loading_data.md) has specific instructions on integrating with the data ingestion procedure. The procedure may be based on the actual storage medium of the dataset, e.g., the server can read data from data files from a prespecified location.

As the [FLEDGE API](https://github.com/WICG/turtledove/blob/main/FLEDGE.md) describes the client side flow, the APIs and procedures related to the server system design and implementation will have the specification posted and updated in the key/value server’s [github repo](https://github.com/privacysandbox/protected-auction-key-value-service).
