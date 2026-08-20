Enterprise DMZ Network Segmentation & ACL Security Lab

Designed and secured a simulated enterprise network using VLAN segmentation, Router-on-a-Stick, a DMZ architecture, and Extended ACLs.

The lab follows a security workflow:

Build → Establish Baseline → Define Policy → Implement Controls → Attack/Test → Verify → Troubleshoot

📌 Overview

This lab demonstrates how to design and secure a segmented company network using Cisco Packet Tracer.

The network separates employees, a DMZ web server, and management devices into different VLANs. Router-on-a-Stick provides inter-VLAN routing, while an Extended ACL controls which traffic employees can send to the web server.

The lab follows a realistic security workflow:

Build → Test → Secure → Test Again → Verify

🎯 Objectives
Create multiple VLANs for network segmentation
Configure 802.1Q trunking
Configure Router-on-a-Stick
Create a DMZ for a web server
Configure default gateways
Implement an Extended ACL
Allow HTTPS traffic
Allow ICMP for connectivity testing
Block Telnet
Block unauthorized traffic
Verify the ACL using match counters
Troubleshoot a misconfigured trunk
🏗️ Network Topology
                         R1
                    G0/0 | 
                         |
                    TRUNK Fa0/5
                         |
                        SW1
              ___________|___________
             |           |           |
          VLAN 10      VLAN 20     VLAN 30
        Employees       DMZ       Management
          | |            |             |
         PC1 PC2       Server         Admin
🌐 IP Addressing
Device	VLAN	IP Address	Gateway
PC1	10	192.168.10.10/24	192.168.10.1
PC2	10	192.168.10.20/24	192.168.10.1
Web Server	20	192.168.20.10/24	192.168.20.1
Admin PC	30	192.168.30.10/24	192.168.30.1
Router gateways
VLAN 10 → 192.168.10.1
VLAN 20 → 192.168.20.1
VLAN 30 → 192.168.30.1
1. VLAN Configuration

Three security zones were created:

VLAN 10 → EMPLOYEES
VLAN 20 → DMZ
VLAN 30 → MANAGEMENT

The purpose of the segmentation is to prevent all devices from existing in the same Layer-2 network.

The web server is placed in the DMZ rather than directly inside the employee network.

2. Switch Configuration
Employee ports
interface range fa0/1-2
switchport mode access
switchport access vlan 10
Web server
interface fa0/3
switchport mode access
switchport access vlan 20
Management PC
interface fa0/4
switchport mode access
switchport access vlan 30
Router uplink

The actual physical connection was:

R1 G0/0 ↔ SW1 Fa0/5

Therefore, Fa0/5 was configured as the trunk:

interface fa0/5
switchport mode trunk
3. Router-on-a-Stick

The router uses subinterfaces to provide Layer-3 gateways for each VLAN.

Employee VLAN
interface gigabitEthernet0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
DMZ
interface gigabitEthernet0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
Management
interface gigabitEthernet0/0.30
encapsulation dot1Q 30
ip address 192.168.30.1 255.255.255.0

Router-on-a-Stick allows one physical router interface to route between multiple VLANs using 802.1Q VLAN tagging.

4. Initial Connectivity Testing

Before implementing security controls, connectivity was tested to establish a baseline.

PC1 → Gateway
ping 192.168.10.1

✅ Successful

PC1 → Web Server
ping 192.168.20.10

✅ Successful

PC1 → Management PC
ping 192.168.30.10

✅ Successful

This confirmed that the VLANs, trunk, router subinterfaces, and routing were functioning correctly.

5. Security Policy

The intended security policy was:

Traffic	Action
Employee → Web Server TCP 443	✅ Allow
Employee → Web Server ICMP	✅ Allow
Employee → Web Server TCP 23	❌ Deny
Employee → Web Server other traffic	❌ Deny

The policy follows the principle of least privilege.

Employees should only have the access they actually need.

6. Extended ACL

The following ACL was configured:

ip access-list extended EMPLOYEE-SECURITY


deny tcp 192.168.10.0 0.0.0.255 host 192.168.20.10 eq 23


permit tcp 192.168.10.0 0.0.0.255 host 192.168.20.10 eq 443


permit icmp 192.168.10.0 0.0.0.255 host 192.168.20.10


deny ip 192.168.10.0 0.0.0.255 host 192.168.20.10

The ACL was applied inbound to the employee VLAN:

interface gigabitEthernet0/0.10
ip access-group EMPLOYEE-SECURITY in
🧠 Why the ACL Order Matters

Cisco ACLs process rules from top to bottom.

For example:

PC1 → Web Server :443

The packet doesn't match the Telnet rule.

It reaches:

permit TCP 443

and is allowed.

But:

PC1 → Web Server :23

matches:

deny TCP 23

and is immediately dropped.

Traffic that isn't HTTPS or ICMP eventually reaches:

deny IP

and is blocked.

7. Security Testing
Telnet
telnet 192.168.20.10

Result:

❌ BLOCKED
Ping
ping 192.168.20.10

Result:

✅ ALLOWED
HTTPS
HTTPS → 192.168.20.10:443

Result:

✅ ALLOWED
Other unauthorized traffic

Result:

❌ BLOCKED
🔎 ACL Verification

The ACL can be inspected with:

show access-lists

The output includes match counters.

For example:

Extended IP access list EMPLOYEE-SECURITY
    10 deny tcp ... eq telnet
    20 permit tcp ... eq 443
    30 permit icmp ...
    40 deny ip ...

The match counters provide evidence that traffic is actually hitting the ACL rules.

🛠️ Troubleshooting

During the lab, the trunk was initially configured on the wrong switch interface.

The actual topology was:

R1 G0/0
   |
SW1 Fa0/5

The incorrect trunk configuration was removed from Fa0/24 and applied to Fa0/5.

Useful commands for identifying and troubleshooting interfaces:

show interfaces status
show interfaces trunk
show ip interface brief
show access-lists

This reinforced the importance of checking the actual physical topology before modifying interface configurations.

🧠 What I Learned

Through this lab I practiced:

VLAN segmentation
Layer-2 vs Layer-3 boundaries
802.1Q trunking
Router-on-a-Stick
Inter-VLAN routing
DMZ architecture
Extended ACLs
Source and destination IP matching
TCP port filtering
ICMP filtering
Least-privilege network security
ACL rule ordering
ACL verification
Network troubleshooting
🚀 Future Improvements

Possible improvements to this environment include:

Add a dedicated firewall
Configure NAT/PAT
Add controlled Internet access
Allow DNS only where required
Restrict management access
Implement DHCP Snooping
Implement Dynamic ARP Inspection
Add Wireshark traffic monitoring
Investigate port scans
Simulate and detect DoS/DDoS traffic
🏆 Final Result

The final network successfully separates:

Employees
192.168.10.0/24

from:

DMZ
192.168.20.0/24

and:

Management
192.168.30.0/24

Employees can access the web server through HTTPS, while Telnet and other unauthorized traffic are blocked.

The lab demonstrates how network segmentation + routing + ACLs can be combined to create a more secure enterprise network.
