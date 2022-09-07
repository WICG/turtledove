# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 5pm Paris time = 3pm UTC (during summer).

This [notes doc](https://docs.google.com/document/d/1Kr0hpfQ_Q1LX1aN00D5k_09yV_a7WE9RSn69nS3nZho/edit#) will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday Aug 31, 2022, since there is a suggested agenda item (see below)


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Chrome)
2. Andrew Aikens (TripleLift)
3. Andrew Pascoe (NextRoll)
4. Paul Jensen (Google Chrome)
5. Brian May (dstillery)
6. Sid Sahoo (Google Chrome)
7. Martin Gruau (Captify)
8. Matt Menke (Google Chrome)
9. Phil Lee (Google Chrome)
10. Sven May (Google Chrome)
11. Don Marti (CafeMedia)
12. Fabian Höring (Criteo)
13. Marco Lugo (NextRoll)
14. Martin Pal (Google Chrome)
15. Peiwen Hu (Google Chrome)
16. Alex Cone (IAB Tech Lab)
17. Supraja Sekhar (Google Ads)
18. Anthony Molinaro (OpenX)
19. Russ Hamilton (Google Chrome)
20. Joel Pfeiffer (MSFT)
21. Tamara Yaeger (Iponweb)
22. John Mooring (MSFT)
23. Paul Farrow (Xandr)
24. Georgia Franklin (Google Chrome)
25. Jonasz Pamula (RTB House)
26. Tris Southey (Google Chrome)
27. Brad Rodriguez (Magnite)
28. Kevin Lee (Google Chrome)
29. Daniel Rojas (Google Chrome)
30. Zheng Wei (Google Ads)
31. Stan Belov (Google Ads)
32. Peter Meric (Google Chrome)
33. Sergey Tumakha (Microsoft Ads)
34. Joel Meyer (OpenX)
35. Chinyet Lin (Google Ads)


## Note taker: Martin Pal


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here]



*   Philip Lee, [Issue 341](https://github.com/WICG/turtledove/issues/341): Support for user-defined-functions in the FLEDGE servers


## Notes

Michael Kleber (warmup): (fyi, we've had a large amount of comment spam on the notes doc. That's why we need to lock it down to comment-only between meetings.)


### Agenda item: [Issue 341](https://github.com/WICG/turtledove/issues/341) User defined functions in the FLEDGE servers

Phil Lee: Two things I want to introduce. 

(1) We published open source implementation of the KV server. Feel free to look, file issues, play with code. Not recommended in production, but we'll be improving and hardening it over time.

(2) We want to discuss possibility of user defined functions in the KV server. 

Reasons - filter logic, denormalized data

We think this doesn't change the privacy model, adds flexibility

We'd like to hear opinions about what language the UDF should be expressed in, what data the UDF should have access to. We're not changing API between KV server and browser.

Brian May: How do we manage load on the server that runs user defined functions? Lots of trial and error to ensure UDF doesn't snarl up the server. Who is responsible for SLA?

Does it make sense to have two different channels: one with UDF, one straight lookup? I don't want the plain lookups to be slowed down.

Phil: great questions. 

Who is going to run UDF: it's the adtech who is operating the KV server, and they also author UDFs. Operator defines the UDFs.

How do we debug, define SLA: this is hard, we're working on it. With TEE, debugging is hard. We don't want to expose the request stream, as that contains private data. 

Hoping we can post an explainer on how we think about debugging.

Straight lookup path: we haven't thought about this much.

Brian: is the KV server going to be run by a 3rd party on behalf of the adtech?

Phil: We'll have a third party (Coordinator) doing server attestation and handing out decryption keys. The server itself is operated by adtech.

Michael Kleber: Same model as the aggregate measurement server. Let me describe the execution model. See diagram in https://github.com/privacysandbox/fledge-docs/blob/main/key_value_service_trust_model.md#privacy-and-security-considerations

The computation has a bunch of parties in it, let's walk through who is who.

Codebase: open source in a github repo, everybody can read, reason about, build, possibly contribute to

Binary: take code at a given commit, do verifiable build using a hermetic toolchain. You get a server binary with a hash that can be verified, so we know it came from this exact code.

Browser: needs to be confident that it is talking to a server built verifiably in that way. That's why we put the server to run in a TEE.

Amazon is one particular cloud provider that offers TEE instances (AWS Nitro). A Nitro Enclave can cryptographically prove to anyone the version of software it's running, without the operator being able to interfere.

So ad tech is responsible for running an AWS Nitro instance that is executing the verifiably-built server.  When that server starts up, it contacts a special party responsible for the cryptographic keys.  That party does the verification that the server running inside the TEE is the real server, and then hands that server the private key.

How does the browser know it's talking to a trusted server? It can fetch the public key from the coordinator, and it knows that only a trusted server can get access to the private key.

John Mooring: Cool to see. Avoids an explosion of keys. Now, can we start including some contextual signals in the server request?

Michael: in FLEDGE beginning, we wanted to keep the set of signals small, to minimize trust in the BYOS server. Once we move to KV servers running in a trusted model, that opens up possibility for sending more signals.

We still require that this server only talks back to the browser, doesn't talk to other backends. 

In contrast PARAKEET allows talking to an untrusted adtech server

Criteo Gatekeeper server also allows talking to an untrusted backend

Can we pass URL of the page? This may get hard, you may want to parse the content. Not appropriate workload in the KV server. Even regularizing a URL can be tricky.  But this is the kind of thing we can look for ways to do, if we can fit it into things that work in a TEE setting

John: that's the answer I was hoping for. You said this can only talk back to browser -- how about talking to other trusted servers?

Michael: chaining calls means some subtle privacy questions (e.g. observing traffic pattern, metadata). But we're thinking about it

Brain May: Adtech sponsors the server environment. Cloud provider owns the hardware. Is that accurate?

Michael: sounds about right. 'Owner' may be an overloaded term

Brian: adtech will have limited access to the server

Phil: channels out of the server (debugging) will need to be designed carefully in order to prevent privacy leakage

Michael: Here is a question that I have, where I don't think I know the answer: seems plausible that each adtech could run their own KV server. It's also possible that KV server could be offered as a service by a company that will evolve to specialize in this. 

Brian's question about KV server instances starving each other may become relevant in the SAAS KV server case.

Stan Belov: Can Key Value server receive more contextual data? Understanding URL is a problem; we don't want to call untrusted backends. But, can contextual signals already computed by adtech (e.g. perBuyerSignals) be made available to the KV server?

Michael: this needs careful thinking through, but in general if adding this is useful and not problematic security/privacy wise it totally makes sense.

Alex Cone: How will things play out as adtechs run this? I see a clear use case for DSPs to run this. Why would a SSP need a KV server?

Michael: In the original FLEDGE design, KV server was DSP-only.  The use case we heard about was SSPs wanting to enforce policies on what creatives are allowed to run on a publisher's page. The SSP may scan the render\_url, have it reviewed, and the KV server may hold information about scanned/allowed creatives.

Alex: Will there be two separate codebases: one for SSP, one for DSP?

Michael: at a high level, it's a read only database. Only one server, works for both

Martin: to be pedantic, our server has two modes: SSP or DSP; you have to choose one at startup. Performance optimizations may end up diverging for the two use cases; we don;t know yet.

Brian: Understand that this is the beginning of a long road.  Sounds like extending the trust boundary to include server-side compute and storage.  With the introduction of UDFs, I wonder if we can extend all of worklets to run server-side instead of browser-side?  Not just worklets from the ad-tech side but also from the browser.

Michael: Yes indeed. That is a separate topic that I don't want to dive into too deeply right now. Issue 341 mentions two different proposals: UDFs (as discussed), and also "proposal for a bidding and auction server", and that is our early exploration of what Brian said.

There are tricky things to understand before we can run the auction on the client. Can help with adtech desire for bidding time compute power.

Alex Cone: Is the auction / bidding proposal getting its own forum, or be discussed here?

Michael: proposal too new to know the answer. This might be a great topic.

Michael: in 2 weeks, W3C TPAC happening, I'll say more in a minute. Let's not have this meeting in 2 weeks; meet again in 4 weeks.

Michael: FLEDGE available in a small fraction live traffic (chrome stable). There may be some hiccups, we'll let you know if we need to pause.

What is available for testing now (discussed for that over the last year) will not change based on discussion today. 

Announcing UDFs should alleviate concerns that migration from BYOS to trusted server will mean giving up this capability.

Think of "FLEDGE now" and "FLEDGE future" as two separate conversation threads. It may make sense to discuss them separately -- we'll need to find out.

Michael: I opened an issue yesterday; [issues/344](https://github.com/WICG/turtledove/issues/344). Details about W3C annual meeting called TPAC. Happening 2 wks from now in Vancouver BC (Canada). Weeklong meeting where business/community/interest groups will meet in hybrid mode: some people in person, some will dial in remotely. Michael will be present in person. Regularly scheduled W3C meetings (including this call) will likely not happen the TPAC week.  The WICG has scheduled an hour (9:30-10:30am on Friday) for us to talk about the FLEDGE and PARAKEET incubations.

Brian: I'm leaving on Friday, will miss Fri meetings. Will there be a recording?

Michael: we'll take notes, post as usual. No recording.

Looking forward to meeting folks in person in Vancouver.

Next call: September 28.
