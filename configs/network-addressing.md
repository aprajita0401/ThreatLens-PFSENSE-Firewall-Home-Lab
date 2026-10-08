# ThreatLens: Network Addressing

This document records the addressing and VirtualBox network design for
the ThreatLens lab. The project documentation includes example private
addresses, but your actual values may differ. 

## Topology summary

 | Component | Intended Role | Network Connection Described in Project Documentation |
| --- | --- | --- |
| **pfSense** | Firewall/router between WAN and internal LAN

 | Adapter 1: Bridged WAN; Adapter 2: Internal Network `LabNet`<br> |
| **Kali Linux** | Controlled test-traffic source

 | Bridged adapter is described in the PDF; verify whether this is the final safe lab topology

 |
| **Ubuntu** | Target host and Wireshark capture system

 | Internal Network is described; the PDF uses the name `intnet`<br> |
| **VirtualBox Host** | Runs the virtual machines

 | Physical network adapter and VirtualBox virtual networks|

The PDF uses `intnet` as internal-network names in
different setup sections. VirtualBox internal network names must match
for machines to share the same internal network. 

## Addressing plan

  ---------------------------------------------------------------------------------
  Network item            Address / range in the PDF        Notes
  ----------------------- --------------------------------- -----------------------
  Example pfSense WAN     `192.168.0.58`                    Example only; WAN is
  address                                                   described as DHCP

  Example upstream router `192.168.0.1`                     Example only
  / DNS                                                     

  Example pfSense LAN     `192.168.1.1`                     Described as the Ubuntu
  gateway                                                   default gateway

  Example LAN subnet      `192.168.1.0/24`                  Inferred from the
                                                            documented DHCP range
                                                            and route

  Example DHCP range      `192.168.1.10`--`192.168.1.245`   Documented range;
                                                            verify current settings

  Kali address            `[record actual value]`           The PDF says Kali
                                                            obtains an address from
                                                            the home router

  Ubuntu address          `[record actual value]`           Record the address on
                                                            Ubuntu's actual lab
                                                            interface
  ---------------------------------------------------------------------------------

## Interface inventory

Complete this table using the VirtualBox settings and the pfSense
console or web interface.

  -------------------------------------------------------------------------------------------------------------------------------
  VM          Adapter     VirtualBox mode / network name             Interface name         IPv4 address /     Default gateway
                                                                                            prefix             
  ----------- ----------- ------------------------------------------ ---------------------- ------------------ ------------------
  pfSense     1           `[verify: Bridged or safer alternative]`   `[WAN interface]`      `[actual value]`   `[actual value]`

  pfSense     2           `Internal Network: [actual name]`          `[LAN interface]`      `[actual value]`   N/A

  Kali Linux  1           `[actual mode and network name]`           `[actual interface]`   `[actual value]`   `[actual value]`

  Ubuntu      1           `Internal Network: [actual name]`          `[actual interface]`   `[actual value]`   `[actual value]`
  -------------------------------------------------------------------------------------------------------------------------------

## Routing notes

The PDF includes this example route command on Kali:

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

## Notes

Use this file as the source of truth for the final lab topology only
after filling in the actual values. If you changed the network design
from the original PDF, document the implemented design rather than
preserving outdated examples.
