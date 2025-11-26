# 🌐 Multi-Protocol Routing & Route Redistribution (OSPF + EIGRP)

![Packet Tracer](https://img.shields.io/badge/Cisco_Packet_Tracer-8.2-005073?style=for-the-badge&logo=cisco&logoColor=white)
![Protocols](https://img.shields.io/badge/Protocols-OSPF_v2_%7C_EIGRP_%7C_Static-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

## 📖 Project Overview
This project simulates a realistic Internet model where Local Area Networks (LANs) connect via Wide Area Networks (WANs) using different routing domains. Unlike single-protocol environments, this topology integrates **three distinct routing methods**:
1.  **OSPF** (Enterprise LAN).
2.  **EIGRP AS 1** (ISP/WAN Backbone).
3.  **EIGRP AS 2** (Public Cloud).

The core objective is to implement **Route Redistribution** to enable full End-to-End connectivity between these isolated Autonomous Systems, ensuring traffic flows correctly between OSPF, EIGRP, and Static Routing domains.

## ⚙️ Technical Implementation

### 1. 🏢 Enterprise Domain (OSPF Area 0)
* **Protocol:** OSPFv2 (Process ID 1) deployed on Gateway and Edge routers (GATE1/2, EDGE1/2).
* **Default Routing:** Configured static default routes on EDGE routers and propagated them into the OSPF domain using `default-information originate`.
* **Security:** Applied passive interfaces on LAN-facing ports.

### 2. 🌍 WAN Backbone (EIGRP AS 1) & Static Integration
* **Protocol:** EIGRP (AS 1) connecting ISP routers.
* **Static Redistribution:** Since the Enterprise connects to the WAN via Static Routing, these static routes are redistributed into the EIGRP WAN process on ISP routers.
* **NBMA Support:** Manually configured EIGRP neighbors for Frame-Relay/Serial links where multicast is restricted.

### 3. ☁️ Public Cloud (EIGRP AS 2) & Mutual Redistribution
* **Protocol:** A separate EIGRP domain (AS 2) for the Public network segment.
* **Mutual Redistribution:** The ISP3 router acts as an **ASBR (Autonomous System Boundary Router)**, performing two-way redistribution between **EIGRP AS 1** and **EIGRP AS 2** to bridge the WAN and Cloud networks.

### 4. ⚖️ Path Optimization
* **Challenge:** Default metrics often lead to suboptimal routing or asymmetric paths.
* **Solution:** Tuned interface metrics (Bandwidth/Delay/Cost) to ensure traffic prefers the Primary Path (GATE1 -> EDGE1 -> ISP1) over backup links.

## 💻 Configuration Snippets

### OSPF Default Route Propagation (Enterprise Edge)
```cisco
router ospf 1
 router-id 3.3.3.3
 passive-interface Serial0/1/0
 network 19.20.22.0 0.0.0.3 area 0
 network 19.20.22.12 0.0.0.3 area 0
 default-information originate
!
ip route 0.0.0.0 0.0.0.0 Serial0/1/0 
ip route 192.168.0.0 255.255.0.0 Null0 
ip route 19.20.0.0 255.255.0.0 Null0 
