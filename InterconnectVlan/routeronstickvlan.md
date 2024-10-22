# (2) Using Router Sub-Interfaces (Router-on-a-Stick)

Configure the link between the switch and the router as a Trunk link, and create sub-interfaces on the router to support VLAN routing. (Sub-interface: A logical interface created on a physical interface of the router that functions like a physical interface.)

Since multiple VLANs need to pass through the link between the switch and the router, the link must be configured as a Trunk, allowing specific VLANs to pass through. An access link cannot meet these requirements.

**Topology Example:**

- **Router R1:**
  - GE0/0/1.10 (VLAN 10 Gateway)
  - GE0/0/1.20 (VLAN 20 Gateway)
  - IP Address for GE0/0/1.20: `192.168.20.254`
- **Switch SW1:**
  - G0/0/24: Trunk (VLAN 10, VLAN 20)
  - GE0/0/2: Access (VLAN 20)
- **PC1**: `192.168.10.2/24`
  - Default Gateway: `192.168.10.254`
- **PC2**: `192.168.20.2/24`
  - Default Gateway: `192.168.20.254`

## How It Works

1. On Router R1's GE0/0/1 interface, create sub-interfaces GE0/0/1.10 and GE0/0/1.20, and configure their respective IP addresses. GE0/0/1.10 acts as the gateway for PCs in VLAN 10, while GE0/0/1.20 acts as the gateway for PCs in VLAN 20.
2. When PC1 sends data to PC2, the switch receives the data, tags it with the corresponding VLAN ID, and forwards it through the Trunk link to the GE0/0/1.10 gateway interface.
3. The router receives the data and checks the VLAN tag. Seeing that it is tagged with VLAN 10, it identifies the corresponding sub-interface GE0/0/1.10 and processes the data. It then looks up the routing table and finds that the target address is directly connected to another sub-interface, GE0/0/1.20. The router sends out the data from GE0/0/1.20, removes the VLAN 10 tag, and tags it with VLAN 20.
4. The switch then forwards the data to PC2.
