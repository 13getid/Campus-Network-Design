Karatasi University - Campus Network Design & Implementation
1. Project Overview
This project presents the comprehensive network design, simulation, and implementation for Albion University, a distributed institution spanning two campuses separated by 20 miles. The topology encompasses the Main Campus (housing Buildings A, B, and C with administrative, faculty, and IT data centers) and a Smaller Campus (hosting Health and Sciences staff and student labs), integrated via WAN serial links, RIPv2 dynamic routing, router-based DHCP, and an external cloud email server.
2. Project Requirements & Architecture
Main Campus: Features a core Layer 3 switch (MAIN-CAMPUS L3 SWITCH) tied to the MAIN-CAMPUS ROUTER, distributing connectivity across three primary buildings:
Building A: Administrative departments (Management, HR, Finance) and the Faculty of Business using distinct VLANs.
Building B: Faculty of Engineering and Computing (E&C) and Faculty of Art and Design (A&D).
Building C: Student labs and the IT department hosting internal Web and FTP servers.
Smaller Campus: Connected via a 20-mile WAN link to the main site, featuring a branch router and switch serving Faculty of Health and Sciences staff and student laboratories on separate floors.
External Cloud: An external cloud model housing the University Email Server connected via a dedicated WAN subnet.
3. Network Topology
4. IP Addressing & VLAN Allocation Matrix
Network / Department
VLAN ID
Subnet / CIDR
Gateway IP
Description
Admin
VLAN 10
192.168.1.0/24
192.168.1.1
Building A - Administration
HR
VLAN 20
192.168.2.0/24
192.168.2.1
Building A - Human Resources
Finance
VLAN 30
192.168.3.0/24
192.168.3.1
Building A - Finance
Business
VLAN 40
192.168.4.0/24
192.168.4.1
Building A - Faculty of Business
Engineering & Computing
VLAN 50
192.168.5.0/24
192.168.5.1
Building B - E&C Faculty
Art & Design
VLAN 60
192.168.6.0/24
192.168.6.1
Building B - A&D Faculty
Student Labs
VLAN 70
192.168.7.0/24
192.168.7.1
Building C - Student Labs
IT Department & Servers
VLAN 80
192.168.8.0/24
192.168.8.1
Building C - IT & Core Servers
Branch Staff (H&S)
VLAN 90
192.168.9.0/24
192.168.9.1
Smaller Campus - Staff
Branch Student Labs
VLAN 100
192.168.10.0/24
192.168.10.1
Smaller Campus - Student Labs
WAN Link (Main to Branch)
N/A
10.10.10.0/30
Inter-router
Serial connection between campuses
WAN Link (Cloud)
N/A
20.0.0.0/30
Inter-router
External email server link

5. Step-by-Step Configuration Guide
Step 1: Core Routing and RIPv2 Configuration



Plaintext
Router> enable
Router# configure terminal
Router(config)# router rip
Router(config-router)# version 2
Router(config-router)# no auto-summary
Router(config-router)# network 192.168.1.0
Router(config-router)# network 192.168.2.0
Router(config-router)# network 10.0.0.0
Router(config-router)# exit


Step 2: Router-Based DHCP Setup (Building A Example)



Plaintext
Router(config)# ip dhcp pool ADMIN_POOL
Router(dhcp-config)# network 192.168.1.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.1.1
Router(dhcp-config)# dns-server 8.8.8.8
Router(dhcp-config)# exit
Router(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.10


Step 3: Layer 3 Switch VLAN & Inter-VLAN Routing Setup



Plaintext
Switch> enable
Switch# configure terminal
Switch(config)# ip routing
Switch(config)# vlan 10
Switch(config-vlan)# name ADMIN
Switch(config-vlan)# exit
Switch(config)# interface vlan 10
Switch(config-if)# ip address 192.168.1.1 255.255.255.0
Switch(config-if)# no shutdown
Switch(config-if)# exit


Step 4: Static Routing for External Cloud Email Server



Plaintext
Router(config)# ip route 20.0.0.0 255.255.255.252 Serial0/0/0


6. How the Network Works
Traffic is isolated across departments and campuses using VLAN segmentation and 802.1Q trunking. Core Layer 3 switches and routers handle inter-VLAN routing internally, while RIPv2 dynamically propagates network routes across the 20-mile inter-campus serial link. End devices automatically lease IP configurations via router-based DHCP pools.
7. Testing and Verification
DHCP Verification: Checked that clients across Building A successfully acquired dynamic IP addresses.
Connectivity Testing: Verified end-to-end communication between the Main Campus and Smaller Campus using recursive ping statements.
External Reachability: Confirmed routing success to the external cloud email server via static routes.
8. How to Open the Project
Download and install Cisco Packet Tracer.
Clone or download this repository.
Open Campus Network Design.pkt inside Cisco Packet Tracer.
9. Future Improvements
Deploy Access Control Lists (ACLs) to regulate student access to administrative subnets.
Introduce redundant serial pathways to improve inter-campus fault tolerance.
