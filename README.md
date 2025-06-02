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
- VLAN 10 – Admin – 192.168.10.0/24
- VLAN 20 – IT – 192.168.20.0/24
- VLAN 30 - HR - 192.168.30.0/24
- VLAN 50 – WiFiCorp – 192.168.50.0/24
- VLAN 99 – Management – 192.168.99.0/24
- DHCP Server: 192.168.99.10
- Redundancy via HSRP (R1 & R2)

**Sucursal (Branch):**
- VLAN 110 – AdminBranch – 192.168.110.0/24
- VLAN 120 – ITBranch – 192.168.120.0/24
- VLAN 140 – GuestWiFi – 192.168.140.0/24
- VLAN 99 – ManagementSucursal – 192.168.99.0/24
- DHCP local in R3 (Branch router)

**WAN Links:**
- R1 ↔ R3: 10.0.0.0/30
- R2 ↔ R3: 10.0.0.4/30
![image alt](https://github.com/hayligg/HSRP---DHCP---AP/blob/74cb5e87f724667a03794be5ea93734946b32da0/Porject%20dhcp%20hsrp.png)
---

## ⚙️ Key Technologies Demonstrated

### 🔀 HSRP (Hot Standby Router Protocol)
Implemented on R1 and R2 for VLANs in HQ, using priorities and preempt to ensure R1 is active unless failure occurs.

<details> 

<summary>Configuracion del Router R1
 
```bash
R1 - HQ Router HSRP - DHCP relay
Subinterfaces VLANs
interface g0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.2 255.255.255.0
 standby 10 ip 192.168.10.1
 standby 10 priority 110
 standby 10 preempt
 ip helper-address 192.168.99.10 ...
```
 </summary>
 
```bash
R1 - HQ Router Config

Subinterfaces VLANs
interface g0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.2 255.255.255.0
 standby 10 ip 192.168.10.1
 standby 10 priority 110
 standby 10 preempt
 ip helper-address 192.168.99.10

interface g0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.2 255.255.255.0
 standby 20 ip 192.168.20.1
 standby 20 priority 110
 standby 20 preempt
 ip helper-address 192.168.99.10

interface g0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.2 255.255.255.0
 standby 300 ip 192.168.30.1
 standby 30 priority 110
 standby 30 preempt
 ip helper-address 192.168.99.10

interface g0/0.50
 encapsulation dot1Q 50
 ip address 192.168.50.2 255.255.255.0
 standby 50 ip 192.168.50.1
 standby 50 priority 110
 standby 50 preempt
 ip helper-address 192.168.99.10

interface g0/0.99
 encapsulation dot1Q 99
 ip address 192.168.99.2 255.255.255.0
 standby 99 ip 192.168.99.1
 standby 99 priority 110
 standby 99 preempt

WAN a R3
interface g0/1
 ip address 10.0.0.1 255.255.255.252
 no shutdown
```
</details>
<details> 

<summary>Configuracion del Router R2
 
```bash
R2 - HQ Standby Router Config

Subinterfaces VLANs
interface g0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.3 255.255.255.0
 standby 10 ip 192.168.10.1
 standby 10 priority 100
 standby 10 preempt
 ip helper-address 192.168.99.10 ...
```
 </summary>
 
```bash
R2 - HQ Standby Router Config

Subinterfaces VLANs
interface g0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.3 255.255.255.0
 standby 10 ip 192.168.10.1
 standby 10 priority 100
 standby 10 preempt
 ip helper-address 192.168.99.10

interface g0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.3 255.255.255.0
 standby 20 ip 192.168.20.1
 standby 20 priority 100
 standby 20 preempt
 ip helper-address 192.168.99.10

interface g0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.3 255.255.255.0
 standby 30 ip 192.168.30.1
 standby 30 priority 100
 standby 30 preempt
 ip helper-address 192.168.99.10

interface g0/0.50
 encapsulation dot1Q 50
 ip address 192.168.50.3 255.255.255.0
 standby 50 ip 192.168.50.1
 standby 50 priority 100
 standby 50 preempt
 ip helper-address 192.168.99.10

interface g0/0.99
 encapsulation dot1Q 99
 ip address 192.168.99.3 255.255.255.0
 standby 99 ip 192.168.99.1
 standby 99 priority 100
 standby 99 preempt

! WAN link to R3
interface g0/1
 ip address 10.0.0.5 255.255.255.252
 no shutdown
```
</details>

⚙️R3 - Pool Local

<details> 

<summary>Configuracion del Router 3
 
```bash
R3 - Branch Router Config

Subinterfaces VLANs
interface g0/0.110
 encapsulation dot1Q 110
 ip address 192.168.110.1 255.255.255.0

interface g0/0.120

```
</summary>

```bash
Subinterfaces VLANs
interface g0/0.110
 encapsulation dot1Q 110
 ip address 192.168.110.1 255.255.255.0

interface g0/0.120
 encapsulation dot1Q 120
 ip address 192.168.120.1 255.255.255.0

interface g0/0.140
 encapsulation dot1Q 140
 ip address 192.168.140.1 255.255.255.0

interface g0/0.99
 encapsulation dot1Q 99
 ip address 192.168.99.1 255.255.255.0

DHCP Pools
ip dhcp pool VLAN110
 network 192.168.110.0 255.255.255.0
 default-router 192.168.110.1
 dns-server 8.8.8.8

ip dhcp pool VLAN120
 network 192.168.120.0 255.255.255.0
 default-router 192.168.120.1
 dns-server 8.8.8.8

ip dhcp pool VLAN140
 network 192.168.140.0 255.255.255.0
 default-router 192.168.130.1
 dns-server 8.8.8.8

```
</details>
📡 OSPF protocolo de ruteo para R1, R1 y R3, anunciando todas las redes locales

**R1 OSPF:**

```bash
router ospf 1
 router-id 1.1.1.1
 network 192.168.10.0 0.0.0.255 area 0
 network 192.168.20.0 0.0.0.255 area 0
 network 192.168.30.0 0.0.0.255 area 0
 network 192.168.50.0 0.0.0.255 area 0
 network 192.168.99.0 0.0.0.255 area 0
 network 10.0.0.0 0.0.0.3 area 0
``` 
**R2 OSPF:**

```bash
router ospf 1
 router-id 2.2.2.2
 network 192.168.10.0 0.0.0.255 area 0
 network 192.168.20.0 0.0.0.255 area 0
 network 192.168.30.0 0.0.0.255 area 0
 network 192.168.50.0 0.0.0.255 area 0
 network 192.168.99.0 0.0.0.255 area 0
 network 10.0.0.4 0.0.0.3 area 0

```

**R3 OSPF:**

```bash
router ospf 1
 router-id 3.3.3.3
 network 192.168.110.0 0.0.0.255 area 0
 network 192.168.120.0 0.0.0.255 area 0
 network 192.168.140.0 0.0.0.255 area 0
 network 192.168.99.0 0.0.0.255 area 0
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

📬 DHCP Server Configuration (Sucursal Local)
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
