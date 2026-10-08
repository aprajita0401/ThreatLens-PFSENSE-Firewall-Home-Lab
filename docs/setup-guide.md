# Setup Guide: Virtualized pfSense Network Security Lab

## Overview
This setup guide provides end-to-end instructions for deploying a virtualized network security lab using Oracle VirtualBox and pfSense[cite: 18, 20]. The environment establishes network segmentation between an external network (WAN) representing the upstream network/internet and an isolated internal virtual network (LAN) hosting an Ubuntu victim machine, with a Kali Linux virtual machine operating as the external adversary[cite: 18, 20, 21].

---

## 1. Prerequisites & Topology

### System Requirements
* **Hypervisor:** Oracle VirtualBox $\ge 7.x$ installed on the host operating system (Windows, macOS, or Linux)[cite: 20].
* **Hardware Resources:** Minimum of 8 GB RAM and 40 GB free storage space on the host machine[cite: 20].
* **Host Privileges:** Administrative/root rights on the host system to create and bind bridged network adapters[cite: 20].

### Virtual Machine Resource Matrix
* **pfSense Gateway VM:**
  * **OS Type:** FreeBSD (64-bit)[cite: 20]
  * **CPUs / RAM:** 2 vCPU / 2048 MB RAM[cite: 20]
  * **Storage:** 20 GB dynamically allocated VDI[cite: 20]
  * **Network Adapter 1 (WAN):** Bridged Adapter bound to the host physical NIC[cite: 20, 21]
  * **Network Adapter 2 (LAN):** Internal Network bound to `intnet` (or `LabNet`)[cite: 20, 21]
* **Kali Linux (Attacker VM):**
  * **OS Type:** Debian (64-bit)
  * **CPUs / RAM:** 2 vCPU / 2048 MB RAM
  * **Network Adapter 1:** Bridged Adapter bound to the host physical NIC[cite: 21]
* **Ubuntu Desktop (Victim VM):**
  * **OS Type:** Ubuntu (64-bit)
  * **CPUs / RAM:** 2 vCPU / 2048 MB RAM
  * **Storage:** 15 GB virtual disk
  * **Network Adapter 1:** Internal Network bound to `intnet`

---

## 2. pfSense Installation & Base Configuration

### 2.1 VirtualBox VM Setup & Installation
1. Create a new virtual machine in VirtualBox named `pfSense` and set the type to **FreeBSD (64-bit)**[cite: 20].
2. Allocate 2 vCPUs, 2 GB RAM, and a 20 GB virtual disk[cite: 20].
3. Navigate to **Settings > Storage** and attach the pfSense Community Edition installer ISO to the optical drive[cite: 20].
4. Navigate to **Settings > Network**:
   * **Adapter 1:** Check *Enable Network Adapter*, set *Attached to:* **Bridged Adapter**, and select your active physical network interface (e.g., Wi-Fi or Ethernet)[cite: 20, 21].
   * **Adapter 2:** Check *Enable Network Adapter*, set *Attached to:* **Internal Network**, and enter the name `intnet`[cite: 20, 21].
5. Start the virtual machine and proceed through the pfSense installer, accepting default partition and file system settings[cite: 20, 22].
6. Once the base package installation completes, shut down the VM, remove the ISO image from the virtual drive, and reboot from the virtual disk[cite: 22].

### 2.2 Console Interface Assignment
During the initial boot, interface assignment will display on the console[cite: 22]:
1. When prompted to configure VLANs, enter `n`.
2. Assign the interfaces:
   * **WAN Interface:** `vtnet0` (or `em0`)[cite: 22]
   * **LAN Interface:** `vtnet1` (or `em1`)[cite: 22]
3. Allow pfSense to complete booting until the numbered options menu (0–16) appears.
4. The WAN interface automatically requests an IPv4 address via DHCP from your home router (e.g., `192.168.0.58` or `192.168.29.238`). Note this IP address[cite: 22].
5. The LAN interface defaults to static IP `192.168.1.1/24`[cite: 22, 23].

### 2.3 Initial WebGUI Access via WAN
pfSense blocks incoming traffic on the WAN interface by default. To configure the firewall from your host browser:
1. In the pfSense console menu, enter option `8` to open the shell prompt[cite: 23].
2. Temporarily disable packet filtering by running[cite: 22, 23]:
   ```bash
   pfctl -d