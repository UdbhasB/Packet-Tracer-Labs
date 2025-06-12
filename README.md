# Static Routing Lab Guide

---

## 1. IP Addressing Table

| Device      | Interface | IP Address   | Subnet Mask   | Network        |
| ----------- | --------- | ------------ | ------------- | -------------- |
| PC0         | Fa0       | 172.16.3.10  | 255.255.255.0 | 172.16.3.0/24  |
| PC1         | Fa0       | 192.168.2.10 | 255.255.255.0 | 192.168.2.0/24 |
| KFA-BLOCK-A | G0/0      | 172.16.3.1   | 255.255.255.0 | 172.16.3.0/24  |
|             | G0/1      | 172.16.2.1   | 255.255.255.0 | 172.16.2.0/24  |
| KFA-BLOCK-B | G0/0      | 172.16.2.2   | 255.255.255.0 | 172.16.2.0/24  |
|             | G0/1      | 192.168.1.1  | 255.255.255.0 | 192.168.1.0/24 |
| KFA-BLOCK-C | G0/0      | 192.168.1.2  | 255.255.255.0 | 192.168.1.0/24 |
|             | G0/1      | 192.168.2.1  | 255.255.255.0 | 192.168.2.0/24 |

---

## 2. Topology Summary

- 3 routers interconnected
- PC0 and PC1 connected to different networks
- No dynamic routing: We'll use **static routes**
- Goal: Enable end-to-end communication between **PC0 and PC1**

---

## 3. PC Configuration

### PC0:

- IP Address: `172.16.3.10`
- Subnet Mask: `255.255.255.0`
- Default Gateway: `172.16.3.1`

### PC1:

- IP Address: `192.168.2.10`
- Subnet Mask: `255.255.255.0`
- Default Gateway: `192.168.2.1`

---

## 4. Router Configuration

### KFA-BLOCK-A

```bash
no ip domain-lookup
hostname KFA-BLOCK-A
service password-encryption
enable secret class
banner motd #Authorized Access Only#
interface GigabitEthernet0/0
 ip address 172.16.3.1 255.255.255.0
 no shutdown
exit
interface GigabitEthernet0/1
 ip address 172.16.2.1 255.255.255.0
 no shutdown
exit
do write
copy running-config startup-config
```

### KFA-BLOCK-B

```bash
no ip domain-lookup
hostname KFA-BLOCK-B
service password-encryption
enable secret class
banner motd #Authorized Access Only#
interface GigabitEthernet0/0
 ip address 172.16.2.2 255.255.255.0
 no shutdown
exit
interface GigabitEthernet0/1
 ip address 192.168.1.1 255.255.255.0
 no shutdown
exit
do write
copy running-config startup-config
```

### KFA-BLOCK-C

```bash
no ip domain-lookup
hostname KFA-BLOCK-C
service password-encryption
enable secret class
banner motd #Authorized Access Only#
interface GigabitEthernet0/0
 ip address 192.168.1.2 255.255.255.0
 no shutdown
exit
interface GigabitEthernet0/1
 ip address 192.168.2.1 255.255.255.0
 no shutdown
exit
do write
copy running-config startup-config
```

---

## 5. Static Routing Configuration

### On KFA-BLOCK-A

```bash
ip route 192.168.2.0 255.255.255.0 172.16.2.2
ip route 192.168.1.0 255.255.255.0 172.16.2.2
do write
```

### On KFA-BLOCK-B

```bash
ip route 172.16.3.0 255.255.255.0 172.16.2.1
ip route 192.168.2.0 255.255.255.0 192.168.1.2
do write
```

### On KFA-BLOCK-C

```bash
ip route 172.16.3.0 255.255.255.0 192.168.1.1
ip route 172.16.2.0 255.255.255.0 192.168.1.1
do write
```

---

## 6. Verification Commands

### Ping Test from PC0 to PC1:

```bash
ping 192.168.2.10
```

### Router Connectivity Check:

```bash
show ip route
show ip interface brief
ping <next hop IP>
traceroute <destination IP>
```

---

*End of Lab Guide*
