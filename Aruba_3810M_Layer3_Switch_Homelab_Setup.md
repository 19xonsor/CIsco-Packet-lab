# Aruba 3810M Layer 3 Switch Homelab Setup

This project documents my hands-on homelab work with a real **Aruba 3810M Layer 3 switch**.  
The goal of this lab was to practice:

- Console access to enterprise networking hardware
- Basic Layer 3 switch configuration
- VLAN interface (SVI) creation
- Assigning management IP addresses
- Testing connectivity from a host device
- Verifying inter-device communication with `ping`

This lab was done using a **USB serial console connection**, **PuTTY**, and a host machine connected by Ethernet.

---

## Lab Goals

In this lab, I wanted to:

1. Access the Aruba 3810M through the console port
2. Verify the switch OS and hardware status
3. Enable and verify Layer 3 functionality
4. Configure VLAN interfaces with IP addresses
5. Connect a PC/Mac to the switch
6. Manually assign host IP settings
7. Test successful communication to the switch using ICMP ping

---

## Hardware / Tools Used

- **Aruba 3810M** enterprise switch
- **USB-to-serial console cable**
- **PuTTY** for console access
- **MacBook Air** for IP configuration and ping testing
- Ethernet cable
- Real hardware homelab environment

---

## Initial Switch Verification

After connecting to the switch through the serial console, I verified that the switch was accessible and running ArubaOS-Switch.

Useful verification commands used:

```bash
show version
show running-config
show ip
show vlan
```

The switch showed that **IP Routing was enabled**, confirming Layer 3 switching capability.

---

## VLAN 1 Management Interface Configuration

I configured a Layer 3 IP address on VLAN 1 to use it as a management interface.

### Configuration

```bash
configure terminal
vlan 1
ip address 192.168.1.1 255.255.255.0
exit
write memory
```

### Host IP Configuration for VLAN 1

The connected host was manually configured with:

- **IP address:** `192.168.1.2`
- **Subnet mask:** `255.255.255.0`
- **Default gateway:** `192.168.1.1`

### Result

The host was able to successfully ping the switch interface on VLAN 1.

#### Screenshot – VLAN 1 Host Ping Test
<img width="1536" height="1152" alt="image" src="https://github.com/user-attachments/assets/7776a552-4068-45c2-bc4d-29d83060fb67" />


#### Screenshot – Host IP Configuration for VLAN 1
<img width="1536" height="1152" alt="image" src="https://github.com/user-attachments/assets/c346f91a-afa1-471d-87c7-22bf9276d85f" />

---

## VLAN 2 Interface Configuration

After verifying connectivity on VLAN 1, I created **VLAN 2** and assigned it a Layer 3 interface.

### Configuration

```bash
configure terminal
vlan 2
name "VLAN2"
ip address 192.168.20.1 255.255.255.0
exit
write memory
```

To test VLAN 2, I assigned the host a new static IP in the same subnet.

### Host IP Configuration for VLAN 2

- **IP address:** `192.168.20.2` or `192.168.20.3`
- **Subnet mask:** `255.255.255.0`
- **Default gateway:** `192.168.20.1`

### Result

The host was able to successfully ping the VLAN 2 SVI at `192.168.20.1`.

#### Screenshot – VLAN 2 Ping Test
<img width="1536" height="1152" alt="image" src="https://github.com/user-attachments/assets/154bad5d-a80d-41a8-a116-d38c2ce3ac88" />

---

## Switch Output / CLI Verification

I verified that the switch was operating in Layer 3 mode and reviewed the IP interface information using:

```bash
show ip
```

This confirmed:

- **IP Routing: Enabled**
- VLAN interfaces configured with IP addresses
- Aruba switch acting as a multilayer switch

## What I Learned

This lab helped me better understand:

- The difference between a **Layer 2 switch** and a **Layer 3 switch**
- How a switch virtual interface (**SVI**) works
- How to assign IP addresses directly to VLANs
- How end hosts communicate with switch management interfaces
- How to manually troubleshoot connectivity issues using CLI commands and host IP settings
- The value of using **real enterprise hardware** instead of only simulators

---

## Troubleshooting Notes

During the lab, I ran into some normal configuration issues and corrected them as part of the learning process:

- Verified whether **IP routing** was enabled
- Reconfigured host IP settings to match the correct VLAN subnet
- Confirmed switch VLAN interfaces using `show ip`
- Tested connectivity with repeated `ping` commands
- Used console access to safely make and verify changes

This troubleshooting process was useful because it reflects real-world networking workflow.

---

## Key Commands Used

```bash
show version
show running-config
show ip
show vlan
configure terminal
vlan 1
ip address 192.168.1.1 255.255.255.0
vlan 2
name "VLAN2"
ip address 192.168.20.1 255.255.255.0
write memory
```

---

## Project Outcome

By the end of this lab, I successfully:

- Connected to an Aruba 3810M through console
- Verified switch OS and Layer 3 capability
- Configured VLAN 1 and VLAN 2 with IP addresses
- Assigned static IP settings to a host
- Successfully pinged the switch from the host on both VLAN networks
- Practiced real networking skills using enterprise hardware

---

## Why This Project Matters

This project demonstrates hands-on experience with:

- Enterprise switching
- VLAN configuration
- Layer 3 switch management
- Basic network troubleshooting
- CLI-based administration
- Real hardware homelab work

This is the type of practice that builds skills relevant to:

- Network Engineering
- Data Center Technician roles
- NOC / Infrastructure roles
- CCNA-level networking concepts

---

## Next Steps

Future improvements for this lab:

- Configure access ports and assign them to specific VLANs
- Enable SSH management
- Practice inter-VLAN routing between multiple devices
- Add another switch or router to expand the topology
- Document MAC address table learning and port states
- Build a larger segmented homelab network

---

## Author

**Amr Elkheir**  
Aspiring Network Engineer | Homelab Builder | IT Student

GitHub: [elkdev0](https://github.com/elkdev0)
