# Lab 06 — IPv4 Addresses

## Overview

In this lab, I configured IPv4 addressing on **R1 and three PCs across three separate networks**.

I assigned IP addresses and subnet masks, enabled and described router interfaces, verified the configuration, saved it, and tested communication between the networks.

## What I Did

### 1. Configured R1's Hostname

I changed the router hostname to R1.

```text
enable
configure terminal
hostname R1
```

![Configuring R1 hostname](<configure R1 hostname.png>)

### 2. Checked R1's Interfaces Before Configuration

Before assigning IP addresses, I checked the current interface status using:

```text
do show ip interface brief
```

![R1 interfaces before configuration](<R1 IP Interface table before changes.png>)

The interfaces were initially **unassigned** and **administratively down**.

### 3. Configured R1's Interfaces

I configured an interface for each of the three networks.

| Interface | IPv4 Address | Subnet Mask | Connected To |
|---|---|---|---|
| G0/0 | 15.255.255.254 | 255.0.0.0 | SW1 |
| G0/1 | 182.98.255.254 | 255.255.0.0 | SW2 |
| G0/2 | 201.191.20.254 | 255.255.255.0 | SW3 |

Example configuration:

```text
interface g0/0
ip address 15.255.255.254 255.0.0.0
description ## to SW1 ##
no shutdown
```

I repeated the process for G0/1 and G0/2 using their assigned addresses.

![Configuring R1 interfaces](<configuring R1 interfaces with IP desc and status.png>)

### 4. Verified R1's Interface Status

After configuring the interfaces, I checked them again:

```text
do show ip interface brief
```

![R1 interfaces after configuration](<R1 IP interface table after changes.png>)

All three configured interfaces now showed:

```text
up / up
```

I also reviewed the running configuration:

```text
show running-config
```

![R1 running configuration](<R1 running-config file.png>)

### 5. Saved R1's Configuration

I saved the running configuration to startup configuration:

```text
copy running-config startup-config
```

![Saving R1 configuration](<copying running-config to startup-config for R1.png>)

### 6. Configured the PCs

Each PC was assigned a static IPv4 address in one of the three networks.

#### PC1 — 15.0.0.0/8

```text
IPv4 Address: 15.0.0.1
Subnet Mask: 255.0.0.0
```

![PC1 IPv4 configuration](<config PC1 IP Address.png>)

#### PC2 — 182.98.0.0/16

```text
IPv4 Address: 182.98.0.1
Subnet Mask: 255.255.0.0
```

![PC2 IPv4 configuration](<config PC2 IP address.png>)

#### PC3 — 201.191.20.0/24

```text
IPv4 Address: 201.191.20.1
Subnet Mask: 255.255.255.0
```

![PC3 IPv4 configuration](<config PC3 IP address.png>)

This gave me hands-on practice working with **/8, /16, and /24** networks in the same topology.

### 7. Tested Connectivity Between Networks

From PC1, I tested communication with PC2 and PC3:

```text
ping 182.98.0.1
ping 201.191.20.1
```

![PC1 pinging PC2 and PC3](<PC1 ping PC2-PC3.png>)

Both remote networks were reachable through R1.

## Troubleshooting

### Issue — Finding the Correct Interface Summary Command

**Problem:**  
I first used:

```text
do show interfaces
```

The command worked, but it displayed much more interface information than I needed.

I remembered using a `brief` command in the lecture and tried:

```text
do show interfaces brief
```

but received an error.

I also tried:

```text
show ip interfaces brief
```

which was incorrect.

**Fix:**  
I checked my notes and found the correct syntax:

```text
do show ip interface brief
```

The important difference was using `ip` and **`interface` singular**.

**What I learned:**  
`show interfaces` provides detailed interface information, while `show ip interface brief` gives me a quick summary of IP addresses and interface status. This also reinforced the importance of exact Cisco IOS command syntax.

## Key Commands

```text
hostname R1

show interfaces
show ip interface brief
do show ip interface brief

interface g0/0
ip address 15.255.255.254 255.0.0.0
description ## to SW1 ##
no shutdown

interface g0/1
ip address 182.98.255.254 255.255.0.0
description ## to SW2 ##
no shutdown

interface g0/2
ip address 201.191.20.254 255.255.255.0
description ## to SW3 ##
no shutdown

show running-config
copy running-config startup-config

ping 182.98.0.1
ping 201.191.20.1
```

## What I Learned

- Router interfaces need an IP address and subnet mask for their connected network.
- `no shutdown` enables an administratively disabled router interface.
- `/8`, `/16`, and `/24` use different subnet masks.
- `show ip interface brief` provides a quick view of interface addressing and status.
- A router can provide Layer 3 connectivity between directly connected networks.
- Interface descriptions make it easier to identify what each router port connects to.
- Cisco IOS command syntax must be entered precisely.

## Study Source

Completed as **Day 08 — IPv4 Addresses** of my CCNA studies using **Jeremy's IT Lab**.

This is **Lab 06** in my GitHub CCNA lab portfolio because not every course day contains a hands-on lab.
