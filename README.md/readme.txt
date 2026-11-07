# Lab 3: VLANs & Network Segmentation (Layer 2 Security)

## Overview
This laboratory experiment demonstrates the configuration of Virtual Local Area Networks (VLANs) on a Cisco Catalyst switch using Cisco Packet Tracer. The objective is to achieve Layer 2 network segmentation to isolate traffic between different organizational departments (HR and Guest Network) for cybersecurity hardening.

---

## Topology & Addressing Table

| Device | Interface | IP Address | Subnet Mask | VLAN | Department |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **PC0** | Fa0/1 | `192.168.1.10` | `255.255.255.0` | VLAN 10 | HR_Department |
| **PC1** | Fa0/2 | `192.168.1.20` | `255.255.255.0` | VLAN 1 (Default) | Default |
| **PC2** | Fa0/3 | `192.168.1.30` | `255.255.255.0` | VLAN 20 | Guest_Network |

---

## Key Configurations & Commands

### 1. VLAN Creation
```text
Switch# configure terminal
Switch(config)# vlan 10
Switch(config-vlan)# name HR_Department
Switch(config-vlan)# vlan 20
Switch(config-vlan)# name Guest_Network
Switch(config-vlan)# exit

Switchport Access Mode Assignment

Switch(config)# interface fastEthernet 0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10

Switch(config)# interface fastEthernet 0/3
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 20
Switch(config-if)# end

Security Verification & Test Results
1. VLAN Status Check (show vlan brief)
Executing show vlan brief on the switch verifies that Fa0/1 is assigned to VLAN 10 and Fa0/3 is assigned to VLAN 20.

2. Inter-VLAN Traffic Isolation Test
A ICMP ping request was initiated from PC0 (192.168.1.10) to PC2 (192.168.1.30):

PC> ping 192.168.1.30
Pinging 192.168.1.30 with 32 bytes of data:

Request timed out.
Request timed out.
Request timed out.
Request timed out.

Ping statistics for 192.168.1.30:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss)

Cybersecurity Key Takeaways
Zero Trust & Blast Radius Control: By logically segmenting host devices into dedicated VLANs, unauthorized lateral movement across network subnets is completely restricted at Layer 2.

Broadcast Isolation: Broadcast frames emitted by VLAN 10 hosts are contained strictly within their broadcast domain, reducing network overhead and preventing ARP spoofing across subnets.
