## Networking Basics:

# Project Overview

This project simulates a basic small network using Cisco Packet Tracer. The network consists of a single router, two switches, and two PCs connected to demonstrate fundamental device connectivity and configuration.

# Devices

- One Cisco 2911 Router
- Two Cisco 2950T Switches
- Each switch connected to one PC

---  

# Network Design and Layout

- Router1 interface GigabitEthernet0/0 connects to Switch 1
- Router2 interface GigabitEthernet0/1 connects to Switch 2
- PC1 connects to Switch 1
- PC2 connects to Switch 2

---

# IP Address Scheme

| Device  | Role         | IP Address   | Subnet Mask   | Default Gateway |
| ------  | ------------ | ------------ | ------------- | --------------- |
| PC1     | End Device 1 | 192.168.1.1 | 255.255.255.0 | 192.168.1.2     |
| PC2     | End Device 2 | 192.168.2.1 | 255.255.255.0 | 192.168.2.2     |
| Router1 | Gateway      | 192.168.1.2 | 255.255.255.0 | —               |
| Router2 | Gateway      | 192.168.2.2  | 255.255.255.0 | —               |

---


# Router Configuration

Assign IP addresses on router interfaces to enable routing between two separate LAN segments:

```bash
enable
configure terminal

interface GigabitEthernet0/0
ip address 192.168.1.2 255.255.255.0
no shutdown
exit

interface GigabitEthernet0/1
ip address 192.168.2.2 255.255.255.0
no shutdown
exit
```

---

# PC Configuration

* PC1:
  IP Address: 192.168.1.1
  Subnet Mask: 255.255.255.0
  Default Gateway: 192.168.1.2

* PC2:
  IP Address: 192.168.2.1
  Subnet Mask: 255.255.255.0
  Default Gateway: 192.168.2.2

---

# Testing Connectivity

* From PC1,
- ping 192.168.1.2
- ping 192.168.2.1
  
* From PC2,
-  ping 192.168.2.2
-  ping 192.168.1.1

Successful pings confirm end-to-end connectivity and routing between the two PCs via the router.





