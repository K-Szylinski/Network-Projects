# 🚀 Advanced EIGRP Implementation & Route Optimization

![Packet Tracer](https://img.shields.io/badge/Cisco_Packet_Tracer-8.2-005073?style=for-the-badge&logo=cisco&logoColor=white)
![Protocol](https://img.shields.io/badge/Protocol-EIGRP_AS_1-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

## 📖 Project Description
This project focuses on deploying and optimizing the **EIGRP (Enhanced Interior Gateway Routing Protocol)** in a complex, redundant Enterprise network topology.

Unlike basic routing configurations, this lab addresses real-world challenges such as **asymmetric routing**, **suboptimal path selection**, and **routing table bloat**. The goal was to achieve full End-to-End connectivity, ensure high availability (HA) via backup links, and optimize routing updates using manual summarization.

## ⚙️ Key Technical Implementations

### 1. 🔄 Dynamic Routing (EIGRP AS 1)
* Deployed EIGRP on 9 routers (Core, Distribution, Access, and Edge layers).
* configured **Passive Interfaces** to suppress routing updates towards LAN segments (Security best practice).
* Disabled `auto-summary` to support discontiguous networks.

### 2. ⚖️ Path Manipulation & Metric Tuning
* **Challenge:** The default EIGRP metric (based on Bandwidth & Delay) caused Traffic to load-balance or take suboptimal paths through backup gateways (GATE2/EDGE2) instead of the primary high-speed link.
* **Solution:** Manually modified the **Interface Delay** parameter on specific WAN links to influence the DUAL algorithm.
* **Result:** Forced traffic to strictly follow the Primary Path (GATE1 -> EDGE1 -> ISP1), keeping the secondary path in the Topology Table as a Feasible Successor (Backup).

### 3. 📉 Route Summarization (Optimization)
* Implemented **Manual Route Summarization** on Edge Routers (`EDGE1`, `EDGE2`).
* Aggregated internal Enterprise networks (192.168.0.0/16) and Public ranges (19.20.0.0/16) into single summary routes advertised to the ISP.
* **Benefit:** Significantly reduced the size of the Global Routing Table and minimized EIGRP query scope.

### 4. 🛡️ Redundancy & Failover Testing
* **Scenario A:** Simulated failure of the Primary Gateway (GATE1). Result: Immediate failover to GATE2.
* **Scenario B:** Simulated ISP link failure. Result: Traffic rerouted via the backup ISP connection.

## 💻 Configuration Snippets

### EIGRP Basic Setup (Example for GATE1)
```cisco
router eigrp 1
 eigrp router-id 1.1.1.1
 no auto-summary
 passive-interface default
 no passive-interface GigabitEthernet0/0/1
 no passive-interface GigabitEthernet0/0/2
 no passive-interface GigabitEthernet0/0/0.99
 network 192.168.0.0 0.0.255.255
 network 19.20.0.0 0.0.255.255
