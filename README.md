\# Network Project: Branch, Main Office, IT \& HR



\## Overview

A corporate network designed in Cisco Packet Tracer with three main sites: Main Office, IT \& HR, and Branch. The network uses EIGRP AS 100 for dynamic routing, DHCP for client devices, VLANs for segmentation, and static addressing for servers and IT/HR devices.



\---



\## Full Network Topology



!\[Full Network Topology](images/full-topology.png)



\*\*Main components:\*\*

\- Main Office (R2, Switch0, DHCP server, PC0, PC1)

\- IT \& HR (R1, Switch2, IT PCs, HR devices, Tech Support, DNS server)

\- Branch (R3, Switch1, PC4, PC5)



\---



\## 1. Branch



!\[Branch Topology](images/branch.png)



\- Router R3, Switch1, PC4, PC5

\- DHCP for PC4 and PC5

\- Loopback: 10.1.1.1/32

\- Serial link to R2: 11.1.1.2

\- LAN: 192.168.3.0/24



\*\*EIGRP on R3:\*\*

```cisco

router eigrp 100

&#x20;network 192.168.3.0 0.0.0.255

&#x20;network 11.1.1.0 0.0.0.255

&#x20;network 10.1.1.0 0.0.0.255

&#x20;no auto-summary

```



\*\*Routing table:\*\*

!\[EIGRP R3](images/eigrp-r3.png)



\---



\## 2. Main Office



!\[Main Office Topology](images/main-office.png)



\- Router R2, Switch0, DHCP Server, PC0, PC1

\- VLAN 10 (users\_office), VLAN 20 (DHCP\_server)

\- Loopback: 9.9.9.1/32

\- Serial link to R3: 11.1.1.1

\- Serial link to R1: 10.1.1.1



\*\*VLAN Database on Switch0:\*\*

!\[VLAN Switch0](images/vlan-switch0.png)



\*\*VLANs configuration:\*\*

```cisco

vlan 10

&#x20;name users\_office

vlan 20

&#x20;name DHCP\_server

interface fa0/1

&#x20;switchport mode trunk

interface fa0/2

&#x20;switchport mode access

&#x20;switchport access vlan 20

interface fa0/3

&#x20;switchport mode access

&#x20;switchport access vlan 10

interface fa0/4

&#x20;switchport mode access

&#x20;switchport access vlan 10

```



\*\*EIGRP on R2:\*\*

```cisco

router eigrp 100

&#x20;network 192.168.20.0 0.0.0.255

&#x20;network 192.168.1.0 0.0.0.255

&#x20;network 9.9.9.0 0.0.0.255

&#x20;network 10.0.0.0 0.0.0.255

&#x20;network 11.1.1.0 0.0.0.255

&#x20;no auto-summary

```



\*\*Routing table:\*\*

!\[EIGRP R2](images/eigrp-r2.png)



\---



\## 3. IT \& HR Department



!\[IT \& HR Topology](images/it-hr.png)



\- Router R1

\- Switch2 with VLANs 30, 40, 14, 1

\- Loopback: 8.8.8.1/32

\- Serial link to R2: 10.1.1.2

\- LAN to Switch2: 192.168.2.1



\### 3.1 IT Department (VLAN 30)



!\[IT Department](images/it-department.png)



| Device | IP | Gateway | DNS |

|--------|-----|---------|-----|

| PC2 | 192.168.50.2 | 192.168.50.1 | 192.168.2.2 |

| PC3 | 192.168.50.3 | 192.168.50.1 | 192.168.2.2 |



\### 3.2 HR Department (VLAN 40)



!\[HR Department](images/hr-department.png)



| Device | IP | Gateway | DNS |

|--------|-----|---------|-----|

| Laptop0 | 192.168.60.2 | 192.168.60.1 | 192.168.2.2 |

| Printer0 | 192.168.60.3 | 192.168.60.1 | 192.168.2.2 |

| Laptop1 | 192.168.60.4 | 192.168.60.1 | 192.168.2.2 |

| Tablet PC0 | 192.168.60.5 | 192.168.60.1 | 192.168.2.2 |



HR devices connect wirelessly through an Access Point.



\### 3.3 Tech Support (VLAN 14)



!\[Tech Support](images/tech-support.png)



| Device | IP | Gateway | DNS |

|--------|-----|---------|-----|

| PC6 | 192.168.70.2 | 192.168.70.1 | 192.168.50.50 |



\### 3.4 DNS Server (VLAN 1)



!\[DNS Server](images/dns-server.png)



| Device | IP | Gateway |

|--------|-----|---------|

| Server-PT DNS | 192.168.2.2 | 192.168.2.1 |



\*\*Router-on-a-Stick configuration (R1):\*\*

```cisco

interface fa0/0.14

&#x20;encapsulation dot1Q 14

&#x20;ip address 192.168.70.1 255.255.255.0

interface fa0/0.40

&#x20;encapsulation dot1Q 40

&#x20;ip address 192.168.60.1 255.255.255.0

```



\*\*EIGRP on R1:\*\*

```cisco

router eigrp 100

&#x20;network 192.168.2.0 0.0.0.255

&#x20;network 192.168.50.0 0.0.0.255

&#x20;network 192.168.70.0 0.0.0.255

&#x20;network 10.1.1.0 0.0.0.255

&#x20;network 192.168.60.0 0.0.0.255

&#x20;network 8.8.8.0 0.0.0.255

&#x20;no auto-summary

```



\*\*Routing table:\*\*

!\[EIGRP R1](images/eigrp-r1.png)



\---



\## Protocols and Technologies Used

\- \*\*EIGRP AS 100\*\* — dynamic routing between R1, R2, R3

\- \*\*DHCP\*\* — automatic IP assignment for Main Office and Branch PCs

\- \*\*VLANs\*\* — network segmentation (10, 20, 30, 40, 14, 1)

\- \*\*802.1Q Trunking\*\* — Router-on-a-Stick on R1

\- \*\*Wireless\*\* — HR devices connect via Access Point

\- \*\*Loopback interfaces\*\* — 8.8.8.1, 9.9.9.1, 10.1.1.1



\## IP Addressing Summary



| Site | Network | Gateway |

|------|---------|---------|

| Branch LAN | 192.168.3.0/24 | 192.168.3.1 |

| Main Office LAN | 192.168.1.0/24 | 192.168.1.1 |

| DHCP Server | 192.168.20.0/24 | 192.168.20.1 |

| DNS Server | 192.168.2.0/24 | 192.168.2.1 |

| IT | 192.168.50.0/24 | 192.168.50.1 |

| HR | 192.168.60.0/24 | 192.168.60.1 |

| Tech Support | 192.168.70.0/24 | 192.168.70.1 |

| R1 ↔ R2 link | 10.1.1.0/24 | — |

| R2 ↔ R3 link | 11.1.1.0/24 | — |



\## Files

\- `network\_project.pkt` — Cisco Packet Tracer file

\- `images/` — Topology and configuration screenshots



\## How to Open

1\. Install Cisco Packet Tracer (v8.x recommended).

2\. Open `network\_project.pkt`.

3\. Verify EIGRP with `show ip route eigrp` on R1, R2, R3.

4\. Verify VLANs with `show vlan brief` on Switch0.



\## Author

\[اكتب اسمك هنا]

