# Lab 06 — IPv4 Addresses

## Overview

In this lab, I configured IPv4 addressing on a Cisco router and three end devices across three separate networks.

I configured R1's hostname, inspected its interfaces, assigned IPv4 addresses and subnet masks to each router interface, enabled the interfaces, added interface descriptions, configured the PCs with static IPv4 addresses, verified the router configuration, saved the configuration, and tested connectivity between the networks.

This lab helped me connect IPv4 addressing concepts with actual Cisco IOS interface configuration and end-to-end network communication.

## Objectives

By completing this lab, I practiced:

- Configuring a Cisco router hostname
- Viewing router interface information
- Using `show ip interface brief`
- Configuring IPv4 addresses on router interfaces
- Applying subnet masks
- Enabling router interfaces with `no shutdown`
- Adding interface descriptions
- Verifying interface status
- Viewing the running configuration
- Saving the running configuration to startup configuration
- Configuring static IPv4 addresses on Packet Tracer PCs
- Testing connectivity between different IPv4 networks
- Troubleshooting Cisco IOS command syntax

## Lab Environment

| Component | Details |
|---|---|
| Platform | Cisco Packet Tracer |
| Router | R1 |
| Switches | SW1, SW2, SW3 |
| End Devices | PC1, PC2, PC3 |
| Main Focus | IPv4 addressing and router interface configuration |
| Networks | 15.0.0.0/8, 182.98.0.0/16, 201.191.20.0/24 |

## Network Topology

The topology contained one router connecting three separate IPv4 networks.

| Network | End Device | End Device IP | Router Interface | Router IP |
|---|---|---|---|---|
| 15.0.0.0/8 | PC1 | 15.0.0.1 | G0/0 | 15.255.255.254 |
| 182.98.0.0/16 | PC2 | 182.98.0.1 | G0/1 | 182.98.255.254 |
| 201.191.20.0/24 | PC3 | 201.191.20.1 | G0/2 | 201.191.20.254 |

The three networks used different prefix lengths:

```text
15.0.0.0/8
182.98.0.0/16
201.191.20.0/24
```

This gave me practice configuring interfaces with different subnet masks.

## Configuring R1's Hostname

I first changed the router's default hostname to R1.

```text
enable
configure terminal
hostname R1
```

![Configuring the R1 hostname](<configure R1 hostname.png>)

*Changing the router hostname from its default name to R1.*

After entering the command, the IOS prompt changed from:

```text
Router(config)#
```

to:

```text
R1(config)#
```

## Viewing R1's Interfaces Before Configuration

Before assigning any IPv4 addresses, I wanted to see a summary of R1's interfaces and their current status.

The command I ultimately used was:

```text
do show ip interface brief
```

![R1 IP interface table before configuration](<R1 IP Interface table before changes.png>)

*Viewing R1's interfaces before configuring IPv4 addresses.*

The output showed that the router interfaces were:

- Unassigned
- Administratively down
- Protocol down

This gave me a baseline before making any configuration changes.

## Troubleshooting — Finding the Correct Interface Summary Command

While trying to view R1's interfaces, I initially used:

```text
do show interfaces
```

The command worked, but it returned a large amount of detailed information for the interfaces.

I remembered from the lecture that there was a `brief` command that provided a much shorter interface summary.

I then attempted:

```text
do show interfaces brief
```

but IOS returned an invalid-input error.

I tried another variation using:

```text
show ip interfaces brief
```

but that was also incorrect.

### Investigation

I referred back to my notes and realized that the correct Cisco IOS command uses:

```text
show ip interface brief
```

with **`interface` singular**.

Because I was still in Global Configuration mode, I also needed to use the `do` prefix.

### Resolution

I entered:

```text
do show ip interface brief
```

The command worked and displayed a concise table showing:

- Interface
- IP address
- Method
- Status
- Protocol status

### What I Learned

This troubleshooting experience helped me understand the difference between:

```text
show interfaces
```

and:

```text
show ip interface brief
```

`show interfaces` provides detailed interface information and can produce a large amount of output.

`show ip interface brief` is much more useful when I want a quick overview of interface addressing and operational status.

It also reinforced two Cisco IOS concepts I encountered in an earlier lab:

1. Command syntax needs to be precise.
2. When running a Privileged EXEC command from configuration mode, I can use `do`.

Instead of guessing repeatedly after the command failed, checking my notes helped me identify the correct syntax.

## Configuring R1's Interfaces

I then configured IPv4 addressing on each of R1's three GigabitEthernet interfaces.

### G0/0 — Network 15.0.0.0/8

```text
interface gigabitethernet0/0
ip address 15.255.255.254 255.0.0.0
no shutdown
description ## to SW1 ##
```

### G0/1 — Network 182.98.0.0/16

```text
interface gigabitethernet0/1
ip address 182.98.255.254 255.255.0.0
no shutdown
description ## to SW2 ##
```

### G0/2 — Network 201.191.20.0/24

```text
interface gigabitethernet0/2
ip address 201.191.20.254 255.255.255.0
no shutdown
description ## to SW3 ##
```

![Configuring R1 interfaces with IPv4 addresses, descriptions, and status](<configuring R1 interfaces with IP desc and status.png>)

*Configuring IPv4 addresses, enabling the interfaces, and adding descriptions to R1.*

The `no shutdown` command was required because router interfaces are administratively disabled by default in this lab environment.

As each interface was enabled, IOS displayed messages indicating that the interface and line protocol had changed to an up state.

## Interface Descriptions

I added descriptions to identify where each router interface connected.

```text
description ## to SW1 ##
description ## to SW2 ##
description ## to SW3 ##
```

These descriptions provide useful documentation directly within the router configuration.

Instead of only seeing an interface name such as:

```text
GigabitEthernet0/1
```

the configuration also provides information about what the interface connects to.

## Verifying R1's Interfaces

After configuring the interfaces, I ran:

```text
do show ip interface brief
```

again.

![R1 IP interface table after configuration](<R1 IP interface table after changes.png>)

*Verifying R1's IPv4 addresses and interface status after configuration.*

The output now showed:

| Interface | IPv4 Address | Status | Protocol |
|---|---|---|---|
| GigabitEthernet0/0 | 15.255.255.254 | up | up |
| GigabitEthernet0/1 | 182.98.255.254 | up | up |
| GigabitEthernet0/2 | 201.191.20.254 | up | up |

This confirmed that:

- The correct IPv4 addresses were assigned
- The interfaces were enabled
- The physical interfaces were operational
- The line protocols were operational

The change from:

```text
administratively down / down
```

to:

```text
up / up
```

showed the effect of configuring and enabling the interfaces.

## Reviewing the Running Configuration

I also reviewed R1's running configuration to confirm the changes.

```text
show running-config
```

![R1 running configuration](<R1 running-config file.png>)

*Reviewing R1's running configuration after completing the interface configuration.*

The configuration showed the IPv4 address and description assigned to each interface.

For example:

```text
interface GigabitEthernet0/0
 description ## to SW1 ##
 ip address 15.255.255.254 255.0.0.0
```

The same structure was visible for G0/1 and G0/2.

This gave me another way to verify that the intended configuration had been applied.

## Saving R1's Configuration

After verifying the running configuration, I saved it to startup configuration.

I used:

```text
copy running-config startup-config
```

When prompted with:

```text
Destination filename [startup-config]?
```

I pressed Enter to accept the default filename.

![Copying R1 running configuration to startup configuration](<copying running-config to startup-config for R1.png>)

*Saving R1's running configuration so the changes can be retained after a restart.*

This also reinforced something I learned in my previous Cisco IOS lab: configuration changes exist in the running configuration until they are intentionally saved.

## Configuring PC1

PC1 was configured for the:

```text
15.0.0.0/8
```

network.

I assigned:

```text
IPv4 Address: 15.0.0.1
Subnet Mask: 255.0.0.0
```

![Configuring PC1 IPv4 address](<config PC1 IP Address.png>)

*Configuring PC1 with a static IPv4 address and /8 subnet mask.*

## Configuring PC2

PC2 was configured for the:

```text
182.98.0.0/16
```

network.

I assigned:

```text
IPv4 Address: 182.98.0.1
Subnet Mask: 255.255.0.0
```

![Configuring PC2 IPv4 address](<config PC2 IP address.png>)

*Configuring PC2 with a static IPv4 address and /16 subnet mask.*

## Configuring PC3

PC3 was configured for the:

```text
201.191.20.0/24
```

network.

I assigned:

```text
IPv4 Address: 201.191.20.1
Subnet Mask: 255.255.255.0
```

![Configuring PC3 IPv4 address](<config PC3 IP address.png>)

*Configuring PC3 with a static IPv4 address and /24 subnet mask.*

## Comparing the Three Subnet Masks

This lab gave me hands-on practice working with three different prefix lengths.

| Prefix | Subnet Mask |
|---|---|
| /8 | 255.0.0.0 |
| /16 | 255.255.0.0 |
| /24 | 255.255.255.0 |

The router required one correctly addressed interface in each network so that it could provide Layer 3 connectivity between them.

## Testing Connectivity

After configuring R1 and the PCs, I used PC1 to test connectivity.

I pinged PC2:

```text
ping 182.98.0.1
```

and PC3:

```text
ping 201.191.20.1
```

![PC1 pinging PC2 and PC3](<PC1 ping PC2-PC3.png>)

*Testing connectivity from PC1 to hosts located on the other IPv4 networks.*

PC1 successfully communicated with both remote networks.

When pinging PC3, the first request timed out, followed by successful replies.

The successful replies confirmed that R1 was able to route traffic between the different connected networks.

The initial timeout also reminded me that the first attempt in Packet Tracer may involve address-resolution activity before subsequent traffic can be forwarded successfully.

## Router's Role in the Topology

PC1, PC2, and PC3 were located on three different IPv4 networks.

A Layer 2 switch alone cannot forward traffic between those separate networks.

R1 had an interface in each network:

```text
G0/0 → 15.0.0.0/8

G0/1 → 182.98.0.0/16

G0/2 → 201.191.20.0/24
```

This allowed R1 to provide Layer 3 connectivity between the three directly connected networks.

This lab helped me see how router interface addressing connects separate IP networks together.

## Commands Used

Cisco IOS commands I practiced during this lab included:

```text
enable
configure terminal
hostname R1

show interfaces
do show interfaces

show ip interface brief
do show ip interface brief

interface gigabitethernet0/0
ip address 15.255.255.254 255.0.0.0
no shutdown
description ## to SW1 ##

interface gigabitethernet0/1
ip address 182.98.255.254 255.255.0.0
no shutdown
description ## to SW2 ##

interface gigabitethernet0/2
ip address 201.191.20.254 255.255.255.0
no shutdown
description ## to SW3 ##

show running-config
copy running-config startup-config
```

PC commands used for verification included:

```text
ping 182.98.0.1
ping 201.191.20.1
```

## Verification

I verified the lab by:

1. Changing the router hostname to R1
2. Viewing the router interfaces before configuration
3. Confirming the interfaces initially had no IPv4 addresses
4. Configuring IPv4 addresses and subnet masks on G0/0, G0/1, and G0/2
5. Enabling each interface with `no shutdown`
6. Adding interface descriptions
7. Running `show ip interface brief` again
8. Confirming all three configured interfaces showed `up/up`
9. Reviewing the running configuration
10. Saving the running configuration to startup configuration
11. Configuring static IPv4 addresses on PC1, PC2, and PC3
12. Pinging PC2 from PC1
13. Pinging PC3 from PC1
14. Confirming successful communication between the separate networks

## What I Learned

This lab helped move IPv4 addressing from something I had primarily studied conceptually into something I configured on actual simulated network interfaces.

### Router Interfaces Belong to Networks

Each R1 interface had an IPv4 address belonging to the network connected to that interface.

```text
G0/0 → 15.255.255.254/8
G0/1 → 182.98.255.254/16
G0/2 → 201.191.20.254/24
```

### Subnet Masks Matter

The subnet mask determines which portion of an IPv4 address identifies the network.

I worked with:

```text
/8
/16
/24
```

during the same lab.

### Router Interfaces Must Be Enabled

Assigning an IP address was not enough by itself.

I also needed:

```text
no shutdown
```

to enable each router interface.

### Verification Commands Save Time

I learned that:

```text
show interfaces
```

provides detailed information, while:

```text
show ip interface brief
```

provides a much faster summary of interface addresses and status.

Knowing which verification command to use depends on how much information I actually need.

### Separate Networks Need Layer 3 Connectivity

PC1, PC2, and PC3 were not all on the same IP network.

R1 provided the Layer 3 connection between those networks through its three configured interfaces.

### Documentation Helps With Device Management

Adding descriptions such as:

```text
description ## to SW1 ##
```

made the purpose of each router interface easier to identify from the configuration.

### Troubleshooting Command Syntax Matters

My mistake with `show interfaces brief` reinforced that knowing the general idea of a command is not always enough.

Cisco IOS expects specific command syntax.

When the command failed, I checked my notes, corrected it to:

```text
show ip interface brief
```

and successfully retrieved the information I needed.

## Screenshots

The screenshots included in this lab document the IPv4 configuration and verification process:

- `configure R1 hostname.png`
- `R1 IP Interface table before changes.png`
- `configuring R1 interfaces with IP desc and status.png`
- `R1 IP interface table after changes.png`
- `R1 running-config file.png`
- `copying running-config to startup-config for R1.png`
- `config PC1 IP Address.png`
- `config PC2 IP address.png`
- `config PC3 IP address.png`
- `PC1 ping PC2-PC3.png`

## Study Source

This lab was completed as **Day 08 — IPv4 Addresses** of my CCNA studies using **Jeremy's IT Lab**.

This is **Lab 06** in my GitHub lab portfolio because not every course day contains a hands-on lab.

The documentation, explanations, screenshots, troubleshooting process, observations, and lessons learned in this repository are written in my own words as a record of my hands-on learning.
