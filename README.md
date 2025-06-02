# 🏢 Enterprise Network Simulation – Multi-VLAN, OSPF, HSRP, DHCP, ACL & WiFi

## 📋 Project Overview
This Packet Tracer project simulates a **medium-sized enterprise network** with a headquarters (HQ) and a branch office (Sucursal). It is designed to demonstrate core CCNA-level enterprise networking concepts including:

- ✅ VLAN segmentation per site
- ✅ Redundant gateway setup using HSRP
- ✅ OSPF routing between routers
- ✅ Local and centralized DHCP configuration
- ✅ Access Control Lists (ACLs) for VLAN isolation
- ✅ Integration of wireless networking via Access Points (APs)

---

## 🧱 Network Topology Summary

**Headquarters (HQ):**
- VLAN 10 – AdminHQ – 192.168.10.0/24
- VLAN 20 – ITHQ – 192.168.20.0/24
- VLAN 50 – WiFiHQ – 192.168.50.0/24
- VLAN 99 – ManagementHQ – 192.168.99.0/24
- DHCP/DNS Server: 192.168.99.10
- Redundancy via HSRP (R1 & R2)

**Sucursal (Branch):**
- VLAN 110 – AdminSucursal – 192.168.110.0/24
- VLAN 120 – ITSucursal – 192.168.120.0/24
- VLAN 130 – WiFiSucursal – 192.168.130.0/24
- VLAN 199 – ManagementSucursal – 192.168.199.0/24
- DHCP local in R3 (Sucursal router)

**WAN Links:**
- R1 ↔ R3: 10.0.0.0/30
- R2 ↔ R3: 10.0.0.4/30
![image alt]([project2drawio.svg](https://github.com/hayligg/HSRP---DHCP---AP/blob/74cb5e87f724667a03794be5ea93734946b32da0/Porject%20dhcp%20hsrp.png))
---

## ⚙️ Key Technologies Demonstrated

### 🔀 HSRP (Hot Standby Router Protocol)
Implemented on R1 and R2 for VLANs in HQ, using priorities and preempt to ensure R1 is active unless failure occurs.

**Example:**
```bash
interface g0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.2 255.255.255.0
 standby 10 ip 192.168.10.1
 standby 10 priority 110
 standby 10 preempt
```

### 📡 OSPF (Open Shortest Path First)
Dynamic routing used across all routers (R1, R2, R3), advertising all local networks.

**R1 OSPF:**
```bash
router ospf 1
 router-id 1.1.1.1
 network 192.168.10.0 0.0.0.255 area 0
 network 10.0.0.0 0.0.0.3 area 0
```

**R3 OSPF:**
```bash
router ospf 1
 router-id 3.3.3.3
 network 192.168.110.0 0.0.0.255 area 0
 network 10.0.0.0 0.0.0.3 area 0
 network 10.0.0.4 0.0.0.3 area 0
```

### 🧭 VLANs & Inter-VLAN Routing
All VLANs are trunked to routers via router-on-a-stick setup using subinterfaces.

```bash
interface g0/0.110
 encapsulation dot1Q 110
 ip address 192.168.110.1 255.255.255.0
```

### 📬 DHCP Server Configuration (Sucursal Local)
```bash
ip dhcp pool VLAN110
 network 192.168.110.0 255.255.255.0
 default-router 192.168.110.1
 dns-server 8.8.8.8
```

### 🚪 Access Control Lists (ACLs)
WiFi in the branch (VLAN 130) is fully isolated from local and HQ networks.

```bash
ip access-list extended WIFI-SUCURSAL-OUT
 deny ip 192.168.130.0 0.0.0.255 192.168.110.0 0.0.0.255
 deny ip 192.168.130.0 0.0.0.255 192.168.10.0 0.0.0.255
 permit ip 192.168.130.0 0.0.0.255 any
```

### 📶 Access Point Integration
WiFi access point configured on VLAN 130 at the branch site, operating in access mode (no VLAN tagging), served by DHCP from R3.

---

## 🧪 What This Project Demonstrates
This network simulation showcases your ability to:

- Design and configure a segmented Layer 2/3 network
- Implement redundancy and high availability
- Configure multi-site routing and failover using OSPF
- Deploy and secure wireless clients using ACLs
- Implement local DHCP per VLAN and per site
- Structure and document full network configs for reuse or training

---

## 📁 Suggested Repository Structure
```
/enterprise-network-simulation
├── README.md
├── topology.drawio
├── configs/
│   ├── router-R1.txt
│   ├── router-R2.txt
│   ├── router-R3.txt
│   ├── switch-HQ.txt
│   ├── switch-Sucursal.txt
│   ├── access-point-branch.txt
├── project.pkt
```

---

## ✅ Next Steps
- Document failover test results (e.g., show standby brief, shut/no shut tests)
- Add optional SNMP or remote syslog features
- Export this documentation as a PDF for portfolio or interview presentation
- Publish repository on GitHub or share in personal portfolio site
