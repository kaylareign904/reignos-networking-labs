# Lab 02 — Connecting Network Devices

## Overview

In this lab, I practiced selecting the appropriate network cabling and interfaces to connect different types of network devices in Cisco Packet Tracer.

The topology required me to consider:

- The types of devices being connected
- The appropriate copper cable type
- The maximum distance supported by copper Ethernet
- When fiber should be used instead of copper
- The difference between single-mode and multimode fiber
- Whether the selected device interface supported the cable being used

This lab helped reinforce that establishing a network connection involves more than simply connecting two devices. The physical media, transmission distance, device types, and available interfaces all have to be considered.

---

## Objectives

By completing this lab, I practiced:

- Manually connecting network devices in Cisco Packet Tracer
- Selecting cables based on the devices being connected
- Using copper straight-through cables
- Using copper crossover cables
- Recognizing the approximate 100-meter distance limitation of twisted-pair Ethernet
- Selecting fiber connections for longer distances
- Distinguishing between single-mode and multimode fiber
- Selecting compatible network interfaces
- Troubleshooting an incompatible cable and port combination

---

## Lab Environment

| Component | Details |
|---|---|
| Platform | Cisco Packet Tracer |
| Routers | R1, R2, R3, R4 |
| Switches | SW1 through SW8 |
| PCs | PC1, PC2, PC3 |
| Servers | SRV1 |
| Media | Copper straight-through, copper crossover, single-mode fiber, multimode fiber |

---

## Network Topology

The completed topology contains multiple routers, switches, PCs, and a server connected using different network media.

Rather than allowing Packet Tracer to automatically select the connection type, I manually determined the appropriate cable for each connection.

![Completed Packet Tracer topology for Lab 02](completed-lab-2.png)

*Completed Lab 02 topology showing my network connections and cable selections.*

---

## Copper Connections

### Copper Straight-Through

I used straight-through copper cables when connecting different types of Ethernet devices.

Examples from the topology include:

- R2 to SW1
- R2 to SW2
- SW3 to PC1
- SW4 to PC2

This reinforced the traditional Ethernet cabling rule that straight-through cables are commonly used when connecting unlike device types.

Examples include:

**Router ↔ Switch**

and

**Switch ↔ End Device**

---

### Copper Crossover

I used crossover cables when directly connecting similar Ethernet device types.

Examples include:

- R1 to R2
- SW1 to SW2
- SW1 to SW3
- SW2 to SW4

This reinforced the traditional Ethernet cabling rule that crossover cables are used when directly connecting similar device types.

Examples include:

**Router ↔ Router**

and

**Switch ↔ Switch**

---

## Distance and Media Selection

One of the important concepts in this lab was determining whether copper or fiber should be used based on the distance between devices.

### R1 to R2 — 50 Meters

For the 50-meter connection between R1 and R2, I selected a:

**Copper crossover cable**

Twisted-pair copper Ethernet can typically support a maximum channel distance of approximately **100 meters**.

Since R1 and R2 were only 50 meters apart, copper was appropriate for this connection.

Because this was a direct Ethernet connection between two routers, I used a crossover cable based on the traditional Ethernet cabling rules covered in the lesson.

---

### R1 to R3 — 3 Kilometers

For the 3-kilometer connection between R1 and R3, I selected:

**Single-mode fiber**

The 3-kilometer distance is far beyond the normal distance supported by twisted-pair Ethernet.

Single-mode fiber is designed for long-distance communication and was therefore the appropriate media choice for this connection.

---

### R3 to R4 — 250 Meters

For the 250-meter connection between R3 and R4, I selected:

**Multimode fiber**

At 250 meters, the connection exceeds the typical 100-meter limitation of twisted-pair Ethernet.

Multimode fiber is suitable for shorter fiber runs such as this one.

---

> **Packet Tracer Note:** Packet Tracer does not differentiate between single-mode and multimode fiber when creating the connection. For this lab, I determined which fiber type would be appropriate based on the required transmission distance.

---

## Cabling Decisions

| Connection | Device Types | Distance / Situation | Cable / Media |
|---|---|---|---|
| R1 ↔ R2 | Router ↔ Router | 50 meters | Copper crossover |
| R1 ↔ R3 | Router ↔ Router | 3 kilometers | Single-mode fiber |
| R3 ↔ R4 | Router ↔ Router | 250 meters | Multimode fiber |
| R2 ↔ SW1 | Router ↔ Switch | Ethernet LAN | Copper straight-through |
| R2 ↔ SW2 | Router ↔ Switch | Ethernet LAN | Copper straight-through |
| SW1 ↔ SW2 | Switch ↔ Switch | Ethernet LAN | Copper crossover |
| SW1 ↔ SW3 | Switch ↔ Switch | Ethernet LAN | Copper crossover |
| SW2 ↔ SW4 | Switch ↔ Switch | Ethernet LAN | Copper crossover |
| SW3 ↔ PC1 | Switch ↔ PC | End-device connection | Copper straight-through |
| SW4 ↔ PC2 | Switch ↔ PC | End-device connection | Copper straight-through |

The same cabling principles were applied to the corresponding connections on the other side of the topology.

---

## Configuration

This lab focused on **Layer 1 physical connectivity**, network media, and interface selection.

No Cisco IOS configuration was required as part of my work in this lab.

---

## Verification

I verified my completed topology by reviewing each connection and checking that my cable selection matched:

1. The types of devices being connected
2. The maximum distance supported by the selected media
3. Whether copper or fiber was appropriate
4. Whether straight-through or crossover copper was appropriate
5. Whether single-mode or multimode fiber was appropriate
6. Whether the selected interface supported the cable

I also reviewed the completed topology to confirm that all required devices had been connected.

---

## Troubleshooting

### Problem Encountered

While creating one of the fiber connections, I accidentally selected the wrong interface on the router.

I attempted to connect the fiber-optic cable using a FastEthernet interface.

Packet Tracer rejected the connection and displayed the following error:

> **"The cable cannot be connected to that port."**

![Packet Tracer error after selecting an incompatible port](wrong-port-selection.png)

*Packet Tracer error displayed after I attempted to connect the fiber cable to an incompatible interface.*

---

### Investigation

After receiving the error, I went back and reviewed the available interfaces on the router.

Packet Tracer displayed several interface options, including:

- FastEthernet1/0
- FastEthernet2/0
- GigabitEthernet3/0

I noticed that Packet Tracer visually distinguished the available interface types.

The FastEthernet interfaces were represented by the **yellow port selections**, while the fiber-capable interface was represented by the **orange fiber-style port selection**.

![Router interface selection menu in Packet Tracer](port-selection-menu.png)

*Interface selection menu I reviewed while determining which port supported the fiber connection.*

---

### Root Cause

The fiber cable itself was not the problem.

The problem was that I had selected a physical interface that was incompatible with the type of cable I was attempting to connect.

I was trying to connect fiber media to a FastEthernet copper interface rather than the appropriate fiber-capable interface.

---

### Resolution

I returned to the router's available interfaces and selected the appropriate fiber-capable interface instead of the FastEthernet interface.

After selecting the correct interface, Packet Tracer allowed me to create the fiber connection successfully.

---

### Verification After the Fix

After correcting the interface selection, I confirmed that:

- Packet Tracer accepted the fiber cable
- The connection appeared correctly in the topology
- The router was connected through the intended interface
- The completed topology matched the required network design

---

## What I Learned From Troubleshooting

This mistake helped reinforce an important Layer 1 concept:

**Choosing the correct cable is only part of creating a physical network connection. The interface also has to support the selected media.**

When troubleshooting a physical connection, I should check:

1. What devices am I connecting?
2. What cable or media am I using?
3. Is that media appropriate for the distance?
4. Which physical interface am I connecting to?
5. Does that interface support the selected media?

Instead of assuming that the fiber cable was incorrect when Packet Tracer displayed an error, I checked the available interfaces and identified that I had selected the wrong port type.

---

## What I Learned

This lab reinforced how several Layer 1 decisions work together when physically connecting a network.

### Device Type Matters

The devices being connected can determine whether a traditional straight-through or crossover copper cable should be used.

### Distance Matters

Copper Ethernet is appropriate for connections within its supported distance, while fiber becomes necessary for longer connections.

For example:

- **50 meters:** Copper was sufficient
- **250 meters:** Fiber was required
- **3 kilometers:** Long-distance single-mode fiber was appropriate

### Fiber Type Matters

Multimode and single-mode fiber serve different purposes.

Multimode fiber can be appropriate for shorter fiber connections, while single-mode fiber supports much longer distances.

### The Interface Matters

Even if I select the correct cable, the connection will not work if I attempt to connect it to an incompatible physical interface.

### Troubleshooting Starts at Layer 1

Before investigating IP addressing, routing, or higher-layer configuration, I should first verify basic physical connectivity.

That includes checking:

- Cable type
- Interface type
- Media compatibility
- Connection distance
- Physical/link status

This lab gave me more experience thinking about **why a particular physical connection should be used instead of simply selecting a cable in Packet Tracer.**

---

## Study Source

This lab was completed as part of my CCNA studies using **Jeremy's IT Lab**.

The documentation, explanations, observations, troubleshooting process, and lessons learned in this repository are written in my own words as a record of my hands-on learning.
