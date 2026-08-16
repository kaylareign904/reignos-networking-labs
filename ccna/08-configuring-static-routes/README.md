# Lab 08 — Configuring Static Routes

## Overview

In this lab, I configured a network with **three routers and two LANs**, then added static routes so PC1 and PC2 could communicate across multiple routers.

The lab helped me practice IPv4 addressing, default gateways, router interface configuration, static routing, and verifying end-to-end connectivity.

## Lab Approach

I completed the device and IP addressing portion on my own.

For the static routing portion, I watched Jeremy's lab demonstration first because I was still a little uncertain about how to determine the destination network and next-hop address when configuring routes across multiple routers.

After watching the demonstration, I completed the static routing portion myself.

## What I Did

### 1. Configured PC1 and PC2

PC1 was configured on the `192.168.1.0/24` LAN.

```text
PC1 IP Address: 192.168.1.1
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.254
```

![PC1 IP address](<Configure PC1 Ip address.png>)

![PC1 default gateway](<Confgiure PC1 Default Gateway.png>)

PC2 was configured on the `192.168.3.0/24` LAN.

```text
PC2 IP Address: 192.168.3.1
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.3.254
```

![PC2 IP address](<configured PC2 ip address.png>)

![PC2 default gateway](<configured PC2 default gateway.png>)

### 2. Configured the Router Interfaces

I configured the router interfaces according to the topology.

The networks used in the lab were:

| Network | Purpose |
|---|---|
| 192.168.1.0/24 | PC1 LAN |
| 192.168.12.0/24 | R1 ↔ R2 |
| 192.168.13.0/24 | R2 ↔ R3 |
| 192.168.3.0/24 | PC2 LAN |

#### R1

```text
G0/1 → 192.168.1.254/24
G0/0 → 192.168.12.1/24
```

![R1 interface IP addresses](<configured R1 interface ip addresses.png>)

#### R2

```text
G0/0 → 192.168.12.2/24
G0/1 → 192.168.13.2/24
```

![R2 interface IP addresses](<configured R2 interface ip addresses.png>)

#### R3

```text
G0/0 → 192.168.13.3/24
G0/1 → 192.168.3.254/24
```

![R3 interface IP addresses](<configured R3 interface ip addresses.png>)

I used `show ip interface brief` to verify the interface IP addresses and confirm that the required interfaces were up.

### 3. Configured Static Routes

Each router already knew about its **directly connected networks**, but static routes were required for networks that were not directly connected.

#### R1 — Route Toward PC2's LAN

PC2 is on:

```text
192.168.3.0/24
```

From R1, the next router toward that network is R2 at:

```text
192.168.12.2
```

I configured:

```text
ip route 192.168.3.0 255.255.255.0 192.168.12.2
```

![R1 static route toward PC2 LAN](<R1 config-static route from R1 to R2 for PC2 LAN.png>)

#### R2 — Route Toward PC1's LAN

PC1 is on:

```text
192.168.1.0/24
```

I configured a static route toward R1.

![R2 static route toward PC1 LAN](<R2 config-static route from R2 to R1 LAN.png>)

#### R2 — Route Toward PC2's LAN

I also configured R2 with a route toward the `192.168.3.0/24` LAN through the R3 side of the topology.

![R2 static route toward PC2 LAN](<R2 config-static route from R2 to R3 LAN.png>)

#### R3 — Route Toward PC1's LAN

PC1 is on:

```text
192.168.1.0/24
```

From R3, the next router toward PC1 is R2 at:

```text
192.168.13.2
```

My final route was:

```text
ip route 192.168.1.0 255.255.255.0 192.168.13.2
```

![R3 static route toward PC1 LAN](<R3 config-static route from R3 to R2 for PC1 LAN.png>)

### 4. Tested End-to-End Connectivity

After configuring the routes, I pinged PC2 from PC1:

```text
ping 192.168.3.1
```

![PC1 successfully pinging PC2](<PC1 successfully ping PC2.png>)

The successful replies confirmed that traffic could travel from PC1 through R1, R2, and R3 to PC2 and return successfully.

## Troubleshooting

### Issue — PC1 Could Not Ping PC2

**Problem:**  
When I initially tried to ping PC2 from PC1, the requests timed out.

**Investigation:**  
I reviewed the static routes I had configured and remembered that on R3 I had specified the interface connected toward R2 instead of using R2's IP address as the next hop.

The R3-to-R2 connection uses:

```text
R3 G0/0 → 192.168.13.3
R2 G0/1 → 192.168.13.2
```

**Fix:**  
I changed the R3 route so the next hop was R2's IP address:

```text
ip route 192.168.1.0 255.255.255.0 192.168.13.2
```

After making the change, the ping from PC1 to PC2 was successful.

**What I learned:**  
When using a next-hop IP address in a static route, the address after the subnet mask should identify the **neighboring router that should receive the packet next**, not my own outgoing interface.

## Understanding Static Route Syntax

This is the part of the lab I want to continue practicing.

The basic syntax is:

```text
ip route <destination-network> <subnet-mask> <next-hop-ip>
```

I can think about it as:

```text
Where am I trying to go?
        ↓
Destination network

What size is that network?
        ↓
Subnet mask

Who should I give the packet to next?
        ↓
Next-hop router IP
```

For example, from R3:

```text
ip route 192.168.1.0 255.255.255.0 192.168.13.2
```

means:

```text
Destination network: 192.168.1.0
Subnet mask:         255.255.255.0
Next-hop router:     192.168.13.2
```

R3 does not need the IP address of PC1 in the route. It needs the **network PC1 belongs to** and the address of the **next router along the path**.

## Key Commands

```text
show ip interface brief
show ip route

ip route <destination-network> <subnet-mask> <next-hop-ip>

ping 192.168.3.1
```

Examples from this lab:

```text
R1:
ip route 192.168.3.0 255.255.255.0 192.168.12.2

R3:
ip route 192.168.1.0 255.255.255.0 192.168.13.2
```

## What I Learned

- Routers automatically know their directly connected networks.
- Static routes are needed when a router needs to reach a network it does not already know about.
- The first address in an `ip route` command identifies the **destination network**, not a specific PC.
- The subnet mask identifies the size of that destination network.
- A next-hop IP identifies the neighboring router that should receive the packet next.
- Static routing has to support traffic in both directions for end-to-end communication to work.
- `show ip route` helps verify which routes a router currently knows.
- I need more practice determining destination networks and next-hop addresses when several routers are involved.

## Study Source

Completed as **Day 11 — Configuring Static Routes** of my CCNA studies using **Jeremy's IT Lab**.

This is **Lab 08** in my GitHub CCNA lab portfolio.
