# Lab 03 — Enterprise VLANs and 802.1Q Trunks

## Change request

NexaTech needs separate HR, Finance, and IT broadcast domains across two floors while retaining same-department connectivity. No inter-VLAN routing yet.

## Reference pin-to-pin wiring

```text
HR-PC1 Fa0  -- SW1 Fa0/1       SW2 Fa0/1 -- HR-PC2 Fa0
FIN-PC1 Fa0 -- SW1 Fa0/2       SW2 Fa0/2 -- FIN-PC2 Fa0
IT-PC1 Fa0  -- SW1 Fa0/3       SW2 Fa0/3 -- IT-PC2 Fa0

               SW1 Fa0/24 ==== SW2 Fa0/24
                    802.1Q trunk
```

| Source port | Destination port | Purpose |
| --- | --- | --- |
| HR-PC1 Fa0 | SW1 Fa0/1 | Access VLAN 10 |
| FIN-PC1 Fa0 | SW1 Fa0/2 | Access VLAN 20 |
| IT-PC1 Fa0 | SW1 Fa0/3 | Access VLAN 30 |
| SW1 Fa0/24 | SW2 Fa0/24 | Trunk carrying 10,20,30 |
| SW2 Fa0/1 | HR-PC2 Fa0 | Access VLAN 10 |
| SW2 Fa0/2 | FIN-PC2 Fa0 | Access VLAN 20 |
| SW2 Fa0/3 | IT-PC2 Fa0 | Access VLAN 30 |

Use copper straight-through for PC-to-switch links and Packet Tracer automatic cable selection for the inter-switch link. **Reference mappings:** verify actual `.pkt` ports.

## IP plan

| PC | VLAN | Address |
| --- | --- | --- |
| HR-PC1 / HR-PC2 | 10 | `192.168.10.11/24` / `192.168.10.12/24` |
| FIN-PC1 / FIN-PC2 | 20 | `192.168.20.11/24` / `192.168.20.12/24` |
| IT-PC1 / IT-PC2 | 30 | `192.168.30.11/24` / `192.168.30.12/24` |

Leave default gateways blank in this Layer 2-only lab.

## Switch configuration

Run on SW1; change hostname to SW2 on the second switch:

```cisco
enable
configure terminal
hostname SW1
vlan 10
 name HR
exit
vlan 20
 name FINANCE
exit
vlan 30
 name IT
exit
interface fastEthernet0/1
 description HR-PC
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
exit
interface fastEthernet0/2
 description FINANCE-PC
 switchport mode access
 switchport access vlan 20
 spanning-tree portfast
exit
interface fastEthernet0/3
 description IT-PC
 switchport mode access
 switchport access vlan 30
 spanning-tree portfast
exit
interface fastEthernet0/24
 description TRUNK-TO-OTHER-SWITCH
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
 no shutdown
exit
end
copy running-config startup-config
```

PortFast is for end-host access ports; do not indiscriminately enable it on switch-to-switch trunks.

## Verification

```cisco
show vlan brief
show interfaces trunk
show mac address-table
show spanning-tree
```

Expect Fa0/1 in VLAN 10, Fa0/2 in VLAN 20, Fa0/3 in VLAN 30. Fa0/24 should be a trunk allowing VLANs 10,20,30; trunk ports need not appear under the access VLAN listings.

| Ping | Expected |
| --- | --- |
| HR-PC1 → HR-PC2 | Success |
| FIN-PC1 → FIN-PC2 | Success |
| IT-PC1 → IT-PC2 | Success |
| HR-PC1 → FIN-PC1 | Fail — separate VLAN/subnet, no routing |
| HR-PC1 → IT-PC2 | Fail — separate VLAN/subnet, no routing |

## Incident INC-003 — HR Floor 2 unreachable

Simulate a trunk error on SW2:

```cisco
configure terminal
interface fastEthernet0/24
 switchport trunk allowed vlan 20,30
end
```

HR-PC1 should lose connectivity to HR-PC2 while Finance and IT remain reachable across the trunk. Compare `show interfaces trunk` on both switches. Repair:

```cisco
configure terminal
interface fastEthernet0/24
 switchport trunk allowed vlan 10,20,30
end
copy running-config startup-config
```

Retest same-VLAN pings.

**Takeaway:** Each VLAN is its own Layer 2 broadcast domain. A tagged trunk carries several VLANs across one link. Communication between VLANs requires Layer 3 routing; VLANs alone are not a complete security policy.
