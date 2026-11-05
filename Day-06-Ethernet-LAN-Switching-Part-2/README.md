# Day 06 — Ethernet LAN Switching (Part 2)

> **Course:** CCNA 200-301  
> **Topic:** Ethernet frame size, ARP, ping, Wireshark, and switch MAC address tables  
> **Purpose:** Simple reference notes for understanding how devices find each other on a LAN and how switches forward traffic.

## Learning objectives

By the end of this lesson, you should be able to:

- Explain Ethernet's minimum frame size and padding.
- Explain why ARP is needed and how an ARP request/reply works.
- Distinguish an ARP table from a switch MAC address table.
- Explain what happens when you ping another device on the LAN.
- Identify ARP and ICMP packets in Wireshark.
- View and clear dynamic MAC address-table entries on a Cisco switch.
- Recognize common EtherType values.

---

## 1. Ethernet frame size

An Ethernet frame has a minimum size. If the data being carried is too small, Ethernet adds extra bytes called **padding**.

### Ethernet II frame fields

| Field | Size | Purpose |
|---|---:|---|
| Destination MAC | 6 bytes | MAC address of the intended Layer 2 receiver |
| Source MAC | 6 bytes | MAC address of the sender |
| EtherType | 2 bytes | Identifies the protocol carried in the payload |
| Data and padding | 46–1500 bytes | Carries the upper-layer data and, when needed, padding |
| FCS | 4 bytes | CRC value used to detect transmission errors |

- Ethernet header = 14 bytes (6 + 6 + 2).
- FCS/trailer = 4 bytes.
- Minimum Ethernet frame = **64 bytes**, counted from the destination MAC field through the FCS.
- Traditional maximum untagged Ethernet frame = **1518 bytes**, excluding preamble and SFD.
- Preamble (7 bytes) and Start Frame Delimiter/SFD (1 byte) are sent before the frame and are not included in the 64-byte minimum.

### Why is padding needed?

The minimum payload area is 46 bytes. If the actual payload is smaller, padding is added until the payload area reaches 46 bytes.

Example:

- Actual payload = 34 bytes.
- Required payload area = 46 bytes.
- Padding = 46 − 34 = **12 bytes**.

Another example:

- Actual payload = 36 bytes.
- Padding = 46 − 36 = **10 bytes**.

Padding is not meaningful application data. It is added to meet Ethernet's minimum frame size.

---

## 2. EtherType

The 2-byte EtherType field tells the receiver which protocol is carried inside the Ethernet frame.

| EtherType | Protocol |
|---|---|
| `0x0800` | IPv4 |
| `0x0806` | ARP |
| `0x86DD` | IPv6 |

The prefix `0x` means the value is written in hexadecimal.

**Remember:** EtherType helps the receiving device know how to process the payload.

---

## 3. Why do we need ARP?

**ARP (Address Resolution Protocol)** helps a device discover the MAC address associated with a known IPv4 address on the local network.

A device may know the destination's IP address but still need its MAC address to build an Ethernet frame.

For example:

- PC-A IP address: `192.168.1.10`
- PC-B IP address: `192.168.1.20`
- PC-A wants to send data to PC-B.
- PC-A knows PC-B's IP address, but does not yet know PC-B's MAC address.

PC-A uses ARP to find it.

### Important local-network rule

- If the destination IP is in the same subnet, the sender resolves the destination device's MAC address.
- If the destination IP is in a different network, the sender sends the traffic to its **default gateway** and resolves the gateway's MAC address instead.

ARP is used to resolve a local-link IPv4 next-hop address. It does not discover the MAC address of a remote internet server.

---

## 4. How an ARP request and reply work

ARP normally uses two messages:

1. **ARP Request:** The sender asks, “Who has this IPv4 address? Tell me your MAC address.”
2. **ARP Reply:** The device that owns the requested IPv4 address responds with its MAC address.

### ARP request

An ARP request is sent in an Ethernet frame with the broadcast destination MAC:

`FF:FF:FF:FF:FF:FF`

This means all devices in the local Layer 2 broadcast domain receive the request. Each device checks the target IP address. The device that owns that IP responds.

### ARP reply

The device with the matching IP address sends an ARP reply containing its MAC address. The reply is usually unicast to the requesting device.

### Example

PC-A wants to contact PC-B:

| Device | IPv4 address | MAC address |
|---|---|---|
| PC-A | 192.168.1.10 | AA:AA:AA:AA:AA:AA |
| PC-B | 192.168.1.20 | BB:BB:BB:BB:BB:BB |

Sequence:

1. PC-A checks its ARP cache for `192.168.1.20`.
2. No entry exists, so PC-A broadcasts an ARP request.
3. The switch receives the frame and learns PC-A's source MAC address on the incoming port.
4. The switch floods the broadcast to eligible ports in the VLAN, except the port where it arrived.
5. PC-B recognizes its IP address in the ARP request.
6. PC-B sends an ARP reply containing its MAC address.
7. The switch learns PC-B's source MAC address from the reply.
8. PC-A stores the IP-to-MAC mapping in its ARP cache.
9. PC-A can now send the Ethernet frame to PC-B's MAC address.

**Key point:** ARP request = broadcast; ARP reply = usually unicast.

---

## 5. ARP table vs. MAC address table

These tables are different and belong to different devices/functions.

| Table | What it maps | Usually found on |
|---|---|---|
| ARP table/cache | IPv4 address → MAC address | PCs, servers, routers |
| MAC address table | MAC address → switch port and VLAN | Ethernet switches |

### ARP table example

| IPv4 address | MAC address |
|---|---|
| 192.168.1.20 | BB:BB:BB:BB:BB:BB |

The computer uses this information to create the Ethernet frame.

### Switch MAC table example

| VLAN | MAC address | Port |
|---:|---|---|
| 1 | AA:AA:AA:AA:AA:AA | Fa0/1 |
| 1 | BB:BB:BB:BB:BB:BB | Fa0/2 |

The switch uses this information to decide which port should receive a frame.

### Useful commands

On Windows or many operating systems:

```bash
arp -a
```

On Cisco IOS:

```text
show arp
```

ARP entries can be dynamic or static. Dynamic entries are learned automatically; static entries are manually configured.

---

## 6. Ping and ICMP

**Ping** is a basic tool used to test IP reachability between devices. It uses **ICMP (Internet Control Message Protocol)**.

A normal ping exchange uses:

- **ICMP Echo Request:** Sent by the device testing connectivity.
- **ICMP Echo Reply:** Sent back by the destination if it receives and responds to the request.

Example:

```bash
ping 192.168.1.20
```

A successful reply indicates that the destination responded to the ICMP Echo Request. A failed ping does not always mean the device is offline; a firewall may block ICMP, or another network issue may prevent a reply.

### What happens during the first ping on a LAN?

If the sender does not know the destination MAC address yet:

1. It sends an ARP request to discover the local destination's MAC address.
2. It receives the ARP reply and caches the mapping.
3. It sends the ICMP Echo Request inside an Ethernet frame.
4. The destination sends an ICMP Echo Reply.
5. The sender receives the reply.

The first ping can sometimes time out while ARP resolution is taking place. This is one possible reason, not the only cause of a failed first ping.

---

## 7. Inspect ARP and ping traffic with Wireshark

Wireshark captures and displays network packets so you can inspect what is happening.

For this lesson, look for:

- **ARP:** Address resolution requests and replies.
- **ICMP:** Ping Echo Requests and Echo Replies.

Useful Wireshark display filters:

| Filter | Shows |
|---|---|
| `arp` | ARP packets |
| `icmp` | ICMP packets |
| `eth.type == 0x0806` | Ethernet frames carrying ARP |
| `eth.type == 0x0800` | Ethernet frames carrying IPv4 |

### What to inspect in an ARP request

- Ethernet destination: `ff:ff:ff:ff:ff:ff` (broadcast).
- Sender IP and sender MAC.
- Target IP being searched for.
- The request asks which device owns the target IP.

### What to inspect in an ARP reply

- The sender/answering device's IP and MAC.
- The reply provides the MAC address requested by the original sender.
- The Ethernet destination is normally the requesting device's MAC address.

### What to inspect in a ping capture

- ICMP Echo Request from the source IP to the destination IP.
- ICMP Echo Reply from the destination IP back to the source IP.
- Ethernet source and destination MAC addresses used on that local link.

**Important distinction:** IP addresses identify the Layer 3 source and destination. MAC addresses identify the Layer 2 sender and next-hop receiver on the local link. When traffic crosses a router, the Ethernet MAC addresses change at each routed link; the IP source and destination usually remain the same, unless NAT is involved.

---

## 8. View the switch MAC address table

A switch learns MAC addresses from the **source MAC address** of frames it receives. It associates each learned address with a VLAN and the port where the frame arrived.

On a Cisco switch, use:

```text
show mac address-table
```

Some older IOS versions also accept:

```text
show mac-address-table
```

The output commonly includes:

| Column | Meaning |
|---|---|
| VLAN | VLAN in which the MAC address was learned |
| MAC Address | Learned Layer 2 address |
| Type | How the entry was learned, such as dynamic or static |
| Ports | Interface associated with the MAC address |

### Dynamic MAC learning

- A switch learns a source MAC address when it receives a frame.
- It records the VLAN and incoming port.
- Dynamic entries can age out after a period without activity.
- The aging timer varies by switch and configuration; do not assume one fixed value.

### Clear dynamic MAC address entries

Use these commands in a lab or when authorized by the network administrator.

Clear all dynamic entries:

```text
clear mac address-table dynamic
```

Clear a specific MAC address:

```text
clear mac address-table dynamic address 0011.2233.4455
```

Clear dynamic entries learned on a specific interface:

```text
clear mac address-table dynamic interface gigabitEthernet 0/1
```

Then verify the table:

```text
show mac address-table
```

**Caution:** Clearing dynamic entries affects the switch's learned forwarding information. Use these commands only in a lab or with authorization, because the switch must relearn addresses from subsequent traffic.

---

## 9. Putting it all together: a PC pings another PC

Imagine PC-A pings PC-B on the same LAN, and PC-A has no ARP entry for PC-B yet.

1. PC-A knows PC-B's IPv4 address.
2. PC-A checks its ARP cache and finds no matching entry.
3. PC-A broadcasts an ARP Request.
4. The switch learns PC-A's source MAC and floods the broadcast within the VLAN.
5. PC-B sends an ARP Reply with its MAC address.
6. The switch learns PC-B's source MAC from the reply.
7. PC-A stores PC-B's IP-to-MAC mapping.
8. PC-A sends an ICMP Echo Request in an Ethernet frame addressed to PC-B's MAC.
9. The switch looks up the destination MAC and forwards the frame to the correct port if the entry is known.
10. PC-B sends an ICMP Echo Reply.
11. The switch forwards the reply toward PC-A.

This sequence demonstrates how ARP, ICMP, Ethernet frames, and switch MAC learning work together.

---

## 10. Practical lab checklist

Use Packet Tracer or a small lab with two PCs and one switch.

1. Connect both PCs to the switch.
2. Configure both PCs with IPv4 addresses in the same subnet, for example:
   - PC-A: `192.168.1.10/24`
   - PC-B: `192.168.1.20/24`
3. Clear or inspect the ARP cache before testing, if your simulator/OS allows it.
4. Ping PC-B from PC-A.
5. Observe the ARP request and reply in Simulation Mode or Wireshark.
6. On the switch, run `show mac address-table`.
7. Identify the MAC addresses and ports learned by the switch.
8. Ping again and compare the traffic with the first attempt.

### Questions to ask yourself

- Why did PC-A need ARP before sending the first IP packet?
- Why was the ARP request broadcast?
- Why was the ARP reply usually unicast?
- What did the switch learn from the ARP request?
- What did it learn from the ARP reply?
- How is the ARP cache different from the switch MAC table?

---

## 11. Quick revision

| Concept | Remember |
|---|---|
| Ethernet minimum frame | 64 bytes, destination MAC through FCS |
| Ethernet header | 14 bytes |
| FCS | 4 bytes; detects errors using CRC |
| Minimum payload area | 46 bytes; padding is added if needed |
| Preamble + SFD | 7 + 1 bytes; excluded from minimum frame size |
| EtherType IPv4 | `0x0800` |
| EtherType ARP | `0x0806` |
| EtherType IPv6 | `0x86DD` |
| ARP purpose | Find a MAC address for a known local IPv4 next hop |
| ARP request | Broadcast |
| ARP reply | Usually unicast |
| ARP table | IPv4 address → MAC address |
| Switch MAC table | MAC address → VLAN/port |
| Ping | Uses ICMP Echo Request and Echo Reply |
| View ARP cache | `arp -a` |
| View Cisco ARP table | `show arp` |
| View switch MAC table | `show mac address-table` |
| Clear dynamic MAC entries | `clear mac address-table dynamic` |

## 12. Knowledge check

Try answering before looking back at the notes.

1. **Why does Ethernet add padding?**  
   To make the payload area at least 46 bytes and meet the 64-byte minimum frame size.

2. **What is the destination MAC of an ARP request?**  
   `FF:FF:FF:FF:FF:FF`.

3. **What does ARP help a host discover?**  
   The MAC address associated with a known local IPv4 next-hop address.

4. **What is the difference between an ARP table and a switch MAC table?**  
   ARP maps IPv4 addresses to MAC addresses; a switch MAC table maps MAC addresses to VLANs and ports.

5. **Which protocol does ping use?**  
   ICMP.

6. **What are the two main ICMP messages used by ping?**  
   Echo Request and Echo Reply.

7. **Which command displays a Cisco switch's MAC address table?**  
   `show mac address-table`.

8. **What does EtherType `0x0806` identify?**  
   ARP.

9. **Why might the first ping fail while later pings work?**  
   ARP resolution may still be taking place, although other causes are possible.

10. **What does the switch learn from an incoming frame?**  
    The source MAC address, associated with the incoming VLAN and port.

---

## Final takeaway

Remember these four ideas:

1. **Ethernet padding** makes sure a frame meets the minimum size.
2. **ARP** discovers the MAC address of a local IPv4 next hop.
3. **ICMP ping** tests reachability using Echo Request and Echo Reply messages.
4. **A switch learns source MAC addresses** and uses its MAC address table to forward frames.

These concepts are essential for understanding LAN communication, packet captures, and basic network troubleshooting.

*Study note for CCNA 200-301 preparation.*
