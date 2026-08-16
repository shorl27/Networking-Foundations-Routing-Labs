Extended ACL — HTTPS-Only Network Access Control
📌 Overview

This project demonstrates the configuration and testing of a Cisco Extended Access Control List (ACL) using Cisco Packet Tracer.

The objective was to create a network security policy that allows an employee PC to:

Ping the server using ICMP
Access the server using HTTPS (TCP 443)

while blocking all other IP traffic.

This lab demonstrates how ACLs can be used to reduce the attack surface of a network through least-privilege access control.

🖥️ Network Topology
                 ┌───────────┐
                 │   SERVER  │
                 │192.168.20.10
                 └─────┬─────┘
                       │
                 192.168.20.1
                       │
                    ┌──┴──┐
                    │  R1 │
                    └──┬──┘
                 192.168.10.1
                       │
                    ┌──┴──┐
                    │ SW1 │
                    └──┬──┘
                       │
                 ┌─────┴─────┐
                 │           │
               PC1         PC2
          192.168.10.10  192.168.10.20
Networks
Device	IP Address	Network
PC1	192.168.10.10	192.168.10.0/24
PC2	192.168.10.20	192.168.10.0/24
R1 G0/0	192.168.10.1	192.168.10.0/24
R1 G0/1	192.168.20.1	192.168.20.0/24
Server	192.168.20.10	192.168.20.0/24
🎯 Objective

The required security policy was:

Traffic	Protocol/Port	Result
Ping	ICMP	✅ Allowed
HTTPS	TCP 443	✅ Allowed
HTTP	TCP 80	❌ Blocked
Telnet	TCP 23	❌ Blocked
FTP	TCP 21	❌ Blocked
Other IP traffic	IP	❌ Blocked

The goal was to allow only the services that were explicitly required.

🔐 ACL Configuration

An Extended ACL was used because the policy requires filtering based on both protocol and port.

access-list 100 permit icmp host 192.168.10.20 host 192.168.20.10
access-list 100 permit tcp host 192.168.10.20 host 192.168.20.10 eq 443
access-list 100 deny ip any any

The ACL was applied inbound on R1's G0/0 interface:

interface gigabitEthernet 0/0
ip access-group 100 in
🧠 How It Works
Rule 1 — Allow ICMP
permit icmp host 192.168.10.20 host 192.168.20.10

Allows PC2 to ping the server.

PC2 ───── ICMP ─────> Server
                       ✅
Rule 2 — Allow HTTPS
permit tcp host 192.168.10.20 host 192.168.20.10 eq 443

Allows PC2 to access the server's HTTPS service.

PC2 ───── TCP 443 ───> Server
                       ✅
Rule 3 — Deny Everything Else
deny ip any any

Any IP traffic that does not match the previous rules is denied.

HTTP       ❌
Telnet     ❌
FTP        ❌
Other IP   ❌
🧪 Testing
Ping Test
ping 192.168.20.10

Result:

Successful ✅

ICMP traffic was allowed.

HTTPS Test
https://192.168.20.10

Result:

Successful ✅

TCP port 443 was allowed.

HTTP Test
http://192.168.20.10

Result:

Blocked ❌
Telnet Test
telnet 192.168.20.10

Result:

Connection timed out ❌
🔎 ACL Verification

The ACL was verified using:

show access-lists

The ACL match counters confirmed that traffic was being evaluated against the configured rules.

Example:

Extended IP access list 100
    10 deny tcp host 192.168.10.20 host 192.168.20.10 eq telnet (24 match(es))
    20 permit ip any any (8 match(es))

These counters provide evidence that the router was actually processing and filtering traffic.

🛡️ Security Concept — Least Privilege

The lab demonstrates the principle of least privilege.

Instead of allowing unnecessary services and attempting to block individual services, the network can be configured to allow only what is required:

Required traffic
      ↓
   ALLOW
      ↓
Everything else
      ↓
   DENY

This reduces the number of services exposed to users and potential attackers.

📚 Skills Demonstrated
Cisco Packet Tracer
IPv4 addressing
Routing between networks
TCP/IP
ICMP
TCP ports
HTTP / HTTPS
Telnet
Extended ACLs
ACL rule ordering
Source and destination matching
Port-based traffic filtering
ACL verification
Network security
Least privilege
Attack-surface reduction
🚀 Key Takeaway

This lab demonstrates the progression from basic networking to network security:

IP Addressing
      ↓
Routing
      ↓
TCP/IP & ICMP
      ↓
Ports & Services
      ↓
Extended ACLs
      ↓
Traffic Filtering
      ↓
Network Security

The main lesson is:

A device being reachable does not mean every service on it should be accessible.

Network security involves controlling who can access what, using which protocol and port.
