# Lab 01 — One Switch, Two PCs

## Service ticket
Two employees need reliable local connectivity. Build an Ethernet LAN, configure static IPv4 addressing, inspect ARP and ICMP, and confirm MAC learning.

## Reference pin-to-pin topology

```text
PC-Alice FastEthernet0 --- SW1 FastEthernet0/1
PC-Bob   FastEthernet0 --- SW1 FastEthernet0/2
```

| Cable | From | To | Type |
| --- | --- | --- | --- |
| 1 | PC-Alice FastEthernet0 | Cisco 2960 SW1 Fa0/1 | Copper straight-through |
| 2 | PC-Bob FastEthernet0 | Cisco 2960 SW1 Fa0/2 | Copper straight-through |

These are **reference** connections; inspect the actual saved Packet Tracer topology for exact ports.

## Address plan

| Host | IPv4 address | Mask | Gateway |
| --- | --- | --- | --- |
| Alice | `192.168.10.10` | `255.255.255.0` | None |
| Bob | `192.168.10.20` | `255.255.255.0` | None |

## Build and verify

1. Place two PCs and one 2960 switch; connect the ports above.
2. Set static addresses under each PC's **Desktop → IP Configuration**.
3. Wait for ports to forward. From Alice run:

```text
ping 192.168.10.20
arp -a
```

4. On SW1:

```cisco
enable
show mac address-table
show interfaces status
```

5. Switch to **Simulation**, filter ARP and ICMP, and inspect request/reply events.

## Expected result

Both hosts share `192.168.10.0/24`; ping succeeds without a router. The switch learns their MAC addresses on access ports. An initial ping can time out while ARP completes.

## Break/fix

Change Bob to `192.168.20.20/24` without adding a router. Alice can no longer reach Bob via normal IP communication because the destination is outside her subnet and she has no gateway. Restore `192.168.10.20/24` and retest.

**Takeaway:** Layer 1 carries signals, Layer 2 forwards Ethernet frames, and ARP resolves local IPv4 addresses to MAC addresses. A Layer 2 switch does not route between IP networks.
