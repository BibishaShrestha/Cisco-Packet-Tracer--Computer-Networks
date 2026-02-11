# DHCP with VLAN

## Project Overview

This project demonstrates a **DHCP-enabled network with VLANs**:

* **Two switches** (Switch0 and Switch1) with **identical VLAN configuration**.
* **DHCP server** providing IP addresses for multiple VLANs.
* **PCs assigned to VLANs for network segmentation**: VLAN10, VLAN20, VLAN30.
* **Trunk port** between switches for inter-VLAN communication.

---

## Devices

| Device  | Role               | Notes                              |
| ------- | ------------------ | ---------------------------------- |
| Server0 | DHCP Server        | IP: 192.168.1.1                    |
| Switch0 | Core/Access Switch | VLANs 10, 20, 30, trunk FA0/4      |
| Switch1 | Access Switch      | Same VLAN configuration as Switch0 |
| PC0–PC5 | End Devices        | Connected to access ports          |

---

## VLAN Design

| VLAN ID | VLAN Name | Switch  | Ports        | PCs              |
| ------- | --------- | ------- | ------------ | ---------------- |
| 10      | ADMIN     | Switch0 | FA0/1        | PC0              |
| 20      | ACCOUNT   | Switch0 | FA0/2        | PC1              |
| 30      | EXAM      | Switch0 | FA0/3–FA0/22 | PC2              |
| 1       | NATIVE    | Switch0 | FA0/4        | Trunk to Switch1 |
| 10      | ADMIN     | Switch1 | FA0/1        | PC3              |
| 20      | ACCOUNT   | Switch1 | FA0/2        | PC4              |
| 30      | EXAM      | Switch1 | FA0/3–FA0/22 | PC5              |
| 1       | NATIVE    | Switch1 | FA0/4        | Trunk to Switch0 |

> **Note:** Both switches have **identical VLANs and trunk setup**.

---

## DHCP Server Configuration

**Server0**:

* IP Address: 192.168.1.1 (static)
* Subnet Mask: 255.255.255.0

**DHCP Pools per VLAN:**

| VLAN | DHCP Pool Range   | Default Gateway | Max Users |
| ---- | ----------------- | --------------- | --------- |
| 10   | 192.168.1.101–106 | 192.168.1.1     | 50        |
| 20   | 192.168.1.101–106 | 192.168.1.1     | 50        |
| 30   | 192.168.1.101–106 | 192.168.1.1     | 50        |


> **Note:** If DHCP server is on VLAN10, use **IP helper addresses** or **router SVI** for VLAN20 & VLAN30 to allow DHCP traffic.

---

## Switch Configuration (Switch0 & Switch1 Identical)

```bash
# Step 1: Create VLANs
vlan database
 vlan 10
  name ADMIN
 vlan 20
  name ACCOUNT
 vlan 30
  name EXAM
exit

# Step 2: Assign access ports
configure terminal

interface fa0/1
 switchport mode access
 switchport access vlan 10
exit

interface fa0/2
 switchport mode access
 switchport access vlan 20
exit

interface range fa0/3 - 22
 switchport mode access
 switchport access vlan 30
exit

# Step 3: Configure trunk port
interface fa0/4
 switchport mode trunk
 switchport trunk native vlan 1
exit

```

> **Tip:** Apply the same commands to **Switch1** — ports may connect to different PCs, but VLANs and trunk remain identical.

---

## PC Configuration

* Set all PCs to **Obtain IP Address Automatically (DHCP)**

| PC  | VLAN | DHCP Assigned IP | Subnet Mask   | Default Gateway |
| --- | ---- | ---------------- | ------------- | --------------- |
| PC0 | 10   | 192.168.1.101    | 255.255.255.0 | 192.168.1.1     |
| PC1 | 20   | 192.168.1.102    | 255.255.255.0 | 192.168.1.1     |
| PC2 | 30   | 192.168.1.103    | 255.255.255.0 | 192.168.1.1     |
| PC3 | 10   | 192.168.1.104    | 255.255.255.0 | 192.168.1.1     |
| PC4 | 20   | 192.168.1.105    | 255.255.255.0 | 192.168.1.1     |
| PC5 | 30   | 192.168.1.106    | 255.255.255.0 | 192.168.1.1     |


*Note:* Actual DHCP IPs may vary based on assignment order.

---

## Testing Connectivity

1. **Verify DHCP assignment:** On each PC, run `ipconfig` to ensure it received an IP in the correct VLAN range.
2. Ping default gateway: Each PC should ping the DHCP server (default gateway).

ping 192.168.1.1   # Default gateway / DHCP server


3. Ping other PCs:

ping 192.168.1.104  # Example: PC0 ping PC3
ping 192.168.1.105  # Example: PC1 ping PC4
ping 192.168.1.106  # Example: PC2 ping PC5
```

---
 This setup now includes:

* **Two switches with identical VLAN configurations**
* **Access ports assigned to VLANs**
* **Trunk port connecting switches**
* **DHCP server providing dynamic IPs for all VLANs**
* **Ready-to-implement in Packet Tracer**

