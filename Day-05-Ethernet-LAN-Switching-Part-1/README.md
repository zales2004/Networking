# Day 05 — Ethernet LAN Switching (Part 1)

> **Course:** CCNA 200-301  
> **Topic:** Ethernet LAN switching fundamentals  
> **Study goal:** Understand how Ethernet switches learn MAC addresses, make forwarding decisions, and handle known unicast, unknown unicast, and broadcast frames.

**Video lesson:** [Free CCNA — Ethernet LAN Switching (Part 1), Day 5](https://youtu.be/u2n762WG0Vo)

---

## 1. What Is a LAN?

A **Local Area Network (LAN)** connects devices within a limited area, such as a home, office, school, or campus building.

A traditional Ethernet LAN commonly uses switches to connect end devices. Each device uses a network interface card (NIC) to send and receive Ethernet frames.

Example:

```text
PC-A ──┐
PC-B ──┼── Ethernet Switch ─── Router
PC-C ──┘
```

The switch forwards Ethernet frames within the LAN based on destination MAC addresses. A router connects different IP networks and forwards packets between them.

## 2. What Is an Ethernet Switch?

An **Ethernet switch** is a Layer 2 networking device that forwards Ethernet frames using destination MAC addresses.

Its main tasks are to:

1. Receive an Ethernet frame on a port.
2. Read the source and destination MAC addresses.
3. Learn the source MAC address and the port on which it arrived.
4. Look up the destination MAC address in its MAC address table.
5. Forward, flood, or filter the frame according to the lookup result.

A switch is more intelligent than a traditional Ethernet hub because it forwards frames selectively instead of repeating every incoming signal out of every other port.

## 3. Ethernet MAC Addresses

A **Media Access Control (MAC) address** is a Layer 2 address used to identify a network interface on an Ethernet network.

A standard Ethernet MAC address is 48 bits (6 bytes) long and is normally written in hexadecimal.

Example:

```text
00:1A:2B:3C:4D:5E
```

The six hexadecimal byte pairs represent 48 bits in total.

### MAC address structure

- The first 24 bits are commonly the **Organizationally Unique Identifier (OUI)**, identifying the organization allocated that address block.
- The remaining 24 bits identify an interface within that block.
- Real-world address allocation and interface behavior can vary; locally administered addresses may not follow the normal vendor-OUI interpretation.

### Common MAC address types

| Type | Description | Example |
|---|---|---|
| Unicast | Identifies one destination interface | `00:1A:2B:3C:4D:5E` |
| Broadcast | Intended for all devices in the Layer 2 broadcast domain | `FF:FF:FF:FF:FF:FF` |
| Multicast | Intended for a group of interested interfaces | Depends on the multicast group |

A broadcast destination MAC address consists of all 1s in binary and is written as `FF:FF:FF:FF:FF:FF` in hexadecimal.

**Remember:** MAC addresses are used for local Layer 2 delivery. IP addresses are Layer 3 logical addresses used to identify endpoints across IP networks.

## 4. How a Switch Learns MAC Addresses

A switch maintains a **MAC address table** (also called a CAM table on many switches). It maps learned MAC addresses to switch ports, and may also track VLAN information.

The key learning rule is:

> A switch learns from the **source MAC address** of an incoming frame, not from its destination MAC address.

### Example

Suppose PC-A is connected to Switch port Fa0/1 and sends a frame.

- Source MAC: PC-A's MAC address
- Incoming port: Fa0/1

The switch records that PC-A's source MAC address can be reached through Fa0/1.

Illustrative table:

| MAC address | Port |
|---|---|
| `00:AA:AA:AA:AA:AA` (PC-A) | Fa0/1 |
| `00:BB:BB:BB:BB:BB` (PC-B) | Fa0/2 |

The switch builds and updates this table dynamically as frames arrive. It does not need an administrator to manually enter every end-device MAC address in a typical access-switch setup.

## 5. How a Switch Makes Forwarding Decisions

After learning the source MAC address, the switch checks the destination MAC address.

There are three important cases.

### Case A — Known unicast

The destination MAC address exists in the MAC address table.

If PC-A sends a frame to PC-B and the switch knows PC-B is reachable through Fa0/2, it forwards the frame out Fa0/2 only, provided that port is eligible for forwarding in the relevant VLAN.

This avoids sending the frame to every device.

### Case B — Unknown unicast

The destination MAC address is not in the table.

The switch **floods** the frame out eligible ports in the same VLAN, except the port on which the frame arrived. The destination device may receive it and reply; that reply helps the switch learn the destination's source MAC address.

Unknown-unicast flooding is normal switch behavior, not automatically evidence of an attack.

### Case C — Broadcast

The destination MAC address is `FF:FF:FF:FF:FF:FF`.

The switch floods the frame out eligible ports in the same VLAN, except the incoming port. Devices in that Layer 2 broadcast domain can receive the frame.

A VLAN defines a separate Layer 2 broadcast domain. Routers do not normally forward Layer 2 broadcasts from one VLAN/subnet into another.

## 6. Forwarding, Flooding, and Filtering

These terms describe different switch actions.

| Action | Meaning | Typical situation |
|---|---|---|
| Forwarding | Send a frame out a specific port | Destination MAC is learned on another port |
| Flooding | Send a frame out multiple eligible ports, except the incoming port | Unknown unicast, broadcast, and certain multicast traffic |
| Filtering | Do not forward a frame out a particular port | Destination is learned on the same port the frame arrived on |

### Example: destination on the same port

If the switch receives a frame on Fa0/1 and its table says the destination MAC is also reachable through Fa0/1, it does not send the frame back out the same port. This is filtering.

**Important:** Switch forwarding behavior is scoped by VLAN and affected by the switch's forwarding state and features. These examples describe the standard learning-switch behavior.

## 7. Following a Frame Through a Switch

Assume PC-A and PC-B are connected to the same switch and VLAN.

### First communication

1. PC-A sends an Ethernet frame toward PC-B.
2. The switch receives it on PC-A's port.
3. The switch learns PC-A's source MAC address on that port.
4. The switch looks up PC-B's destination MAC address.
5. If PC-B is unknown, the switch floods the frame in the VLAN, excluding the incoming port.
6. PC-B receives the frame if it is connected to an eligible flooded port and the frame is intended for it.

### Reply from PC-B

1. PC-B replies to PC-A.
2. The switch learns PC-B's source MAC address on PC-B's port.
3. The switch looks up PC-A's MAC address.
4. Since PC-A is now known, the switch forwards the reply only toward PC-A's port.

Once both addresses are learned, subsequent unicast frames can generally be forwarded selectively.

## 8. MAC Address Table Aging

Switches do not necessarily retain every dynamically learned MAC address forever.

A dynamically learned entry is generally removed after an **aging timer** expires without the switch seeing relevant source traffic for that address. The entry can be refreshed when frames from that source are received again.

Aging helps keep the table current when devices move, disconnect, or change network attachment points.

The default aging time and configuration commands vary by switch platform and software version.

## 9. Collision Domains and Broadcast Domains

These concepts are often tested in CCNA exams.

### Collision domain

A collision domain is a network segment where simultaneous transmissions can collide under shared-medium, half-duplex Ethernet operation.

- A traditional hub creates one shared collision domain.
- Each switch port is a separate collision domain.
- Modern full-duplex switched Ethernet does not experience Ethernet collisions in normal operation.

### Broadcast domain

A broadcast domain is the set of Layer 2 interfaces that receive a Layer 2 broadcast within a VLAN.

- A basic Layer 2 switch forwards broadcasts within the VLAN.
- Each VLAN is a separate Layer 2 broadcast domain.
- A router or Layer 3 boundary separates broadcast domains by default.

### Hub vs. switch

| Feature | Traditional hub | Ethernet switch |
|---|---|---|
| OSI layer commonly associated | Layer 1 | Layer 2 |
| Reads destination MAC addresses | No | Yes |
| Forwards traffic | Repeats signals to other ports | Uses MAC table and frame type |
| Collision domains | One shared collision domain | One per switch port |
| Bandwidth | Shared | Dedicated per port in typical switched Ethernet |
| Duplex | Typically half-duplex | Commonly full-duplex |

## 10. Full-Duplex and Half-Duplex Ethernet

- **Half-duplex:** A device can transmit or receive at a given moment, but not both simultaneously. Collisions can occur on shared Ethernet media.
- **Full-duplex:** A device can transmit and receive simultaneously. Normal full-duplex switched Ethernet does not use collision detection as half-duplex Ethernet does.

Modern switch-to-host Ethernet links typically operate in full-duplex mode. Speed and duplex negotiation should be configured consistently at both ends when manual settings are required.

## 11. Cisco IOS Commands for Switch Verification

The exact output and supported options vary by switch model and IOS version.

### Display the MAC address table

```text
Switch# show mac address-table
```

This command displays learned MAC addresses and the ports or VLANs associated with them.

### Display interface status

```text
Switch# show interfaces status
```

Useful for checking port status, VLAN assignment, duplex, and speed where supported.

### Inspect an interface

```text
Switch# show interfaces fastethernet 0/1
```

Use the interface name supported by your platform. For example, modern devices may use names such as `GigabitEthernet1/0/1`.

### Display VLAN information

```text
Switch# show vlan brief
```

Useful for checking VLANs and access-port membership on common Cisco Catalyst platforms.

### Discover available commands

```text
Switch# show mac address-table ?
```

Use `?` to discover options supported by the device.

## 12. Practical Lab: Observe MAC Learning

**Goal:** Observe how a switch learns MAC addresses and forwards frames.

### Topology

```text
PC-A ── Fa0/1 ┐
              ├── Switch
PC-B ── Fa0/2 ┘
```

### Procedure

1. Build the topology in Cisco Packet Tracer or use a supported lab switch.
2. Place PC-A and PC-B in the same VLAN and assign them IP addresses in the same subnet.
3. Before generating traffic, inspect the switch's MAC address table.
4. From PC-A, ping PC-B.
5. Inspect the MAC address table again.
6. Identify the learned MAC address and associated port for each PC.
7. Repeat the ping and reason about why subsequent traffic can be forwarded using learned entries.

### What to record

| Observation | Your result |
|---|---|
| PC-A MAC address | |
| PC-A switch port | |
| PC-B MAC address | |
| PC-B switch port | |
| Were MAC addresses learned after traffic? | |
| Which command displayed the table? | |

**Expected learning:** The switch learns source MAC addresses from frames it receives. Once the destination MAC is known, unicast traffic can be sent to the corresponding port rather than flooded across all eligible ports.

## 13. Troubleshooting and Common Misconceptions

- **“A switch learns destination MAC addresses.”** It learns from the source MAC address of incoming frames.
- **“Unknown unicast frames are dropped by default.”** A typical learning switch floods unknown unicast frames within the relevant VLAN.
- **“A switch sends every frame out every port.”** It selectively forwards known unicast traffic and floods only when the forwarding rules call for it.
- **“A switch separates all broadcasts.”** A Layer 2 switch forwards broadcasts within a VLAN; separate VLANs define separate broadcast domains.
- **“A switch eliminates every possible collision.”** Each switch port creates its own collision domain; full-duplex operation avoids normal Ethernet collisions on modern switched links.
- **“MAC addresses and IP addresses are interchangeable.”** MAC addresses are used for Layer 2 delivery; IP addresses support Layer 3 addressing and routing.
- **“Flooding always means malicious activity.”** Flooding can be normal behavior, though unusually high broadcast or unknown-unicast traffic may warrant investigation.

## 14. Quick Revision Table

| Question | Key answer |
|---|---|
| What does a Layer 2 switch use to forward Ethernet frames? | Destination MAC address |
| Which MAC address does the switch learn from an incoming frame? | Source MAC address |
| Where are learned MAC addresses stored? | MAC address table / CAM table |
| What happens to a known unicast frame? | Forwarded toward the learned destination port |
| What happens to an unknown unicast frame? | Flooded within the VLAN, excluding the incoming port |
| What happens to a broadcast frame? | Flooded within the VLAN, excluding the incoming port |
| What is filtering? | Not forwarding a frame out a port where the destination is already reachable, such as the incoming port |
| What is one collision domain on a switch? | Each switch port is a separate collision domain |
| What defines a Layer 2 broadcast domain on a VLAN-aware switch? | The VLAN |
| What command shows learned MAC addresses? | `show mac address-table` |

## 15. Self-Test

Try answering these without looking at the notes.

1. A frame arrives on Fa0/3 with source MAC `AAAA.AAAA.AAAA`. What does the switch learn?
2. The destination MAC is already learned on Fa0/8. Which port should receive the frame?
3. What does a switch do when the destination MAC is unknown?
4. What is the destination MAC address for an Ethernet broadcast?
5. Why does a switch not normally send a frame back out the same port it arrived on when the destination is also learned on that port?
6. How does a VLAN affect broadcast forwarding?
7. What is the difference between a collision domain and a broadcast domain?
8. Which IOS command displays the MAC address table?

### Answers

1. It associates the source MAC address with incoming port Fa0/3 in the relevant VLAN.
2. Fa0/8, assuming the entry is valid and the port is eligible for forwarding.
3. It floods the frame to eligible ports in the same VLAN, excluding the incoming port.
4. `FF:FF:FF:FF:FF:FF`.
5. It filters the frame rather than sending it back through the incoming port.
6. Broadcasts are flooded within the VLAN, not normally across separate VLANs.
7. A collision domain relates to shared-medium transmission collisions; a broadcast domain defines where Layer 2 broadcasts are propagated.
8. `show mac address-table`.

---

## Key takeaway

A switch **learns from source MAC addresses** and uses the **destination MAC address** to decide whether to forward, flood, or filter an Ethernet frame. Understanding this behavior is foundational for Ethernet switching, VLANs, and network troubleshooting.

*Study note for CCNA 200-301 preparation. Verify platform-specific command syntax on the Cisco device or simulator being used.*
