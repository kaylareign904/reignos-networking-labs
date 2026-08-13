# Lab 03 — Identifying OSI Model Layers

## Overview

In this lab, I used **Cisco Packet Tracer Simulation Mode** to inspect network traffic and identify which OSI layers were involved in different PDUs.

I observed Layer 2 traffic and generated DHCP traffic so I could see how protocols at multiple layers work together during network communication.

## Lab Approach

Before completing the lab myself, I watched Jeremy's lab demonstration because two parts were new to me:

- Packet Tracer **Simulation Mode**
- `ipconfig /release` and `ipconfig /renew`

After seeing how those tools worked, I completed the lab and used the PDU information to identify the OSI layers involved in the traffic.

## What I Did

### 1. Used Simulation Mode to Inspect Layer 2 Traffic

I opened an STP event and reviewed its PDU information.

![STP PDU being analyzed in Packet Tracer](stp-layer-2-analysis.png)

*Inspecting an STP PDU in Packet Tracer Simulation Mode.*

The PDU showed activity at **Layer 2 — Data Link**, including the Ethernet header and STP BPDU.

This showed me that some network protocols can operate without using the upper OSI layers.

### 2. Generated DHCP Traffic

On PC1, I used:

```text
ipconfig
ipconfig /release
ipconfig /renew
```

to release and renew its DHCP-assigned network configuration.

![Generating DHCP traffic using ipconfig](dhcp-release-renew.png)

*Using `ipconfig /release` and `ipconfig /renew` to generate DHCP traffic for analysis.*

The purpose of these commands in this lab was to create traffic that I could inspect in Simulation Mode.

### 3. Identified the Layers Used by DHCP

I opened the DHCP PDU and reviewed the information shown at each active OSI layer.

![DHCP PDU analyzed through the OSI model](dhcp-osi-analysis.png)

*Inspecting DHCP traffic through multiple OSI layers.*

I observed:

| Layer | Information Observed |
|---|---|
| Layer 7 — Application | DHCP |
| Layer 4 — Transport | UDP |
| Layer 3 — Network | IP addressing |
| Layer 2 — Data Link | Ethernet and MAC addressing |
| Layer 1 — Physical | Physical transmission/interface |

This helped me visualize how an application-layer protocol relies on lower layers to move across the network.

## Verification

I verified the lab by:

- Entering Simulation Mode
- Opening individual PDUs
- Identifying which OSI layers were active
- Observing STP as Layer 2 traffic
- Generating DHCP traffic from PC1
- Following DHCP through Layers 7, 4, 3, 2, and 1

## What I Learned

- Packet Tracer Simulation Mode lets me inspect what is happening inside network traffic.
- Different protocols do not necessarily use every OSI layer.
- STP demonstrated Layer 2 communication.
- DHCP allowed me to see multiple OSI layers working together.
- A single communication can include DHCP, UDP, IP, Ethernet, and physical transmission.
- The OSI model is more useful when I connect each layer to traffic I can actually observe.

## Study Source

Completed as part of my CCNA studies using **Jeremy's IT Lab**.

This is **Lab 03** in my GitHub CCNA lab portfolio.
