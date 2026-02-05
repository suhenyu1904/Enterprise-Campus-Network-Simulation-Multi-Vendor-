# 🏢 Enterprise Campus Network Topology
## Multi-Vendor High Availability Infrastructure

![Network Badge](https://img.shields.io/badge/Network-Enterprise-blue?style=for-the-badge&logo=cisco)
![Vendor Badge](https://img.shields.io/badge/Multi--Vendor-Cisco%20|%20Huawei%20|%20MikroTik%20|%20Fortinet-orange?style=for-the-badge)
![Status Badge](https://img.shields.io/badge/Status-Production%20Ready-success?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

---

## 📋 Table of Contents
- [Project Overview](#-project-overview)
- [Network Architecture](#-network-architecture--topology)
- [VLAN Design](#-vlan-design--segmentation)
- [Technologies Used](#-key-technologies--protocols)
- [Device Configurations](#-device-configurations)
- [Redundancy & High Availability](#-redundancy--high-availability)
- [Security Implementation](#-security-implementation)
- [Testing & Validation](#-testing--validation)
- [Troubleshooting](#-troubleshooting-guide)
- [Future Enhancements](#-future-enhancements)

---

## 🎯 Project Overview

This project presents a comprehensive **Enterprise Campus Network** simulation designed for educational institutions requiring **High Availability (HA)**, **Multi-Vendor Integration**, and **Layered Security**. 

### 🔑 Key Objectives
- ✅ **Zero-Downtime Architecture**: Automatic failover mechanisms ensure continuous service availability
- ✅ **Multi-Vendor Interoperability**: Seamless integration of Cisco, Huawei, MikroTik, and Fortinet devices
- ✅ **Scalable Design**: Hierarchical model supporting future expansion
- ✅ **Security-First Approach**: Defense-in-depth strategy with VLAN segmentation and firewall policies
- ✅ **Real-World Simulation**: Mirrors production network environments

### 💼 Use Cases
- University/College campus networks
- Corporate multi-building environments
- Training and certification labs
- Network simulation for disaster recovery planning

---

## 🏗️ Network Architecture & Topology

### Hierarchical Network Design Model

```
┌─────────────────────────────────────────────────────────────┐
│                         INTERNET                            │
└─────────────────────┬───────────────────────────────────────┘
                      │
┌─────────────────────▼───────────────────────────────────────┐
│  EDGE LAYER:  Fortinet FortiGate (Firewall/NAT/Gateway)    │
└─────────────────────┬───────────────────────────────────────┘
                      │
┌─────────────────────▼───────────────────────────────────────┐
│  BORDER:      MikroTik RouterOS (Border Router)            │
└─────────────────────┬───────────────────────────────────────┘
                      │
        ┌─────────────┴─────────────┐
        │                           │
┌───────▼────────┐         ┌────────▼───────┐
│ CORE LAYER:    │         │                │
│ Cisco CSR 1000v│◄────────┤ Huawei NE40    │
│ (OSPF Area 0)  │  OSPF   │ (OSPF Area 0)  │
└───────┬────────┘         └────────┬───────┘
        │                           │
        └─────────────┬─────────────┘
                      │
┌─────────────────────▼───────────────────────────────────────┐
│  DISTRIBUTION:  Cisco 3650 (L3 Switch - HSRP Active)       │
│                 Cisco 3650 (L3 Switch - HSRP Standby)      │
└─────────────────────┬───────────────────────────────────────┘
                      │
        ┌─────────────┼─────────────┬─────────────┐
        │             │             │             │
┌───────▼──────┐ ┌───▼──────┐ ┌───▼──────┐ ┌───▼──────┐
│ ACCESS LAYER │ │          │ │          │ │          │
│ Cisco 2960   │ │ 2960     │ │ 2960     │ │ 2960     │
│ VLAN 10,20   │ │ VLAN 30  │ │ VLAN 40  │ │ VLAN 50  │
│ 40,50        │ │          │ │          │ │          │
└──────┬───────┘ └────┬─────┘ └────┬─────┘ └────┬─────┘
       │              │            │            │
   [Faculty]    [Students]    [Lab PCs]    [Guests]
```

### Layer Descriptions

#### 🌍 **Edge & Security Layer**
| Device | Role | Responsibilities |
|--------|------|------------------|
| **Fortinet FortiGate** | Internet Gateway | • NAT/PAT<br>• Stateful Firewall<br>• IPS/IDS<br>• Web Filtering<br>• Static Routing to ISP |
| **MikroTik RouterOS** | Border Router | • OSPF Redistribution<br>• Route Aggregation<br>• Backup Gateway<br>• Policy-Based Routing |

#### 🚀 **Core Layer**
| Device | Role | Responsibilities |
|--------|------|------------------|
| **Cisco CSR 1000v** | Core Router 1 | • OSPF Area 0 (Backbone)<br>• Layer 3 Routing<br>• ECMP Load Balancing |
| **Huawei NE40** | Core Router 2 | • OSPF Area 0 (Backbone)<br>• MTU Tuning for Multi-Vendor<br>• Redundant Path |

#### 🔀 **Distribution Layer**
| Device | Role | Responsibilities |
|--------|------|---|
| **Cisco 3650 (Primary)** | L3 Switch | • HSRP Active (Priority 150)<br>• Inter-VLAN Routing<br>• DHCP Relay |
| **Cisco 3650 (Secondary)** | L3 Switch | • HSRP Standby (Priority 100)<br>• Automatic Failover<br>• STP Root Bridge |

#### 💻 **Access Layer**
| Device | VLAN Support | Port Configuration |
|--------|--------------|-------------------|
| **Cisco 2960 x4** | All VLANs | • Access Ports (User-facing)<br>• Trunk Ports (Uplink)<br>• Port Security<br>• BPDU Guard |

#### 🖥️ **Server Farm**
| Server | Services | Redundancy |
|--------|----------|-----------|
| **Ubuntu Server 22.04** | • DHCP Server<br>• DNS Caching<br>• Syslog Server | • Dual Gateway (VRRP)<br>• RAID Storage<br>• Backup Scripts |

---

## 🏷️ VLAN Design & Segmentation

### VLAN Allocation Table

| VLAN ID | Name | Subnet | Gateway (HSRP) | Purpose | User Count |
|---------|------|--------|----------------|---------|------------|
| **10** | Faculty | 192.168.10.0/24 | 192.168.10.1 | Teaching staff & administration | ~50 |
| **20** | Students | 192.168.20.0/23 | 192.168.20.1 | Student devices | ~500 |
| **30** | Lab-Network | 192.168.30.0/24 | 192.168.30.1 | **Computer labs & research** | ~100 |
| **40** | Servers | 192.168.40.0/24 | 192.168.40.1 | Infrastructure servers | ~20 |
| **50** | Guest | 192.168.50.0/24 | 192.168.50.1 | Visitor access (isolated) | ~30 |
| **99** | Management | 10.10.99.0/24 | 10.10.99.1 | Network device management | N/A |

### VLAN Security Policies

#### VLAN 10 (Faculty)
```
✅ Full internet access
✅ Access to all server VLANs (40)
✅ Restricted access to Student VLAN (20)
❌ No access to Guest VLAN (50)
```

#### VLAN 20 (Students)
```
✅ Internet access (rate-limited)
✅ Access to Lab VLAN (30) during lab hours
✅ Limited server access (web/file servers only)
❌ No inter-VLAN routing to Faculty (10)
```

#### VLAN 30 (Lab-Network) **[NEW]**
```
✅ High-bandwidth internet (priority QoS)
✅ Access to specialized software servers (VLAN 40)
✅ Time-based access control (08:00-20:00)
✅ Deep packet inspection disabled (for development)
❌ Isolated from Guest VLAN (50)
```

#### VLAN 40 (Servers)
```
✅ No direct internet access (DMZ design)
✅ Accessible from all VLANs (controlled by ACLs)
✅ Backup VLAN via separate trunk
⚠️ Requires firewall authentication
```

#### VLAN 50 (Guest)
```
✅ Internet-only access
❌ Completely isolated from internal VLANs
⚠️ Captive portal authentication (via FortiGate)
⏱️ Session timeout: 8 hours
```

#### VLAN 99 (Management)
```
✅ SSH/HTTPS access to all network devices
✅ SNMP polling for monitoring
✅ Syslog collection
❌ No internet routing
🔐 AAA Authentication required
```

---

## 🛠️ Key Technologies & Protocols

### Routing Protocols
- **OSPF v2** (Core Layer - Area 0)
- **Static Routing** (Edge to ISP)
- **Route Redistribution** (MikroTik border)

### High Availability
- **HSRP** (Hot Standby Router Protocol) - Gateway Redundancy
- **VRRP** (Virtual Router Redundancy Protocol) - Server Farm
- **STP** (Spanning Tree Protocol) - Layer 2 Loop Prevention

### Network Services
- **DHCP** (Centralized on Ubuntu Server)
- **DNS** (Forwarder to public resolvers)
- **NTP** (Time synchronization)
- **SNMP v3** (Monitoring with encryption)

### Security Features
- **ACLs** (Access Control Lists) per VLAN
- **Port Security** (MAC address limiting)
- **DHCP Snooping** (DHCP attack prevention)
- **DAI** (Dynamic ARP Inspection)
- **IP Source Guard**
- **802.1X** (Port-based authentication - optional)

---

## 📝 Device Configurations

### 1️⃣ Fortinet FortiGate (Edge Firewall)

#### Basic Configuration
```fortinet
config system interface
    edit "wan1"
        set mode static
        set ip 203.0.113.2/30
        set allowaccess ping https ssh
    next
    edit "internal"
        set mode static
        set ip 10.0.0.1/30
        set allowaccess ping https ssh
    next
end

config router static
    edit 1
        set gateway 203.0.113.1
        set device "wan1"
    next
    edit 2
        set dst 192.168.0.0/16
        set gateway 10.0.0.2
        set device "internal"
    next
end

config firewall policy
    edit 1
        set name "Internal-to-Internet"
        set srcintf "internal"
        set dstintf "wan1"
        set srcaddr "all"
        set dstaddr "all"
        set action accept
        set schedule "always"
        set service "ALL"
        set nat enable
    next
end
```

#### Guest VLAN Captive Portal
```fortinet
config user setting
    set auth-type http
    set auth-ports 1000
end

config firewall policy
    edit 10
        set name "Guest-Captive-Portal"
        set srcintf "internal"
        set dstintf "wan1"
        set srcaddr "VLAN50"
        set action accept
        set schedule "always"
        set service "ALL"
        set nat enable
        set captive-portal enable
    next
end
```

---

### 2️⃣ MikroTik RouterOS (Border Router)

#### Interface & IP Configuration
```mikrotik
/interface ethernet
set [ find default-name=ether1 ] name=ether1-to-FortiGate
set [ find default-name=ether2 ] name=ether2-to-Core

/ip address
add address=10.0.0.2/30 interface=ether1-to-FortiGate
add address=10.0.1.1/30 interface=ether2-to-Core

/ip route
add dst-address=0.0.0.0/0 gateway=10.0.0.1
add dst-address=192.168.0.0/16 gateway=10.0.1.2
```

#### OSPF Configuration
```mikrotik
/routing ospf instance
add name=default router-id=10.0.1.1 redistribute-connected=yes

/routing ospf area
add name=backbone area-id=0.0.0.0 instance=default

/routing ospf interface
add interface=ether2-to-Core area=backbone network-type=point-to-point
```

#### Firewall & NAT
```mikrotik
/ip firewall filter
add chain=input action=accept protocol=icmp
add chain=input action=accept connection-state=established,related
add chain=input action=drop

/ip firewall nat
add chain=srcnat out-interface=ether1-to-FortiGate action=masquerade
```

---

### 3️⃣ Cisco CSR 1000v (Core Router 1)

#### Basic Configuration
```cisco
hostname Core-Router-Cisco
!
interface GigabitEthernet1
 description Link to MikroTik
 ip address 10.0.1.2 255.255.255.252
 ip ospf 1 area 0
 no shutdown
!
interface GigabitEthernet2
 description Link to Huawei Core
 ip address 10.0.2.1 255.255.255.252
 ip ospf 1 area 0
 ip ospf network point-to-point
 ip mtu 1500
 no shutdown
!
interface GigabitEthernet3
 description Link to Distribution Switch
 ip address 10.0.3.1 255.255.255.252
 ip ospf 1 area 0
 no shutdown
!
router ospf 1
 router-id 1.1.1.1
 log-adjacency-changes
 passive-interface default
 no passive-interface GigabitEthernet1
 no passive-interface GigabitEthernet2
 no passive-interface GigabitEthernet3
 default-information originate
!
ip route 0.0.0.0 0.0.0.0 10.0.1.1
!
! SNMP Configuration
snmp-server community NetAdmin RO
snmp-server location Core-Room-A
snmp-server contact netadmin@university.edu
!
! NTP Configuration
ntp server 10.10.99.10
!
! Logging
logging host 192.168.40.10
logging trap informational
```

---

### 4️⃣ Huawei NE40 (Core Router 2)

#### System Configuration
```huawei
sysname Core-Router-Huawei
!
interface GigabitEthernet0/0/1
 description Link to Cisco Core
 ip address 10.0.2.2 255.255.255.252
 undo shutdown
!
interface GigabitEthernet0/0/2
 description Link to Distribution Switch
 ip address 10.0.4.1 255.255.255.252
 undo shutdown
!
ospf 1 router-id 2.2.2.2
 area 0.0.0.0
  network 10.0.2.0 0.0.0.3
  network 10.0.4.0 0.0.0.3
!
! MTU Tuning for Multi-Vendor Compatibility
interface GigabitEthernet0/0/1
 mtu 1500
 ospf mtu-enable
```

---

### 5️⃣ Cisco 3650 Distribution Switch (HSRP Primary)

#### VLAN Creation
```cisco
hostname Distribution-SW1
!
vlan 10
 name Faculty
vlan 20
 name Students
vlan 30
 name Lab-Network
vlan 40
 name Servers
vlan 50
 name Guest
vlan 99
 name Management
!
! Trunk to Core
interface GigabitEthernet1/0/1
 description Uplink to Cisco Core
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,40,50,99
 no shutdown
!
! Trunk to Access Switches
interface range GigabitEthernet1/0/2-5
 description Downlink to Access Switches
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,40,50
 spanning-tree portfast trunk
 no shutdown
```

#### HSRP Configuration (Gateway Redundancy)
```cisco
! VLAN 10 - Faculty
interface Vlan10
 ip address 192.168.10.2 255.255.255.0
 standby 10 ip 192.168.10.1
 standby 10 priority 150
 standby 10 preempt
 standby 10 track GigabitEthernet1/0/1 50
 ip helper-address 192.168.40.10
 no shutdown
!
! VLAN 20 - Students
interface Vlan20
 ip address 192.168.20.2 255.255.255.0
 standby 20 ip 192.168.20.1
 standby 20 priority 150
 standby 20 preempt
 standby 20 track GigabitEthernet1/0/1 50
 ip helper-address 192.168.40.10
 no shutdown
!
! VLAN 30 - Lab Network (NEW)
interface Vlan30
 ip address 192.168.30.2 255.255.255.0
 standby 30 ip 192.168.30.1
 standby 30 priority 150
 standby 30 preempt
 standby 30 track GigabitEthernet1/0/1 50
 ip helper-address 192.168.40.10
 no shutdown
!
! VLAN 40 - Servers
interface Vlan40
 ip address 192.168.40.2 255.255.255.0
 standby 40 ip 192.168.40.1
 standby 40 priority 150
 standby 40 preempt
 no shutdown
!
! VLAN 50 - Guest
interface Vlan50
 ip address 192.168.50.2 255.255.255.0
 standby 50 ip 192.168.50.1
 standby 50 priority 150
 standby 50 preempt
 ip helper-address 192.168.40.10
 no shutdown
!
! VLAN 99 - Management
interface Vlan99
 ip address 10.10.99.2 255.255.255.0
 standby 99 ip 10.10.99.1
 standby 99 priority 150
 standby 99 preempt
 no shutdown
!
ip routing
```

#### Access Control Lists (Inter-VLAN Security)
```cisco
! Block Students from accessing Faculty VLAN
ip access-list extended STUDENT-ACL
 deny   ip 192.168.20.0 0.0.1.255 192.168.10.0 0.0.0.255
 permit ip any any
!
! Block Guest from all internal VLANs
ip access-list extended GUEST-ACL
 permit udp any eq bootpc any eq bootps
 permit udp any any eq domain
 deny   ip any 192.168.0.0 0.0.255.255
 permit ip any any
!
! Apply ACLs
interface Vlan20
 ip access-group STUDENT-ACL in
!
interface Vlan50
 ip access-group GUEST-ACL in
```

#### DHCP Relay Configuration
```cisco
! Already configured in VLAN interfaces above with:
! ip helper-address 192.168.40.10
```

---

### 6️⃣ Cisco 3650 Distribution Switch (HSRP Standby)

```cisco
hostname Distribution-SW2
!
! VLAN configuration identical to SW1
vlan 10,20,30,40,50,99
!
! HSRP Priority = 100 (Lower than Primary)
interface Vlan10
 ip address 192.168.10.3 255.255.255.0
 standby 10 ip 192.168.10.1
 standby 10 priority 100
 standby 10 preempt
 ip helper-address 192.168.40.10
!
interface Vlan20
 ip address 192.168.20.3 255.255.255.0
 standby 20 ip 192.168.20.1
 standby 20 priority 100
 standby 20 preempt
 ip helper-address 192.168.40.10
!
interface Vlan30
 ip address 192.168.30.3 255.255.255.0
 standby 30 ip 192.168.30.1
 standby 30 priority 100
 standby 30 preempt
 ip helper-address 192.168.40.10
!
! (Repeat for VLANs 40, 50, 99)
```

---

### 7️⃣ Cisco 2960 Access Switch Configuration

```cisco
hostname Access-SW1
!
! VLAN Creation
vlan 10,20,30,40,50
!
! Trunk to Distribution
interface GigabitEthernet0/1
 description Uplink to Distribution
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,40,50
 no shutdown
!
! Access Ports for Faculty (VLAN 10)
interface range FastEthernet0/1-10
 description Faculty Offices
 switchport mode access
 switchport access vlan 10
 switchport port-security
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 spanning-tree portfast
 spanning-tree bpduguard enable
 no shutdown
!
! Access Ports for Students (VLAN 20)
interface range FastEthernet0/11-20
 description Student Area
 switchport mode access
 switchport access vlan 20
 switchport port-security
 switchport port-security maximum 1
 spanning-tree portfast
 spanning-tree bpduguard enable
 no shutdown
!
! Access Ports for Lab (VLAN 30) - NEW
interface range FastEthernet0/21-35
 description Computer Lab
 switchport mode access
 switchport access vlan 30
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation shutdown
 spanning-tree portfast
 spanning-tree bpduguard enable
 no shutdown
!
! Guest Ports (VLAN 50)
interface range FastEthernet0/36-40
 description Guest Access
 switchport mode access
 switchport access vlan 50
 spanning-tree portfast
 no shutdown
!
! Global Port Security
ip dhcp snooping
ip dhcp snooping vlan 10,20,30,50
no ip dhcp snooping information option
!
! Management
interface Vlan99
 ip address 10.10.99.11 255.255.255.0
 no shutdown
!
ip default-gateway 10.10.99.1
```

---

### 8️⃣ Ubuntu Server 22.04 (DHCP & DNS)

#### Network Configuration
```bash
# /etc/netplan/01-netcfg.yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    ens33:
      addresses:
        - 192.168.40.10/24
      routes:
        - to: default
          via: 192.168.40.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
```

Apply configuration:
```bash
sudo netplan apply
```

#### DHCP Server Configuration (ISC-DHCP)

Install DHCP Server:
```bash
sudo apt update
sudo apt install isc-dhcp-server -y
```

Configure `/etc/dhcp/dhcpd.conf`:
```bash
# Global Options
default-lease-time 86400;
max-lease-time 172800;
authoritative;

# VLAN 10 - Faculty
subnet 192.168.10.0 netmask 255.255.255.0 {
    range 192.168.10.100 192.168.10.200;
    option routers 192.168.10.1;
    option domain-name-servers 8.8.8.8, 8.8.4.4;
    option domain-name "faculty.university.edu";
}

# VLAN 20 - Students
subnet 192.168.20.0 netmask 255.255.254.0 {
    range 192.168.20.100 192.168.21.250;
    option routers 192.168.20.1;
    option domain-name-servers 8.8.8.8, 8.8.4.4;
    option domain-name "students.university.edu";
}

# VLAN 30 - Lab Network (NEW)
subnet 192.168.30.0 netmask 255.255.255.0 {
    range 192.168.30.50 192.168.30.200;
    option routers 192.168.30.1;
    option domain-name-servers 8.8.8.8, 192.168.40.10;
    option domain-name "lab.university.edu";
    # Static assignments for lab machines
    host lab-pc-01 { hardware ethernet 00:50:56:01:01:01; fixed-address 192.168.30.10; }
    host lab-pc-02 { hardware ethernet 00:50:56:01:01:02; fixed-address 192.168.30.11; }
    # ... (continue for all lab PCs)
}

# VLAN 40 - Servers (Static IPs only)
subnet 192.168.40.0 netmask 255.255.255.0 {
    # No dynamic range - static only
}

# VLAN 50 - Guest
subnet 192.168.50.0 netmask 255.255.255.0 {
    range 192.168.50.10 192.168.50.100;
    option routers 192.168.50.1;
    option domain-name-servers 8.8.8.8;
    max-lease-time 28800; # 8 hours max for guests
}
```

Set interface in `/etc/default/isc-dhcp-server`:
```bash
INTERFACESv4="ens33"
```

Start DHCP service:
```bash
sudo systemctl enable isc-dhcp-server
sudo systemctl start isc-dhcp-server
sudo systemctl status isc-dhcp-server
```

#### DNS Cache Configuration (dnsmasq)

Install dnsmasq:
```bash
sudo apt install dnsmasq -y
```

Configure `/etc/dnsmasq.conf`:
```bash
# Listen on server interface
interface=ens33
listen-address=192.168.40.10

# Upstream DNS servers
server=8.8.8.8
server=8.8.4.4

# Cache size
cache-size=1000

# Local domain resolution
domain=university.edu
local=/university.edu/

# Static DNS records for internal servers
address=/dhcp.university.edu/192.168.40.10
address=/firewall.university.edu/192.168.40.5
```

Restart service:
```bash
sudo systemctl restart dnsmasq
sudo systemctl enable dnsmasq
```

---

## 🔄 Redundancy & High Availability

### Failover Scenarios

#### Scenario 1: Core Router Failure
```
Normal Operation:
Internet → FortiGate → MikroTik → [Cisco Core + Huawei Core] → Distribution

Cisco Core Fails:
Internet → FortiGate → MikroTik → Huawei Core → Distribution
⏱️ Failover Time: ~3 seconds (OSPF convergence)
```

#### Scenario 2: Distribution Switch Failure
```
Normal: All VLANs use SW1 (HSRP Priority 150)

SW1 Fails:
- HSRP transitions to SW2 (Priority 100)
- All gateway IPs remain the same (transparent to users)
⏱️ Failover Time: <3 seconds
✅ Zero client reconfiguration needed
```

#### Scenario 3: Link Failure (Core to Distribution)
```
Dual-homed Distribution switches prevent single point of failure:
- Each Distribution SW has 2 uplinks (to both Core routers)
- STP blocks redundant path until needed
- Automatic failover if primary link fails
```

### Testing High Availability

**Test HSRP Failover:**
```cisco
! On Distribution-SW1 (Primary)
interface GigabitEthernet1/0/1
 shutdown

! Monitor on SW2
show standby brief
! Should show "Active" state for all VLANs

! On client PC
ping 192.168.20.1 -t
! Should show <3 seconds downtime
```

**Test OSPF Convergence:**
```cisco
! On Cisco Core
interface GigabitEthernet2
 shutdown

! Monitor routing table on Huawei
display ip routing-table
! Should see alternate path via MikroTik

! Measure convergence time
debug ospf events
```

---

## 🔒 Security Implementation

### Defense-in-Depth Strategy

#### Layer 1: Edge Security (FortiGate)
- ✅ Stateful firewall inspection
- ✅ Application control (blocking P2P, torrents)
- ✅ Web filtering (malware/phishing)
- ✅ Rate limiting (DDoS mitigation)

#### Layer 2: Access Layer Security (Switches)
```cisco
! Port Security
switchport port-security maximum 2
switchport port-security violation restrict
switchport port-security mac-address sticky

! DHCP Snooping
ip dhcp snooping
ip dhcp snooping vlan 10,20,30,50

! Dynamic ARP Inspection
ip arp inspection vlan 10,20,30,50
ip arp inspection validate src-mac dst-mac ip

! IP Source Guard
ip verify source
```

#### Layer 3: VLAN Isolation
- Students cannot access Faculty VLAN
- Guest VLAN completely isolated
- Lab VLAN restricted to working hours

#### Layer 4: Management Security
```cisco
! SSH Only (No Telnet)
line vty 0 4
 transport input ssh
 login local
 exec-timeout 10 0

! Strong passwords
enable secret <strong-password>
username admin privilege 15 secret <strong-password>

! SNMP v3 (Encrypted)
snmp-server group SNMPv3Group v3 priv
snmp-server user SNMPv3User SNMPv3Group v3 auth sha AuthPass priv aes 128 PrivPass
```

---

## ✅ Testing & Validation

### Connectivity Tests

#### Test 1: VLAN Connectivity
```bash
# From Faculty PC (VLAN 10)
ping 192.168.10.1    # Gateway - Should succeed
ping 192.168.20.1    # Student gateway - Should FAIL (ACL)
ping 8.8.8.8         # Internet - Should succeed
```

#### Test 2: DHCP Assignment
```bash
# On Windows client
ipconfig /release
ipconfig /renew
ipconfig /all

# Expected output:
# IPv4 Address: 192.168.20.xxx (from DHCP range)
# Default Gateway: 192.168.20.1 (HSRP Virtual IP)
# DNS Servers: 8.8.8.8, 8.8.4.4
```

#### Test 3: Redundancy Validation
```bash
# Continuous ping during failover
ping 192.168.20.1 -t

# Shutdown primary distribution switch
# Expected: Max 3 packet losses during HSRP transition
```

### Routing Verification

#### OSPF Neighbor Check
```cisco
! Cisco Core
show ip ospf neighbor

Expected:
Neighbor ID  Pri  State      Dead Time  Address      Interface
2.2.2.2      0    FULL/  -   00:00:35   10.0.2.2     Gi2
10.0.1.1     0    FULL/  -   00:00:38   10.0.1.1     Gi1
```

#### Route Table Verification
```cisco
show ip route ospf

O    192.168.10.0/24 [110/20] via 10.0.3.2
O    192.168.20.0/23 [110/20] via 10.0.3.2
O    192.168.30.0/24 [110/20] via 10.0.3.2
```

---

## 🔧 Troubleshooting Guide

### Common Issues

#### Issue 1: No DHCP Address
```bash
# Check DHCP relay
show ip interface Vlan20
! Verify: IP Helper-address configured

# Check DHCP server
sudo systemctl status isc-dhcp-server
sudo tail -f /var/log/syslog | grep dhcp

# Check port security
show port-security interface Fa0/11
```

#### Issue 2: HSRP Not Activating
```cisco
# Verify HSRP status
show standby brief

# Check interface tracking
show standby Vlan20

# Debug HSRP
debug standby events terse
```

#### Issue 3: OSPF Neighbors Not Forming
```cisco
# Check OSPF configuration
show ip ospf interface brief

# Verify MTU matching
show interface Gi2 | include MTU

# Check area configuration
show ip ospf

# Debug OSPF
debug ip ospf adj
```

#### Issue 4: Inter-VLAN Routing Fails
```cisco
# Verify routing enabled
show ip interface brief | include up

# Check VLAN interfaces
show ip route connected

# Verify SVIs are up
show interface status | include Vlan
```

---

## 🚀 Future Enhancements

### Phase 2 Upgrades
- [ ] **IPv6 Dual-Stack** Implementation
- [ ] **SD-WAN** Integration for multi-ISP failover
- [ ] **QoS** (Quality of Service) for VoIP and video conferencing
- [ ] **802.1X** Port-based authentication
- [ ] **Wireless Controller** integration for WiFi access

### Phase 3 Advanced Features
- [ ] **Network Monitoring** (LibreNMS/Zabbix)
- [ ] **Automated Backups** via Ansible/Python
- [ ] **Netflow** analysis for traffic monitoring
- [ ] **VPN Concentrator** for remote access
- [ ] **IPS/IDS** integration with Suricata

---

## 📚 Documentation & Resources

### Network Diagrams
- `docs/topology-diagram.png` - Physical topology
- `docs/logical-diagram.vsdx` - Logical design
- `docs/vlan-map.xlsx` - VLAN allocation table

### Configuration Backups
```bash
configs/
├── fortinet-fortigate-config.conf
├── mikrotik-border-router.rsc
├── cisco-core-csr1000v.txt
├── huawei-core-ne40.cfg
├── cisco-dist-sw1.txt
├── cisco-dist-sw2.txt
├── cisco-access-sw1-4.txt
└── ubuntu-server-configs/
    ├── dhcpd.conf
    ├── dnsmasq.conf
    └── netplan-config.yaml
```

### Useful Commands Cheat Sheet

#### Cisco IOS
```bash
show ip interface brief           # Interface summary
show ip route                     # Routing table
show standby brief                # HSRP status
show ip ospf neighbor             # OSPF neighbors
show vlan brief                   # VLAN list
show spanning-tree summary        # STP status
```

#### Huawei VRP
```bash
display interface brief           # Interface summary
display ip routing-table          # Routing table
display ospf peer                 # OSPF neighbors
display vlan                      # VLAN list
```

#### MikroTik RouterOS
```bash
/interface print                  # Interfaces
/ip address print                 # IP addresses
/routing ospf neighbor print      # OSPF neighbors
/ip route print                   # Routes
```

#### FortiGate FortiOS
```bash
get system interface physical     # Interfaces
get router info routing-table all # Routes
diagnose debug flow               # Traffic debugging
```

---

## 👥 Contributors & Credits

**Project Lead:** [Your Name]  
**Institution:** [University Name]  
**Course:** Advanced Network Design  
**Semester:** Fall 2024

### Acknowledgments
- Cisco Networking Academy
- Huawei ICT Academy
- MikroTik Certified Trainers
- Fortinet NSE Program

---

## 📄 License

This project is licensed under the **MIT License** - see [LICENSE](LICENSE) file for details.

---

## 📞 Support & Contact

For questions or issues:
- 📧 Email: netadmin@university.edu
- 💬 Discord: [Network Lab Server]
- 📖 Documentation Wiki: [Internal GitLab]

---

**Last Updated:** February 2026  
**Version:** 2.1.0  
**Status:** ✅ Production Ready

---

<div align="center">

**⭐ Star this repository if you find it helpful!**

[![GitHub Stars](https://img.shields.io/github/stars/yourusername/enterprise-network?style=social)](https://github.com/yourusername/enterprise-network)

</div>
