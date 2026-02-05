# Enterprise-Campus-Network-Simulation-Multi-Vendor-
Designing an Infrastructure of Campus Network Topology

# 🏢 Enterprise Campus Network Topology (Multi-Vendor High Availability)

![Network Badge](https://img.shields.io/badge/Network-Enterprise-blue?style=for-the-badge&logo=cisco)
![Vendor Badge](https://img.shields.io/badge/Multi--Vendor-Cisco%20|%20Huawei%20|%20MikroTik%20|%20Fortinet-orange?style=for-the-badge)
![Status Badge](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

## 📋 Project Overview
Project ini adalah simulasi desain jaringan kampus skala Enterprise yang dirancang untuk **High Availability (HA)**, **Redundansi**, dan **Keamanan Berjenjang**. Topologi ini mengintegrasikan perangkat dari berbagai vendor (Cisco, Huawei, MikroTik, Fortinet, Linux) untuk mensimulasikan lingkungan kerja dunia nyata yang heterogen.

Tujuan utama project ini adalah membangun infrastruktur jaringan yang tahan banting (*fault-tolerant*), di mana kegagalan pada satu perangkat atau link tidak akan memutus koneksi internet user.

---

## 🏗️ Network Architecture & Topology

![Topology Diagram](path/to/your-topology-image.png)

Desain jaringan menggunakan model **Hierarchical Network Design** (Core, Distribution, Access, Edge):

### 1. 🌍 Edge & Security Layer
* **Fortinet FortiGate:** Bertindak sebagai Gateway Internet utama, menangani **NAT**, **Firewall Filtering**, dan Routing Static ke arah ISP.
* **MikroTik RouterOS:** Berfungsi sebagai Border Router yang menghubungkan Edge dengan Core Network. Menggunakan routing static ke Fortinet dan redistribusi route ke OSPF internal.

### 2. 🚀 Core Layer (The Backbone)
* **Multi-Vendor Routing:** Menggabungkan **Cisco CSR** dan **Huawei NE40** dalam satu area OSPF (Area 0).
* **Redundancy:** Dual-uplink ke MikroTik (ECMP) menjamin jika salah satu router Core mati, trafik internet tetap berjalan via router lain.
* **Protocol:** OSPFv2 dengan tuning MTU untuk interoperabilitas antar-vendor.

### 3. 🔀 Distribution Layer
* **Cisco Switch L3:** Menggunakan **HSRP (Hot Standby Router Protocol)** untuk menyediakan Gateway Redundan bagi VLAN user.
* **DHCP Relay:** Meneruskan request IP dari VLAN User ke Server Farm terpusat.

### 4. 💻 Access Layer
* Segmentasi VLAN untuk keamanan dan manajemen broadcast:
    * **VLAN 10:** Dosen/Staff
    * **VLAN 20:** Mahasiswa
    * **VLAN 50:** Tamu/Guest

### 5. 🖥️ Server Farm
* **Linux Ubuntu Server:** Berfungsi sebagai **DHCP Server** terpusat.
* **VRRP Gateway:** Menggunakan VRRP antara Cisco dan Huawei agar Server memiliki gateway redundan.

---

## 🛠️ Key Technologies & Configurations

### ✅ High Availability (HSRP & VRRP)
Implementasi Gateway Redundancy agar User tidak perlu ganti IP Gateway saat router utama down.

**Cisco (HSRP Configuration Snippet):**
```bash
interface Vlan20
 ip address 192.168.20.2 255.255.255.0
 standby 20 ip 192.168.20.1
 standby 20 priority 150
 standby 20 preempt
