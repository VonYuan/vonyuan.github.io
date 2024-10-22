---
layout: default
title: BGP Configuration
parent: BGP-Protocol
nav_order: 1
nav_enabled: true
---
# BGP Configuration Example

## Step 1: Configure BGP on R1 and R2

To start BGP, use the `router bgp` command followed by the Autonomous System (AS) number. You will need to manually configure BGP neighbors using the `neighbor x.x.x.x remote-as` command. This setup is used for **External BGP (eBGP)**, where peers are in different AS numbers.

### Configuration on R1

```bash
R1(config)# router bgp 1
R1(config-router)# neighbor 192.168.12.2 remote-as 2
```

### Configuration on R2

```bash
R2(config)# router bgp 2
R2(config-router)# neighbor 192.168.12.1 remote-as 1
```

## Step 2 Verify BGP Neighbor Adjacency

After configuring the neighbors, if everything is correct, you should see messages indicating a new BGP neighbor adjacency:

```bash
R1# %BGP-5-ADJCHANGE: neighbor 192.168.12.2 Up
R2# %BGP-5-ADJCHANGE: neighbor 192.168.12.1 Up
```

### Step 3: Configure MD5 Authentication (Optional)

You can enable MD5 authentication for security. This ensures that both routers calculate an MD5 digest for every TCP segment they send, verifying each other's identity.

### Configuration on R1 (Optional)

```bash
R1(config)# router bgp 1
R1(config-router)# neighbor 192.168.12.2 password MYPASS
```

### Configuration on R2 (Optional)

```bash
R2(config)# router bgp 2
R2(config-router)# neighbor 192.168.12.1 password MYPASS
```

### Step 4: Verify BGP Status

To check the BGP status and summary, use the show ip bgp summary command on each router. This command displays information such as the BGP router identifier, the local AS number, and the status of each BGP neighbor.

### R1's BGP Summary

```bash
R1# show ip bgp summary
BGP router identifier 1.1.1.1, local AS number 1
BGP table version is 1, main routing table version 1

Neighbor     V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
192.168.12.2 4     2      10      10        1    0    0 00:07:12        0
```

### R2's BGP Summary

```bash
R2# show ip bgp summary
BGP router identifier 2.2.2.2, local AS number 2
BGP table version is 1, main routing table version 1

Neighbor     V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
192.168.12.1 4     1      11      11        1    0    0 00:08:33        0
```
