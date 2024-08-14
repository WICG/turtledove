---
coding: utf-8

title: Trusted Auction Services
docname: draft-ietf-trusted-auction-services
category: std

area: TODO
workgroup: TODO
keyword: Internet-Draft

ipr: trust200902

stand_alone: yes
pi: [toc, sortrefs, symrefs]

author:
 -
    name: Daniel Kocoj
    organization: Google
    email: dankocoj@google.com
 -
    name: Benjamin "Russ" Hamilton
    organization: Google
    email: behamilton@google.com

normative:
  CBOR: RFC7049
  CDDL: RFC8610
  OHTTP: RFC9458

informative:

--- abstract

Trusted Auction Services provide a way for advertising auctions to execute in a
remote trusted execution environment while preserving user privacy.

--- middle

Introduction        {#problems}
============

Today, real-time bidding and ad auctions are executed on servers that may not
provide technical guarantees of security. Some users have concerns about how
their data is handled to generate relevant ads and in how that data is shared.
Protected Audience API ([Android](https://developer.android.com/design-for-safety/privacy-sandbox/fledge),
[Chrome](https://developer.chrome.com/docs/privacy-sandbox/fledge/)) provides
ways to preserve privacy and limit third-party data sharing by serving
personalized ads based on previous mobile app or web engagement.

For all platforms, Protected Audience may require [real-time services](https://github.com/privacysandbox/fledge-docs/blob/main/trusted_services_overview.md).
In the initial [proposal by Chrome](https://github.com/WICG/turtledove/blob/main/FLEDGE.md),
bidding and auction for Protected Audience ad demand is executed locally. This
can demand computation requirements that may be impractical to execute on
devices with limited processing power, or may be too slow to render ads due to
network latency.

The Trusted Auction Services proposal outlines a way to allow Protected Audience
computation to take place on cloud servers in a trusted execution environment,
rather than running locally on a user's device. Moving computations to cloud in
a [Trusted Execution Environment (TEE)](https://github.com/privacysandbox/fledge-docs/blob/main/trusted_services_overview.md#trusted-execution-environment)
have the following benefits:

*   Scalable auctions.
  *   A scalable ad auction may include several buyers and sellers and that
      can demand more compute resources and network bandwidth.
*   System health of the user's device.
  *   Ensure better system health of the user's device by freeing up
      computational cycles and network bandwidth.
*   Better latency of ad auctions.
  *   Server to server communication on the cloud is faster than multiple device to server calls.
  *   Adtech code can execute faster on servers with higher computing power compared to a device.
*   Servers have better processing power.
  *   Adtechs can run more compute intensive workloads on a server compared to a device for better utility.
*   [trusted execution environment](https://github.com/privacysandbox/fledge-docs/blob/main/trusted_services_overview.md#trusted-execution-environment)
    can protect confidentiality of adtech code and signals.

Standardized protocols for interacting with Bidding and Auction Services are
essential to creating a diverse and healthy ecosystem for such services.

This document does not describe distribution of private keys to trusted auction
services.

Use Cases    {#use-cases}
---------

Terminology          {#Terminology}
-----------

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL
NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED",
"MAY", and "OPTIONAL" in this document are to be interpreted as
described in BCP 14 {{!RFC2119}} {{!RFC8174}} when, and only when, they
appear in all capitals, as shown here.

# Formats   {#format}

## Browser to Ad Server Request Message {#request}
The message is encrypted using HPKE with the encapsulation performed according
to {{OHTTP}}. Since we are repurposing the OHTTP encapsulation mechanism, we
define the media type "message/auction request" which is used for encryption.

The plaintext message has the following framing:
TODO
where the the first 3 bits of the frame header specify the version and the following
5 bits specify the compression according to the following table:
TODO.

The request message has the following format:

~~~~~ cddl
request = {
  version: int,

  publisher: origin,

  interestGroups: {
    * origin => bstr
    ; CBOR encoded list of interest groups compressed
    ; using the method described in `compression`.
  },

  generationId: uuid,

  ? enableDebugReporting: bool
}
origin = tstr .regexp "https://([^/:](:[0-9]+)?/"
uuid = tstr .regexp "[a-fA-F0-9]{8}-[a-fA-F0-9]{4}-[a-fA-F0-9]{4}-[a-fA-F0-9]{4}-[a-fA-F0-9]{12}"
~~~~~


# Security Considerations

TODO

# IANA Considerations

TODO

--- back

# Acknowledgments
{:numbered="false"}

TODO
