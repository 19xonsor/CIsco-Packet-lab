🧠 Multi-Layer Switch (MLSW) Network Lab
📌 Overview

This project focuses on building and configuring a multi-site network using multi-layer switches (Layer 3 switches), routers, and VLAN segmentation. The goal was to simulate a real-world enterprise network with routing, switching, and dynamic IP assignment.

🏗️ What I Built
Designed a multi-router topology (R1, R2, R3) connected via serial links
Deployed two multilayer switches (3650 MLSW1 & MLSW2) for inter-VLAN routing
Configured access layer switches (2960) for end devices
Created multiple VLANs (10, 20, 30, 40) across the network
Implemented:
Inter-VLAN routing using MLSW
DHCP services hosted on Layer 3 switches
EIGRP dynamic routing across routers and switches
Integrated:
Wireless network (WRT300N + smartphone)
External ISP connection with server reachability
📚 What I Learned
How Layer 3 switches combine switching + routing
Difference between:
Traditional router-on-a-stick vs MLSW routing
How to:
Assign IPs directly to VLAN interfaces (SVIs)
Use MLSW as default gateway instead of routers
VLSM subnetting for efficient IP allocation
End-to-end packet flow using simulation mode (ICMP PDU tracking)
🛠️ Skills Gained
Networking Concepts
VLAN segmentation & trunking
Inter-VLAN routing
DHCP configuration
Dynamic routing (EIGRP)
Subnetting (VLSM)
Default gateway design
Practical Skills
Building full topology in Packet Tracer
Troubleshooting connectivity (ping, routing tables)
Reading and verifying configs
Understanding packet flow in OSI model
