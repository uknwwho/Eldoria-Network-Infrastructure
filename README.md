# 🌍 The Four Nations Communication Alliance

> **Role:** Network Architect | **Tools:** Cisco Packet Tracer | **Protocols:** VLSM, OSPF/RIPv2, DHCP, DNS, SMTP

## 📜 Project Overview
Eldoria, a land divided into four nations (Aeris, Pyron, Tundra, Terranova) and one rogue state (Umbra), requires a resilient communication network to defend against intercepted messages and sabotage. 

As the lead engineer for the **Council of Connectivity**, I designed and implemented a unified network infrastructure that adheres to strict geopolitical constraints, diverse topographical challenges, and varying security levels.

![Network Topology](topology_diagram.png)

## 🛠️ Technical Specifications

### 1. VLSM Addressing Architecture
[cite_start]The network is divided into 5 primary LAN subnets and multiple WAN links using a custom VLSM tree[cite: 1, 3]:

| Nation | Subnet ID | Mask | CIDR | Usable Hosts | Gateway IP |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Aeris** | 172.16.0.0 | 255.255.248.0 | /21 | 1520 | 172.16.0.1 |
| **Terranova** | 172.16.8.0 | 255.255.252.0 | /22 | 1020 | 172.16.8.1 |
| **Pyron** | 172.16.12.0 | 255.255.252.0 | /22 | 830 | 172.16.12.1 |
| **Tundra** | 172.16.16.0 | 255.255.254.0 | /23 | 410 | 172.16.16.1 |
| **Umbra** | 172.16.18.0 | 255.255.255.192 | /26 | 45 | 172.16.18.1 |

### 2. Routing Protocols
* [cite_start]**Static Routing:** Implemented for high-security zones (Aeris & Pyron) to strictly control traffic paths[cite: 62, 92].
* [cite_start]**RIPv2 Dynamic Routing:** Deployed in Terranova and Tundra to handle unstable connections and simplify redundancy[cite: 141, 170].
* [cite_start]**Floating Static Route:** Aeris is configured with a backup route to Tundra (via Pyron) with an Administrative Distance (AD) of 25, ensuring connectivity if the primary dynamic route fails[cite: 89].

### 3. DHCP & Relay Configuration
* [cite_start]**Central Server:** Terranova (`172.16.8.10`) acts as the central DHCP server for Terranova, Tundra, and Umbra[cite: 58].
* [cite_start]**DHCP Relay:** Tundra and Umbra routers use the `ip helper-address 172.16.8.10` command to forward broadcast requests across the WAN[cite: 157, 177].
* [cite_start]**Local Scope:** Pyron maintains its own isolated DHCP server for military security[cite: 56].

## ⚙️ Key Configuration Snippets

**Aeris Router (Static & Backup)**
```cisco
interface s0/1/1
 description PRIMARY_LINK_TO_TUNDRA
 ip address 172.16.18.85 255.255.255.252
!
ip route 172.16.16.0 255.255.254.0 172.16.18.65 25  <-- Floating Static Route

**Tundra Router (DHCP Relay)**
```cisco
interface g0/0/0
 ip address 172.16.16.1 255.255.254.0
 ip helper-address 172.16.8.10  <-- Forwards requests to Terranova Server

**Terranova Router (RIPv2)**
```cisco
router rip
 version 2
 network 172.16.8.0
 redistribute static  <-- Shares static routes with RIP neighbors

 🚀 How to Run
1. Download: Clone this repository and open Four_Nations_Final.pkt in Cisco Packet Tracer.

2. Verify Web Services: Access www.aeris-alliance.net from any PC to see the "Peace Through Connection" site (Hosted on Aeris Server).

3. Test Email: Send an email from User@Pyron to User@Terranova to verify the SMTP link.

4. Check Redundancy: Use "Simulation Mode" to trace packets from Aeris to Tundra to see the primary vs. backup path selection.

📝 License
This project was created for educational purposes.
