🏢 Simulación de Red Empresarial – VLANs, OSPF, HSRP, DHCP, ACL & WiFi
## 👤 Autor
- **Carlos Saldaña**                                                          
- **Correo**: [c.a.saldana20@gmail.com](mailto:c.a.sadlana20@gmail.com)
- **LinkedIn**: [linkedin.com/in/carlos-saldana](www.linkedin.com/in/carlos-saldaña-candanedo-720426183)
## 📋 Descripción General del Proyecto
Este proyecto realizado en Packet Tracer simula una red de una empresa de tamaño medio, con una sede central (HQ) y una sucursal remota (Branch). Está diseñado para demostrar habilidades esenciales de redes a nivel CCNA, incluyendo:

- ✅ Segmentación por VLAN por sede
- ✅ Configuración de gateway redundante con HSRP
- ✅ Enrutamiento dinámico con OSPF
- ✅ Asignación de direcciones IP con DHCP local y centralizado
- ✅ Control de tráfico entre VLANs mediante ACLs
- ✅ Integración de conectividad inalámbrica (APs)

---

## 🧱 Topologia de red

**Headquarters (HQ):**
- VLAN 10 – Admin – 192.168.10.0/24
- VLAN 20 – IT – 192.168.20.0/24
- VLAN 30 - HR - 192.168.30.0/24
- VLAN 50 – WiFiCorp – 192.168.50.0/24
- VLAN 99 – Management – 192.168.99.0/24
- DHCP Server: 192.168.99.10
- Redundancia via HSRP (R1 y R2)

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
Configurado en R1 y R2 para garantizar alta disponibilidad en la sede central. R1 actúa como router activo y R2 como respaldo, con prioridad y preempt.

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

### ⚙️ VLANs y Switch (SW1 y SW2) config
Los puertos hacia los AP estan en modo access(VLAN taggin not suppore on AP).
<details> 
 
```bash
! HQ Switch config

! Crear VLANs
vlan 10
 name Admin
vlan 20
 name ITH
vlan 30
 name HR
vlan 50
 name WiFiHQ
vlan 99
 name ManagementHQ

! Puertos Access
interface range f0/1 - 5
 switchport mode access
 switchport access vlan 10

interface range f0/6 - 10
 switchport mode access
 switchport access vlan 20

interface range f0/11 - 15
 switchport mode access
 switchport access vlan 30

interface f0/20
 switchport mode access
 switchport access vlan 50

interface f0/24
 switchport mode access
 switchport access vlan 99

! Puerto Trunk hacia router R1
interface g0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,50,99

! Puerto Trunk hacia router R2
interface g0/2
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,50,99

! Sucursal Switch Configuration

! Crear VLANs
vlan 110
 name AdminBranch
vlan 120
 name ITBRanch
vlan 140
 name GuestWiFi
vlan 99
 name Management

! Puertos Access
interface range f0/1 - 5
 switchport mode access
 switchport access vlan 110

interface range f0/6 - 10
 switchport mode access
 switchport access vlan 120

interface f0/20
 switchport mode access
 switchport access vlan 140



! Puerto Trunk hacia el router R3
interface g0/1
 switchport mode trunk
 switchport trunk allowed vlan 110,120,140

``` 
</details>
 
📡 OSPF protocolo de ruteo para R1, R1 y R3, anunciando todas las redes locales.

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
### 📶 Access Point 
Un punto de acceso inalámbrico está configurado en VLAN 50 por un Servidor DHCP y en VLAN 140 con obtención automática de IP por DHCP desde R3.


### 🚪 Access Control Lists (ACLs)
WiFi branch (VLAN 140) totalmente aislada de la red local y HQ.

```bash
Bloquea acceso a otras VLAN locales en Branch
 deny ip 192.168.140.0 0.0.0.255 192.168.110.0 0.0.0.255
 deny ip 192.168.140.0 0.0.0.255 192.168.120.0 0.0.0.255
 deny ip 192.168.140.0 0.0.0.255 192.168.99.0 0.0.0.255


Bloquea acceso a HQ
 deny ip 192.168.140.0 0.0.0.255 192.168.10.0 0.0.0.255
 deny ip 192.168.140.0 0.0.0.255 192.168.20.0 0.0.0.255
 deny ip 192.168.140.0 0.0.0.255 192.168.30.0 0.0.0.255
 deny ip 192.168.140.0 0.0.0.255 192.168.50.0 0.0.0.255
 deny ip 192.168.140.0 0.0.0.255 192.168.20.0 0.0.0.255

Permite todo lo demás (Internet, DNS externo, etc.)
permit ip 192.168.140.0 0.0.0.255 any

Interface g0/0.140
ip access-group 140 in

```

WiFi HQ (VLAN 50) totalmente aislado de la red local y la sucursal.

```bash
Bloquea acceso a otras VLAN locales en HQ
deny ip 192.168.50.0 0.0.0.255 192.168.10.0 0.0.0.255
deny ip 192.168.50.0 0.0.0.255 192.168.20.0 0.0.0.255
deny ip 192.168.50.0 0.0.0.255 192.168.30.0 0.0.0.255
deny ip 192.168.50.0 0.0.0.255 host 192.168.99.10



Bloquea acceso a HQ
 deny ip 192.168.50.0 0.0.0.255 192.168.110.0 0.0.0.255
 deny ip 192.168.50.0 0.0.0.255 192.168.120.0 0.0.0.255
 deny ip 192.168.50.0 0.0.0.255 192.168.140.0 0.0.0.255


Permisos
permit udp host 192.168.99.10 eq 67 any eq 68
permit udp host 192.168.99.10 eq 68 any eq 67
permit ip 192.168.50.0 0.0.0.255 any 

Interface g0/0.50
ip access-group 150 in

```
---

## 🧪 Proposito de proyecto
Este laboratorio demuestra que soy capaz de:

- Diseñar y configurar una red segmentada por VLAN y subredes
- Implementar redundancia y alta disponibilidad con HSRP
- Usar enrutamiento dinámico multisitio con OSPF
- Integrar WiFi seguro con ACLs y DHCP
- Configurar routers, switches y APs en un entorno empresarial
- Documentar cada dispositivo y estructura de red de forma profesional
## 📸 Capturas
R1(HOT) en fallo, entra R2(Standy).

![image alt](https://github.com/hayligg/HSRP---DHCP---AP/blob/a89be025b5fa85ceb0756f417d78a901fb382bfa/R2%20HSRP%20pt.png).

R2 pasa a estar activo.

![image alt](https://github.com/hayligg/HSRP---DHCP---AP/blob/a89be025b5fa85ceb0756f417d78a901fb382bfa/R2%20HSRP.png).

Servidor DHCP funcionando.

![image alt](https://github.com/hayligg/HSRP---DHCP---AP/blob/a89be025b5fa85ceb0756f417d78a901fb382bfa/DHCP%20working.png).

Access list en funcionamiendo al enviar ping a la VLAN 140 (192.168.140.11) desde la red local VLAN 110 (192.168.110.11).

Pings hacia VLAN 140 no podra mandar respuesta de regreso generando el error "Request time out".

![image alt](https://github.com/hayligg/HSRP---DHCP---AP/blob/abed8cf8c4669552a8510f55bbe0b141555653af/ping%20a%20acl.png)

Pings fuera de esta VLAN seran bloqueador por el ACL dando el error "Destination host unreachable".

![image alt](https://github.com/hayligg/HSRP---DHCP---AP/blob/abed8cf8c4669552a8510f55bbe0b141555653af/ping%20desde%20acl.png)

## ⬇️ Descarga de archivo pkt.
[Download Packet Tracer file](https://github.com/hayligg/HSRP---DHCP---AP/raw/57543eb78fdd3abc833bd380d441d26d5caabe14/Proyect%202.pkt)
