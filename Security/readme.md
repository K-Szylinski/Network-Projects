# 🛡️ Advanced Router Hardening & Network Security

![Packet Tracer](https://img.shields.io/badge/Cisco_Packet_Tracer-8.2-005073?style=for-the-badge&logo=cisco&logoColor=white)
![Security](https://img.shields.io/badge/Security-SSH_v2_%7C_ACL_%7C_AAA-red?style=for-the-badge)
![Compliance](https://img.shields.io/badge/Compliance-NTP_%7C_Syslog_%7C_RBAC-green?style=for-the-badge)

## 📖 Project Overview
This project focuses on **Infrastructure Hardening** and implementing a **Defense-in-Depth** strategy for enterprise network devices.

Unlike standard routing labs, this exercise emphasizes securing the **Management Plane** and **Control Plane** of routers. The goal is to mitigate common attack vectors (e.g., unauthorized access, route injection, man-in-the-middle) and establish robust auditing and monitoring capabilities using NTP and Syslog.

## 🗺️ Network Topology
![Network Topology Diagram](topology.png)
*(Click the image to view full resolution)*

## 🔐 Security Implementations

### 1. 🛂 Secure Management Access (SSH & ACLs)
* **Transition from Telnet to SSH:** Disabled insecure Telnet access and implemented **SSHv2** with 1536-bit RSA keys to encrypt management traffic.
* **Management Plane Protection (MPP):** Applied **Standard Access Control Lists (ACLs)** to VTY lines, restricting administrative access to a specific "Admin PC" only.
* **Login Security:** Enforced minimum password length, configured `service password-encryption`, and set aggressive execution timeouts.

### 2. 📡 Control Plane Security (OSPF Authentication)
* **Route Authentication:** Configured **Cryptographic Authentication (MD5/HMAC)** for OSPFv2 neighbors.
* **Objective:** Prevents rogue routers from forming adjacencies and injecting malicious routes (poisoning) into the global routing table.

### 3. 👁️ Auditing & Monitoring (NTP & Syslog)
* **Secure NTP:** Configured Authenticated NTP (Network Time Protocol) hierarchical topology (Stratum 3) to ensure precise timestamp synchronization across all devices.
* **Centralized Logging:** Enabled **Syslog** forwarding to a central cloud server to store audit trails of configuration changes and interface events.

### 4. 🎭 Role-Based Access Control (RBAC)
* **Parser Views:** Implemented **Cisco IOS Role-Based CLI Access**.
* **Segregation of Duties:** Created restricted views (e.g., `Admin2`), limiting junior admins to specific commands (e.g., `show ip`, `debug`) while denying access to global configuration modes.

## 💻 Configuration Snippets

### SSHv2 & VTY Access Control
```cisco
! Enable SSH Version 2
ip ssh version 2
crypto key generate rsa general-keys modulus 1536
!
! Restrict VTY Access via ACL
access-list 1 permit host 192.168.99.99
access-list 1 deny any
!
line vty 0 4
 transport input ssh
 access-class 1 in
