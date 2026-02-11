# DNS + HTTP Server Network

## Project Overview

This project demonstrates a network where a PC can access multiple websites via hostnames using a DNS server.

* Router0 connects to **PC0** and **Switch0**.
* Switch0 connects **DNS server** and four HTTP servers (Google, Gmail, Facebook, Instagram).
* PC0 uses DNS to access the websites.
* Each server has custom web page content.

---

## 🛠 Devices and Interfaces

| Device     | Role             | Interface | Connected To     | IP Address   | Subnet Mask   | Default Gateway |
| ---------- | ---------------- | --------- | ---------------- | ------------ | ------------- | --------------- |
| Router0    | Router / Gateway | G0/0      | Switch0          | 10.10.10.1   | 255.255.255.0 | —               |
| Router0    | Router / Gateway | G0/1      | PC0              | —            | —             | —               |
| PC0        | Client           | FA0       | Router0 G0/1     | 192.168.1.1  | 255.255.255.0 | 192.168.1.2     |
| Switch0    | Switch           | —         | Router0, Servers | —            | —             | —               |
| DNS Server | DNS + HTTP       | G0/2      | Switch0          | 10.10.10.10  | 255.255.255.0 | 10.10.10.1      |
| Google     | HTTP             | FA0/4     | Switch0          | 10.10.10.250 | 255.255.255.0 | 10.10.10.1      |
| Gmail      | HTTP             | FA0/3     | Switch0          | 10.10.10.200 | 255.255.255.0 | 10.10.10.1      |
| Facebook   | HTTP             | FA0/2     | Switch0          | 10.10.10.150 | 255.255.255.0 | 10.10.10.1      |
| Instagram  | HTTP             | FA0/1     | Switch0          | 10.10.10.100 | 255.255.255.0 | 10.10.10.1      |

---

## Router0 Configuration

```bash
enable
configure terminal

# Interface to Switch0
interface g0/0
 ip address 10.10.10.1 255.255.255.0
 no shutdown
exit

# Interface to PC0
interface g0/1
 no shutdown
exit

```

---

## 🔧 DNS Server Configuration

* IP: **10.10.10.10**
* Default Gateway: **10.10.10.1**
* Enable **DNS Service** in Packet Tracer
* Add DNS entries:

| Hostname  | IP Address   |
| --------- | ------------ |
| google    | 10.10.10.250 |
| gmail     | 10.10.10.200 |
| facebook  | 10.10.10.150 |
| instagram | 10.10.10.100 |

---

## 🔧 HTTP Server Configuration (Google, Gmail, Facebook, Instagram)

* Enable **HTTP service** in Packet Tracer
* Customize `index.html` on each server:

| Server    | Example `index.html` Content |
| --------- | ---------------------------- |
| Google    | “Welcome to Google”          |
| Gmail     | “Welcome to Gmail”           |
| Facebook  | “Welcome to Facebook”        |
| Instagram | “Welcome to Instagram”       |

---

## PC0 Configuration

* IP Address: 192.168.1.1
* Subnet Mask: 255.255.255.0
* Default Gateway: 192.168.1.2
* DNS Server: 10.10.10.10

**Test in Web Browser:**

* Type the hostnames:

```
google
gmail
facebook
instagram
```

* Each hostname should open the corresponding server’s web page.

---

## Testing Connectivity

1. **Ping all servers from PC0:**

```bash
ping 10.10.10.10   # DNS Server
ping 10.10.10.250  # Google
ping 10.10.10.200  # Gmail
ping 10.10.10.150  # Facebook
ping 10.10.10.100  # Instagram
```

2. **Access websites using DNS names:**

```text
Type in browser: google → displays Google page
Type in browser: instagram → displays Instagram page
```

3. **Verify DNS resolution:**

```bash
nslookup google
nslookup instagram
```

---

**Key Notes**

* Router0 manages traffic between PC0 and servers.
* Switch0 connects all servers and the router.
* DNS Server resolves hostnames to server IPs.
* HTTP servers serve web pages (`index.html`).
* PC0 uses DNS to access websites without needing IP addresses.

---


