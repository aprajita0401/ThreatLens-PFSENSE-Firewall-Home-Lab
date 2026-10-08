---

### `testcase.md`

``'
# Test Cases: Virtualized pfSense Network Security Lab

## Test Matrix Summary

| Test ID | Test Scenario | Execution Node | Target / Tool | Expected Result | Pass / Fail Criteria |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-NET-01** | pfSense Gateway WAN Provisioning | pfSense Console | DHCP via Host Router | WAN receives valid IPv4 address; routing operational. | Pass if WAN acquires valid IP from upstream subnet. |
| **TC-NET-02** | LAN DHCP Scope Lease | Ubuntu VM | `ifconfig` / `dhclient` | Ubuntu leases IP between `192.168.1.10` and `.245` with gateway `192.168.1.1`. | Pass if assigned IP is inside pool range and default route is `192.168.1.1`. |
| **TC-NET-03** | End-to-End Egress Connectivity | Ubuntu VM | `ping -c 3 google.com` | ICMP packets reach external host; DNS resolves public FQDN. | Pass if 0% packet loss reported across 3 ICMP replies. |
| **TC-RTE-01** | Inter-Subnet Ingress Routing | Kali VM | `ip route` / `ping` | Static route forwards packets destined to `192.168.1.0/24` via pfSense WAN. | Pass if routing table contains entry and ping reaches LAN boundary. |
| **TC-SEC-01** | Temporary Pass Rule Validation | pfSense / Kali | WAN Rule / `hping3` | Ingress packets from Kali reach Ubuntu LAN host successfully. | Pass if packets arrive at destination and counters register. |
| **TC-DOS-01** | ICMP Volumetric Flood Generation | Kali VM | `hping3 -1 --flood` | Kali generates rapid packet stream; Ubuntu Wireshark registers high volume. | Pass if `hping3` transmits packets with sustained traffic spike in Wireshark. |
| **TC-DOS-02** | TCP SYN Flood Generation | Kali VM | `hping3 --flood -S -p 80` | High-rate TCP SYN stream generated targeting port 80. | Pass if Wireshark shows repeated SYN flags on target interface. |
| **TC-FW-01** | Packet Filter Mitigation Enforcement | pfSense WebGUI | WAN Block Rule | Stateful filter terminates and drops malicious Kali flood traffic. | Pass if Wireshark shows zero incoming attack packets on Ubuntu. |
| **TC-LOG-01** | Rule Telemetry & Log Auditing | pfSense WebGUI | System Logs (Firewall) | Block events registered in Normal and Dynamic Views with rule IDs. | Pass if system log entries display red drop icons for Kali source IP. |

---

## Detailed Test Procedures

### Test Case: TC-NET-03 (End-to-End Egress Connectivity)
* **Objective:** Ensure the internal LAN workstation can reach the public internet through the pfSense NAT gateway.
* **Pre-conditions:**
  * pfSense VM running with active WAN lease.
  * Ubuntu VM running with LAN IP assigned via DHCP.
* **Execution Steps:**
  1. Open a terminal inside Ubuntu Desktop.
  2. Execute:
     ```bash
     ping -c 3 google.com
     ```
* **Expected Outcome:** Domain resolves to an external IP, 3 packets are transmitted, 3 packets are received, and packet loss is 0%.

---

### Test Case: TC-DOS-02 (TCP SYN Flood Detection)
* **Objective:** Validate that external volumetric attack traffic crosses the perimeter when an allow rule is active and can be analyzed in Wireshark.
* **Pre-conditions:**
  * Pass rule active on pfSense WAN allowing Kali IP to Ubuntu LAN IP.
  * Wireshark active on Ubuntu VM capturing on interface `enp0s3`.
* **Execution Steps:**
  1. In Kali Linux terminal, run:
     ```bash
     sudo hping3 --flood -S -p 80 <Ubuntu_LAN_IP>
     ```
  2. Let the command run for 3–5 seconds, then stop it with `Ctrl + C`.
  3. Inspect the live capture window in Wireshark on Ubuntu.
* **Expected Outcome:** Wireshark captures a massive surge of TCP SYN packets displaying `[TCP Port numbers reused]` targeting destination port 80.

---

### Test Case: TC-FW-01 (Firewall Mitigation & Telemetry Verification)
* **Objective:** Confirm that the pfSense top-level block rule halts the attack and generates telemetry.
* **Pre-conditions:**
  * Top-level rule created on pfSense WAN with Action `Block`, Protocol matching the attack type, Source matching Kali IP, Destination matching Ubuntu LAN IP, and `Log` enabled.
  * Wireshark running on Ubuntu.
* **Execution Steps:**
  1. From Kali, execute the attack flood:
     ```bash
     sudo hping3 --flood -S -p 80 <Ubuntu_LAN_IP>
     ```
  2. Observe the Wireshark capture window on Ubuntu.
  3. Stop the attack on Kali (`Ctrl + C`).
  4. In the pfSense WebGUI, navigate to:
     * **Firewall > Rules > WAN** (check state/byte counters).
     * **Status > System Logs > Firewall > Dynamic View**.
* **Expected Outcome:**
  * Wireshark displays no attack packets reaching the Ubuntu interface during the flood.
  * The pfSense rule counter shows packet/byte incrementation.
  * The firewall dynamic log displays red `X` drop events recording the Kali IP address as the blocked source.
