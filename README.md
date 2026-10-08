# ThreatLens \| pfSense Firewall Home Lab

**Network Security · Firewall Configuration · Packet Analysis · DoS
Mitigation**

ThreatLens is a virtualized network security lab built with Oracle
VirtualBox, pfSense, Kali Linux, Ubuntu, and Wireshark. The lab
demonstrates a controlled workflow for generating test traffic,
inspecting packet behavior, applying firewall filtering rules, and
reviewing logs to validate the rule's effect.

> **Scope:** This is a learning lab for systems and networks you own or
> are explicitly authorized to test. Do not generate flood traffic
> against public, production, or third-party systems.

## Objectives

-   Build a virtualized network with a firewall-controlled internal LAN.
-   Configure pfSense interfaces, DHCP, routing, and firewall rules.
-   Capture and inspect network traffic using Wireshark.
-   Generate controlled ICMP or TCP SYN test traffic from Kali Linux.
-   Apply a targeted blocking rule and review the resulting packet
    captures and firewall logs.

## Technology Stack

  -----------------------------------------------------------------------
  Technology                          Role
  ----------------------------------- -----------------------------------
  Oracle VirtualBox                   Hosts the virtual machines and
                                      virtual network adapters

  pfSense                             Firewall, routing, filtering, and
                                      traffic logging

  Kali Linux                          Source of controlled test traffic

  Ubuntu                              Target host and packet-capture
                                      system

  Wireshark                           Packet capture and protocol
                                      analysis

  hping3                              Generates crafted test packets
  -----------------------------------------------------------------------

## Lab Architecture

The documented lab uses three virtual machines:

-   **pfSense:** WAN-facing and internal-LAN firewall/router.
-   **Kali Linux:** Test-traffic source.
-   **Ubuntu:** Target host used to inspect traffic with Wireshark.

## Workflow

1.  Create the pfSense, Kali Linux, and Ubuntu virtual machines.
2.  Configure VirtualBox adapters and assign pfSense WAN and LAN
    interfaces.
3.  Configure LAN DHCP and any required routes.
4.  Verify basic connectivity between the intended lab hosts.
5.  Capture baseline traffic in Wireshark.
6.  Generate controlled test traffic from Kali within the isolated lab.
7.  Add and apply a specific pfSense blocking rule.
8.  Review Wireshark captures and pfSense logs to determine whether the
    traffic was blocked as expected.

## Validation

-   `screenshots/01-pfsense-dashboard.png`
-   `screenshots/02-network-configuration.png`
-   `screenshots/03-wireshark-before.png`
-   `screenshots/04-firewall-block-rule.png`
-   `screenshots/05-wireshark-after.png`
-   `screenshots/06-pfsense-firewall-logs.png`
-   `screenshots/07-pfsense-firewall-logs.png`
-   `screenshots/08-pfsense-firewall-logs.png`
-   `screenshots/09-pfsense-firewall-logs.png`
-   `screenshots/10-pfsense-firewall-logs.png`
-   `screenshots/11-pfsense-firewall-logs.png`

## Repository Layout

``` text
ThreatLens/
├── README.md
├── .gitignore
├── docs/
│   ├── setup-guide.md
│   └── ThreatLens.pdf
├── screenshots/
│   ├── 01-pfsense-dashboard.png
│   ├── 02-network-configuration.png
│   ├── 03-wireshark-before.png
│   ├── 04-firewall-block-rule.png
│   ├── 05-wireshark-after.png
│   ├── 06-pfsense-firewall-logs.png
│   ├── 07-pfsense-firewall-logs.png
│   ├── 08-pfsense-firewall-logs.png
│   ├── 09-pfsense-firewall-logs.png
│   ├── 10-pfsense-firewall-logs.png
│   └── 11-pfsense-firewall-logs.png
├── configs/
│   ├── firewall-rules.md
│   └── network-addressing.md
└── test-cases/
    └── test-cases.md
```

## Security Notes

-   Keep pfSense management access restricted to a trusted management
    network or interface.
-   Do not disable the firewall as a routine setup step.
-   Avoid exposing the pfSense web interface on the WAN.
-   Keep test traffic inside an isolated lab and verify the network path
    before testing.
-   Do not commit passwords, private keys, tokens, sensitive firewall
    exports, or unredacted personal network details.
-   Do not upload VM disk images such as `.vdi` files or full machine
    exports unless you have carefully reviewed their contents and have a
    specific reason to share them.

## Learning Outcomes

-   Virtual network segmentation and routing
-   Stateful firewall policy configuration
-   Packet capture and protocol analysis
-   Basic DoS traffic investigation in a controlled environment
-   Firewall logging and mitigation verification


