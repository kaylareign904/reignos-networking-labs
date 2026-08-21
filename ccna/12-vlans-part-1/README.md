# Lab 12 — VLANs (Part 1)

## Overview

In this lab, I configured **three VLANs** on SW1 and separated six PCs into Engineering, HR, and Sales networks.

I also configured one R1 interface as the default gateway for each VLAN, verified connectivity between VLANs, and used a broadcast ping to observe how broadcasts stay within their VLAN.

## What I Did

### 1. Used Three Separate VLAN Networks

The topology used three `/26` networks:

| VLAN | Name | Network | PCs | Gateway |
|---|---|---|---|---|
| VLAN 10 | Engineering | `10.0.0.0/26` | PC1 `.1`, PC2 `.2` | `10.0.0.62` |
| VLAN 20 | HR | `10.0.0.64/26` | PC3 `.65`, PC4 `.66` | `10.0.0.126` |
| VLAN 30 | Sales | `10.0.0.128/26` | PC5 `.129`, PC6 `.130` | `10.0.0.190` |

The **last usable address** in each subnet was used as its default gateway.

### 2. Configured R1 as the Gateway for Each VLAN

R1 used a separate physical interface for each VLAN:

```text
G0/0 → 10.0.0.62/26  (VLAN 10)
G0/1 → 10.0.0.126/26 (VLAN 20)
G0/2 → 10.0.0.190/26 (VLAN 30)
```

![R1 gateway addresses for each VLAN](<shows the gateway address for all vlans-the R1 interface ip addresses .png>)

Each PC was configured to use the router interface in its own subnet as its default gateway.

### 3. Configured SW1 Access Ports and VLAN Names

I assigned the switch interfaces to their appropriate VLANs using access mode.

Example:

```text
switchport mode access
switchport access vlan 10
```

I repeated the process for VLANs 20 and 30 and named them:

```text
vlan 10
name Engineering

vlan 20
name HR

vlan 30
name Sales
```

I then used:

```text
show vlan brief
```

to verify the VLANs and port assignments.

![SW1 VLAN configuration and names](<config switch interfaces with vlans-names.png>)

The port groups were:

```text
VLAN 10 → G0/1, F3/1, F4/1
VLAN 20 → G1/1, F5/1, F6/1
VLAN 30 → G2/1, F7/1, F8/1
```

### 4. Tested Communication Between VLANs

From PC1 in VLAN 10, I pinged PCs located in the other VLANs.

![PC1 successfully pinging PCs in other VLANs](<PC1 ping PCs in other VLANs successful.png>)

The successful pings confirmed that devices in the separate VLANs could communicate through R1.

This helped demonstrate that VLANs separate devices at Layer 2, while a router is required for communication between different VLAN networks.

### 5. Tested a Broadcast Ping

I also sent a ping to VLAN 10's broadcast address:

```text
ping 10.0.0.63
```

![PC1 pinging the VLAN broadcast address](<PC1 pinging Broadcast IP.png>)

I used Packet Tracer Simulation Mode to observe which devices received the broadcast.

The broadcast remained within **VLAN 10** instead of being forwarded into VLAN 20 or VLAN 30.

## Key Commands

```text
switchport mode access
switchport access vlan <vlan-id>

vlan 10
name Engineering

vlan 20
name HR

vlan 30
name Sales

show vlan brief

ping <destination-ip>
ping <broadcast-address>
```

## What I Learned

- VLANs logically separate switch ports into different Layer 2 networks.
- Devices in different VLANs need Layer 3 routing to communicate.
- Access ports belong to a specific VLAN.
- VLANs can be assigned names to make their purpose easier to identify.
- Each VLAN in this lab used its own IP subnet and default gateway.
- A broadcast sent inside one VLAN stays within that VLAN's broadcast domain.
- `show vlan brief` provides a quick way to verify VLANs and their assigned switch ports.

## Study Source

Completed as **Day 16 — VLANs (Part 1)** of my CCNA studies using **Jeremy's IT Lab**.

This is **Lab 12** in my GitHub CCNA lab portfolio.
