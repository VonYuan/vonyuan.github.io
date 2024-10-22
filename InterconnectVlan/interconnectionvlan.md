---
layout: default
title: HoInterconnection Between VLANs
nav_enabled: true
---
# Interconnection Between VLANs

VLAN technology divides a large local area network (LAN) into multiple smaller LANs and isolates broadcast domains, preventing communication between users in different broadcast domains, as shown in the diagram below.

## Topology Example

- VLAN 200
- VLAN 100

## 1. What if different VLANs need to communicate with each other?

Hosts in different VLANs cannot communicate directly, so routing between VLANs must be done using a layer 3 routing device to forward packets from one VLAN to another.

## 2. How to use a Layer 3 device for VLAN communication?

As shown in the diagram:

1. The topology includes 2 VLANs, and devices in different VLANs belong to different subnets.
2. The switch connects to two ports of the router.
3. When a data packet is sent to the switch, the switch forwards it to the router over the data link.
4. The router, upon receiving the data packet, uses the routing table to forward the packet back to the switch, which then forwards it by comparing PVID and VLAN ID.

## Key Components

- Layer 2 Interface
- Layer 3 Interface
- Router
