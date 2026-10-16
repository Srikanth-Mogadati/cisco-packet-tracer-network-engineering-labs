# Cisco Packet Tracer — Network Engineering Labs

Hands-on networking labs documenting a progressive learning journey through Ethernet switching, IPv4 routing, VLANs, and troubleshooting.

## Learning path

| Lab | Scenario | Skills | Project file |
| --- | --- | --- | --- |
| [01 — Basic LAN](lab-01-basic-lan/README.md) | Two PCs communicating through one switch | Cabling, IPv4, ARP, ICMP, MAC learning | `lab-01-basic-lan/lab-01.pkt` |
| [02 — Inter-network routing](lab-02-inter-network-routing/README.md) | HR employee Sarah reaches IT employee David | Default gateways, router CLI, connected routes, troubleshooting | `lab-02-inter-network-routing/lab-02.pkt` |
| [03 — VLANs and trunks](lab-03-vlans-and-trunking/README.md) | Separate HR, Finance and IT across two switches | VLANs, access ports, 802.1Q trunking, STP, fault isolation | `lab-03-vlans-and-trunking/lab-03.pkt` |

## How to use

1. Install Cisco Packet Tracer from Cisco Networking Academy.
2. Open the lab's `.pkt` project when available; read its README for objectives, reference port mapping and addressing.
3. Compare the **reference** topology to the actual saved Packet Tracer project. Adjust interface names or port assignments if different.
4. Use **Realtime** for connectivity testing and **Simulation** to inspect ARP/ICMP.
5. Perform the break/fix exercise, verify recovery, and save your device configuration.

> **Accuracy note:** Reference port mappings and CLI examples describe the teaching scenario, **not a programmatically verified extraction** of proprietary `.pkt` topology data. Confirm physical ports in Packet Tracer. Files may be uploaded separately from this documentation.

## Milestones

- **Lab 01:** Local Layer 2 forwarding and ARP.
- **Lab 02:** Route between `192.168.10.0/24` and `192.168.20.0/24`.
- **Lab 03:** Three Layer 2 VLANs, trunking between floors, and isolation without a router.

These are ordered learning milestones, not claims about historical GitHub publication. Git commits retain their genuine creation timestamps.

## Troubleshooting habits

Check physical link and interface state first; then MAC learning, VLANs and trunks; then host IP/mask, default gateways and routes. Distinguish an initial ARP-related ping timeout from persistent loss. See [common issues](troubleshooting/common-issues.md).

## Attribution

Educational lab documentation. Cisco and Packet Tracer are trademarks of their respective owners; Packet Tracer software is not redistributed here.
