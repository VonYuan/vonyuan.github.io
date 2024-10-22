---
layout: default
title: OSPF
nav_enabled: true
---
# OSPF Configuration Guide

## Step 1: Configure OSPF on R2 and R3

To enter the OSPF configuration, use the `router ospf` command followed by a process ID (which can be any number you choose).

### Configuration on R2

```bash
R2(config)# router ospf 1
R2(config-router)# network 192.168.23.0 0.0.0.255 area 0
```

### Configuration on R3

```bash
R3(config)# router ospf 1
R3(config-router)# network 192.168.23.0 0.0.0.255 area 0
```

Configuring OSPF Areas
Verifying OSPF Neighbor Adjacency

```bash
area 0
R3# %OSPF-5-ADJCHG: Process 1, Nbr 192.168.23.2 on FastEthernet0/0 from LOADING to FULL, Loading Done
R2# %OSPF-5-ADJCHG: Process 1, Nbr 192.168.23.3 on FastEthernet1/0 from LOADING to FULL, Loading Done
```

### Check OSPF Neighbors

```bash
R3# show ip ospf neighbor

Neighbor ID      Pri   State       Dead Time     Address          Interface
192.168.23.2      1   FULL/BDR    00:00:36      192.168.23.2    FastEthernet0/0

R2# show ip ospf neighbor

Neighbor ID      Pri   State       Dead Time     Address          Interface
192.168.23.3      1   FULL/DR     00:00:32      192.168.23.3    FastEthernet1/0
```

When the state is FULL, the routers have successfully become neighbors.

## Checking Router ID

### Each OSPF router has a router ID that you can check with

```bash

R2# show ip protocols


Routing Protocol is "ospf 1"
Outgoing update filter list for all interfaces is not set
Incoming update filter list for all interfaces is not set
Router ID 192.168.23.2


R3# show ip protocols


Routing Protocol is "ospf 1"
Outgoing update filter list for all interfaces is not set
Incoming update filter list for all interfaces is not set
Router ID 192.168.23.3
```

## Creating a Loopback Interface

### To change the router ID, create a loopback interface on R2

```bash
R2(config)# interface loopback 0
R2(config-if)# ip address 2.2.2.2 255.255.255.0
Check Router ID Again
```

### After creating the loopback, check the router ID

```bash

R2# show ip protocols

Routing Protocol is "ospf 1"
Outgoing update filter list for all interfaces is not set
Incoming update filter list for all interfaces is not set
Router ID 192.168.23.2
```

### To apply the change, reset the OSPF process

```bash
R2# clear ip ospf process
Reset ALL OSPF processes? [no]: yes
```

### Check the router ID again

```bash
R2# show ip protocols


Routing Protocol is "ospf 1"
Outgoing update filter list for all interfaces is not set
Incoming update filter list for all interfaces is not set
Router ID 2.2.2.2

R3(config-router)# router-id 3.3.3.3
Reset OSPF Process
```

### To apply the change, reset the OSPF process

```bash
R3# clear ip ospf process
Reset ALL OSPF processes? [no]: yes
```

Verify Router ID
After clearing the process, check the router ID:

```bash
R3# show ip protocols

Routing Protocol is "ospf 1"
Outgoing update filter list for all interfaces is not set
Incoming update filter list for all interfaces is not set
Router ID 3.3.3.3
```

### Configuring Additional OSPF Neighbors

Now let’s configure OSPF to establish neighbor adjacencies between R2 and R1, and between R1 and R3.

Configuration on R2:

```bash
R2(config)# router ospf 1
R2(config-router)# network 192.168.12.0 0.0.0.255 area 0
Configuration on R1:

R1(config)# router ospf 1
R1(config-router)# network 192.168.12.0 0.0.0.255 area 0
R1(config-router)# network 192.168.13.0 0.0.0.255 area 0
Configuration on R3:

R3(config)# router ospf 1
R3(config-router)# network 192.168.13.0 0.0.0.255 area 0
```

#### Verify OSPF Neighbors Again

After configuration, check the OSPF neighbors:

```bash
R2# show ip ospf neighbor

Neighbor ID      Pri   State     Dead Time     Address          Interface
192.168.13.1      1   FULL/BDR  00:00:31      192.168.12.1    Ethernet0/0
3.3.3.3           1   FULL/DR   00:00:38      192.168.23.3    FastEthernet1/0

R1# show ip ospf neighbor

Neighbor ID      Pri   State     Dead Time     Address          Interface
3.3.3.3           1   FULL/BDR  00:00:33      192.168.13.3    FastEthernet1/0
2.2.2.2           1   FULL/DR   00:00:30      192.168.12.2    Ethernet0/0

R3# show ip ospf neighbor

Neighbor ID      Pri   State     Dead Time     Address          Interface
192.168.13.1      1   FULL/DR   00:00:37      192.168.13.1    FastEthernet1/0
2.2.2.2           1   FULL/BDR  00:00:30      192.168.23.2    FastEthernet0/0
```
