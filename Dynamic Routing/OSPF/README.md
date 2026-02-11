# OSPF Dynamic Routing 

## Project Overview

This network demonstrates **OSPF dynamic routing** with three routers, each connected directly to one PC. Routers are connected via **serial links** forming a triangle. PCs are connected directly to the routers via **GigabitEthernet0/0**.

---

##  Devices

* Routers: Router0, Router1, Router2 (Cisco 2911)
* PCs: PC0, PC1, PC2

---

### Router Connections

* Router0 ↔ Router1: Serial0/3/0 ↔ Serial0/3/0
* Router0 ↔ Router2: Serial0/3/1 ↔ Serial0/3/1
* Router1 ↔ Router2: Serial0/3/1 ↔ Serial0/3/0
* PC0 ↔ Router0 Gig0/0
* PC1 ↔ Router1 Gig0/0
* PC2 ↔ Router2 Gig0/0

---

##  IP Address Scheme

| Device  | Interface   | IP Address  | Subnet Mask   | Default Gateway |
| ------- | ----------- | ----------- | ------------- | --------------- |
| PC0     | —           | 192.168.1.1 | 255.255.255.0 | 192.168.1.2     |
| Router0 | Gig0/0      | 192.168.1.2 | 255.255.255.0 | —               |
| Router0 | Serial0/3/0 | 10.0.0.2    | 255.255.255.0 | —               |
| Router0 | Serial0/3/1 | 20.0.0.2    | 255.255.255.0 | —               |
| PC1     | —           | 192.168.2.1 | 255.255.255.0 | 192.168.2.2     |
| Router1 | Gig0/0      | 192.168.2.2 | 255.255.255.0 | —               |
| Router1 | Serial0/3/0 | 10.0.0.1    | 255.255.255.0 | —               |
| Router1 | Serial0/3/1 | 30.0.0.1    | 255.255.255.0 | —               |
| PC2     | —           | 192.168.3.1 | 255.255.255.0 | 192.168.3.2     |
| Router2 | Gig0/0      | 192.168.3.2 | 255.255.255.0 | —               |
| Router2 | Serial0/3/0 | 30.0.0.2    | 255.255.255.0 | —               |
| Router2 | Serial0/3/1 | 20.0.0.1    | 255.255.255.0 | —               |

---

## 🔧 Router Configurations (OSPF)

### Router0

```bash
enable
configure terminal

interface GigabitEthernet0/0
 ip address 192.168.1.2 255.255.255.0
 no shutdown
exit

interface Serial0/3/0
 ip address 10.0.0.2 255.255.255.0
 no shutdown
exit

interface Serial0/3/1
 ip address 20.0.0.2 255.255.255.0
 no shutdown
exit

router ospf 1
 network 192.168.1.0 0.0.0.255 area 0
 network 10.0.0.0 0.0.0.255 area 0
 network 20.0.0.0 0.0.0.255 area 0
exit

```

---

### Router1

```bash
enable
configure terminal

interface GigabitEthernet0/0
 ip address 192.168.2.2 255.255.255.0
 no shutdown
exit

interface Serial0/3/0
 ip address 10.0.0.1 255.255.255.0
 no shutdown
exit

interface Serial0/3/1
 ip address 30.0.0.1 255.255.255.0
 no shutdown
exit

router ospf 1
 network 192.168.2.0 0.0.0.255 area 0
 network 10.0.0.0 0.0.0.255 area 0
 network 30.0.0.0 0.0.0.255 area 0
exit

```

---

### Router2

```bash
enable
configure terminal

interface GigabitEthernet0/0
 ip address 192.168.3.2 255.255.255.0
 no shutdown
exit

interface Serial0/3/0
 ip address 30.0.0.2 255.255.255.0
 no shutdown
exit

interface Serial0/3/1
 ip address 20.0.0.1 255.255.255.0
 no shutdown
exit

router ospf 1
 network 192.168.3.0 0.0.0.255 area 0
 network 30.0.0.0 0.0.0.255 area 0
 network 20.0.0.0 0.0.0.255 area 0
exit

```

---

## PC Configuration

| PC  | IP Address  | Subnet Mask   | Default Gateway |
| --- | ----------- | ------------- | --------------- |
| PC0 | 192.168.1.1 | 255.255.255.0 | 192.168.1.2     |
| PC1 | 192.168.2.1 | 255.255.255.0 | 192.168.2.2     |
| PC2 | 192.168.3.1 | 255.255.255.0 | 192.168.3.2     |

---

## Testing Connectivity

From any PC, test pinging:

```bash
ping <router LAN interface>
ping <other PCs>
```

**Example from PC0**:

```bash
ping 192.168.1.2
ping 192.168.2.2
ping 192.168.3.2
ping 192.168.2.1
ping 192.168.3.1
```

All successful pings confirm **OSPF is working** and all networks are reachable.


---

If you want, I can also **draw a clean Packet Tracer topology diagram** for this exact setup — it would be ready to implement.

Do you want me to make that diagram?

