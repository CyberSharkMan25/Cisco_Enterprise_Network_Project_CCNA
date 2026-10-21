# Cisco Enterprise Network Simulation & Security Integration
![Cisco](https://img.shields.io/badge/Cisco-Packet%20Tracer-blue?logo=cisco)
![CCNA](https://img.shields.io/badge/Certification-CCNA%20Topics-005073)
![Firewall](https://img.shields.io/badge/Security-Cisco%20ASA%20%2F%20MPF-red)

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
| **Servers** | VLAN 100 | `10.10.14.0/24` | `10.10.14.255` | Internal DNS, DHCP, and Web services |

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
 switchport access vlan 50 # assign the custom, non-voice vlan
 switchport voice vlan 20 # assign the voice vlan
 spanning-tree portfast
 switchport port-security violation shutdown # if any other mac address attempts to connect to the port, then the port is shut down and triggering a log alert. This is the most extreme option for port security.
 switchport port-security mac-address sticky # sticky mac dynamically learns MAC addresses and adds them to running-configuration to ensure the same port and MAC address relationships are persistent if the switch reboots.

!
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,40,50
```
*| **Layer 3 SVIs for Multilayer Switch Washington D.C.-1:**|
| :--- | :--- | 
| Vlan25 | 10.10.10.1 |      
| Vlan30 | 10.10.10.66 |     
| Vlan40 | 10.10.20.1 |      
| Vlan100 | 10.10.14.1 |      
| Vlan112 | 10.10.12.1 |     
```
* | **Layer 3 SVIs on Multilayer Switch Washington D.C.-2:**|
| :--- | :--- |
| Vlan50 | 10.10.10.129 |    
| Vlan55 | 10.10.10.161 | 
| Vlan58 | 10.10.15.1 |     
| Vlan59 | 10.10.11.1 |


### 3. Routing Architecture
* **Routing Protocols:** The primary routing protocol is OSPF (Open Shortest Path First) and backup static routes if OSPF adjacency failture occurs.  
* **Routing Scheme:**
  | Connected Devices | IP Subnet | 
  | :--- | :--- | 
  | **Multilayer-Switch-Washington-D.C.-1 & Router-Washington-D.C.-1** | 192.168.0.8/30 |
  | **Router-Washington-D.C.-1 & ASA Firewall (Interface G1/1)** | 192.168.0.20/30 |
  | **Router-Washington-D.C.-2 & ASA Firewall (Interface G1/2)** | 192.168.0.24/30 |
  | **Multilayer-Switch-Washington-D.C. & Router-Washington-D.C.-2** | 192.168.0.0/30 |
  
* **External Network:**
* There are two external ISP routers (Verizon (24.0.0.0/30), Comcast (23.24.0.0/30)) 

### 4. ASA-5506X Firewall Configuration
* **Network Interfaces:** Both internal interfaces "INTERNAL-1" and "INTERNAL-2", while the external interfaces "EXTERNAL-Verizon" "EXTERNAL-Comcast"
* **Network Objects & PAT Configuration:**
1. object network INSIDE-RANGE-1
   subnet 10.10.0.0 255.255.224.0
   nat (INTERNAL-1,EXTERNAL-VERIZON) dynamic interface
2. object network INSIDE-RANGE-2
   subnet 10.10.0.0 255.255.224.0
   nat (INTERNAL-2,EXTERNAL-VERIZON) dynamic interface
3. object network INSIDE-RANGE-3
   subnet 10.10.0.0 255.255.224.0
   nat (INTERNAL-1,EXTERNAL-COMCAST) dynamic interface
4. object network INSIDE-RANGE-4
   subnet 10.10.0.0 255.255.224.0
   nat (INTERNAL-2,EXTERNAL-COMCAST) dynamic interface
* **Access Lists:**
1. access-list EXTERNAL_TO_INTERNAL extended permit icmp any any echo-reply #allows for ICMP replies from blocking external to internal ASA-5506X firewall
2. access-list EXTERNAL_TO_INTERNAL extended permit icmp any any unreachable #prevents any ICMP replies from blocking external to internal interfaces on ASA-5506X firewalls 
3. access-list INTERNAL-INTERNAL extended permit icmp any any echo-reply
4. access-list INTERNAL-INTERNAL extended permit icmp any any unreachable
* **Routing Protocols:** Both OSPF and static routes are used to managed internal and external traffic.
* **Modular Policy Framework (MPF):** Allows for filtering by protocols on the router.
  Steps:
  1. Class-map <name-of-class-map>
  2. Class inspection_default -> this is a built-in object that includes common protocols such as DNS, FTP, HTTP, ICMP, and TFTP
  3. Inspect protocol-name

**Troubleshooting Tools:**

Cisco Documentation: Cisco’s documentation can be very difficult to read and can sometimes feel overwhelming. Nevertheless, it is always good practice to get good at reading difficult documentation and translating it into language understandable to your learning style.

AI Models: I primarily used Google Gemini as I found its research and explanations extremely helpful compared to OpenAI or Claude. Copilot was a very close second, but often provided incorrect commands that didn’t apply to packet tracer.



