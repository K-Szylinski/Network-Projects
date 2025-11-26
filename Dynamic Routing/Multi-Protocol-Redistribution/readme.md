# 🌐 Multi-Protocol Routing & Route Redistribution (OSPF + EIGRP)

![Packet Tracer](https://img.shields.io/badge/Cisco_Packet_Tracer-8.2-005073?style=for-the-badge&logo=cisco&logoColor=white)
![Protocols](https://img.shields.io/badge/Protocols-OSPF_v2_%7C_EIGRP_%7C_Static-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

## 📖 Project Overview
[cite_start]This project simulates a realistic Internet model where Local Area Networks (LANs) connect via Wide Area Networks (WANs) using different routing domains[cite: 90]. Unlike single-protocol environments, this topology integrates **three distinct routing methods**:
1.  [cite_start]**OSPF** (Enterprise LAN)[cite: 91].
2.  [cite_start]**EIGRP AS 1** (ISP/WAN Backbone)[cite: 91].
3.  [cite_start]**EIGRP AS 2** (Public Cloud)[cite: 91].

[cite_start]The core objective is to implement **Route Redistribution** to enable full End-to-End connectivity between these isolated Autonomous Systems, ensuring traffic flows correctly between OSPF, EIGRP, and Static Routing domains[cite: 94, 95].

## ⚙️ Technical Implementation

### 1. 🏢 Enterprise Domain (OSPF Area 0)
* [cite_start]**Protocol:** OSPFv2 (Process ID 1) deployed on Gateway and Edge routers (GATE1/2, EDGE1/2)[cite: 140].
* [cite_start]**Default Routing:** configured static default routes on EDGE routers and propagated them into the OSPF domain using `default-information originate`[cite: 144].
* [cite_start]**Security:** Applied passive interfaces on LAN-facing ports[cite: 143].

### 2. 🌍 WAN Backbone (EIGRP AS 1) & Static Integration
* [cite_start]**Protocol:** EIGRP (AS 1) connecting ISP routers[cite: 204].
* [cite_start]**Static Redistribution:** Since the Enterprise connects to the WAN via Static Routing [cite: 93][cite_start], these static routes are redistributed into the EIGRP WAN process on ISP routers[cite: 218].
* [cite_start]**NBMA Support:** Manually configured EIGRP neighbors for Frame-Relay/Serial links where multicast is restricted[cite: 208].

### 3. ☁️ Public Cloud (EIGRP AS 2) & Mutual Redistribution
* [cite_start]**Protocol:** A separate EIGRP domain (AS 2) for the Public network segment[cite: 228].
* [cite_start]**Mutual Redistribution:** The ISP3 router acts as an **ASBR (Autonomous System Boundary Router)**, performing two-way redistribution between **EIGRP AS 1** and **EIGRP AS 2** to bridge the WAN and Cloud networks[cite: 233].

### 4. ⚖️ Path Optimization
* **Challenge:** Default metrics often lead to suboptimal routing or asymmetric paths.
* [cite_start]**Solution:** Tuned interface metrics (Bandwidth/Delay/Cost) to ensure traffic prefers the Primary Path (GATE1 -> EDGE1 -> ISP1) over backup links[cite: 319].

## 💻 Configuration Snippets

### OSPF Default Route Propagation (Enterprise Edge)
```cisco
router ospf 1
 router-id 3.3.3.3
 passive-interface Serial0/1/0
 network 19.20.22.0 0.0.0.3 area 0
 network 19.20.22.12 0.0.0.3 area 0
 default-information originate

ip route 0.0.0.0 0.0.0.0 Serial0/1/0 
ip route 192.168.0.0 255.255.0.0 Null0 
ip route 19.20.0.0 255.255.0.0 Null0 
