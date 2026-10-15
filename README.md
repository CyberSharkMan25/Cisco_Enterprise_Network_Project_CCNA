# Cisco Enterprise Network Simulation & Security Integration
![Cisco](https://img.shields.io/badge/Cisco-Packet%20Tracer-blue?logo=cisco)
![CCNA](https://img.shields.io/badge/Certification-CCNA%20Topics-005073)
![Firewall](https://img.shields.io/badge/Security-Cisco%20ASA%20%2F%20MPF-red)
![License](https://img.shields.io/badge/License-MIT-green)

## Executive Summary
The primary objective of this project was to synthesize core routing, switching, and security principles mastered during **Cisco Certified Network Associate (CCNA)** preparation into an enterprise-grade simulation environment. Built within Cisco Packet Tracer, the topology incorporates multi-layer switching, dynamic routing protocols, VLAN segmentation, and automated network service provisioning.

To elevate the lab beyond basic academic routing and switching, the environment expands into **Perimeter Security and Threat Management**, integrating a Cisco Adaptive Security Appliance (ASA) firewall configured with Network Address Translation (NAT/PAT), access control lists (ACLs), and the Modular Policy Framework (MPF).

---

## Network Architecture & Core Infrastructure

### 1. Subnetting & Addressing Strategy
* **Design Philosophy:** Subnetting was designed with modular growth in mind, utilizing Variable Length Subnet Masking (VLSM) to accommodate future department expansion without wasting address space.
* **Subnet Allocation Table:**

| Department / Zone | VLAN ID | IP Subnet Range | Gateway Address | Description |
| :--- | :--- | :--- | :--- | :--- |
| **Management** | VLAN 10 | `10.10.10.0/24` | `10.10.10.1` | Network infrastructure management |
| **Sales / Users** | VLAN 20 | `192.168.20.0/24` | `192.168.20.1` | Corporate desktop endpoints |
| **Engineering** | VLAN 30 | `192.168.30.0/24` | `192.168.30.1` | High-bandwidth workstations |
| **Voice / VoIP** | VLAN 40 | `192.168.40.0/24` | `192.168.40.1` | IP Telephony infrastructure |
| **DMZ / Servers** | VLAN 50 | `172.16.50.0/24` | `172.16.50.1` | Internal DNS, DHCP, and Web services |

---

### 2. Layer 2 & Layer 3 Switching Architecture
* **Access vs. Distribution Layers:** Utilized Layer 2 switches at the access tier for endpoint connectivity and Layer 3 switches at the core/distribution tier for high-speed inter-VLAN routing via Switched Virtual Interfaces (SVIs).
* **Trunking Best Practices:** 802.1Q trunk links are configured strictly between switch-to-switch and switch-to-router connections, passing tagged traffic only for permitted VLANs.
* **Access Port Hardening:** Endpoint interfaces (PC, Servers, IP Phones) are locked to access mode, isolating voice traffic on dedicated Voice VLANs alongside data VLANs.

```text
! Sample Access Switch Trunk & Access Config
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 20
 switchport voice vlan 40
 spanning-tree portfast
!
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,40,50
