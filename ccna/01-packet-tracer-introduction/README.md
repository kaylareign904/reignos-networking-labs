# Lab 01 — Packet Tracer Introduction

## Overview

This lab introduced me to Cisco Packet Tracer and its basic network-building environment.

I worked with several types of network devices and end devices while completing a topology representing two branch networks connected through the Internet.

The lab helped me become familiar with locating devices in Packet Tracer, placing devices into a topology, identifying different network components, and creating physical/logical connections between devices.

---

## Objectives

By completing this lab, I practiced:

- Navigating the Cisco Packet Tracer interface
- Identifying common network devices
- Identifying end devices
- Adding devices to a network topology
- Connecting devices using appropriate interfaces
- Recognizing the role different devices play in a network
- Building out a provided network topology

---

## Lab Environment

| Component | Details |
|---|---|
| Platform | Cisco Packet Tracer |
| Routers | R1, R2, Internet Router |
| Switches | SW1, SW2 |
| Firewalls | FW1, FW2 |
| PCs | PC1, PC2 |
| Servers | SVR1, SVR2 |
| Other End Device | Attacker laptop |

---

## Network Topology

The completed topology represents two branch LANs connected through an Internet to create WAN segment.

### New York Branch

PC1 and PC2 connect to switch SW1.

SW1 connects to router R1, which connects toward firewall FW1 and the Internet.

### Tokyo Branch

The Internet connection reaches router R2.

R2 connects to firewall FW2, which connects to switch SW2.

SW2 provides connectivity to servers SVR1 and SVR2.

### Internet Segment

An additional laptop labeled **Attacker** is connected to the Internet portion of the topology.

A screenshot of the completed topology is included with this lab documentation.

---

## Devices and Their Roles

### PCs

PC1 and PC2 represent user end devices on the New York branch LAN.

### Switches

SW1 and SW2 provide connectivity between devices within their respective LAN.

### Routers

R1 and R2 represent routers connecting the branch LANs toward external networks.

### Firewalls

FW1 and FW2 represent security devices positioned between portions of the branch infrastructure and external network connectivity.

### Servers

SVR1 and SVR2 represent server systems connected to the Tokyo branch LAN.

### Attacker Workstation

The attacker laptop represents an external device connected through the Internet portion of the topology.

---

## Configuration

This introductory lab focused primarily on becoming familiar with Packet Tracer and constructing the network topology.

No Cisco IOS configuration was performed as part of my work in this lab.

---

## Verification

I visually verified the completed topology by confirming that:

- All required devices were present
- Devices were positioned in the appropriate branch
- The required connections had been created
- The New York branch topology was complete
- The Tokyo branch topology was complete
- The Internet portion of the topology was connected

---

## Troubleshooting

This lab introduced the process of checking a network topology for incorrect or missing connections.

When completing the topology, I could compare the expected network layout with the devices and connections present in Packet Tracer and correct any missing components or links.

As I progress into configuration-based labs, I will expand this section to document specific problems, troubleshooting commands, root causes, and resolutions.

---

## What I Learned

This lab helped me become more comfortable navigating Cisco Packet Tracer and working with a visual network topology.

I reinforced the distinction between several common network components:

- End devices such as PCs and servers
- Switches that connect devices within a LAN
- Routers that connect different networks
- Firewalls that provide security controls between network segments

I also gained experience looking at a topology as a complete network rather than viewing each device individually.

---

## Study Source

This lab was completed as part of my CCNA studies using **Jeremy's IT Lab**.

The documentation and explanations in this repository are written in my own words as a record of my hands-on learning.
