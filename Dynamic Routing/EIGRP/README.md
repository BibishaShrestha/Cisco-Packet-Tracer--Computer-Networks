# EIGRP Dynamic Routing — Linear Topology with GigabitEthernet Links

##  Project Overview

A network with 3 routers connected in a line, each router connected to a switch and a PC. Routers are connected via **GigabitEthernet interfaces** (no serial links). EIGRP is used for dynamic routing.

---

## Devices

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

##  IP Address Scheme

| Device  | Interface | IP Address | Subnet Mask   | Default Gateway |
| ------- | --------- | ---------- | ------------- | --------------- |
| PC0     | —         | 10.0.0.1   | 255.255.255.0 | 10.0.0.2        |
| Switch0 | —         | —          | —             | —               |
| Router0 | Gig0/0    | 10.0.0.2   | 255.255.255.0 | —               |
| Router0 | Gig0/1    | 20.0.0.1   | 255.255.255.0 | —               |
| Router1 | Gig0/0    | 30.0.0.2   | 255.255.255.0 | —               |
| Router1 | Gig0/1    | 20.0.0.2   | 255.255.255.0 | —               |
| Router1 | Gig0/2    | 40.0.0.1   | 255.255.255.0 | —               |
| Router2 | Gig0/0    | 50.0.0.2   | 255.255.255.0 | —               |
| Router2 | Gig0/2    | 40.0.0.2   | 255.255.255.0 | —               |
| PC1     | —         | 30.0.0.1   | 255.255.255.0 | 30.0.0.2        |
| PC2     | —         | 50.0.0.1   | 255.255.255.0 | 50.0.0.2        |

---

## Router Configuration

### Router0

```bash
enable
configure terminal

interface GigabitEthernet0/0
 ip address 10.0.0.2 255.255.255.0
 no shutdown
exit

interface GigabitEthernet0/1
 ip address 20.0.0.1 255.255.255.0
 no shutdown
exit

router eigrp 1
 network 10.0.0.0 0.255.255.255
 network 20.0.0.0 0.255.255.255
 no auto-summary
exit

write memory
```

---

### Router1

```bash
enable
configure terminal

interface GigabitEthernet0/0
 ip address 30.0.0.2 255.255.255.0
 no shutdown
exit

interface GigabitEthernet0/1
 ip address 20.0.0.2 255.255.255.0
 no shutdown
exit

interface GigabitEthernet0/2
 ip address 40.0.0.1 255.255.255.0
 no shutdown
exit

router eigrp 1
 network 20.0.0.0 0.255.255.255
 network 30.0.0.0 0.255.255.255
 network 40.0.0.0 0.255.255.255
 no auto-summary
exit

```

---

### Router2

```bash
enable
configure terminal

interface GigabitEthernet0/0
 ip address 50.0.0.2 255.255.255.0
 no shutdown
exit

interface GigabitEthernet0/2
 ip address 40.0.0.2 255.255.255.0
 no shutdown
exit

router eigrp 1
 network 40.0.0.0 0.255.255.255
 network 50.0.0.0 0.255.255.255
 no auto-summary
exit

```

---

## PC Configuration

| PC  | IP Address | Subnet Mask   | Default Gateway |
| --- | ---------- | ------------- | --------------- |
| PC0 | 10.0.0.1   | 255.255.255.0 | 10.0.0.2        |
| PC1 | 30.0.0.1   | 255.255.255.0 | 30.0.0.2        |
| PC2 | 50.0.0.1   | 255.255.255.0 | 50.0.0.2        |

---

## Testing Connectivity

From **any PC**, test pinging:

* Its own router interface (default gateway)
* Other routers’ LAN interfaces
* Other PCs

Example from **PC0**:

```bash
ping 10.0.0.2
ping 20.0.0.1
ping 30.0.0.1
ping 50.0.0.1
```
---

