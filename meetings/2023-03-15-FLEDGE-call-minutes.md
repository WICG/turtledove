# FLEDGE WICG Calls: Agenda & Notes

Calls take place on some Wednesdays, at 11am US Eastern time.

That's 8am California = 4pm Paris time (**THIS WEEK ONLY!**) = 3pm UTC (during summer).

This notes doc will be editable during the meeting — if you can only comment, hit reload

Notes from past calls are all on GitHub [in this directory](https://github.com/WICG/turtledove/tree/main/meetings).


# Next meeting: Wednesday March 15, 2023



*   NOTE that for March 15, the US has changed clocks for summer but Europe has not.
*   So EU and UK folks, please join an hour earlier than you consider normal, this time only!
*   Back to our usual time offsets on March 29th.


## Attendees: please sign yourself in!	



1. Michael Kleber (Google Privacy Sandbox)
2. Shawn Hong (Google Display & Video 360)
3. Don Marti (CafeMedia)
4. Orr Bernstein (Google Chrome)
5. Alonso Velasquez (Google Chrome)
6. Paul Jensen (Google Chrome)
7. Manny Isu (Google Chrome)
8. Alex Cone (Coir)
9. Jonasz Pamuła (RTB House)
10. Caleb Raitto (Google Chrome)
11. Priyanka Chatterjee (Google Privacy Sandbox)
12. Arun Nair (Google)
13. Matt Menke (Google Chrome)
14. David Dabbs (Epsilon)
15. Martin Pal (Google Privacy Sandbox)
16. Wendell Baker (Yahoo)
17. Harshad Mane (PubMatic)
18. Risako Hamano (Yahoo Japan)
19. Marco Lugo (NextRoll)
20. Kevin Nolan (NextRoll)
21. Kevin Lee (Google Chrome)
22. Dominic Farolino (Google Chrome)
23. Itay Sharfi (Google Chrome)
24. Maciej Zdanowicz (RTB House)
25. Xing Gao(Google)
26. Zheng Wei (Google)
27. Jaspreet Arora(Google Privacy Sandbox)
28. Andrey Prokofyev (Google)
29. Andrew Aikens (TripleLift)
30. Andrew Pascoe (NextRoll)
31. Peiwen Hu (Google Chrome)
32. Julien Aimonino (Criteo)
33. Daniel Rojas (Google Chrome)
34. Akshay Pundle (Google Privacy Sandbox)
35. Daniel Kocoj (Google Privacy Sandbox)
36. Roopal Nahar (Google Privacy Sandbox)
37. Rashmi Rao (Google Privacy Sandbox)
38. Abishai Gray (Google Chrome)
39. Bartosz Łoś (RTB House)
40. Matt Davies (Iponweb)


## Note taker: Manny Isu


## To join the speaker queue:

Please use the "Raise My Hand" feature in Google Meet.


# Agenda


### Process reminder: Join WICG

If you want to participate in the call, please make sure you join the WICG: https://www.w3.org/community/wicg/ 


## [Suggest agenda items here]

New agenda items:



*   Bidding and Auction Services (Itay Sharfi, Priyanka Chatterjee) 

    Updated Explainer: https://github.com/privacysandbox/fledge-docs/blob/main/bidding_auction_services_api.md

*   Migrating from URNs to FencedFrameConfigs to navigate fenced frames (Dominic Farolino). Summary doc: [FencedFrameConfig Transition](https://docs.google.com/document/d/1a6DFN3d4EEuWFM8lnk7uCeRmPFwniTGirilG33DjOM8/edit#)
*   Creative Macros proposal (Shawn Hong)


# Notes

**Bidding and Auction Services (Itay Sharfi, Priyanka Chatterjee)**



*   The current fledge on device flow - reseller code. The seller creates an auction config and then there is the auction on device, select winning ad, winning impression, and rendering the ad.
*   B&A is another performance optimization option - offloading the network compute to a TEE. Networking will be done within servers and not on a device. It is a new drop-in replacement for adtech.
*   **[Deck Presentation]**
    *   https://drive.google.com/file/d/1W1qXvdis--yyRyV1sO_PmUqXoUg9z00g/view
*   Near Drop In Replacement
    *   DSP
        *   Same
            *   IG based Targeting
            *   DSP Code
            *   BYOS Buyer KV
            *   RTB Buyer Stack
        *   New
            *   Cloud Investment: Operate 2 TS (BuyerFrontEnd, Bidding)
    *   SSP
        *   Same
            *   SSP Code
            *   BYOS Seller KV
        *   Same/New
            *   Ad Exchange supports HTTP POST with compressed payload 50-100KB
        *   New
            *   Seller’s Ad Service calls B&A
            *   Cloud Investment: Operate 2 TS (SellerFrontEnd, Auction)
*   **[B&A Architecture presentation]**
    *    slide number 8: ![Architecture diagram.](https://github.com/privacysandbox/fledge-docs/blob/main/images/unified-contextual-remarketing-bidding-auction-services.png)
*   [Harshad] In the current cookie world, not able to understand the second state and full state?
    *   The RTB world is exactly the same. Today there is 3PC - not much to distinguish what is for FLEDGE vs other kinds of targeting. The reason we have 2, 3 and 4 after that is that the FLEDGE auction cannot take part in the existing stack. The goal is open source and that is the big difference between the two items
    *   **Clarification:** The arrows within the diagram are bidirectional - we could update the diagram to show the response arrows, but it will not necessarily depict the orders in the way things happen.
*   [Bartosz Łoś] How will things work with communication with the buyer in terms of contextual signals? 
    *   We see a lot of threats around this, and we did not include it in our proposal. The contextual signals need to come back from the ad service.
    *   Everything in the flow is server to server, nothing goes back to the device
*   [Jonasz] For reportwin, what mechanisms like registeradbeacon - will it also work?
    *   [Paul] We can run the reportwin worklet in one the buyer’s services, and pass back to the device - this means response #1 will package up all the information inside the worklets and have the browser do the work at the end
    *   [Itay] B&A services will be fully compatible - multiseller, loss report
*   [Zheng] If reportwin is running a bidding server, then how does those signals get passed in the registerbeacon call?
    *   Contextual signals can flow unto the server. Also possibility of contextual signals coming back to the RTB servers
*   [David] Can you give us a little color on this proposal - is it separate from the ondevice model? Is it the same as the server based?
    *   [Itay] B&A is optional - we believe that ondevice is sufficient
    *   It is also the same code
    *   For current implementation, when a server is deciding if the auction should be ondevice or server, a decision has to be made - recommendation is to continue working as is and when B&A comes available, it can be activated if desired
    *   [David] It doesn’t feel like it is the same. This is not just a drop-in replacement.
    *   [Peiwen] The UDF is only specific to the trusted KV server, it is not part of B&A server.
    *   [Arun] The idea of the B&A is to maintain compatibility without much lift. The things you do on the chrome side should be compatible on B&A. We are also spending a lot of time thinking about the debugging aspect. We plan to have public explainers on some of these in the future.
    *   [David] On the KV server, there are much more involved response protocols - I think it will be mandated on the B&A services. Correct?
    *   [Itay] For KV server, no change using B&A. 
    *   [David] For encryption, it looks like there is one payload that has to go up - Is the data going out to the buyer, does this mean it has been encrypted with keys from the client and passed through to the seller?
    *   [Priyanka] We will stick to the overview explainer - the client fetches the public keys, the servers will fetch split private keys from KMS, and use that to decrypt the client. The response is also encrypted.
