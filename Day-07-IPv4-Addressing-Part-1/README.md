# Day 07 — IPv4 Addressing (Part 1)

> **Course:** CCNA 200-301 — Jeremy's IT Lab  
> **Topic:** IPv4 addresses, binary conversion, address classes, subnet masks, network addresses, and broadcast addresses  
> **Purpose:** Clear reference notes for revision and future networking practice.

## Learning objectives

After this lesson, you should be able to:

- Explain the role of the OSI Network layer and routers.
- Describe the structure of an IPv4 address.
- Convert 8-bit binary values to decimal and decimal values to binary.
- Identify network and host portions using a prefix length.
- Recognize the traditional IPv4 address classes and loopback range.
- Convert common prefix lengths to dotted-decimal subnet masks.
- Identify network and broadcast addresses in a conventional IPv4 subnet.

---

## 1. Layer 3 and routing

The **Network layer (Layer 3)** provides logical addressing and enables communication between different IP networks.

| Layer | Address | Main purpose | Common device |
|---|---|---|---|
| Layer 2 — Data Link | MAC address | Delivers frames across a local link/VLAN | Layer 2 switch |
| Layer 3 — Network | IP address | Moves packets between IP networks | Router |

A switch uses destination MAC addresses to make Layer 2 forwarding decisions. A router examines the destination IP address and its routing table to decide where to send an IP packet next.

### Why are routers needed?

Devices in the same IP subnet can generally communicate through Layer 2 switching. To communicate between different IP networks, traffic is sent to a router or another Layer 3 device that can route between those networks.

Example:

- Network A: `192.168.1.0/24`
- Network B: `192.168.2.0/24`

A router needs an appropriately configured Layer 3 interface in each network to route traffic between them.

### Broadcast boundaries

An Ethernet broadcast uses destination MAC address `FF:FF:FF:FF:FF:FF`. Switches normally flood broadcasts within the relevant VLAN, except the incoming port. Routers do not normally forward local Layer 2 broadcasts between interfaces.

---

## 2. IPv4 address structure

**IPv4 (Internet Protocol version 4)** uses a 32-bit address to identify an interface in an IP network.

An IPv4 address consists of:

- 32 bits in total.
- 4 groups of 8 bits.
- Each 8-bit group is called an **octet**.
- Four decimal numbers separated by dots are called **dotted-decimal notation**.

Example:

```text
192.168.1.254
```

Its binary form is:

```text
11000000.10101000.00000001.11111110
```

Each octet can represent a value from **0 to 255**:

- Lowest value: `00000000` = 0
- Highest value: `11111111` = 255

Remember: 4 octets × 8 bits = 32 bits.

### IPv4 header — basic context

An IPv4 packet has a header that carries information needed to deliver and process the packet. Important fields include:

- **Source IP address:** The IPv4 address of the packet's sender.
- **Destination IP address:** The IPv4 address the packet is being sent to.
- **TTL (Time to Live):** Decreased by routers to prevent packets circulating indefinitely.
- **Protocol:** Identifies the payload protocol, such as TCP or UDP.
- **Header checksum:** Checks the IPv4 header for errors.

The header contains other fields too; this lesson's main focus is IPv4 addressing and how the address is divided into network and host portions.

---

## 3. Number systems

### Decimal — base 10

Decimal uses the digits 0–9. Each position represents a power of 10.

Example:

```text
3294 = (3 × 1000) + (2 × 100) + (9 × 10) + 4
     = 3000 + 200 + 90 + 4
```

### Hexadecimal — base 16

Hexadecimal uses sixteen symbols:

```text
0 1 2 3 4 5 6 7 8 9 A B C D E F
```

| Hex digit | Decimal value |
|---|---:|
| A | 10 |
| B | 11 |
| C | 12 |
| D | 13 |
| E | 14 |
| F | 15 |

Each hexadecimal digit represents 4 bits. Hexadecimal is commonly used to display MAC addresses and packet data compactly.

Example:

```text
CDE₁₆ = (12 × 16²) + (13 × 16) + 14
      = 3072 + 208 + 14
      = 3294₁₀
```

### Binary — base 2

Binary uses only 0 and 1. Each position represents a power of 2.

The eight place values of an IPv4 octet are:

| Bit position | 8 | 7 | 6 | 5 | 4 | 3 | 2 | 1 |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Place value | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |

To convert binary to decimal, add the place values where the bit is 1.

Example:

```text
11000000 = 128 + 64 = 192
```

To convert decimal to binary, determine which place values add up to the decimal number. Write 1 for each used value and 0 for each unused value.

Example:

```text
221 = 128 + 64 + 16 + 8 + 4 + 1
    = 11011101
```

Always write all eight bits for an IPv4 octet. Decimal 1 is `00000001`, not just `1`.

---

## 4. Binary-to-decimal conversion

Use the place values `128 64 32 16 8 4 2 1`. Add only the values whose corresponding bit is 1.

### Example A: `10001111`

```text
128 + 8 + 4 + 2 + 1 = 143
```

Answer: **143**

### Example B: `01110110`

```text
64 + 32 + 16 + 4 + 2 = 118
```

Answer: **118**

### Example C: `11101011`

```text
128 + 64 + 32 + 8 + 2 + 1 = 235
```

Answer: **235**

### Example D: `11101100`

```text
128 + 64 + 32 + 8 + 4 = 236
```

Answer: **236**

### Complete IPv4 example

Convert `11000000.10101000.00000001.11111110`:

| Binary octet | Calculation | Decimal |
|---|---|---:|
| `11000000` | 128 + 64 | 192 |
| `10101000` | 128 + 32 + 8 | 168 |
| `00000001` | 1 | 1 |
| `11111110` | 128 + 64 + 32 + 16 + 8 + 4 + 2 | 254 |

Answer: **`192.168.1.254`**

---

## 5. Decimal-to-binary conversion

Use the same place values: `128 64 32 16 8 4 2 1`.

For each place value, write 1 if it can be included in the remaining number; otherwise write 0.

### Example A: Convert 221

```text
221 = 128 + 64 + 16 + 8 + 4 + 1
221 = 11011101
```

Answer: **`11011101`**

### Example B: Convert 127

```text
127 = 64 + 32 + 16 + 8 + 4 + 2 + 1
127 = 01111111
```

Answer: **`01111111`**

### Example C: Convert 207

```text
207 = 128 + 64 + 8 + 4 + 2 + 1
207 = 11001111
```

Answer: **`11001111`**

### Example D: Convert 192

```text
192 = 128 + 64
192 = 11000000
```

Answer: **`11000000`**

### Example E: Convert 10

```text
10 = 8 + 2
10 = 00001010
```

Answer: **`00001010`**

**Conversion tip:** Memorize the eight place values. Practise converting one octet at a time instead of trying to convert the entire 32-bit address at once.

---

## 6. Network portion and host portion

An IPv4 address has a **network portion** and a **host portion**. The subnet mask or prefix length tells us where the boundary is.

Example:

```text
192.168.1.254/24
```

The `/24` prefix means the first 24 bits are the network portion and the remaining 8 bits are the host portion.

| Address | Network portion | Host portion |
|---|---|---|
| `192.168.1.10/24` | `192.168.1` | `10` |
| `192.168.1.20/24` | `192.168.1` | `20` |
| `192.168.2.10/24` | `192.168.2` | `10` |

The first two addresses belong to the same `192.168.1.0/24` subnet. The third belongs to a different subnet.

### Common prefix lengths

| Prefix | Network bits | Host bits | Boundary |
|---|---:|---:|---|
| `/8` | 8 | 24 | First octet |
| `/16` | 16 | 16 | First two octets |
| `/24` | 24 | 8 | First three octets |

A prefix counts bits, not octets. For example, `/24` means 24 network bits, equivalent to three complete octets.

---

## 7. Traditional IPv4 address classes

The older **classful addressing** system grouped IPv4 addresses into classes based on the first octet. It associated Classes A, B, and C with default network sizes.

| Class | First-octet range | Leading bits | Traditional default prefix | Use |
|---|---:|---|---|---|
| A | 0–127* | `0` | `/8` | Unicast addressing |
| B | 128–191 | `10` | `/16` | Unicast addressing |
| C | 192–223 | `110` | `/24` | Unicast addressing |
| D | 224–239 | `1110` | None | Multicast |
| E | 240–255 | `1111` | None | Reserved/experimental |

*The traditional usable Class A unicast range is generally described as first octets 1–126. The 0/8 range has special uses, and 127/8 is reserved for loopback.

### Class A

- Traditional default mask: `255.0.0.0`
- Prefix: `/8`
- First octet is the network portion under the classful default.
- Remaining three octets are the host portion.

### Class B

- Traditional default mask: `255.255.0.0`
- Prefix: `/16`
- First two octets are the network portion.
- Remaining two octets are the host portion.

### Class C

- Traditional default mask: `255.255.255.0`
- Prefix: `/24`
- First three octets are the network portion.
- Final octet is the host portion.

### Class D and Class E

- **Class D:** Used for multicast addressing.
- **Class E:** Reserved/experimental in the traditional classful scheme; not for normal host assignment.

### Important modern context

Modern IP networks use **CIDR (Classless Inter-Domain Routing)**, which allows flexible prefix lengths such as `/20`, `/27`, or `/30`. Network boundaries are determined by the configured prefix, not by the old class alone. Classful defaults remain useful for understanding historical terminology and exam questions.

---

## 8. Loopback addresses

The IPv4 loopback range is:

```text
127.0.0.0/8
```

It spans `127.0.0.0` through `127.255.255.255`. The commonly used loopback address is:

```text
127.0.0.1
```

Loopback traffic stays on the local device and is useful for testing the local IP networking stack.

Example on Windows:

```powershell
ping 127.0.0.1
```

A successful loopback test does not prove that a physical network interface, cable, Wi-Fi connection, router, or internet connection is working. It tests the local TCP/IP stack.

---

## 9. Subnet masks and prefix lengths

A **subnet mask** identifies the network and host bits of an IPv4 address.

- Network bits are represented by consecutive binary 1s.
- Host bits are represented by consecutive binary 0s.
- The prefix length counts the number of network bits.

### Common default masks

| Traditional class | Prefix | Subnet mask | Network bits | Host bits |
|---|---|---|---:|---:|
| A | `/8` | `255.0.0.0` | 8 | 24 |
| B | `/16` | `255.255.0.0` | 16 | 16 |
| C | `/24` | `255.255.255.0` | 24 | 8 |

### Convert `/24` to dotted decimal

A `/24` mask contains 24 ones followed by 8 zeros:

```text
11111111.11111111.11111111.00000000
```

Each octet converts as follows:

- `11111111` = 255
- `11111111` = 255
- `11111111` = 255
- `00000000` = 0

Mask: **`255.255.255.0`**

### Convert `/16` to dotted decimal

```text
11111111.11111111.00000000.00000000
255.255.0.0
```

Mask: **`255.255.0.0`**

### Convert `/8` to dotted decimal

```text
11111111.00000000.00000000.00000000
255.0.0.0
```

Mask: **`255.0.0.0`**

**Remember:** A subnet mask is not an IP address assigned to a host. It defines which bits of that host's IP address represent the network.

---

## 10. Network addresses

A **network address** identifies the subnet itself. In a conventional IPv4 subnet, all host bits are set to 0 in the network address.

Example:

```text
IP address:  192.168.1.10/24
Subnet mask: 255.255.255.0
Network:     192.168.1.0/24
```

For a `/24`, the final octet is the host portion. Setting the host bits to zero gives `192.168.1.0`.

The network address is not normally assigned to a regular host interface in a conventional subnet.

---

## 11. Broadcast addresses

A **subnet broadcast address** is used to send an IPv4 packet to all hosts in that subnet. In a conventional IPv4 subnet, all host bits are set to 1 in the broadcast address.

For `192.168.1.0/24`, the host portion is the final octet. Setting all eight host bits to 1 gives:

```text
11111111 = 255
```

Therefore, the broadcast address is **`192.168.1.255`**.

### Network vs. broadcast address

| Address type | Host bits | Example in `192.168.1.0/24` |
|---|---|---|
| Network address | All 0 | `192.168.1.0` |
| First usable host | — | `192.168.1.1` |
| Last usable host | — | `192.168.1.254` |
| Broadcast address | All 1 | `192.168.1.255` |

A `/24` contains 256 total addresses. Under conventional subnetting, the network and broadcast addresses are reserved, leaving 254 addresses for host assignment.

### Layer 2 broadcast vs. IPv4 broadcast

These are related but different:

- **Ethernet broadcast MAC:** `FF:FF:FF:FF:FF:FF`
- **IPv4 subnet broadcast:** An IP address with all host bits set to 1, such as `192.168.1.255` for a `/24`.

When an IPv4 subnet-broadcast packet is carried over Ethernet, the Ethernet destination is normally the broadcast MAC address. Switches flood the frame within the VLAN, excluding the incoming port. Routers do not normally forward local subnet broadcasts to other networks.

---

## 12. Worked examples

### Example 1: Identify the network and host portions

Given `192.168.1.25/24`:

- Prefix: `/24`
- Network bits: 24
- Host bits: 8
- Network address: `192.168.1.0`
- Broadcast address: `192.168.1.255`
- Conventional usable host range: `192.168.1.1–192.168.1.254`

### Example 2: Identify the default class and mask

Given `172.16.5.10` under traditional classful addressing:

- First octet: 172
- 172 is within 128–191.
- Traditional class: B
- Default mask: `255.255.0.0` (`/16`)

In modern networks, the actual configured prefix must still be checked; do not assume that every 172.16 address uses `/16`.

### Example 3: Convert the address to binary

Given `10.200.231.91`:

| Decimal | Binary |
|---:|---|
| 10 | `00001010` |
| 200 | `11001000` |
| 231 | `11100111` |
| 91 | `01011011` |

Binary address:

```text
00001010.11001000.11100111.01011011
```

---

## 13. Video quiz — conversion answer key

Use these answers to check your conversion method.

### Binary to dotted decimal

| No. | Binary address | Dotted-decimal answer |
|---:|---|---|
| 1 | `00111111.00111000.11100111.00010011` | `63.56.231.19` |
| 2 | `11110011.01111111.01100010.00000001` | `243.127.98.1` |
| 3 | `01101111.00000110.01011001.11000111` | `111.6.89.199` |
| 4 | `11001111.11000110.00101111.01001100` | `207.198.47.76` |
| 5 | `01100100.11001001.00100001.11111101` | `100.201.33.253` |

### Dotted decimal to binary

| No. | Dotted-decimal address | Binary answer |
|---:|---|---|
| 6 | `88.46.90.91` | `01011000.00101110.01011010.01011011` |
| 7 | `221.234.246.163` | `11011101.11101010.11110110.10100011` |
| 8 | `3.41.143.222` | `00000011.00101001.10001111.11011110` |
| 9 | `10.200.231.91` | `00001010.11001000.11100111.01011011` |
| 10 | `248.87.255.152` | `11111000.01010111.11111111.10011000` |

**Conversion check:** Decimal 221 is `11011101`. Decimal 220 is `11011100`.

---

## 14. Quick revision sheet

### IPv4 facts

- IPv4 address length: 32 bits.
- Four octets per address; each octet has 8 bits.
- Each octet ranges from 0 to 255.
- Binary place values: 128, 64, 32, 16, 8, 4, 2, 1.

### Traditional address classes

| Class | First octet | Default prefix | Default mask |
|---|---|---|---|
| A | 0–127* | `/8` | `255.0.0.0` |
| B | 128–191 | `/16` | `255.255.0.0` |
| C | 192–223 | `/24` | `255.255.255.0` |
| D | 224–239 | — | Multicast |
| E | 240–255 | — | Reserved/experimental |

*Special-use ranges exist; 127/8 is loopback. Modern networks use CIDR prefixes rather than relying on classful defaults.

### Addressing rules

- Prefix length = number of network bits.
- Network address = all host bits set to 0.
- Broadcast address = all host bits set to 1.
- `127.0.0.1` is the commonly used loopback address.
- A router routes packets between IP networks.
- The configured subnet mask/prefix determines the network boundary.

---

## 15. Self-test

1. How many bits are in an IPv4 address? **32 bits.**
2. What is the decimal range of one octet? **0–255.**
3. What is decimal 192 in binary? **`11000000`.**
4. What is decimal 221 in binary? **`11011101`.**
5. What does `/24` mean? **24 network bits and 8 host bits.**
6. What is the default Class B mask in classful addressing? **`255.255.0.0`.**
7. What is the commonly used loopback address? **`127.0.0.1`.**
8. What is the network address for `192.168.1.25/24`? **`192.168.1.0`.**
9. What is the broadcast address for `192.168.1.0/24`? **`192.168.1.255`.**
10. What is the difference between a network address and a broadcast address? **The network address has all host bits set to 0; the broadcast address has all host bits set to 1.**

---

## Final takeaway

IPv4 addressing becomes easier when you understand three fundamentals:

1. An IPv4 address is 32 bits divided into four octets.
2. The subnet mask or prefix length defines the boundary between network bits and host bits.
3. The network address has all host bits set to 0, while the subnet broadcast address has all host bits set to 1.

Practise binary-to-decimal and decimal-to-binary conversion until you can do each octet confidently. These skills are the foundation for the subnetting lessons that follow.

*Study note for CCNA 200-301 preparation.*
