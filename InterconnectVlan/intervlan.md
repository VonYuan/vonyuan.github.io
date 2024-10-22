---
layout: default
title: Home
nav_enabled: true
---

# 2. Methods for Inter-VLAN Communication

## (1) Using Router Physical Interfaces

Configure VLANs on a Layer 2 switch, with each VLAN using a dedicated physical link to connect to a router interface, as shown in the diagram:

1. PC1's address is 192.168.10.2/24, belonging to VLAN 10, and PC2's address is 192.168.20.0/24, belonging to VLAN 20.
2. Switch SW1 connects to two physical interfaces of Router R1, serving as the gateways for PCs in VLAN 10 and VLAN 20.
3. Since router interfaces cannot process data frames with VLAN tags, the interface on Switch SW1 connecting to Router R1 needs to be of access type. Before forwarding data frames to the router, the switch removes the VLAN tag. (VLAN tags were explained earlier, feel free to learn more if interested!)
4. When PC1 wants to send a data frame to PC2, the packet is first transmitted to the switch. The switch removes the VLAN tag and forwards the frame to the appropriate network's gateway.
5. Upon receiving the data frame, the gateway checks its routing table, identifies the corresponding output interface for the target network, and forwards the packet. The switch then receives the data, detects the untagged data frame, checks which VLAN the receiving interface belongs to, re-tags the frame with the corresponding VLAN ID, and transmits it to PC2.
