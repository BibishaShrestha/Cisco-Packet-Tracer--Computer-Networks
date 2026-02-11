# DHCP Configuration

## Project Overview

This project demonstrates a **DHCP server assigning IP addresses dynamically** to PCs in a network with two switches. One DHCP server is connected to Switch0, which is connected to Switch1. PCs automatically receive IP addresses from the DHCP server.

---

## Devices

* **Server0**: DHCP Server (IP: 192.168.1.1)
* **Switch0, Switch1**: Cisco 2950T
* **PC0, PC1, PC2**: connected to Switch0
* **PC3, PC4, PC5**: connected to Switch1

---

### Connection Details

* Server0 → Switch0: GigabitEthernet0/1
* Switch0 ↔ Switch1: FastEthernet0/4 on both sides
* Switch0 → PC0: FA0/1
* Switch0 → PC1: FA0/2
* Switch0 → PC2: FA0/3
* Switch1 → PC3: FA0/1
* Switch1 → PC4: FA0/2
* Switch1 → PC5: FA0/3

---

## DHCP Server Configuration

**Server0**:

* IP Address: 192.168.1.1 (static, for DHCP server)
* Subnet: 255.255.255.0
* DHCP Pool: 50 addresses, starting from 192.168.1.101

### Packet Tracer DHCP Setup Steps

1. On Server0, go to **Config → DHCP**.
2. Click **Add** under the DHCP pools:

| Parameter       | Value         |
| --------------- | ------------- |
| Pool Name       | LAN_Pool      |
| Default Gateway | 192.168.1.1   |
| DNS Server      | 192.168.1.1   |
| Start IP        | 192.168.1.101 |
| Subnet Mask     | 255.255.255.0 |
| Max Users       | 50            |

3. Enable DHCP service.

---

## PC Configuration

All PCs are set to **Obtain IP Address Automatically** (DHCP).

| PC  | Interface | DHCP Assigned IP Example | Subnet Mask   | Default Gateway |
| --- | --------- | ------------------------ | ------------- | --------------- |
| PC0 | FA0/1     | 192.168.1.101            | 255.255.255.0 | 192.168.1.1     |
| PC1 | FA0/2     | 192.168.1.102            | 255.255.255.0 | 192.168.1.1     |
| PC2 | FA0/3     | 192.168.1.103            | 255.255.255.0 | 192.168.1.1     |
| PC3 | FA0/1     | 192.168.1.104            | 255.255.255.0 | 192.168.1.1     |
| PC4 | FA0/2     | 192.168.1.105            | 255.255.255.0 | 192.168.1.1     |
| PC5 | FA0/3     | 192.168.1.106            | 255.255.255.0 | 192.168.1.1     |

*Note: Actual DHCP addresses may vary depending on assignment order.*

---

## Switch Configuration

**Switch0**:

```bash
enable
configure terminal

interface range fa0/1 - 3
 switchport mode access
exit

interface fa0/4
 switchport mode trunk
exit

interface gig0/1
 switchport mode access
exit

```

**Switch1**:

```bash
enable
configure terminal

interface range fa0/1 - 3
 switchport mode access
exit

interface fa0/4
 switchport mode trunk
exit

```
---
* For basic DHCP setups, you don’t actually need to configure the switches at all.
* By default, all switch ports are in access mode, and traffic between PCs and the DHCP server will pass fine. The switchport commands are only needed if you want to manually configure VLANs or trunks, which isn’t required here.

---

##  Testing Connectivity

1. On each PC, ensure IP is obtained automatically (DHCP).
2. Ping the DHCP server:

```bash
ping 192.168.1.1
```

3. Ping other PCs to ensure network connectivity.

**Example: PC0**

```bash
ping 192.168.1.102
ping 192.168.1.105
```

---

 This configuration now accurately reflects:

* 1 DHCP server
* 2 switches connected via trunk
* 6 PCs assigned DHCP IPs automatically
* Start IP: 192.168.1.101
* Max 50 users

---
