# Lab 09 — Troubleshooting Static Routes

## Overview

In this lab, I troubleshot a network where **PC1 and PC2 could not communicate because each router contained one misconfiguration**.

I used interface and routing information to identify the problems on R1, R2, and R3, corrected each configuration, and verified connectivity in both directions.

## What I Did

### 1. Checked R1

I started by checking R1's interfaces using:

```text
show ip interface brief
```

![Checking R1 interfaces](<R1 troubleshooting check ip interface table.png>)

The interfaces were correctly addressed and operational, so I checked the routing table.

I found that R1's static route to the `192.168.3.0/24` network was using the wrong next-hop address:

```text
192.168.12.3
```

![R1 misconfigured static route](<R1 route to R2 IP misconfigured.png>)

R2's actual address on the directly connected `192.168.12.0/24` network was:

```text
192.168.12.2
```

I removed the incorrect route and added the correct next hop:

```text
no ip route 192.168.3.0 255.255.255.0 192.168.12.3
ip route 192.168.3.0 255.255.255.0 192.168.12.2
```

![Fixed R1 static route](<Fix R1 route to R2 IP.png>)

### 2. Checked R2

I checked R2's interface addressing first.

```text
show ip interface brief
```

![Checking R2 interfaces](<R2 troubelshooting check ip int table.png>)

I then reviewed its routes and found that the route toward PC2's `192.168.3.0/24` LAN was configured using:

```text
GigabitEthernet0/0
```

![R2 misconfigured route toward R3](<R2 to R3 interface misconfigured.png>)

G0/0 connects R2 toward **R1**, while traffic for PC2 should travel toward **R3 through G0/1**.

I attempted to replace the incorrect interface with G0/1, but I ran into an issue in Packet Tracer when changing the route.

Instead, I configured the route using R3's next-hop IP address on the directly connected link:

```text
192.168.13.3
```

The corrected route was:

```text
ip route 192.168.3.0 255.255.255.0 192.168.13.3
```

![Fixed R2 route toward R3](<Fix R2 to R3 IP.png>)

Using the next-hop address also helped reinforce that a static route can tell the router **which neighboring router should receive the packet next**, rather than only specifying an exit interface.

### 3. Checked R3

On R3, I used:

```text
show ip interface brief
```

and noticed that G0/0 had:

```text
192.168.23.3
```

![R3 G0/0 incorrect IP address](<R3 interface 00 ip address misconfigured.png>)

The network between R2 and R3 is:

```text
192.168.13.0/24
```

so R3 G0/0 should have been:

```text
192.168.13.3
```

I corrected the interface configuration:

```text
interface g0/0
no ip address
ip address 192.168.13.3 255.255.255.0
```

I then verified the change with:

```text
do show ip interface brief
```

![Fixed R3 G0/0 IP address](<Fix R3 int 00 ip address.png>)

## Verification

After correcting all three routers, I tested communication between the two LANs.

### PC1 to PC2

```text
ping 192.168.3.1
```

![PC1 successfully pinging PC2](<PC1 successfully pings PC2.png>)

### PC2 to PC1

```text
ping 192.168.1.1
```

![PC2 successfully pinging PC1](<PC2 successfully pings PC1.png>)

Successful replies in both directions confirmed that the interface addressing and static routes were now working correctly.

## Troubleshooting Summary

| Device | Problem | Fix |
|---|---|---|
| R1 | Wrong next hop for `192.168.3.0/24` | Changed `192.168.12.3` to `192.168.12.2` |
| R2 | Route toward PC2 LAN used the wrong outgoing interface | Used R3 next-hop address `192.168.13.3` |
| R3 | G0/0 used `192.168.23.3` instead of `192.168.13.3` | Corrected G0/0 to `192.168.13.3/24` |

## Key Commands

```text
show ip interface brief
show ip route
show running-config | include ip route

no ip route <destination> <mask> <next-hop-or-interface>
ip route <destination> <mask> <next-hop>

interface g0/0
no ip address
ip address <address> <mask>

ping <destination>
```

## What I Learned

- `show ip interface brief` helps me quickly identify incorrect interface addressing.
- `show ip route` helps me verify where a router currently plans to send traffic.
- A static route must point in the correct direction toward the destination network.
- When using a next-hop static route, the next-hop IP should belong to the neighboring router along the path.
- A single incorrect router interface address can prevent communication across multiple networks.
- Testing connectivity in **both directions** helps confirm that the complete routing path is working.
- Troubleshooting one router at a time made it easier to isolate each problem instead of changing multiple configurations at once.

## Study Source

Completed as **Day 11 Part 2 — Troubleshooting Static Routes** of my CCNA studies using **Jeremy's IT Lab**.

This is **Lab 09** in my GitHub CCNA lab portfolio.
