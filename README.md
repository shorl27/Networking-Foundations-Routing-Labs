# Networking Foundations & Routing Labs

A hands-on networking lab portfolio built using **Cisco Packet Tracer** to develop practical networking and troubleshooting skills.

This project documents my progression from basic Layer 2 networking to Layer 3 routing, with each lab focusing on understanding how network communication actually works rather than simply following configuration commands.

---

## 🎯 Objectives

The main goals of this project are to:

* Understand how computers communicate across networks
* Understand MAC addresses and IP addresses
* Learn how switches forward Ethernet frames
* Understand ARP
* Understand subnetting and local vs. remote networks
* Configure Cisco routers and switches
* Understand default gateways
* Understand routing tables
* Troubleshoot network connectivity
* Build realistic small-business network topologies

---

# Lab 01 — Network Fundamentals

## Topology

```text
PC1 ───── Switch ───── PC2
```

## Configuration

### PC1

```text
IP Address:    192.168.1.10
Subnet Mask:   255.255.255.0
```

### PC2

```text
IP Address:    192.168.1.20
Subnet Mask:   255.255.255.0
```

Both devices belong to:

```text
192.168.1.0/24
```

## Concepts Practiced

### Switching

The switch connects devices within the local network and forwards Ethernet frames using MAC addresses.

### MAC Addresses

Each network interface has a MAC address used for Layer 2 communication.

### IP Addresses

IP addresses provide logical Layer 3 addressing.

### ARP

ARP resolves an IPv4 address to a MAC address on the local network.

```text
IPv4 Address
      ↓
     ARP
      ↓
MAC Address
```

### Ping

ICMP ping was used to test connectivity between PC1 and PC2.

Example:

```text
ping 192.168.1.20
```

## Verification

The connection between PC1 and PC2 was successfully tested using ICMP.

I also inspected the ARP table using:

```text
arp -a
```

and examined the switch MAC address table using:

```text
show mac address-table
```

---

# Lab 02 — Routing & Default Gateways

## Topology

```text
PC1 ── Switch 1 ── Router ── Switch 2 ── PC2
```

The network contains two separate IP networks.

```text
Network 1
192.168.1.0/24

        │
        │
      Router
        │
        │

Network 2
192.168.2.0/24
```

## IP Addressing

| Device | Interface | IP Address   | Subnet Mask   | Default Gateway |
| ------ | --------- | ------------ | ------------- | --------------- |
| PC1    | NIC       | 192.168.1.10 | 255.255.255.0 | 192.168.1.1     |
| R1     | G0/0      | 192.168.1.1  | 255.255.255.0 | —               |
| R1     | G0/1      | 192.168.2.1  | 255.255.255.0 | —               |
| PC2    | NIC       | 192.168.2.10 | 255.255.255.0 | 192.168.2.1     |

---

## Router Configuration

The router interfaces were configured with:

```text
interface gigabitEthernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
```

and:

```text
interface gigabitEthernet 0/1
ip address 192.168.2.1 255.255.255.0
no shutdown
```

Interfaces were verified using:

```text
show ip interface brief
```

---

## Routing

The router automatically learned both networks because they were directly connected.

The routing table was inspected using:

```text
show ip route
```

The router contained routes similar to:

```text
C 192.168.1.0/24
C 192.168.2.0/24
```

`C` represents a directly **Connected** network.

---

# 🔎 What Happens During Communication?

PC1:

```text
192.168.1.10/24
```

attempts to communicate with:

```text
192.168.2.10
```

PC1 determines that the destination is not part of its local network.

Therefore, it sends the traffic toward its default gateway:

```text
192.168.1.1
```

Conceptually:

```text
PC1
 ↓
Checks destination
 ↓
Destination is remote
 ↓
Uses default gateway
 ↓
Router receives packet
 ↓
Router checks routing table
 ↓
Router identifies 192.168.2.0/24
 ↓
Router forwards traffic toward PC2
 ↓
PC2 receives packet
```

---

# 🧪 Troubleshooting Exercises

To understand how failures occur, I intentionally introduced configuration errors.

## Incorrect Default Gateway

PC1's gateway was changed from:

```text
192.168.1.1
```

to an incorrect address.

The remote ping failed because PC1 could no longer correctly forward traffic toward the remote network.

After restoring the correct gateway, connectivity returned.

## Router Interface Shutdown

A router interface was intentionally disabled using:

```text
shutdown
```

This prevented communication with the corresponding network.

The interface was restored using:

```text
no shutdown
```

---

# 🧠 Key Concepts Learned

### Layer 2

* Ethernet
* MAC addresses
* Switches
* ARP
* MAC address tables

### Layer 3

* IPv4 addresses
* Subnet masks
* Networks
* Routers
* Routing tables
* Default gateways

### Troubleshooting

* `ping`
* `arp -a`
* `show ip interface brief`
* `show ip route`
* `show mac address-table`

---

# 🛠️ Tools

* Cisco Packet Tracer
* Cisco IOS CLI

---

# 📈 Progression

This repository will continue to grow as I build increasingly realistic networking and cybersecurity labs.

Planned topics include:

* [ ] Subnetting & CIDR
* [ ] VLANs
* [ ] Trunking
* [ ] Inter-VLAN Routing
* [ ] DHCP
* [ ] DNS
* [ ] NAT/PAT
* [ ] TCP/UDP & Ports
* [ ] Access Control Lists
* [ ] Firewalls
* [ ] VPNs
* [ ] Nmap
* [ ] Wireshark
* [ ] Vulnerability Scanning
* [ ] Small-Business Network Security

---

# 📌 Project Goal

The goal of this project is not simply to memorize networking commands.

The goal is to understand **why networks work, how devices communicate, and how to troubleshoot them when they don't.**

Each lab will be built, tested, intentionally broken, and documented to develop practical troubleshooting skills.
