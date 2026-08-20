Inter-VLAN Routing & Router-on-a-Stick Troubleshooting
📌 Project Overview

This project demonstrates how to configure inter-VLAN routing using a Cisco router and switch in Cisco Packet Tracer.

The network contains two separate VLANs. Initially, devices in different VLANs cannot communicate because VLANs create separate Layer 2 broadcast domains.

A Cisco router was configured using router-on-a-stick to allow communication between the VLANs.

🎯 Objectives
Create and configure VLANs
Assign switch ports to VLANs
Configure access ports
Configure a trunk connection
Configure router subinterfaces
Configure 802.1Q VLAN tagging
Configure default gateways
Enable communication between different VLANs
Troubleshoot network connectivity
🖥️ Network Topology
                 ┌──────────────┐
                 │   Router R1  │
                 │              │
                 │ G0/0         │
                 └──────┬───────┘
                        │
                     TRUNK
                        │
                 ┌──────┴───────┐
                 │   Switch SW1 │
                 └──────┬───────┘
                       / \
                      /   \
                     /     \
                   PC1     PC2
                 VLAN 10  VLAN 20
🌐 Network Design
VLAN 10
VLAN ID:       10
Network:       192.168.10.0/24
Gateway:       192.168.10.1
VLAN 20
VLAN ID:       20
Network:       192.168.20.0/24
Gateway:       192.168.20.1
📋 IP Addressing
Device	VLAN	IP Address	Subnet Mask	Default Gateway
PC1	10	192.168.10.10	255.255.255.0	192.168.10.1
PC2	20	192.168.20.10	255.255.255.0	192.168.20.1
R1 G0/0.10	10	192.168.10.1	255.255.255.0	—
R1 G0/0.20	20	192.168.20.1	255.255.255.0	—
⚙️ Configuration
1. Creating the VLANs
vlan 10
name VLAN10
exit

vlan 20
name VLAN20
exit
2. Configuring PC1's Switch Port

PC1 is connected to Fa0/1.

interface fastEthernet 0/1
switchport mode access
switchport access vlan 10
exit

PC1 is therefore assigned to VLAN 10.

3. Configuring PC2's Switch Port

PC2 is connected to Fa0/2.

interface fastEthernet 0/2
switchport mode access
switchport access vlan 20
exit

PC2 is therefore assigned to VLAN 20.

4. Configuring the Trunk

The switch connects to the router through Fa0/24.

The link must carry traffic for both VLANs, so it is configured as a trunk.

interface fastEthernet 0/24
switchport mode trunk
exit

The trunk allows VLAN 10 and VLAN 20 traffic to travel across the same physical connection.

🛜 Router Configuration

The router uses subinterfaces to provide a gateway for each VLAN.

VLAN 10
interface gigabitEthernet 0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
VLAN 20
interface gigabitEthernet 0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0

The physical interface must also be enabled:

interface gigabitEthernet 0/0
no shutdown
🧠 How Inter-VLAN Routing Works

PC1 is:

192.168.10.10

PC2 is:

192.168.20.10

Because they are on different networks, PC1 cannot communicate directly with PC2.

PC1 sends the traffic to its default gateway:

192.168.10.1

The router receives the traffic through its VLAN 10 subinterface.

The router determines that:

192.168.20.0/24

is reachable through VLAN 20.

It then forwards the traffic toward:

192.168.20.10

The return traffic follows the same process in reverse.

PC1
192.168.10.10
      ↓
192.168.10.1
      ↓
    Router
      ↓
192.168.20.1
      ↓
PC2
192.168.20.10
🧪 Verification & Testing
Check VLAN Configuration
show vlan brief

Expected result:

VLAN 10 → Fa0/1
VLAN 20 → Fa0/2
Check the Trunk
show interfaces trunk

Expected result:

Fa0/24

should appear as a trunk.

Check Router Interfaces
show ip interface brief

The router should show:

G0/0.10    192.168.10.1    up    up
G0/0.20    192.168.20.1    up    up
✅ Connectivity Tests
PC1 → VLAN 10 Gateway
ping 192.168.10.1

Result: Successful ✅

PC2 → VLAN 20 Gateway
ping 192.168.20.1

Result: Successful ✅

PC1 → PC2
ping 192.168.20.10

Result: Successful ✅

This confirms that inter-VLAN routing is functioning correctly.

🛠️ Troubleshooting

During the lab, the router interface was found to be administratively down.

The issue was resolved by enabling the physical interface:

interface gigabitEthernet 0/0
no shutdown

The trunk was also intentionally changed from:

switchport mode trunk

to:

switchport mode access

to observe how breaking the trunk affected communication between the VLANs.

The trunk was then restored:

interface fastEthernet 0/24
switchport mode trunk

This demonstrated the importance of verifying both the physical interface state and trunk configuration when troubleshooting inter-VLAN connectivity.

📚 Key Concepts Learned
VLAN: A logical Layer 2 network used to separate devices into different broadcast domains.
Access Port: A switch port assigned to a single VLAN, typically used for end devices.
Trunk Port: A switch port capable of carrying traffic from multiple VLANs.
802.1Q: VLAN tagging standard used to identify VLAN traffic across trunk links.
Subinterface: A logical interface created on a physical router interface.
Default Gateway: The device a host uses to reach networks outside its own subnet.
Inter-VLAN Routing: Routing traffic between different VLANs using a Layer 3 device.
Router-on-a-Stick: A configuration where one physical router interface uses multiple subinterfaces to route between VLANs.
🎯 Final Result

The completed lab successfully demonstrated:

VLAN 10
   │
   │
   ▼
 SWITCH
   │
 TRUNK
   │
   ▼
 ROUTER
   │
   │
   ▼
VLAN 20

PC1 in VLAN 10 was successfully able to communicate with PC2 in VLAN 20 through the router.

Status: ✅ Completed

🛠️ Tools Used
Cisco Packet Tracer
Cisco 2960 Switch
Cisco 1941 Router
IPv4
VLANs
802.1Q
Router-on-a-Stick
ICMP/Ping
