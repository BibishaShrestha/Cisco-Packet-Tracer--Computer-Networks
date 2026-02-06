# Dynamic Routing with RIP — Linear Topology

## Project Overview

A network with 3 routers connected in a line via GigabitEthernet interfaces, each router connected to a switch and a PC. Routers use RIP(Routing Information Protocol) for dynamic routing.

---

##  Devices

* Router0, Router1, Router2 (Cisco 2911)
* Switch0, Switch1, Switch2 (Cisco 2950T)
* PC0, PC1, PC2

---

##  Network Design and Layout

* Router0 GigabitEthernet0/0 → Switch0 → PC0
* Router1 GigabitEthernet0/0 → Switch1 → PC1
* Router2 GigabitEthernet0/0 → Switch2 → PC2
* Router0 GigabitEthernet0/1 → Router1 GigabitEthernet0/1
* Router1 GigabitEthernet0/2 → Router2 GigabitEthernet0/2

---

## IP Address Scheme

| Device  | Interface | IP Address | Subnet Mask   | Default Gateway |
| ------- | --------- | ---------- | ------------- | --------------- |
| PC0     | —         | 100.0.0.1   | 255.255.255.0 | 100.0.0.2        |
| Switch0 | —         | —          | —             | —               |
| Router0 | Gig0/0    | 100.0.0.2   | 255.255.255.0 | —               |
| Router0 | Gig0/1    | 120.0.0.1   | 255.255.255.0 | —               |
| Router1 | Gig0/0    | 130.0.0.2   | 255.255.255.0 | —               |
| Router1 | Gig0/1    | 120.0.0.2   | 255.255.255.0 | —               |
| Router1 | Gig0/2    | 140.0.0.1   | 255.255.255.0 | —               |
| Router2 | Gig0/0    | 150.0.0.2   | 255.255.255.0 | —               |
| Router2 | Gig0/2    | 140.0.0.2   | 255.255.255.0 | —               |
| PC1     | —         | 130.0.0.1   | 255.255.255.0 | 130.0.0.2        |
| PC2     | —         | 150.0.0.1   | 255.255.255.0 | 150.0.0.2        |

---

## Router Configuration

### Router0

```bash
enable
configure terminal

interface GigabitEthernet0/0
 ip address 100.0.0.2 255.255.255.0
 no shutdown
exit

interface GigabitEthernet0/1
 ip address 120.0.0.1 255.255.255.0
 no shutdown
exit

router rip
 version 2
 network 100.0.0.0
 network 120.0.0.0
exit

```

---

### Router1

```bash
enable
configure terminal

interface GigabitEthernet0/0
 ip address 130.0.0.2 255.255.255.0
 no shutdown
exit

interface GigabitEthernet0/1
 ip address 120.0.0.2 255.255.255.0
 no shutdown
exit

interface GigabitEthernet0/2
 ip address 140.0.0.1 255.255.255.0
 no shutdown
exit

router rip
 version 2
 network 120.0.0.0
 network 130.0.0.0
 network 140.0.0.0
exit

```

---

### Router2

```bash
enable
configure terminal

interface GigabitEthernet0/0
 ip address 150.0.0.2 255.255.255.0
 no shutdown
exit

interface GigabitEthernet0/2
 ip address 140.0.0.2 255.255.255.0
 no shutdown
exit

router rip
 version 2
 network 140.0.0.0
 network 150.0.0.0
exit

```

---

## PC Configuration

| PC  | IP Address | Subnet Mask   | Default Gateway |
| --- | ---------- | ------------- | --------------- |
| PC0 | 100.0.0.1   | 255.255.255.0 | 100.0.0.2        |
| PC1 | 130.0.0.1   | 255.255.255.0 | 130.0.0.2        |
| PC2 | 150.0.0.1   | 255.255.255.0 | 150.0.0.2        |

---

## Testing Connectivity

From **any PC**, test pinging:

* Its default gateway
* Other routers’ LAN interfaces
* Other PCs

Example from **PC0**:

```bash
ping 100.0.0.2
ping 120.0.0.1
ping 130.0.0.1
ping 150.0.0.1
```
