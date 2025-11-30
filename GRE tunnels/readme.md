
# 🔒 Site-to-Site VPN Implementation using GRE Tunnels

![Packet Tracer](https://img.shields.io/badge/Cisco_Packet_Tracer-8.2-005073?style=for-the-badge&logo=cisco&logoColor=white)
![Technology](https://img.shields.io/badge/Tech-GRE_Tunneling-blue?style=for-the-badge)
![Routing](https://img.shields.io/badge/Overlay_Routing-EIGRP_AS_100-orange?style=for-the-badge)

## 📖 Project Overview
This project demonstrates the deployment of a **Site-to-Site VPN (Virtual Private Network)** using **GRE (Generic Routing Encapsulation)** tunnels.

The goal was to create a logical **Overlay Network** that connects geographically dispersed sites (Enterprise HQ, Remote Branch, and Cloud) over a public **Underlay Network** (ISP/WAN). This allows private traffic (e.g., from `UserA` to `UserC`) to be routed transparently across the Internet using private IP addressing.

## 🗺️ Network Topology
![Network Topology Diagram](topology.png)
*(Click the image to view full resolution)*

## ⚙️ Technical Architecture

### 1. 🚇 GRE Tunneling (The Overlay)
* **Technology:** GRE (Generic Routing Encapsulation) creates point-to-point virtual links between routers.
* **Topology:** Hub-and-Spoke design where `GATE1` (HQ) acts as the Hub, connecting to `GATE3` (Cloud) and `GATE4` (Remote).
* **Addressing:** Used a dedicated private subnet `3.3.3.0/29` for tunnel interfaces (/30 for each link) and `3.3.4.0/29` for buckup tunnel interfaces.
* **Encapsulation:** Tunnel Source is the physical WAN interface (Public IP); Tunnel Destination is the Peer's Public IP.

### 2. 🔀 Overlay Routing (EIGRP AS 101)
* **Protocol:** A dedicated EIGRP instance (AS 101) runs *inside* the GRE tunnels.
* **Function:** It exchanges private routes (192.168.x.x, 10.0.10.x) between sites.
* **Benefit:** The underlying ISP routers do not see the private subnets; they only forward the encapsulated GRE packets.

### 3. 🏠 Remote Site Configuration (GATE4)
* **NAT Overload (PAT):** Configured to allow local Internet access for the Remote Branch.
* **DHCP:** Local DHCP server for the 10.0.10.0/24 network.
* **Split Tunneling:** Traffic to the Internet goes out via NAT; traffic to HQ goes via the GRE Tunnel.

### 4. 🛡️ Redundancy Challenge
* **Scenario:** Simulated failure of the Primary Gateway (`GATE1`).
* **Solution:** Designed backup tunnels on `GATE2` to takeover communication, ensuring High Availability (HA) for the VPN service.

## 💻 Configuration Snippets

### GRE Tunnel Setup (Hub - GATE1)
```cisco
! Tunnel to Cloud (GATE3)
interface Tunnel1
 ip address 3.3.3.1 255.255.255.252
 tunnel source GigabitEthernet0/0/1
 tunnel destination 1.2.3.2
!
interface Tunnel2
 ip address 3.3.3.6 255.255.255.252
 mtu 1476
 tunnel source GigabitEthernet0/0/1
 tunnel destination 200.1.1.2
!
