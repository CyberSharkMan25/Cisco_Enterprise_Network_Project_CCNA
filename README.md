# Cisco_Enterprise_Network_Project_CCNA
Enterprise Cisco Packet Tracer Simulation & Security Integration

Project Overview

The primary objective of this project was to reinforce and apply core routing, switching, and security principles mastered during my Cisco Certified Network Associate (CCNA) certification preparation. The network architecture incorporates multi-layer switching, dynamic routing protocols, VLAN segmentation, and automated service provisioning.

Beyond core CCNA competencies, this project expands into enterprise perimeter security, implementing Cisco ASA Firewall policies, Network Address Translation (NAT/PAT), and Modular Policy Framework (MPF) inspection rules to emulate a production-grade enterprise environment.

Core Network Architecture & Implementation

1. Subnetting & Addressing

Design Strategy: Subnets were strategically engineered with future organizational scalability and growth in mind, ensuring sufficient address space per department without excessive waste.

Allocation Overview:

[Insert your specific subnet ranges here, e.g., 192.168.10.0/24 for Engineering, 192.168.20.0/24 for Sales, etc.]

2. Routing Protocols (OSPF & Static Routing)

Open Shortest Path First (OSPF): Configured as the primary interior gateway protocol across all core router links within the 192.168.0.0/24 infrastructure range. Adjacencies were established and verified using explicit wildcard masking to ensure comprehensive interface coverage:

network 192.168.0.0 0.0.0.255 area 0


Static Routing: Implemented as reliable backup routes to maintain high availability and fault tolerance in the event of an OSPF path failure.

3. Layer 2 & Layer 3 Switching

Topology Integration: Utilized a combination of Layer 2 switches (access layer) and Layer 3 switches (distribution/core layer) to optimize traffic flow, minimize broadcast domains, and gain hands-on experience with inter-VLAN routing and Switched Virtual Interfaces (SVIs).

4. VLAN Segmentation & Port Security

Trunk Ports: Configured strictly according to enterprise best practices, utilizing dedicated trunk links exclusively between switches to propagate VLAN tagging information.

Access Ports: All end-user endpoints (workstations, servers, and IP phones) were mapped to dedicated access ports, strictly limited to a single data VLAN and voice VLAN where applicable.

5. Dynamic Host Configuration Protocol (DHCP)

Provisioning: Configured centralized DHCP pools across the environment to automatically provision IP configurations to endpoints across all departments.

Troubleshooting Note: Addressed early configuration hurdles regarding IP helper-address (ip helper-address) configurations to ensure seamless DHCP discovery packet propagation from VLANs on Floor 1 across to the centralized DHCP server.

6. Domain Name System (DNS)

Enterprise Simulation: Deployed a dedicated internal DNS server to mirror production network security standards. DNS configurations are automatically pushed to clients via DHCP option parameters across all subnet pools.

Perimeter Security & Firewall Integration (Cisco ASA)

To transition the lab from a standard academic network to an enterprise-grade architecture, a Cisco ASA firewall was integrated at the perimeter.

Network Objects: Grouped specific subnet ranges into logical objects to streamline access control lists (ACLs) and NAT translation statements.

NAT Configuration via Port Address Translation (PAT): Implemented PAT to conserve public IPv4 address space for external communication. This mapped internal private subnets to external simulated ISP uplinks (Verizon and Comcast).

Interfaces & Security Levels: Configured physical and logical interface IP addresses, subnet masks, and security zones to establish clear trust boundaries.

Modular Policy Framework (MPF): Leveraged MPF to enable application inspection and traffic normalization.

Class-Map Implementation: Utilized the built-in inspection_default class-map to handle common application protocols securely (including DNS, FTP, HTTP, ICMP, and TFTP):

class-map inspection_default
inspect dns
inspect http
inspect icmp


Access Control Lists (ACLs): Configured targeted inbound ACL rules on external interfaces to enforce granular traffic filtering and least-privilege access.

Methodology & Engineering Resources

Building complex simulation environments requires robust troubleshooting workflows and reference utilization:

Cisco Official Documentation: Navigating detailed, enterprise-grade technical documentation proved invaluable for translating complex theory into precise command-line configurations.

AI-Assisted Engineering: Utilized Google Gemini as a primary technical research assistant for deep-dive protocol explanations and syntax validation, outperforming alternative platforms in accuracy for network simulation debugging.
