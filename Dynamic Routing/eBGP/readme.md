
# 🌍 Global Internet Routing with eBGP & Multi-AS Interconnection

![Packet Tracer](https://img.shields.io/badge/Cisco_Packet_Tracer-8.2-005073?style=for-the-badge&logo=cisco&logoColor=white)
![Protocols](https://img.shields.io/badge/Protocols-eBGP_%7C_OSPF_%7C_EIGRP-red?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

## 📖 Project Overview
This advanced lab simulates a **Global Internet Backbone** connecting four distinct Autonomous Systems (AS). Unlike previous intra-domain labs, this project focuses on **Inter-Domain Routing** using **eBGP (External Border Gateway Protocol)** – the standard protocol of the Internet.

The objective was to establish BGP peering between an Enterprise Network, two ISP Backbones, and a Public Cloud Provider, while simultaneously managing internal routing via OSPF and EIGRP.

## 🗺️ Network Topology
<img width="1699" height="660" alt="image" src="https://github.com/user-attachments/assets/7d309e55-4423-4ea8-b397-f61a9555cc6a" />
*(Click the image to view full resolution)*

## ⚙️ Autonomous System Architecture

| AS Number | Role | Internal Protocol (IGP) | Connected To |
| :--- | :--- | :--- | :--- |
| **AS 1** | Enterprise HQ | OSPF | ISP1 (AS 11), ISP2 (AS 22) |
| **AS 11** | ISP Backbone A | OSPF | Enterprise, Cloud (AS 13) |
| **AS 22** | ISP Backbone B | EIGRP | Enterprise, Cloud (AS 13) |
| **AS 13** | Public Cloud | Static/Connected | ISP1, ISP2 |

## 🔧 Technical Implementation

### 1. 🤝 eBGP Peering Implementation
* Established eBGP neighbor relationships between edge routers:
    * `EDGE1` (AS 1) ↔ `ISP1` (AS 11)
    * `EDGE2` (AS 2) ↔ `ISP2` (AS 22)
    * `GATE3` (AS 13) ↔ `ISP3` & `ISP4`
* Configured manual neighbor definitions since eBGP does not support auto-discovery.

### 2. 📢 Prefix Advertisement & Filtering
* **Controlled Advertisement:** Instead of advertising the entire internal routing table, I selectively advertised only **Public Services** (Server Farm) and **IoT Segments** to the global Internet to preserve security and routing table efficiency.
* **Command Used:** `network [prefix] mask [mask]` under BGP process.

### 3. 🔄 BGP to IGP Redistribution
* **Scenario:** The ISP internal routers (like `ISP3`) need to know how to reach the Enterprise networks learned via BGP at the edge (`ISP1`).
* **Solution:** Redistributed BGP routes **into OSPF** (on ISP1) and **into EIGRP** (on ISP2) to propagate external reachability information throughout the ISP backbones.
* **Metric Tuning:** Assigned seed metrics during redistribution to ensure proper route installation.

### 4. 🔗 Full End-to-End Connectivity
* Achieved full connectivity between the private Enterprise LAN (UserA) and the remote Public Cloud (UserC) traversing multiple Autonomous Systems.
* Validated path selection using `traceroute`, confirming traffic flows through the correct ISP peerings.

## 💻 Configuration Snippets

### eBGP Peering Setup (Enterprise Edge)
```cisco
 bgp router-id 1.12.1.1
 neighbor 19.20.23.2 remote-as 11
 network 192.168.93.0
 network 19.20.21.0 mask 255.255.255.0
 network 19.20.22.0 mask 255.255.255.252
 network 19.20.22.12 mask 255.255.255.252
 network 19.20.23.0 mask 255.255.255.252
!
ip route 192.168.0.0 255.255.0.0 Null0 
ip route 19.20.0.0 255.255.0.0 Null0 
