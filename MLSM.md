# 🌐 Week 4 – Multilayer Switch (MLSW) Configuration Lab

> **Tool:** Cisco Packet Tracer  
> **Topic:** Layer 3 Switching, VLANs, EIGRP, DHCP, Inter-VLAN Routing

---

## 📌 What I Built

A multi-site enterprise network simulation in Cisco Packet Tracer featuring:

- **3 ISR 4321 Routers** (R1, R2, R3) connected in a triangle via Serial DCE links
- **2 Multilayer Switches** (MLSW1, MLSW2 — Cisco 3650) acting as both switches and routers
- **4 Access Layer Switches** (2960-24TT) serving end devices
- **8 PCs** distributed across two sites (PC0–PC3 on left, PC4–PC7 on right)
- **2 DHCP Servers** — one on each multilayer switch (pools for VLANs 10, 20, 30, 40)
- **WRT300N Wireless Router** + Smartphone for wireless connectivity
- **CSU/DSU + ISP Cloud** simulating WAN/internet uplink from R1
- **EIGRP routing** across all routers and multilayer switches
- **VLSM subnetting** on all inter-router and router-to-MLSW links

### Network Topology Overview

```
                         [ ISP Cloud ]
                              |
                         [ CSU-DSU ]
                              |
           [WiFi AP] --- [ R1 (ISR4321) ]
                         /           \
               Se0/1/0  /             \ Se0/1/1
                       /               \
              [ R3 ] ---- Se0/1/0----- [ R2 ]
                 |                        |
             G0/0/0                    G0/0/0
                 |                        |
           [ MLSW2 ]                 [ MLSW1 ]
           /       \                /        \
       [Sw3]      [Sw4]         [Sw1]        [Sw2]
      PC5,PC6   PC7,PC8       PC0,PC1       PC2,PC3
```

---

## 🧠 What I Learned

### 1. Layer 3 Switching (Multilayer Switching)
The Cisco 3650 switch is capable of both switching **and** routing. Unlike a regular Layer 2 switch, a multilayer switch can be assigned IP addresses on VLAN interfaces and route traffic between VLANs — eliminating the need for a dedicated router for inter-VLAN routing.

### 2. Inter-VLAN Routing via SVI (Switched Virtual Interfaces)
Instead of using a router-on-a-stick setup, I configured SVIs directly on MLSW1 and MLSW2. Each VLAN (10, 20, 30, 40) gets its own virtual interface with an IP address that acts as the default gateway for hosts in that VLAN.

### 3. VLSM (Variable Length Subnet Masking)
Used different subnet masks (`/30` for point-to-point links, `/24` for LAN segments) to efficiently allocate IP space. Point-to-point links between routers used `/30` subnets (only 2 usable hosts needed).

### 4. DHCP on a Multilayer Switch
Configured DHCP pools directly on MLSW1 and MLSW2 — one pool per VLAN — so that PCs automatically receive IP addresses, subnet masks, and default gateways without a dedicated DHCP server device.

### 5. EIGRP (Enhanced Interior Gateway Routing Protocol)
Enabled EIGRP on all three routers **and** both multilayer switches so all network segments could advertise and learn routes from each other dynamically.

### 6. Trunk Ports on MLSW
All uplink ports on the multilayer switches were configured as trunks to carry traffic from multiple VLANs between the MLSW and the access layer switches.

### 7. Packet Tracer Simulation Mode
Used Simulation Mode with ICMP filters to visually trace the path of a ping from PC1 to PC3, confirming that inter-VLAN traffic was handled entirely by MLSW1 — never leaving the local switch.

---

## 🛠️ Skills Gained

| Skill | Description |
|-------|-------------|
| Layer 3 Switch Config | Enabling IP routing on a 3650, configuring SVIs |
| VLAN Management | Creating and assigning VLANs on access and multilayer switches |
| Trunk Configuration | Setting ports to trunk mode to pass multiple VLANs |
| DHCP Pool Setup | Configuring per-VLAN DHCP pools on a switch |
| EIGRP Routing | Enabling and configuring EIGRP on routers and Layer 3 switches |
| VLSM Subnetting | Assigning /30 masks for point-to-point, /24 for LANs |
| Serial DCE Links | Connecting routers via serial cables, setting clock rates |
| Packet Tracer Simulation | Tracing PDU paths to verify routing behavior |
| `show` Command Verification | Verifying configuration state with show commands |

---

## 💻 Commands Used

### Router Interface Configuration (example: R2)
```bash
Router> enable
Router# configure terminal
Router(config)# interface GigabitEthernet0/0/0
Router(config-if)# ip address 10.23.1.193 255.255.255.252
Router(config-if)# no shutdown

Router(config)# interface Serial0/1/0
Router(config-if)# ip address 10.23.1.181 255.255.255.252
Router(config-if)# clock rate 500000
Router(config-if)# no shutdown
```

### VLAN + Trunk Config on Access Layer Switch (example: Sw1)
```bash
Switch(config)# vlan 10
Switch(config-vlan)# name VLAN10
Switch(config)# vlan 20
Switch(config-vlan)# name VLAN20

Switch(config)# interface FastEthernet0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10

Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# switchport mode trunk
```

### Multilayer Switch – Trunk All Uplinks (MLSW1)
```bash
Switch(config)# interface range GigabitEthernet1/0/1-2
Switch(config-if-range)# switchport trunk encapsulation dot1q
Switch(config-if-range)# switchport mode trunk
```

### Multilayer Switch – Routed Port to R2
```bash
Switch(config)# interface GigabitEthernet1/0/24
Switch(config-if)# no switchport
Switch(config-if)# ip address 10.23.1.194 255.255.255.252
Switch(config-if)# no shutdown
```

### Multilayer Switch – VLAN SVIs (Inter-VLAN Routing)
```bash
Switch(config)# interface vlan 10
Switch(config-if)# ip address 192.168.10.1 255.255.255.0
Switch(config-if)# no shutdown

Switch(config)# interface vlan 20
Switch(config-if)# ip address 192.168.20.1 255.255.255.0
Switch(config-if)# no shutdown
```

### Enable IP Routing on MLSW
```bash
Switch(config)# ip routing
```
> ⚠️ Without this command, the multilayer switch will not route between VLANs or participate in EIGRP properly.

### DHCP Pools on MLSW1
```bash
Switch(config)# ip dhcp pool VLAN10
Switch(dhcp-config)# network 192.168.10.0 255.255.255.0
Switch(dhcp-config)# default-router 192.168.10.1

Switch(config)# ip dhcp pool VLAN20
Switch(dhcp-config)# network 192.168.20.0 255.255.255.0
Switch(dhcp-config)# default-router 192.168.20.1
```

### EIGRP Configuration (Routers + MLSW)
```bash
Router(config)# router eigrp 1
Router(config-router)# network 10.23.1.0 0.0.0.255
Router(config-router)# network 192.168.10.0 0.0.0.255
Router(config-router)# no auto-summary
```

### Verification Commands
```bash
# Check interface status and IPs
show ip interface brief

# Verify routing table
show ip route

# Confirm VLANs and trunks
show vlan brief
show interfaces trunk

# Check DHCP bindings
show ip dhcp binding

# Verify EIGRP neighbors
show ip eigrp neighbors

# Full running config check
show running-config
```

---

## 📡 IP Addressing Table (VLSM)

| Device | Port | Connected To | IP Address | Subnet Mask |
|--------|------|-------------|------------|-------------|
| R1 | G0/0/1 | CSU-DSU | 11.0.0.23 | 255.255.255.0 |
| R1 | G0/0/0 | WiFi Router | 10.23.1.189 | 255.255.255.252 |
| R1 | S0/1/0 | R2 | 10.23.1.177 | 255.255.255.252 |
| R1 | S0/1/1 | R3 | 10.23.1.186 | 255.255.255.252 |
| R2 | S0/1/0 | R3 | 10.23.1.181 | 255.255.255.252 |
| R2 | S0/1/1 | R1 | 10.23.1.178 | 255.255.255.252 |
| R2 | G0/0/0 | MLSW1 | 10.23.1.193 | 255.255.255.252 |
| R3 | S0/1/0 | R1 | 10.23.1.185 | 255.255.255.252 |
| R3 | S0/1/1 | R2 | 10.23.1.182 | 255.255.255.252 |
| R3 | G0/0/0 | MLSW2 | 10.23.1.197 | 255.255.255.252 |

---

## ✅ Key Takeaways

- A **multilayer switch** (Layer 3 switch) combines the functions of a switch and a router — it can forward frames at Layer 2 **and** route packets at Layer 3.
- `ip routing` must be explicitly enabled on a Cisco 3650 — it doesn't route by default.
- DHCP pools configured on the MLSW serve all hosts in each VLAN, making the network more scalable and easier to manage than static IPs.
- EIGRP propagates routes between routers and MLSWs so all devices can reach each other across the full network.
- Using **Simulation Mode** in Packet Tracer is a powerful way to verify that traffic is taking the expected path through the network.

---

## 🔜 Next Steps / Improvements

- [ ] Add a centralized DHCP server instead of per-switch pools for easier management
- [ ] Configure ACLs to restrict traffic between VLANs
- [ ] Add NAT/PAT on R1 for internet-bound traffic
- [ ] Explore OSPF as an alternative to EIGRP

---

*Lab completed in Cisco Packet Tracer as part of Week 4 of Introduction to Routing and Switching.*
