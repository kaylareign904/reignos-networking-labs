# Lab 11 — VLSM

## Overview

In this lab, I used **Variable Length Subnet Masking (VLSM)** to divide the `192.168.5.0/24` network based on the host requirements of four LANs and a point-to-point connection between R1 and R2.

I calculated the required subnets by hand, assigned IP addresses to the PCs and router interfaces, configured static routes, and verified that all four PCs could communicate.

## What I Did

### 1. Calculated the Required Subnets

Before configuring anything in Packet Tracer, I worked through the VLSM calculations by hand.

The topology required addressing for:

- LAN1 — 45 hosts
- LAN2 — 64 hosts
- LAN3 — 14 hosts
- LAN4 — 9 hosts
- R1 ↔ R2 — 2 hosts

I started with the subnet requiring the most hosts and worked from largest to smallest.

For each subnet, I determined:

- Required host bits
- Prefix length
- Network address
- Broadcast address
- Usable host range

My final addressing plan was:

| Network | Host Requirement | Subnet | Usable Host Range | Broadcast |
|---|---:|---|---|---|
| LAN2 | 64 | `192.168.5.0/25` | 192.168.5.1 – 192.168.5.126 | 192.168.5.127 |
| LAN1 | 45 | `192.168.5.128/26` | 192.168.5.129 – 192.168.5.190 | 192.168.5.191 |
| LAN3 | 14 | `192.168.5.192/28` | 192.168.5.193 – 192.168.5.206 | 192.168.5.207 |
| LAN4 | 9 | `192.168.5.208/28` | 192.168.5.209 – 192.168.5.222 | 192.168.5.223 |
| R1 ↔ R2 | 2 | `192.168.5.224/30` | 192.168.5.225 – 192.168.5.226 | 192.168.5.227 |

![Calculated VLSM subnets](<Calculated subnets for LAN 1 -4 -P2P.png>)

### My Handwritten Calculations

I documented the calculations I used to determine the subnet sizes, host bits, prefix lengths, network addresses, and broadcast addresses.

📄 **[View my handwritten VLSM calculations](<a01c1998-1ba0-4baa-987b-4c5ef429348c.pdf>)**

These calculations helped me determine the addressing plan before applying it in Packet Tracer.

### 2. Assigned PC and Gateway Addresses

The lab required the **first usable address** in each LAN to be assigned to the PC and the **last usable address** to the router interface.

For example, PC1 was configured with:

```text
IP Address: 192.168.5.129
Subnet Mask: 255.255.255.192
Default Gateway: 192.168.5.190
```

![PC1 address and subnet mask](<PC1 Address set with subnet mask.png>)

![PC1 default gateway](<PC1 Default set to R1 g0:0.png>)

The same addressing method was used for the remaining LANs.

### 3. Configured Router Interfaces and Static Routes

After assigning the router interface addresses, I configured static routes so R1 and R2 could reach the LANs on the opposite side of the point-to-point connection.

I used:

```text
show ip interface brief
show ip route
```

to verify the interface addressing and routing tables.

![R1 interfaces and static routes](<R1 Interfaces and static routes configured.png>)

![R2 interfaces and static routes](<R2 interfaces and static routes configured.png>)

## Troubleshooting

### Issue — `%Inconsistent address and mask`

**Problem:**  
While configuring static routes, IOS returned:

```text
%Inconsistent address and mask
```

I was using the **PC's actual IP address** as the destination instead of the network address of the subnet the PC belonged to.

For example, PC3 uses:

```text
192.168.5.193/28
```

but PC3 belongs to the network:

```text
192.168.5.192/28
```

**Cause:**  
A network static route needs the **destination network address** and its matching subnet mask.

I was entering the host address instead of the subnet address.

**Fix:**  
After researching the error and reviewing the static route syntax, I changed the destination to the PC's **network address**.

For example:

```text
ip route 192.168.5.192 255.255.255.240 <next-hop>
```

Once I used the correct subnet addresses and masks, I was able to configure the required static routes on R1 and R2.

**What I learned:**  
The destination in a normal network static route identifies the **network I want to reach**, not the individual host inside that network.

## Verification

After completing the VLSM addressing and static routing configuration, I tested connectivity between all four PCs.

### PC1

![PC1 successfully pinging the other PCs](<PC1 successful pings to other PCs.png>)

### PC2

![PC2 successfully pinging the other PCs](<PC2 successful pings to other PCs.png>)

### PC3

![PC3 successfully pinging the other PCs](<PC3 successful pings to other PCs.png>)

### PC4

![PC4 successfully pinging the other PCs](<PC4 successful pings to other PCs.png>)

Successful pings between all four LANs confirmed that the VLSM addressing and static routes were working correctly.

## Key Concepts

| Concept | What I Practiced |
|---|---|
| VLSM | Assigning different subnet sizes based on host requirements |
| Network Address | Identifying the beginning of each subnet |
| Broadcast Address | Identifying the final address in each subnet |
| Usable Range | Determining addresses available to hosts |
| `/25` | Used for the largest LAN |
| `/26` | Used for the 45-host LAN |
| `/28` | Used for the smaller LANs |
| `/30` | Used for the R1-to-R2 point-to-point connection |
| Static Routing | Routing traffic between the different subnets |

## Key Commands

```text
show ip interface brief
show ip route

ip route <destination-network> <subnet-mask> <next-hop>

ping <destination-ip>
```

## What I Learned

- VLSM allows different subnet sizes to be assigned based on actual host requirements.
- I should work from the largest host requirement to the smallest when building a VLSM addressing plan.
- The network and broadcast addresses cannot be assigned to hosts.
- Different prefix lengths provide different numbers of usable addresses.
- A `/30` provides two usable addresses for the point-to-point router connection in this lab.
- Static routes use the **destination network address**, not the individual PC address.
- The subnet mask in the static route must match the destination subnet.
- Successful pings between all four PCs confirmed that both the subnetting and routing were configured correctly.

## Study Source

Completed as part of **Days 13–15 — Subnetting/VLSM** of my CCNA studies using **Jeremy's IT Lab**.

The Packet Tracer exercise was the **Day 15 — VLSM Lab**.

This is **Lab 11** in my GitHub CCNA lab portfolio.
