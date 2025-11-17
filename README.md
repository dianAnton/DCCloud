# ☁️ DCCloud: Enterprise Network Infrastructure Simulation

<div align="center">

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco%20Packet%20Tracer-8.2.2-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white)
![Networking](https://img.shields.io/badge/Networking-IPv4%20%2F%20TCP%2FIP-blue?style=for-the-badge)
![Protocols](https://img.shields.io/badge/Protocols-DNS%20%7C%20HTTP%20%7C%20SMTP%20%7C%20OSPF-orange?style=for-the-badge)

**A comprehensive network topology simulation designed to restore and secure connectivity between residential, public, and enterprise zones.**

</div>

---

## 📖 Overview

**DCCloud** is a network engineering project designed to simulate a scalable infrastructure connecting three distinct environments: a Residential Area (Home Network), a Public Access Zone (CyberCafe), and an Enterprise Data Center (University Servers).

The project focuses on implementing robust **Static Routing**, **NAT (Network Address Translation)**, and essential Application Layer services (DNS, Web, and Email) to ensure seamless connectivity and data integrity across different subnets.

## 🏗️ Network Topology & Design

The architecture consists of a multi-router backbone connecting edge networks. A dedicated aggregation router (`Router-Servidores`) was introduced to manage traffic load for the DNS and Web/Email servers specifically.

![Network Topology](https://github.com/user-attachments/assets/58f02207-82d6-4116-83ff-1f3723f70aa1)
*(Snapshot of the DCCloud Topology in Packet Tracer)*

### 🗺️ Addressing Scheme (Subnetting)
The backbone utilizes a structured private IP addressing plan to interconnect the core routers:

| Network Segment | Network Address | CIDR | Connection Type |
| :--- | :--- | :--- | :--- |
| **Router 1 ↔ Router 2** | `10.0.1.0` | `/24` | Serial / Copper Cross |
| **Router 1 ↔ Router 3** | `10.0.2.0` | `/24` | Serial / Copper Cross |
| **Router 2 ↔ Router 3** | `10.0.3.0` | `/24` | Serial / Copper Cross |
| **Router 2 ↔ Home Gateway** | `10.0.4.0` | `/24` | Ethernet |
| **Router 1 ↔ Server Router** | `10.0.5.0` | `/24` | Ethernet |

## ⚙️ Key Implementations

### 1. Routing & Switching
* **Static Routing:** Manually configured routes ensure deterministic packet delivery between the three main routers, optimizing hop counts.
* **Inter-Router Connectivity:** High-speed Copper Crossover cables are used for backbone stability.

### 2. Services & Protocols
* **Home Network (DHCP & NAT):**
    * Implemented **DHCP** on the Home Gateway to dynamically assign IPs to mobile devices and laptops.
    * **NAT** is operational, translating private internal IPs (e.g., `172.20.x.x`) to the gateway's public IP (`10.0.4.2`) for outbound traffic.
* **CyberCafe (Static Addressing):**
    * Fixed IP allocation (`10.79.12.0/24`) ensures reliability for public terminals and network printers.
* **Data Center (DNS, HTTP, Email):**
    * **DNS Server (`120.136.24.2`):** Resolves `www.portal.uc.cl` and `uc.cl` MX records.
    * **Web/Mail Server (`90.120.12.2`):** Hosts the HTTP portal and manages SMTP/POP3 services.

## Protocol Analysis & Simulation Results

As part of the validation process, deep packet inspection (PDU analysis) was performed to verify protocol behavior.

### Email Communication Flow
The system successfully simulates the full email lifecycle using **SMTP** (Sending) and **POP3** (Retrieving).
1.  **Resolution:** Client queries DNS for `uc.cl` -> DNS returns `90.120.12.2`.
2.  **Transport:** A **TCP 3-Way Handshake** is established to guarantee reliable delivery.
3.  **Transmission:**
    * **SMTP:** Pushes the email from the sender to the server.
    * **POP3:** Authenticates the receiver and pulls the email to the client.

### HTTP Web Traffic
Analysis of the web traffic to `www.portal.uc.cl` revealed:
* **Fragmentation:** Large assets (like base64 images) are segmented into multiple TCP packets.
* **Acknowledgement:** The final TCP ACK confirms the successful reassembly of the web page at the Application Layer.

## 🚀 How to Run

To explore the network configuration and simulation:

1.  **Prerequisites:** Ensure you have **Cisco Packet Tracer 8.2.2** installed.
2.  **Clone the repo:**
    ```bash
    git clone https://github.com/yourusername/DCCloud.git
    ```
3.  **Open the Project:** Launch `T3-8.2.2.pkt` in Packet Tracer.
4.  **Simulate:** Switch to "Simulation Mode" to visualize ICMP, HTTP, and SMTP packets flowing through the network.
