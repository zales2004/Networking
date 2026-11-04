# Day 05 — Ethernet LAN Switching (Part 1)

> **Course:** CCNA 200-301  
> **Topic:** Ethernet LAN switching fundamentals  
> **Study focus:** Ethernet frames, MAC addressing, number systems, and how switches learn and forward frames.

## Learning objectives

By the end of this lesson, you should be able to:

- Explain the role of the Physical and Data Link layers in Ethernet communication.
- Describe the purpose and fields of an Ethernet frame.
- Read and interpret 48-bit MAC addresses written in hexadecimal.
- Understand decimal and hexadecimal number systems.
- Distinguish unicast, broadcast, and multicast communication.
- Explain how a switch learns MAC addresses and builds its MAC address table.
- Describe known-unicast forwarding and unknown-unicast flooding.
- Trace how MAC addresses are learned when two switches are connected.

---

## 1. The Physical Layer and Data Link Layer

Ethernet communication involves more than one layer of the OSI model.

### Layer 1 — Physical layer

The Physical layer is responsible for transmitting raw bits across a physical medium.

It deals with characteristics such as:

- Electrical, optical, or radio signals.
- Cables, connectors, and transceivers.
- Signalling and bit transmission.
- Data rates and physical link characteristics.

At this layer, the information is handled as a stream of bits: 1s and 0s.

### Layer 2 — Data Link layer

The Data Link layer provides communication across a local network link. Ethernet operates at this layer.

Its responsibilities include:

- Framing: organizing bits into frames.
- Physical addressing: using source and destination MAC addresses.
- Identifying the protocol carried inside the frame.
- Detecting transmission errors using the Frame Check Sequence (FCS).

A Layer 2 Ethernet switch examines Ethernet frames and uses MAC addresses to make forwarding decisions.

**Remember:** Layer 1 moves bits across the medium; Layer 2 organizes those bits into frames and provides local-link delivery.

## 2. What is a LAN?

A **Local Area Network (LAN)** connects devices within a limited geographical area, such as a home, office, laboratory, or campus.

An Ethernet LAN commonly uses switches to connect end devices.

Example:

~~~text
PC-A ──┐
PC-B ──┼── Ethernet Switch
PC-C ──┘
~~~

The switch allows devices in the LAN to communicate. A router is used when traffic needs to travel between different IP networks.

A switch makes its Layer 2 forwarding decisions using destination MAC addresses.

## 3. Protocol Data Units (PDUs)

As data moves through the network stack, each layer handles the information in a particular form. These forms are called **Protocol Data Units (PDUs)**.

Common names include:

| OSI layer | PDU name |
|---|---|
| Application, Presentation, Session | Data |
| Transport | Segment (TCP) / Datagram (UDP) |
| Network | Packet |
| Data Link | Frame |
| Physical | Bits |

For this lesson, focus on the Data Link layer: its PDU is the **frame**.

The Data Link layer takes the network-layer packet, adds Ethernet information around it, and creates an Ethernet frame. The Physical layer then transmits the frame as bits.

This process is part of **encapsulation**.

## 4. Ethernet Frame Structure

An Ethernet frame contains control information and the data being transported.

A traditional Ethernet frame has these fields:

| Field | Size | Purpose |
|---|---:|---|
| Preamble | 7 bytes | Helps the receiving device synchronize with the incoming signal |
| Start Frame Delimiter (SFD) | 1 byte | Marks the end of the preamble and indicates that the frame begins next |
| Destination MAC address | 6 bytes | Identifies the intended Layer 2 destination |
| Source MAC address | 6 bytes | Identifies the sending interface |
| Type/Length | 2 bytes | Identifies the encapsulated protocol in Ethernet II, or indicates payload length in IEEE 802.3 |
| Data and padding | 46–1500 bytes in the traditional Ethernet format | Carries the encapsulated data; padding is added when required to meet the minimum frame size |
| Frame Check Sequence (FCS) | 4 bytes | Contains a CRC value used to detect errors |

### Preamble and SFD

The **Preamble** is 7 bytes long. It provides a repeating bit pattern that helps the receiving network interface synchronize its timing with the incoming transmission.

The **Start Frame Delimiter (SFD)** is 1 byte long. It marks the end of the preamble and signals that the actual Ethernet frame is about to begin.

The preamble and SFD are transmitted before the destination and source MAC address fields.

### Destination and source MAC addresses

Both fields are 6 bytes (48 bits) long.

- **Destination MAC:** Where the frame is intended to go on the local Layer 2 network.
- **Source MAC:** The MAC address of the interface that transmitted the frame.

A switch uses the source MAC address to learn where a device is connected. It uses the destination MAC address to decide where to send the frame.

### Type/Length field

The Type/Length field is 2 bytes (16 bits) long.

In Ethernet II, it identifies the protocol carried in the payload. Common EtherType examples include:

| EtherType | Encapsulated protocol |
|---|---|
| 0x0800 | IPv4 |
| 0x0806 | ARP |
| 0x86DD | IPv6 |

In IEEE 802.3 framing, this field can represent the payload length. The interpretation depends on the Ethernet framing format.

### Data and padding

The payload carries the data from the upper layer, such as an IP packet.

In traditional Ethernet, the minimum payload is 46 bytes. If the actual payload is smaller, padding is added to meet the minimum frame size.

The traditional maximum payload is 1500 bytes (excluding the Ethernet header and FCS).

### Frame Check Sequence (FCS)

The FCS is 4 bytes long and contains a **Cyclic Redundancy Check (CRC)** value.

The receiving interface calculates a CRC value from the received frame and compares it with the FCS. If the values do not match, the frame is considered corrupted and is normally discarded.

**Important:** FCS detects transmission errors. It does not correct the corrupted frame.

### Header, payload, and trailer

For Ethernet II:

- The Ethernet header is 14 bytes: destination MAC (6), source MAC (6), and Type (2).
- The payload is normally 46–1500 bytes in the traditional format.
- The FCS trailer is 4 bytes.
- Preamble and SFD are transmitted before the MAC frame.

The traditional minimum Ethernet frame size is 64 bytes, measured from the destination MAC address through the FCS. The traditional maximum is 1518 bytes for an untagged Ethernet frame, excluding preamble and SFD.

## 5. MAC Addresses

A **Media Access Control (MAC) address** is a Layer 2 address assigned to a network interface.

A standard Ethernet MAC address is:

- 48 bits long.
- Equal to 6 bytes.
- Written as 12 hexadecimal digits.
- Commonly displayed as six pairs separated by colons or hyphens.

Example:

~~~text
00:1A:2B:3C:4D:5E
~~~

Each pair represents one byte (8 bits). Six bytes multiplied by 8 bits gives 48 bits.

### MAC address structure

A traditionally globally administered MAC address is divided into two parts:

- **OUI (Organizationally Unique Identifier):** The first 24 bits, commonly associated with the organization to which the address block was allocated.
- **Device/interface portion:** The remaining 24 bits, used to identify an interface within that block.

The first bit of the first octet also indicates whether the address is individual (unicast) or group (multicast). The next bit indicates whether the address is universally or locally administered.

Locally administered MAC addresses are also common, so the OUI does not always identify the actual manufacturer of the device currently using an address.

### MAC address notation

The same MAC address can be displayed in different formats:

~~~text
00:1A:2B:3C:4D:5E
00-1A-2B-3C-4D-5E
001A.2B3C.4D5E
~~~

Cisco IOS commonly displays MAC addresses in dotted hexadecimal notation, for example 001A.2B3C.4D5E.

## 6. Decimal Number System

The decimal number system is **base 10**. It uses ten digits:

0 1 2 3 4 5 6 7 8 9

Each position represents a power of 10.

For example:

\[
352 = (3 \times 100) + (5 \times 10) + (2 \times 1)
\]

\[
352 = 300 + 50 + 2
\]

Networking often uses decimal numbers when representing values such as IPv4 octets. However, MAC addresses are normally represented in hexadecimal.

## 7. Hexadecimal Number System

The hexadecimal number system is **base 16**. It uses sixteen symbols:

0 1 2 3 4 5 6 7 8 9 A B C D E F

The letters represent decimal values as follows:

| Hexadecimal | Decimal |
|---|---:|
| A | 10 |
| B | 11 |
| C | 12 |
| D | 13 |
| E | 14 |
| F | 15 |

Each hexadecimal digit represents exactly 4 bits (one nibble).

| Hex | Binary |
|---|---|
| 0 | 0000 |
| 1 | 0001 |
| 2 | 0010 |
| 3 | 0011 |
| 4 | 0100 |
| 5 | 0101 |
| 6 | 0110 |
| 7 | 0111 |
| 8 | 1000 |
| 9 | 1001 |
| A | 1010 |
| B | 1011 |
| C | 1100 |
| D | 1101 |
| E | 1110 |
| F | 1111 |

### Why hexadecimal is useful in networking

A MAC address contains 48 bits. Writing 48 individual binary digits is inconvenient, so hexadecimal provides a shorter representation.

Since one hexadecimal digit represents 4 bits:

\[
48 \div 4 = 12
\]

Therefore, a 48-bit MAC address is represented by 12 hexadecimal digits.

### Hexadecimal-to-decimal examples

Each position in a hexadecimal number represents a power of 16.

Example 1: Convert 2F to decimal.

\[
2F_{16} = (2 \times 16) + (15 \times 1)
\]

\[
= 32 + 15 = 47_{10}
\]

Example 2: Convert 17 to decimal.

\[
17_{16} = (1 \times 16) + (7 \times 1)
\]

\[
= 16 + 7 = 23_{10}
\]

### Convert a hexadecimal byte to binary

Take A5:

- A = 1010
- 5 = 0101

Therefore:

\[
A5_{16} = 10100101_2
\]

**Exam tip:** Memorize the 0–F hexadecimal-to-binary table. It makes reading MAC addresses and packet captures much easier.

## 8. Unicast, Broadcast, and Multicast

Ethernet frames can use different destination MAC address types.

### Unicast

A **unicast** frame is sent from one source to one specific destination.

Example:

~~~text
PC-A ─────────────> PC-B
       Unicast
~~~

A unicast destination MAC address identifies an individual interface.

### Broadcast

A **broadcast** frame is intended for all devices in the local Layer 2 broadcast domain.

The Ethernet broadcast MAC address is:

~~~text
FF:FF:FF:FF:FF:FF
~~~

All 48 bits are set to 1.

A switch floods broadcast frames to eligible ports in the same VLAN, except the port on which the frame arrived.

### Multicast

A **multicast** frame is intended for a group of devices that are interested in receiving that traffic.

It is not addressed to just one interface, and it is not necessarily intended for every device in the LAN. Switches may use multicast-related features such as IGMP snooping to control how multicast traffic is forwarded.

### Comparison

| Type | Intended recipient |
|---|---|
| Unicast | One interface |
| Broadcast | All devices in the Layer 2 broadcast domain |
| Multicast | A selected group of interfaces |

## 9. The MAC Address Table

An Ethernet switch maintains a **MAC address table**, also called a MAC table or CAM table.

The table maps a MAC address to the switch port through which that address can be reached. On VLAN-aware switches, the VLAN is also part of the forwarding context.

Example:

| MAC address | VLAN | Switch port |
|---|---:|---|
| 00:AA:AA:AA:AA:AA | 1 | Fa0/1 |
| 00:BB:BB:BB:BB:BB | 1 | Fa0/2 |
| 00:CC:CC:CC:CC:CC | 1 | Fa0/3 |

The switch uses this table to decide where to forward frames.

### How does a switch learn MAC addresses?

The most important rule:

> **A switch learns the source MAC address of an incoming frame.**

When a frame arrives, the switch:

1. Reads the source MAC address.
2. Checks the port on which the frame arrived.
3. Adds or updates a MAC-table entry associating that source MAC with the incoming port and VLAN.
4. Examines the destination MAC address to decide what to do next.

The switch does not learn a device's location from the destination MAC address. The source address is the evidence that the device transmitted a frame through that port.

### Dynamic MAC address learning

In normal operation, switches learn MAC addresses dynamically as traffic arrives. An administrator does not need to manually configure every end-device MAC address.

Dynamic entries can age out after a period without traffic from that source. The exact aging behavior and timer depend on the switch platform and configuration.

## 10. Known Unicast — Forwarding

A **known unicast** frame is a unicast frame whose destination MAC address is already present in the switch's MAC address table.

Example:

- PC-A is connected to Fa0/1.
- PC-B is connected to Fa0/2.
- The switch has learned PC-B's MAC address on Fa0/2.
- PC-A sends a frame to PC-B.

The switch:

1. Receives the frame on Fa0/1.
2. Learns or refreshes PC-A's source MAC address on Fa0/1.
3. Looks up PC-B's destination MAC address.
4. Finds PC-B associated with Fa0/2.
5. Forwards the frame out Fa0/2.

~~~text
PC-A ── Fa0/1 ── [ SWITCH ] ── Fa0/2 ── PC-B
                         |
                Destination known:
                forward to Fa0/2
~~~

The switch does not need to send this known-unicast frame out every port. It sends it toward the learned destination port, subject to VLAN and forwarding-state rules.

## 11. Unknown Unicast — Flooding

An **unknown unicast** frame is a unicast frame whose destination MAC address is not currently present in the switch's MAC address table.

Example:

- PC-A sends a frame to PC-B.
- The switch has learned PC-A's source MAC address.
- The switch does not yet have an entry for PC-B's destination MAC address.

The switch cannot identify the destination port, so it **floods** the frame.

Flooding means sending the frame out all eligible ports in the same VLAN, except the port on which the frame arrived.

~~~text
                         ┌── PC-B
PC-A ──> [ SWITCH ] ─────┼── PC-C
                         └── PC-D

Unknown destination:
flood out eligible ports except the incoming port
~~~

If the destination device receives the frame and replies, the switch can learn that device's source MAC address and the port on which the reply arrived.

**Important:** Unknown-unicast flooding is a normal part of Ethernet switching. It does not automatically mean that an attack is occurring.

## 12. Learning and Forwarding — Step-by-Step Example

Consider three PCs connected to one switch:

- PC-A on Fa0/1
- PC-B on Fa0/2
- PC-C on Fa0/3

Assume the switch's MAC address table is initially empty.

### Step 1 — PC-A sends a frame to PC-B

The switch receives the frame on Fa0/1.

- Source MAC: PC-A
- Destination MAC: PC-B

The switch learns PC-A's MAC address on Fa0/1. It then looks up PC-B's destination MAC. Because the table is empty for PC-B, the switch floods the frame out Fa0/2 and Fa0/3 (assuming both are eligible and in the same VLAN).

### Step 2 — PC-B replies to PC-A

PC-B's reply arrives on Fa0/2.

- Source MAC: PC-B
- Destination MAC: PC-A

The switch learns PC-B's MAC address on Fa0/2. It looks up PC-A and finds PC-A on Fa0/1, so it forwards the reply only out Fa0/1.

### Step 3 — PC-A sends another frame to PC-B

The switch already knows:

| MAC address | Port |
|---|---|
| PC-A's MAC | Fa0/1 |
| PC-B's MAC | Fa0/2 |

The switch learns/refreshes PC-A's source entry, looks up PC-B's destination entry, and forwards the frame only out Fa0/2.

### What should you understand?

- The switch learns from the source MAC address.
- It looks up the destination MAC address.
- Unknown destination: flood within the VLAN, excluding the incoming port.
- Known destination: forward toward the learned port.
- A reply from a previously unknown device allows the switch to learn that device's location.

## 13. MAC Learning Across Two Switches

MAC learning also works when switches are connected to one another.

Consider this topology:

~~~text
PC-A ── Switch 1 ───── Switch 2 ── PC-B
                         |
                        PC-C
~~~

Suppose PC-A sends a frame to PC-B and neither switch has learned the destination yet.

1. Switch 1 receives the frame from PC-A and learns PC-A's source MAC address on the port connected to PC-A.
2. Switch 1 does not know PC-B's destination MAC address, so it floods the frame out eligible ports in that VLAN, including the link toward Switch 2.
3. Switch 2 receives the frame on its inter-switch port. It learns PC-A's source MAC address on that inter-switch port.
4. If Switch 2 does not know PC-B's destination MAC address, it floods the frame out its other eligible ports in that VLAN, excluding the incoming inter-switch port.
5. PC-B can receive the frame.
6. When PC-B replies, Switch 2 learns PC-B's source MAC address on PC-B's port.
7. Switch 2 forwards the reply toward the inter-switch link because it has learned PC-A through that link.
8. Switch 1 receives the reply and learns PC-B's source MAC address on the port connected to Switch 2.
9. Switch 1 already knows PC-A's MAC address, so it forwards the reply toward PC-A.

After this exchange, both switches have learned where the two PCs are reachable.

**Key point:** A switch learns the source MAC address of every frame it receives, including frames received over a link connected to another switch. It does not need to be directly connected to the end device to learn the device's MAC address.

## 14. Common Mistakes to Avoid

| Incorrect understanding | Correct understanding |
|---|---|
| A switch learns the destination MAC address. | It learns the source MAC address of incoming frames. |
| A known unicast frame is flooded. | It is forwarded toward the port associated with its destination MAC address. |
| An unknown unicast frame is always dropped. | A typical switch floods it within the relevant VLAN. |
| The switch learns a MAC only when a device is directly connected. | It can learn a source MAC from a frame received through another switch. |
| The preamble is part of the destination/source MAC header. | Preamble and SFD come before the MAC frame fields. |
| FCS corrects corrupted data. | FCS/CRC detects errors; it does not repair the frame. |
| Every device receives a unicast frame. | A known unicast is sent toward its learned destination port. |

## 15. Knowledge Check

Try to answer each question before checking the answer.

**1. What is the purpose of the Ethernet preamble?**

It helps the receiving interface synchronize with the incoming transmission.

**2. What does the Start Frame Delimiter (SFD) indicate?**

It marks the end of the preamble and indicates that the Ethernet frame begins next.

**3. Which field does a switch use to learn a device's location?**

The source MAC address of the incoming frame.

**4. How large is the Ethernet FCS field?**

4 bytes.

**5. What happens when a switch receives a known-unicast frame?**

It forwards the frame toward the port associated with the destination MAC address in the MAC table.

**6. What happens when a switch receives an unknown-unicast frame?**

It floods the frame out eligible ports in the same VLAN, except the incoming port.

**7. How many bits are in a standard Ethernet MAC address?**

48 bits, or 6 bytes.

**8. How many bits does one hexadecimal digit represent?**

4 bits.

**9. What is the decimal value of hexadecimal 2F?**

47, because (2 × 16) + 15 = 47.

**10. What is the Ethernet broadcast MAC address?**

FF:FF:FF:FF:FF:FF.

## 16. Quick Revision Sheet

### Ethernet frame facts

- Preamble: 7 bytes.
- SFD: 1 byte.
- Destination MAC: 6 bytes.
- Source MAC: 6 bytes.
- Type/Length: 2 bytes.
- FCS: 4 bytes.
- MAC header in Ethernet II: 14 bytes.
- Traditional minimum Ethernet frame: 64 bytes, from destination MAC through FCS.
- Traditional maximum untagged Ethernet frame: 1518 bytes, from destination MAC through FCS.

### MAC address facts

- 48 bits = 6 bytes = 12 hexadecimal digits.
- OUI is traditionally the first 24 bits of a globally administered MAC address.
- Unicast: one destination.
- Broadcast: all devices in the Layer 2 broadcast domain.
- Multicast: a selected group of destinations.
- Broadcast MAC: FF:FF:FF:FF:FF:FF.

### Switching facts

- A switch learns from the **source MAC address**.
- A switch looks up the **destination MAC address**.
- Known unicast: forward toward the learned port.
- Unknown unicast: flood within the VLAN, except the incoming port.
- Broadcast: flood within the VLAN, except the incoming port.
- A switch learns MAC addresses from frames received on inter-switch links too.

### The three-step switching mental model

1. **Receive and learn:** Receive the frame and learn its source MAC address on the incoming port/VLAN.
2. **Look up:** Check the destination MAC address in the MAC address table.
3. **Decide:** Forward a known unicast toward its learned port; flood an unknown unicast or broadcast to eligible ports in the VLAN, excluding the incoming port.

---

## Final takeaway

The central concept of Ethernet LAN switching is simple:

**A switch learns where devices are from the source MAC addresses of incoming frames, then uses the destination MAC address to decide where the frame should go.**

Understand the Ethernet frame fields, MAC address format, and the difference between known-unicast forwarding and unknown-unicast flooding. These ideas form the foundation for learning VLANs, switching operations, and network troubleshooting.

*Study note for CCNA 200-301 preparation.*
