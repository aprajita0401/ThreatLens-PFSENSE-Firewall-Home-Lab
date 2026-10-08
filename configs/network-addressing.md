# ThreatLens: Network Addressing

This document records the addressing and VirtualBox network design for
the ThreatLens lab. The project documentation includes example private
addresses, but your actual values may differ. 

## Topology summary

 | Component | Intended Role | Network Connection Described in Project Documentation |
| --- | --- | --- |
| **pfSense** | Firewall/router between WAN and internal LAN | Adapter 1: Bridged WAN; Adapter 2: Internal Network `intet`<br> |
| **Kali Linux** | Controlled test-traffic source| Bridged adapter |
| **Ubuntu** | Target host and Wireshark capture system| Internal Network with the name `intnet`<br> |
| **VirtualBox Host** | Runs the virtual machines| Physical network adapter and VirtualBox virtual networks|

The PDF uses `intnet` as internal-network names in
different setup sections. VirtualBox internal network names must match
for machines to share the same internal network. 

## Addressing plan

| Network item | Address / Range | 
| --- | --- | 
| **Example pfSense WAN address** | 192.168.0.58 | 
| **Example upstream router / DNS** | 192.168.0.1 |
| **Example pfSense LAN gateway** | 192.168.1.1 | 
| **Example LAN subnet** | 192.168.1.0/24 | 
| **Example DHCP range** | 192.168.1.10–192.168.1.245 | 
| **Kali address** | 192.168.x.x | 
| **Ubuntu address** | 192.168.x.x | 

## Interface inventory

| VM | Adapter | VirtualBox Mode / Network Name | Interface Name | 
| --- | --- | --- | --- |
| **pfSense**<br> | 1 | Bridged | WAN |
| **pfSense**<br> | 2 | Internal Network: intNet | LAN |
| **Kali Linux**<br> | 1 | Bridged | eth0 | 
| **Ubuntu**<br> | 1 | Bridged & Internal Network: intNet | eth0 | 

## Routing notes

``` bash
sudo ip route add 192.168.1.0/24 via <PFSENSE_WAN_IP>
```

This command is specific to the topology described in the PDF. Do not
use it unchanged without verifying that the gateway is reachable from
Kali and that the intended routing path is correct. A bridged Kali VM
may sit directly on the physical LAN, so the topology can expose the
home network to lab traffic if it is not carefully isolated.

Record the actual route after validating it:

``` bash
ip route
ip addr
```

On Ubuntu, inspect the address and routes with:

``` bash
ip addr
ip route
```

## Network safety checklist

-   [ ] The Kali-to-target test path stays within an isolated,
    authorized lab.
-   [ ] The pfSense WAN is not unintentionally exposed to inbound
    management access.
-   [ ] No firewall-wide disable command is required for normal
    operation.
-   [ ] VirtualBox internal network names match where connectivity is
    intended.
-   [ ] IP addresses and routes in this document match the final working
    configuration.
-   [ ] Screenshots redact sensitive public IPs and personal network
    details.
