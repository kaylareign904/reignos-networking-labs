# Lab 10 — Life of a Packet

## Overview

In this lab, I analyzed how **source and destination MAC addresses change as traffic moves through a network**.

Instead of configuring devices, I worked through questions in the provided Packet Tracer topology and identified the MAC addresses used at different points along the path between end devices.

## What I Did

### 1. Traced Traffic Between Different Networks

The topology included two LANs connected through three routers:

```text
PC1 LAN → R1 → R2 → R3 → PC4 LAN
```

I worked through the path of a ping from **PC1 to PC4** and identified the source and destination MAC addresses used on each network segment.

This helped me see that although the packet continues toward the same final destination, the **Ethernet frame changes as it passes through routers**.

### 2. Traced Traffic Within the Same LAN

I also analyzed communication between **PC1 and PC3**, which are on the same `192.168.1.0/24` network.

Because the traffic stays within the same LAN, the frame does not need to pass through a router.

### 3. Traced the Return Path

I then worked through traffic traveling in the opposite direction from **PC4 back to PC1**.

This gave me practice identifying the correct source and destination MAC addresses at each point as the packet moved back across the routers.

![Identifying source and destination MAC addresses as packets move through networks](<lab 10 - identifying the destination and source mac address as packet moves through networks.png>)

*Completed Lab 10 questions identifying source and destination MAC addresses at different points in the network.*

## Key Concepts

| Concept | What I Practiced |
|---|---|
| Source MAC | Identifying the device sending the current Ethernet frame |
| Destination MAC | Identifying the next Layer 2 destination |
| Same LAN | Frames can be delivered without passing through a router |
| Different Networks | Traffic must pass through routers |
| Router Hop | A new Layer 2 frame is created for the next network segment |
| IP Addresses | Continue identifying the original source and final destination at Layer 3 |
| MAC Addresses | Change based on the current Layer 2 segment |

## What I Learned

- MAC addresses are used for communication on the **current local network segment**.
- When a packet crosses a router, the Layer 2 frame is changed for the next segment.
- The source MAC becomes the MAC address of the interface sending the new frame.
- The destination MAC becomes the MAC address of the next device on that local segment.
- Switches forward frames within the LAN without replacing the original source and destination MAC addresses.
- Traffic between devices on the same LAN does not need to pass through a router.
- Following a packet hop-by-hop helped me better understand the difference between **Layer 2 delivery and Layer 3 routing**.

## Study Source

Completed as **Day 12 — Life of a Packet** of my CCNA studies using **Jeremy's IT Lab**.

This is **Lab 10** in my GitHub CCNA lab portfolio.
