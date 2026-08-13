# Lab 07 — Interface Configuration

## Overview

In this lab, I configured and managed interfaces on **R1, SW1, and SW2** in Cisco Packet Tracer.

The lab focused on IPv4 addressing, interface speed and duplex, interface descriptions, verifying port status, and disabling unused switch ports.

## What I Did

### 1. Configured Device Hostnames

I changed the default hostnames to match the topology.

```text
hostname R1
hostname SW1
hostname SW2
```

![R1 hostname change](<R1 hostname change.png>)

![SW1 hostname change](<SW1 hostname change.png>)

![SW2 hostname change](<SW2 hostname change.png>)

### 2. Configured IPv4 Addressing

I configured R1 G0/0 with:

```text
interface g0/0
ip address 172.16.255.254 255.255.0.0
no shutdown
```

![R1 IP address configuration](<R1 ip address set.png>)

I also assigned static IPv4 addresses to the four PCs on the `172.16.0.0/16` network.

| Device | IPv4 Address | Subnet Mask |
|---|---|---|
| PC1 | 172.16.0.1 | 255.255.0.0 |
| PC2 | 172.16.0.2 | 255.255.0.0 |
| PC3 | 172.16.0.3 | 255.255.0.0 |
| PC4 | 172.16.0.4 | 255.255.0.0 |

![PC1 IP address](<PC1 ip address set.png>)

![PC2 IP address](<PC2 ip address set.png>)

![PC3 IP address](<PC3 ip address set.png>)

![PC4 IP address](<PC4 ip address set.png>)

### 3. Configured Speed, Duplex, and Descriptions

For links between networking devices, I manually configured:

```text
speed 1000
duplex full
```

This included:

- R1 G0/0 ↔ SW1 G0/1
- SW1 G0/2 ↔ SW2 G0/1

![R1 G0/0 speed and duplex](<R1-G-00 config speed-duplex.png>)

![SW1 G0/1 speed and duplex](<SW1-G01 config speed-duplex.png>)

![SW1 G0/2 speed and duplex](<SW1-G02 config speed-duplex.png>)

![SW2 G0/1 speed and duplex](<SW2-G01 config speed-duplex.png>)

I also added interface descriptions to identify connected devices.

![SW2 interface descriptions](<SW2 Fa interface configuration -descriptions.png>)

### 4. Verified Interface Status

I used:

```text
show interfaces status
```

to verify interface descriptions, connection status, speed, and duplex.

![SW1 interface status](<SW1 displaying interfaces and status.png>)

![SW2 interface status](<SW2 displaying interfaces and status.png>)

### 5. Disabled Unused Switch Ports

I used `interface range` to configure multiple unused ports at the same time.

```text
interface range f0/3 - 0/24
description ## not in use ##
shutdown
```

![Disabling unused switch ports](<SW2 configuring unused interfaces and disabling.png>)

This allowed me to disable unused interfaces much faster than configuring each port individually.

## Troubleshooting

### Issue 1 — IP Address Command Would Not Work

**Problem:**  
I tried to enter:

```text
ip address 172.16.255.254 255.255.0.0
```

from Global Configuration mode and received an error.

**Cause:**  
I had not entered the configuration mode for R1's G0/0 interface.

**Fix:**

```text
interface g0/0
ip address 172.16.255.254 255.255.0.0
```

**What I learned:**  
An IP address must be configured from Interface Configuration mode, not directly from Global Configuration mode.

### Issue 2 — SW1 G0/1 Showed as Unconnected

**Problem:**  
After configuring speed and duplex, SW1 G0/1 showed as **unconnected**, even though it was physically connected to R1 G0/0.

**Investigation:**  
I checked R1 using:

```text
show ip interface brief
```

and found that R1 G0/0 was:

```text
administratively down
```

**Fix:**

```text
interface g0/0
no shutdown
```

![Enabling R1 G0/0](<R1 enabling the interface as it was down.png>)

After enabling R1 G0/0, SW1 G0/1 showed as connected.

**What I learned:**  
When troubleshooting a link, I should check **both ends of the connection**. The issue appeared on SW1, but the root cause was the router interface on the other side being administratively down.

## Key Commands

```text
hostname R1
hostname SW1
hostname SW2

interface g0/0
ip address 172.16.255.254 255.255.0.0
no shutdown

speed 1000
duplex full

description ## to DEVICE ##

show ip interface brief
show interfaces status

interface range f0/3 - 0/24
description ## not in use ##
shutdown
```

## What I Learned

- Cisco IOS commands must be entered from the correct configuration mode.
- Router interfaces may need `no shutdown` before a link becomes operational.
- `show interfaces status` provides a quick view of switch port status, speed, duplex, and descriptions.
- Both sides of a link should be checked during troubleshooting.
- `interface range` makes configuring multiple ports faster.
- Unused switch ports can be administratively disabled until needed.

## Study Source

Completed as **Day 09 — Interface Configuration** of my CCNA studies using **Jeremy's IT Lab**.

This is **Lab 07** in my GitHub portfolio because not every course day contains a hands-on lab.
