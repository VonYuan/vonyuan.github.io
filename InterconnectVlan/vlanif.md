---
layout: default
title: VLAN IF
nav_enabled: true
---

## Using VLAN IF Technology (Layer 3 Switch)

On a Layer 3 switch, create logical interfaces as gateways. The VLANIF interface number corresponds to the VLAN ID, such as VLAN 10 corresponding to VLANIF 10.

- **Layer 2 Switch**: Only supports basic switching communication.
- **Layer 3 Switch**: Supports all Layer 2 switch functions and can also perform routing through logical interfaces.

## Topology Example

- **SWA**:
  - VLANIF 2: `192.168.2.254/24`
  - VLANIF 3: `192.168.3.254/24`
- **Host A**:
  - Gateway: `192.168.2.254`
  - VLAN 2
- **Host B**:
  - Gateway: `192.168.2.254`
- **Host C**:
  - Gateway: `192.168.3.254`
- **Host D**:
  - Gateway: `192.168.3.254`

## Configuration Example

### 1. Router-on-a-Stick

```bash
<Huawei> system-view // Enter system view
[Huawei] sysname SW-1 // Rename the switch
[SW-1] vlan batch 10 20 // Create VLANs 10 and 20
[SW-1] interface e0/0/1 // Enter interface e0/0/1
[SW-1-Ethernet0/0/1] port link-type access // Set mode to access
[SW-1-Ethernet0/0/1] port default vlan 10 // Assign e0/0/1 to VLAN 10
[SW-1-Ethernet0/0/1] int e0/0/2 // Switch to interface e0/0/2
[SW-1-Ethernet0/0/2] port link-type access // Set mode to access
[SW-1-Ethernet0/0/2] port default vlan 20 // Assign e0/0/2 to VLAN 20
[SW-1-Ethernet0/0/2] int g0/0/1 // Switch to interface g0/0/1
[SW-1-GigabitEthernet0/0/1] port link-type trunk // Set mode to trunk
[SW-1-GigabitEthernet0/0/1] port trunk allow-pass vlan all // Allow all VLANs through

<Huawei> system-view // Enter system view
[Huawei] sysname AR-1 // Rename the router to AR-1
[AR-1] int g0/0/1.1 // Enter sub-interface g0/0/1.1
[AR-1-GigabitEthernet0/0/1.1] dot1q termination vid 10 // Set dot1q encapsulation for VLAN 10
[AR-1-GigabitEthernet0/0/1.1] ip address 192.168.10.254 24 // Set IP address and subnet mask
[AR-1-GigabitEthernet0/0/1.1] arp broadcast enable // Enable ARP broadcast
[AR-1-GigabitEthernet0/0/1.1] int g0/0/1.2 // Switch to sub-interface g0/0/1.2
[AR-1-GigabitEthernet0/0/1.2] dot1q termination vid 20 // Set dot1q encapsulation for VLAN 20
[AR-1-GigabitEthernet0/0/1.2] ip address 192.168.20.254 24 // Set IP address and subnet mask
[AR-1-GigabitEthernet0/0/1.2] arp broadcast enable // Enable ARP broadcast
