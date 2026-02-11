# ACL (Access Control List)

## Project Overview

This project demonstrates **basic ACL configuration** to control traffic between PCs in different subnets:

* Router0 connects to **Switch0 → PC0** and Router1.
* Router1 connects to **Switch1 → PC1 & PC2**.
* ACL applied on **Router0** denies traffic from **PC1** while allowing other traffic.

---

## Devices and Interfaces

| Device  | Interface | Connected To | IP Address   | Subnet Mask   | Default Gateway |
| ------- | --------- | ------------ | ------------ | ------------- | --------------- |
| Router0 | G0/0      | Router1      | 20.0.0.1     | 255.255.255.0 | —               |
| Router0 | G0/1      | Switch0      | 10.0.0.2     | 255.255.255.0 | —               |
| Router1 | G0/0      | Router0      | 20.0.0.2     | 255.255.255.0 | —               |
| Router1 | G0/1      | Switch1      | 192.168.1.2  | 255.255.255.0 | —               |
| Switch0 | FA0/2     | PC0          | —            | —             | —               |
| Switch1 | FA0/1     | PC2          | —            | —             | —               |
| Switch1 | FA0/2     | PC1          | —            | —             | —               |
| PC0     | FA0/1     | Switch0      | 10.0.0.1     | 255.255.255.0 | 10.0.0.2        |
| PC1     | FA0/1     | Switch1      | 192.168.1.1  | 255.255.255.0 | 192.168.1.2     |
| PC2     | FA0/1     | Switch1      | 192.168.1.20 | 255.255.255.0 | 192.168.1.2     |

---

## Router0 Configuration (with ACL)

```bash
enable
configure terminal

# Interface to Switch0
interface g0/1
 ip address 10.0.0.2 255.255.255.0
 no shutdown
exit

# Interface to Router1
interface g0/0
 ip address 20.0.0.1 255.255.255.0
 no shutdown
exit

# Step 1: Create ACL to deny PC1 (192.168.1.1)
access-list 1 deny 192.168.1.1 
access-list 1 permit any

# Step 2: Apply ACL outbound on G0/1 
interface g0/1
 ip access-group 1 out
exit

```

---

## Router1 Configuration

```bash
enable
configure terminal

# Interface to Router0
interface g0/0
 ip address 20.0.0.2 255.255.255.0
 no shutdown
exit

# Interface to Switch1
interface g0/1
 ip address 192.168.1.2 255.255.255.0
 no shutdown
exit

```

---

## PC Configuration

| PC  | IP Address   | Subnet Mask   | Default Gateway |
| --- | ------------ | ------------- | --------------- |
| PC0 | 10.0.0.1     | 255.255.255.0 | 10.0.0.2        |
| PC1 | 192.168.1.1  | 255.255.255.0 | 192.168.1.2     |
| PC2 | 192.168.1.20 | 255.255.255.0 | 192.168.1.2     |

---

## Testing Connectivity

1. **Ping PC0 from PC1:**

   * Should **fail** (ACL denies PC1).

```bash
ping 10.0.0.1   # Should fail
```

2. **Ping PC0 from PC2:**

   * Should **succeed** (ACL allows other traffic).

```bash
ping 10.0.0.1   # Should succeed
```

3. **Ping Router0 from PC1 and PC2:**

   * PC1: **Should fail**
   * PC2: **Should succeed**

---

**Key Points**

* ACL is applied **inbound on Router0 G0/0** to control traffic from Router1 / PCs.
* Only **PC1 (192.168.1.1)** is blocked; PC2 can communicate freely.
* Router0 routes traffic for **PC0**.
* Simple **standard ACL (1)** used for filtering by host IP.

---


