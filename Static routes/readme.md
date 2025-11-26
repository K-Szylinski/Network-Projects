# 🏢 Enterprise Network Simulation with Static Routing

![Packet Tracer](https://img.shields.io/badge/Cisco_Packet_Tracer-8.2-005073?style=for-the-badge&logo=cisco&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

## 📖 Project Overview
This project demonstrates a comprehensive **Enterprise Network design** simulated in Cisco Packet Tracer. The primary goal was to design a scalable infrastructure with **High Availability (HA)** and manually configured **Static Routing** to ensure precise traffic control between the Corporate LAN, WAN, and Remote Sites.

The topology simulates a real-world scenario connecting a Main Campus (HQ) to an ISP backbone and remote cloud services.

## 🗺️ Network Topology
<img width="964" height="367" alt="image" src="https://github.com/user-attachments/assets/39fe71c8-d5ba-4a23-b166-32ed3c8e7b68" />

*(Click the image to view full resolution)*

## ⚙️ Key Features & Technologies

### 1. 🔀 Routing Strategy (Static Routing)
* **Manual Route Configuration:** Full implementation of static routes across all routers to establish end-to-end connectivity.
* **Next-Hop Logic:** Precise definition of gateways for LAN-to-WAN traffic.
* **Default Routes:** Configuration of Gateway of Last Resort for edge devices.

### 2. 🏗️ High Availability & Redundancy
* **Redundant Links:** The LAN design includes dual connections between Access, Distribution, and Core layers to prevent Single Points of Failure (SPOF).
* **Redundant Gateways:** Implementation of backup routers (GATE1/GATE2) ensuring continuous uptime.

### 3. 🛡️ Network Segmentation (VLANs & Zones)
* **Private Zone:** Dedicated VLANs for internal users (UserA, UserB).
* **IoT Zone:** A separate segment for smart devices (Cameras, SBC) connected via Wireless AP.
* **Server Farm:** Isolated DMZ-like area for public-facing servers (ServPub).

### 4. 🌐 WAN & ISP Backbone
* **Serial Connections:** Simulation of long-distance WAN links using Serial interfaces (red links).
* **ISP Simulation:** A mesh of routers (ISP1-ISP4) simulating the Internet backbone connecting the HQ to Cloud Services.

## 🛠️ Hardware & Configuration
* **Routers:** Cisco ISR 4331 / 2911 (Edge & Core Routing).
* **Switches:** Cisco 2960 (Access) & 3650 (Multilayer Distribution).
* **Endpoints:** PCs, Laptops, IoT Devices, Servers.
* **Connections:** GigabitEthernet (LAN), Serial (WAN), Wireless (IoT).

## 🚀 How to Run
1.  Ensure you have **Cisco Packet Tracer 8.0** or newer installed.
2.  Download the `.pkt` file from this repository.
3.  Open the file and use the **Simulation Mode** to trace packets (ICMP) from `UserA` to `Cloud Server`.

---
*Created by [Krzysztof Szyliński](https://github.com/K-Szylinski)*
