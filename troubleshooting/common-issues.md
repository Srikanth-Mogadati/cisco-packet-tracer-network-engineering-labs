# Troubleshooting quick reference

| Symptom | Check first | Useful commands |
| --- | --- | --- |
| Link down/red | Cable, NIC, switch port, router shutdown | `show ip interface brief`, `show interfaces status` |
| Switch port amber | STP state and convergence | `show spanning-tree` |
| First ping lost, next works | ARP discovery | PC `arp -a`; Simulation ARP events |
| Same-subnet ping fails | IP/mask, VLAN, cabling, MAC learning | `show vlan brief`, `show mac address-table` |
| Cross-subnet ping fails | Default gateway, router interface, routes | `show ip route`, `show ip interface brief` |
| Same VLAN across switches fails | Trunk mode, allowed VLANs, VLAN existence, STP | `show interfaces trunk`, `show vlan brief` |
| Different VLANs cannot ping in Lab 03 | Expected: no inter-VLAN routing | Check the intended design |

**Workflow:** Observe → identify affected layer → inspect state → make one controlled change → retest → document and save.
