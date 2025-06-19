# Static Routing Lab Guide with Floating Static Route

This guide provides a comprehensive walkthrough for configuring a network topology using static routing, including a floating static route for redundancy. The lab uses a diamond topology with four routers and two end-user PCs.

---

## 1. Topology Summary

- **Routers**: 4 Cisco 2901 routers interconnected in a diamond topology.
- **End Devices**: 2 PCs on separate LANs.
- **Routing Method**: Static routes only. A floating static route is configured on ROUTER-A for redundancy.
- **Goal**: Achieve end-to-end communication and verify both primary and backup (floating) routes.

---

## 2. Router Naming Convention

Routers are named in clockwise order starting from the left:

- `ROUTER-A`: Leftmost, connected to PC-PT (Left)
- `ROUTER-B`: Top
- `ROUTER-C`: Right, connected to PC-PT (Right)
- `ROUTER-D`: Bottom

---

## 3. IP Addressing Table

| Device        | Interface      | IP Address     | Subnet Mask       | Network        |
|---------------|----------------|----------------|-------------------|----------------|
| PC-PT (Left)  | Fa0            | 172.16.3.10    | 255.255.255.0     | 172.16.3.0/24  |
| PC-PT (Right) | Fa0            | 192.168.2.10   | 255.255.255.0     | 192.168.2.0/24 |
| ROUTER-A      | G0/0           | 172.16.3.1     | 255.255.255.0     | 172.16.3.0/24  |
|               | S0/1/0         | 172.16.2.1     | 255.255.255.0     | 172.16.2.0/24  |
|               | S0/1/1         | 172.16.1.1     | 255.255.255.0     | 172.16.1.0/24  |
| ROUTER-B      | S0/1/0         | 172.16.2.2     | 255.255.255.0     | 172.16.2.0/24  |
|               | S0/1/1         | 192.168.1.1    | 255.255.255.0     | 192.168.1.0/24 |
| ROUTER-C      | G0/0           | 192.168.2.1    | 255.255.255.0     | 192.168.2.0/24 |
|               | S0/1/0         | 192.168.1.2    | 255.255.255.0     | 192.168.1.0/24 |
|               | S0/1/1         | 192.168.3.2    | 255.255.255.0     | 192.168.3.0/24 |
| ROUTER-D      | S0/1/0         | 172.16.1.2     | 255.255.255.0     | 172.16.1.0/24  |
|               | S0/1/1         | 192.168.3.1    | 255.255.255.0     | 192.168.3.0/24 |

---

## 4. PC Configuration

### PC-PT (Left)
- IP Address: `172.16.3.10`
- Subnet Mask: `255.255.255.0`
- Default Gateway: `172.16.3.1`

### PC-PT (Right)
- IP Address: `192.168.2.10`
- Subnet Mask: `255.255.255.0`
- Default Gateway: `192.168.2.1`

---

## 5. Router Configuration

### ROUTER-A

```bash
no ip domain-lookup
hostname ROUTER-A
service password-encryption
enable secret class
banner motd #Authorized Access Only#

interface GigabitEthernet0/1
 ip address 172.16.3.1 255.255.255.0
 no shutdown

interface Serial0/1/0
 ip address 172.16.2.1 255.255.255.0
 clock rate 64000
 no shutdown

interface Serial0/1/1
 ip address 172.16.1.1 255.255.255.0
 clock rate 64000
 no shutdown
