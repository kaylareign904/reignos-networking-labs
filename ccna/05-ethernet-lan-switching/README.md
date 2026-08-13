# Lab 05 — Ethernet LAN Switching

## Overview

In this lab, I used **Cisco Packet Tracer Simulation Mode** to observe how switches learn MAC addresses and how ARP and ICMP traffic move across a switched LAN.

The lab started with empty dynamic MAC address tables on the switches and empty ARP tables on the PCs. I used communication between PC1 and PC3 to observe how ARP, MAC learning, and ICMP work together.

## What I Did

### 1. Predicted PC1-to-PC3 Communication

Before running the simulation, I worked through what I expected to happen when PC1 attempted to ping PC3.

Because PC1 knew PC3's IP address but did not initially know its MAC address, I expected ARP to occur before the ICMP ping could be delivered.

![PC1 to PC3 communication predictions](<lab-day 6-question 1- predictions.png>)

My prediction included:

1. PC1 sending an ARP request
2. The ARP request being broadcast across the LAN
3. The switches learning MAC addresses from incoming frames
4. PC3 responding with its MAC address
5. PC1 then sending the ICMP echo request
6. PC3 returning an ICMP echo reply

### 2. Verified Communication Between PC1 and PC3

I used:

```text
ping 192.168.1.3
```

from PC1.

![PC1 ping to PC3](<PC1 ping to PC3 to verify question 1.png>)

Using Simulation Mode, I observed both **ARP** and **ICMP** traffic.

Before PC1 could send the ping normally, ARP was used to determine the MAC address associated with PC3's IP address.

The ARP request used the Ethernet broadcast MAC address:

```text
FFFF.FFFF.FFFF
```

Once PC1 learned PC3's MAC address, the ICMP traffic could be sent as unicast traffic.

### 3. Viewed PC1's ARP Table

After PC1 communicated with PC3, I checked its ARP table using:

```text
arp -a
```

![PC1 ARP table](<PC1 arp table after ping PC3.png>)

The ARP table now contained a dynamic mapping for PC3.

This helped me understand that an ARP table maps:

```text
IP address → MAC address
```

### 4. Viewed SW1's MAC Address Table

I checked SW1 using:

```text
show mac address-table
```

![SW1 MAC address table](<show mac table for SW1.png>)

The MAC address table showed dynamically learned MAC addresses and the switch interfaces associated with them.

This reinforced that switches learn MAC addresses from the **source MAC address** of incoming Ethernet frames.

A switch's MAC address table maps:

```text
MAC address → switch interface
```

### 5. Cleared Dynamic MAC Addresses

I cleared SW1's dynamically learned MAC address entries using:

```text
clear mac address-table dynamic
```

I then viewed the MAC address table again.

![Clearing dynamic MAC addresses on SW1](<clearing dynamic mac address for SW1.png>)

The dynamic entries were removed and would need to be learned again as new traffic passed through the switch.

## Key Concepts

| Concept | What I Observed |
|---|---|
| ARP Request | Sent as an Ethernet broadcast |
| Broadcast MAC | `FFFF.FFFF.FFFF` |
| MAC Learning | Switches learn from source MAC addresses |
| Known Unicast | Forwarded toward the known destination |
| Unknown Unicast | Flooded when the destination MAC is unknown |
| ARP Table | Maps IP addresses to MAC addresses |
| MAC Address Table | Maps MAC addresses to switch interfaces |
| ICMP | Used for the ping after address resolution |

## Key Commands

```text
ping 192.168.1.3
arp -a

show mac address-table
clear mac address-table dynamic
```

## What I Learned

- ARP is used when a host knows an IPv4 address but still needs the destination MAC address.
- ARP requests use the Ethernet broadcast address `FFFF.FFFF.FFFF`.
- Switches learn MAC addresses from the source MAC address of incoming frames.
- A PC's ARP table maps **IP addresses to MAC addresses**.
- A switch's MAC address table maps **MAC addresses to switch interfaces**.
- A ping may require ARP before the ICMP traffic can be delivered.
- Dynamic MAC address entries can be cleared and learned again when new traffic crosses the switch.

## Study Source

Completed as part of my CCNA studies using **Jeremy's IT Lab**.

This is **Lab 05** in my GitHub CCNA lab portfolio.
