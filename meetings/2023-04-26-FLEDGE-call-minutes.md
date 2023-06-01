# Protected Audience (formerly FLEDGE) WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday April 26, 2023


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Brian May (Dstillery)
3. Sven May (Google Privacy Sandbox)
4. Wendell Baker (Yahoo)
5. David Dabbs (Epsilon)
6. Don Marti (Raptive) (which used to be CafeMedia)
7. Caleb Raitto (Google Chrome)
8. Paul Jensen (Google Chrome)
9. Fabian Höring (Criteo)
10. Phil Lee (Google Privacy Sandbox)
11. David Eilertsen (Remerge)
12. Risako Hamano (Yahoo Japan) 
13. Kevin Graney (Google Privacy Sandbox)
14. Andrew Aikens (TripleLift)
15. Marco Lugo (NextRoll)
16. Itay Sharfi (Google)
17. Maciej Zdanowicz (RTB House)
18. Connor Doherty (Amazon)
19. Russ Hamilton (Google Chrome)
20. Orr Bernstein (Google Chrome)
21. Zheng Wei (Google Ads)
22. Patrick McCann (Raptive)
23. Andrew Pascoe (NextRoll)
24. Supraja Sekhar (Google Ads)


## Note taker: Paul Jensen


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here]



*   Key-Value Server [Itay Sharfi]: the KV V0.9 release ([slides](https://drive.google.com/file/d/16IW1n00M7FLx_yLyJXCKiMBzshNYNdSn/view))


# Notes


## Key-Value Server [Itay Sharfi]: the KV V0.9 release

Itay: Hi, PM at Google on Bidding and Auction and KV servers.  Recent KV v0.9 release happened.  Like to introduce Phil and Brian.

Phil: Hi, we talked about KV work in the past.

Brian: Hi, also worked on KV.

Alex: Hi, also an engineer on KV.

Itay: (starts presentation) Going to explain more about the KV 0.9 release.  KV is our server that loads and serves data.  KV called either through B&A service or directly from browser.  B&A service running in TEE can call KV server to retrieve data for auction.  BYOS KV supported until 2025Q3.  0.9 release relates to TEE KV.  TEE KV is ready to be used for non-production.  BYOS KV allowed post 3rd party cookie deprecation.  KV TEE required for enhanced privacy.  KV TEE not required for BA TEE.  BA also runs in a TEE.

What 0.9 KV can do: real-time low-latency updates supported.  Single server with multiple regions.  Runs on AWS. User defined functions  (UDFs) supported for complex transformations.  Source code is open and online for exploration and contributing.

KV service target use cases: For SSP: keyed on renderURL; relatively simple request processing through a yes/no table.  For DSP: Retrieve info about ad candidates.  Complex lookups across multiple tables.  Adtechs have asked for metadata access (e.g. IP address for geolocation).  Real-time budgeting supported.

KV BYOS to KV TEE migration: Same: Keys, Values, Blobs.  New: Different endpoint, cloud investment, operating trusted servers.  Need to put your data into cloud.

KV service diagram: For on-device execution: client/browser calls KV service.  KV service accesses cloud storage bucket which is filled from adtech uploads.

KV design: operated by DSP or SSP separately.  Data source owned by Adtechs.  Open source code and APIs.  Run in TEE.  Server binary transparency.  Multi Cloud Solution (AWS now, GCP coming soon).

Estimated roadmap/timeline: Scaling 23Q3, Productionizing 23Q4, Feature complete post-25Q3.  KV policy enforcement coming after that.

KV Privacy and Security: Secure sandboxed environment on the cloud.  Open source on GitHub.  Adtechs deploy services to TEE on public cloud platform.  Coordinators generate and manage crypto keys. Adtech’s code executes in the sandbox.

Adoption: Looking for adtechs to test and deploy to provide feedback.  Benefits: developing expertise in KV and TEE.  Eliminate future migration.  Work closely with KV team.  Validate functionality.

Getting started: links to codebase and deployment and integration and UDF support.

Looking for feedback on: Sharding, load time, UDFs, targeted use-case, ease of use, authentication, external contributions process/interest, capacity / data size (KV store currently in-memory, exploring).

David Dabbs: Itay mentioned eliminating migrations.  FLEDGE explainer mentions trusted server required in the future.  Do you imagine registration process?  so browser knows it’s running in TEE.  How do adtechs register?



*   Itay: Phil do you want to answer on registration?
*   Phil: Similar to what’s happening for Aggregation service.  There’s a trusted 3p who works as a coordinator.  Adtechs register with this 3p.  Encryption keys only given out to registered instances.
*   David: This means there will be a cert that gets distributed to browsers to know which adtechs to trust?  Moving to a new server infrastructure would use the new setup, but wouldn’t need to register.
*   Brian: Not just a flip the switch one day from BYOS to TEE.  TEE version of KV server does work with today’s FLEDGE auctions running in browser.  Can test using TEE KV before running in TEE today.
*   David: Not sure data would be in impression logs to know which TEE implementation versus another.
*   Itay: How to do experimentations is an interesting general topic.

Brian May: Is there going to be multiple write paths from adtechs?  will there be a notion of priority in case some data wants to get to KV first?



*   Brian Schneider: Two paths: you write a file of mutations then upload to S3 bucket.  This is the slow path.  Low latency path: Alex implemented a way to publish mutation directly, rather than getting pub-sub notification of new file of mutations.  10’s of thousands of mutations per second.
*   Brian M: Good answer!

Brian M: Are we changing the name of the group?



*   Michael: We’ve changed the name from FLEDGE to Protected Audiences.  Name has matured and gotten a non-bird name.  Doesn’t imply changes to code or usage.  Changes the name of what we talk about.  They don’t let me name things anymore…no more bird names.  There will be references to both names for a bit.  Trying to change the name to the new name.  Github has been largely changed over.  We didn’t actually use as many bird names as we had.
*   Brian M: Need to keep old names for a bit to avoid email filters missing things.

Michael: Going back to David’s question about switching over.  When creating an IG, you specify KV service URL.  You can set that URL to a KV in a TEE instead of a BYOS KV.  Can happen on a user by user basis.  Choice happens when user is joined to an IG.  As Itay mentioned, that changeover is not the same thing as requiring KV request to be encrypted using coordinator key.  You can use a regular KV request without encryption.  In the future having the encryption key in the browser and TEE will be required no sooner than 2025Q3.

Brian M: slides link?



*   Itay: Yes, will include in the notes: https://drive.google.com/file/d/16IW1n00M7FLx_yLyJXCKiMBzshNYNdSn/view

David: In slide with writing once and reading anywhere, with on-device auctions: today HTTP cache mechanics are used today to cache responses.  Does B&A also honor caching semantics?



*   Arun: With B&A it’s a super fast inter-server networking that fetches from the KV so initial implementation hasn’t had this optimization.
*   Brian: Perhaps premature optimization.
*   Michael: Use browser cache for now.  May not hit as much because exact set of IGs.
*   Paul: and publisher URL has to match too
*   Michael: May want smarter caching in the future.
*   Itay: We allow batching today.
*   David: Coalescing?
*   Itay: Can batch.
*   Paul: Going to be a little different when we start introducing encryption.  Today the browser HTTP cache knows about the real unencrypted URLs, even though every time you encrypt you get a different result.  When we wrap the keys in another layer of encryption, the browser will need additional insight in order to keep doing caching.
*   Michael: any other questions?
*   Itay: Code is available, happy to get feedback.  Working with partners.
*   Michael: PATCG is next week.  Two weeks is May 10th, looking forward to meeting then if we have agenda items.
