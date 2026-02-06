# Static Routing:

# Project Overview

This project simulates a small network with two routers connected, each router connected to a switch and a PC. It demonstrates basic static routing configuration to enable communication between PCs on different networks.

---

# Devices

* Two Cisco 2911 Routers (Router0 and Router1)
* Two Cisco 2950T Switches (Switch0 and Switch1)
* Two PCs (PC0 and PC1)

---

# Network Design and Layout

* Router0 interface GigabitEthernet0/0 connects to Switch0
* Router1 interface GigabitEthernet0/0 connects to Switch1
* Router0 interface GigabitEthernet0/1 connects to Router1 interface GigabitEthernet0/1
* PC0 connects to Switch0
* PC1 connects to Switch1

---

# IP Address Scheme

| Device  | Interface          | IP Address   | Subnet Mask     | Default Gateway |
| ------- | ------------------ | ------------ | --------------- | --------------- |
| PC0     | —                  | 192.168.1.1 | 255.255.255.0   | 192.168.1.2     |
| Switch0 | —                  | —            | —               | —               |
| Router0 | GigabitEthernet0/0 | 192.168.1.2  | 255.255.255.0   | —               |
| Router0 | GigabitEthernet0/1 | 192.168.3.1  | 255.255.255.0 | —               |
| Router1 | GigabitEthernet0/1 | 192.168.3.2  | 255.255.255.0 | —               |
| Router1 | GigabitEthernet0/0 | 192.168.2.2  | 255.255.255.0   | —               |
| Switch1 | —                  | —            | —               | —               |
| PC1     | —                  | 192.168.2.1  | 255.255.255.0   | 192.168.2.2     |

---

# Router Configuration

### Router0

```bash
enable
configure terminal

interface GigabitEthernet0/0
 ip address 192.168.1.2 255.255.255.0
 no shutdown
exit

interface GigabitEthernet0/1
 ip address 192.168.3.1 255.255.255.0
 no shutdown
exit

ip route 192.168.2.0 255.255.255.0 192.168.3.2 # Static route to Router1 LAN

exit

```

### Router1

```bash
enable
configure terminal

interface GigabitEthernet0/0
 ip address 192.168.2.2 255.255.255.0
 no shutdown
exit

interface GigabitEthernet0/1
 ip address 192.168.3.2 255.255.255.0
 no shutdown
exit

ip route 192.168.1.0 255.255.255.0 192.168.3.1  # Static route to Router0 LAN

exit

```

---

# PC Configuration

* **PC0:**
  IP Address: 192.168.1.1
  Subnet Mask: 255.255.255.0
  Default Gateway: 192.168.1.2

* **PC1:**
  IP Address: 192.168.2.1
  Subnet Mask: 255.255.255.0
  Default Gateway: 192.168.2.2

---

# Testing Connectivity

From **PC0**:

* `ping 192.168.1.2` (Router0 interface)
* `ping 192.168.3.2` (Router1 interface)
* `ping 192.168.2.1` (PC1 IP)

From **PC1**:

* `ping 192.168.2.2` (Router1 interface)
* `ping 192.168.3.1` (Router0 interface)
* `ping 192.168.1.1` (PC0 IP)

Successful ping tests verify correct static routing and connectivity across routers.


