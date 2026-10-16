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
* **Default Gateways:** All default gateways were set as the first usable IP address in the subnet.
* **Subnet Allocation Table:**

| Department / Zone | VLAN ID | IP Subnet Range | Gateway Address | Description |
| :--- | :--- | :--- | :--- | :--- |
| **Sales** | VLAN 50 | `10.10.10.129/29` | `10.10.10.130` | Corporate sales department desktop endpoints |
| **Customer Service** | VLAN 59 | `10.10.11.0/29` | `10.10.11.1` | Corporate desktop endpoints |
| **Product Development** | VLAN 58 | `10.10.15.0/29` | `10.10.15.2` | Corporate product development department desktop endpoints |
| **Marketing** | VLAN 20 | `10.10.10.161/29` | `10.10.10.162` | Corporate marketing department desktop endpoints |
| **Human Resources** | VLAN 30 | `10.10.10.66/29` | `10.10.10.67` | Corporate human resource department desktop endpoints |
| **Finance** | VLAN 40 | `10.10.20.0/29` | `10.10.20.2` | Corporate Finance department desktop endpoints |
| **Information Technology** | VLAN 20 | `10.10.10.0/25` | `10.10.10.1` | Corporate IT department desktop endpoints |
| **Legal** | VLAN 112 | `10.10.12.0/28` | `10.10.12.2` | Corporate legal department desktop endpoints |
| **Voice / VoIP** | VLAN 20 | `192.168.40.0/24` | `192.168.40.1` | IP Telephony infrastructure |
| **DMZ / Servers** | VLAN 100 | `10.10.14.0/24` | `10.10.14.255` | Internal DNS, DHCP, and Web services |

---

### 2. Layer 2 & Layer 3 Switching Architecture
* **Access vs. Distribution Layers:** Utilized Layer 2 switches at the access tier for endpoint connectivity and Layer 3 switches at the core/distribution tier for high-speed inter-VLAN routing via Switched Virtual Interfaces (SVIs).
* **Trunking Best Practices:** 802.1Q trunk links are configured strictly between switch-to-switch and switch-to-router connections, passing tagged traffic only for permitted VLANs.
* **Native VLAN:** **415** is the native VLAN for the network.
* **Trunk Connections:**:
  | Connected Devices | VLANS Included | 
  | :--- | :--- | 
  | **Switch-Washington-D.C.-1 & Multilayer-Switch-Washington-D.C.-1** | 50,55,58,59 |
  | **Switch-Washington-D.C.-2 & Multilayer-Switch-Washington-D.C.-2** | 10,20,25,30,40,112 |

* **Access Port Hardening:** Endpoint interfaces (PC, Servers, IP Phones) are locked to access mode, isolating voice traffic on dedicated Voice VLANs alongside data VLANs.

```text
! Sample Access Switch Trunk & Access Config
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 50
 switchport voice vlan 20
 spanning-tree portfast
!
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,40,50
```

### 3. Routing Architecture
* **Routing Protocols:** The primary routing protocol is OSPF (Open Shortest Path First) and backup static routes if OSPF adjacency failture occurs.  
| Department / Zone | VLAN ID | IP Subnet Range | Gateway Address | Description |
| :--- | :--- | :--- | :--- | :--- |
| **Management** | VLAN 10 | `10.10.10.0/24` | `10.10.10.1` | Network infrastructure management |

### 4. Port Security


### 5. ASA-5506X Firewall Configuration


