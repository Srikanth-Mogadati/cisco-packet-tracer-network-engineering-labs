# Lab 02 — HR-to-IT Routing

## Incident ticket

HR employee Sarah must reach IT employee David across two separate IPv4 subnets. Configure a Cisco 2911 router, verify connected routes, and troubleshoot an administratively disabled interface.

## Reference pin-to-pin topology

```text
PC-Sarah Fa0 -- SW-HR Fa0/1
SW-HR Fa0/24 -- R1 GigabitEthernet0/0
R1 GigabitEthernet0/1 -- SW-IT Fa0/24
SW-IT Fa0/1 -- PC-David Fa0
```

| Link | From | To | Cable |
| --- | --- | --- | --- |
| 1 | PC-Sarah Fa0 | SW-HR Fa0/1 | Copper straight-through |
| 2 | SW-HR Fa0/24 | R1 G0/0 | Copper straight-through |
| 3 | R1 G0/1 | SW-IT Fa0/24 | Copper straight-through |
| 4 | SW-IT Fa0/1 | PC-David Fa0 | Copper straight-through |

SW-HR and SW-IT are Cisco 2960 switches. **Reference only:** verify saved `.pkt` ports before applying CLI.

## Addressing

| Interface | IPv4/mask | Default gateway |
| --- | --- | --- |
| Sarah | `192.168.10.10/24` | `192.168.10.1` |
| R1 G0/0 | `192.168.10.1/24` | — |
| R1 G0/1 | `192.168.20.1/24` | — |
| David | `192.168.20.10/24` | `192.168.20.1` |

## Router CLI

```cisco
enable
configure terminal
hostname R1-NEXATECH
interface gigabitEthernet0/0
 description TO-SW-HR
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit
interface gigabitEthernet0/1
 description TO-SW-IT
 ip address 192.168.20.1 255.255.255.0
 no shutdown
exit
end
copy running-config startup-config
```

At `Destination filename [startup-config]?`, press Enter to accept the default.

## Verification

```cisco
show ip interface brief
show ip route
```

Both router interfaces should show `up/up`. Expect connected routes `C 192.168.10.0/24` and `C 192.168.20.0/24`, and local `L` routes for interface IPs. `Gateway of last resort is not set` is normal here.

From Sarah:

```text
ping 192.168.10.1
ping 192.168.20.1
ping 192.168.20.10
tracert 192.168.20.10
```

The first ping to David may lose one packet due to ARP; repeat to distinguish convergence from a real fault.

## Break/fix — IT network unreachable

```cisco
configure terminal
interface gigabitEthernet0/1
 shutdown
end
```

Sarah should still reach her own gateway but not David. Diagnose using `show ip interface brief`; restore `no shutdown`, confirm `up/up`, and retest. If still failing, inspect PC gateways, masks, links, and routing table.

**Takeaway:** Switches forward frames within a Layer 2 network; routers select paths between IP networks, replace Ethernet headers across hops, and decrement IP TTL.
