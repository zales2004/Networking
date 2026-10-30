# Day 02 — Interfaces and Cables

> **Course:** Jeremy's IT Lab — Free CCNA 200-301  
> **Video:** [Free CCNA | Interfaces and Cables | Day 2](https://youtu.be/ieTH5lVhNaY)  
> **Learning goal:** Understand Ethernet ports and standards, bits and bytes, copper Ethernet cabling and pinouts, straight-through vs crossover cables, Auto MDI-X, fiber-optic cables, and how to choose an appropriate cable for a network connection.

These notes summarize the concepts covered in the lesson in a structured format for learning, revision, and hands-on practice.

## Contents

1. [Interfaces, ports, RJ-45 and Ethernet](#1-interfaces-ports-rj-45-and-ethernet)
2. [Standards and interoperability](#2-standards-and-interoperability)
3. [Bits, bytes and network speed](#3-bits-bytes-and-network-speed)
4. [Copper Ethernet and IEEE 802.3](#4-copper-ethernet-and-ieee-8023)
5. [UTP cable construction and twisted pairs](#5-utp-cable-construction-and-twisted-pairs)
6. [Straight-through and crossover cables](#6-straight-through-and-crossover-cables)
7. [T568A and T568B pinouts](#7-t568a-and-t568b-pinouts)
8. [MDI, MDIX and Auto MDI-X](#8-mdi-mdix-and-auto-mdi-x)
9. [Gigabit Ethernet pin usage](#9-gigabit-ethernet-pin-usage)
10. [Fiber-optic cabling](#10-fiber-optic-cabling)
11. [Single-mode vs multimode fiber](#11-single-mode-vs-multimode-fiber)
12. [Choosing copper or fiber](#12-choosing-copper-or-fiber)
13. [Practical Packet Tracer lab](#13-practical-packet-tracer-lab)
14. [Troubleshooting checklist](#14-troubleshooting-checklist)
15. [Exam revision sheet](#15-exam-revision-sheet)
16. [Knowledge check and answers](#16-knowledge-check-and-answers)
17. [Glossary](#17-glossary)
---

## 1. Interfaces, ports, RJ-45 and Ethernet

### What is an interface?

An **interface** is the point where a device connects to another device or network. In a wired Ethernet network, the interface is commonly a physical network port.

The words **interface** and **port** are often used interchangeably in this lesson. For example, a switch may have 24 Ethernet ports so that many computers, printers, access points, servers, or other network devices can connect to it.

### What is RJ-45?

In everyday networking language, **RJ-45** commonly refers to the familiar eight-position, eight-contact modular connector used with twisted-pair Ethernet cables. You will see this connector on many PCs, switches, routers, and wall network outlets.

- The **plug** is attached to the end of the cable.
- The **port/jack** is the socket on the device.
- A copper Ethernet cable with the appropriate modular plug connects the two.

Technically, the connector is often more precisely described as an 8P8C modular connector, but RJ-45 is the common networking term.

### What is Ethernet?

**Ethernet** is a family of networking technologies widely used for wired local-area networks (LANs). Ethernet defines how devices send and receive data over a link, including physical-layer specifications and data-link-layer behaviour.

A successful connection requires more than a plug that physically fits. The devices, cable, connectors, signaling and negotiated link capabilities must be compatible.

### Ethernet NIC

A computer typically connects through a **Network Interface Card (NIC)** or built-in network adapter. Its Ethernet port accepts the cable and provides the computer's wired network connection. A switch has many ports so that many end devices can join the same LAN.

**Remember:** a port is the connection point; the cable is the medium; Ethernet is the technology/standard family used to communicate over that connection.

## 1. Standards and interoperability

Network equipment is made by many different vendors. Standards let equipment from different manufacturers work together.

Imagine that one manufacturer made a switch port with one connector shape and another manufacturer made a cable with an incompatible plug. They could not connect. The same issue would arise if two connected devices used incompatible signaling or communication rules.

Standards define agreed requirements such as:

- Connector shape and dimensions.
- Cable and wiring requirements.
- Electrical or optical signaling.
- Supported data rates and link characteristics.
- Rules that let equipment from different manufacturers interoperate.

**Physical standards** cover items such as connectors, cable properties and signaling. **Logical/protocol standards** cover how information is represented and communicated. Ethernet and IP are both standards-based technologies, but they operate at different layers and define different things.

### Why this matters

When selecting a cable, check both ends of the connection: the connector type, the supported Ethernet speed, the medium (copper or fiber), and the interface modules required. A cable that fits physically is not automatically guaranteed to support the desired speed or distance.

## 1. Bits, bytes and network speed

Link speeds are normally stated in **bits per second (bit/s or bps)**.

- **bit (b):** one binary digit, either 0 or 1.
- **byte (B):** commonly 8 bits.
- **Kbit/s, Mbit/s, Gbit/s:** thousands, millions, or billions of bits per second in networking's decimal rate notation.
- **Mbps:** megabits per second.
- **Gbps:** gigabits per second.

Examples:

- 10 Mbps = 10 million bits per second.
- 100 Mbps = 100 million bits per second.
- 1 Gbps = 1,000 Mbps.
- 10 Gbps = 10,000 Mbps.

Because a byte contains 8 bits, a theoretical 100 Mbps link corresponds to at most 12.5 MB/s before accounting for Ethernet/IP/TCP overhead and other real-world limits. **Mbps and MB/s are not the same unit.**

A link's advertised speed is a signaling/data-rate capability, not a promise that an application will achieve that exact throughput. Actual throughput depends on protocol overhead, the remote endpoint, congestion, latency, hardware and other factors.

## 1. Copper Ethernet and IEEE 802.3

A large part of traditional Ethernet LAN cabling uses **twisted-pair copper cable** terminated with modular Ethernet plugs. IEEE 802.3 is the major family of standards associated with Ethernet.

Ethernet names often encode the technology's rate and medium. Common examples include:

| Ethernet name | Nominal rate | Typical copper medium / notes |
|---|---:|---|
| 10BASE-T | 10 Mbps | Twisted-pair copper |
| 100BASE-TX (Fast Ethernet) | 100 Mbps | Twisted-pair copper; commonly uses two pairs |
| 1000BASE-T (Gigabit Ethernet) | 1 Gbps | Twisted-pair copper; uses all four pairs |
| 10GBASE-T | 10 Gbps | Higher-performance twisted-pair copper and compatible equipment required |

The exact cable category, link length, port capability and installation quality determine whether a particular rate is supported. For conventional copper Ethernet horizontal cabling, a common channel limit is up to 100 metres under the applicable cabling specification, but the supported rate depends on the Ethernet standard and cable category.

### Full-duplex communication

Modern switched Ethernet links commonly operate in **full-duplex** mode: each side can transmit and receive at the same time. This is different from older shared, half-duplex Ethernet environments where collisions could occur. Full duplex requires a compatible link configuration at both ends.

## 1. UTP cable construction and twisted pairs

**UTP** means **Unshielded Twisted Pair**. The cable contains insulated copper conductors grouped into pairs, with the two wires in each pair twisted together. A typical Ethernet cable has four pairs, or eight conductors.

Why twist the wires?

- Twisting helps reduce electromagnetic interference and crosstalk.
- The balanced pair design helps the receiver distinguish the intended signal from noise.
- The twists must be preserved as much as practical when terminating the cable; untwisting too much can degrade performance.

**STP** is a broad term commonly used for shielded twisted-pair cable. Shielding can help in particular electrical environments, but it must be selected and installed correctly; shielding does not automatically make every cable run better.

### Cable categories

You may encounter labels such as Cat 5e, Cat 6 and Cat 6A. The category indicates cabling performance requirements, including frequency-related characteristics and support for specified applications when installed correctly. Do not choose a cable solely by appearance: check the category marking and the requirement for the intended Ethernet rate.

## 1. Straight-through and crossover cables

The difference between these cable types is the wiring arrangement at the two ends.

### Straight-through cable

The same wiring standard is used at both ends—for example, T568B on both ends or T568A on both ends. Each conductor's pin position maps straight across to the corresponding pin position at the other end.

Traditionally, straight-through cables are used between unlike Ethernet interface types, such as:

- PC to switch.
- Router Ethernet port to switch.
- Server to switch.
- Printer to switch.

### Crossover cable

A crossover cable uses different pin assignments at its ends so that the relevant transmit and receive pairs are crossed. For classic 10/100 Mbps Ethernet, this usually means crossing pins 1–2 with pins 3–6.

Traditionally, crossover cables were used to connect similar interface types, such as:

- PC to PC directly.
- Switch to switch.
- Hub to hub.
- PC to router Ethernet port directly in classic setups.

The traditional rule is a useful exam concept, but it is not a universal rule for every modern device. Many contemporary Ethernet ports support **Auto MDI-X**, which automatically adjusts for straight-through or crossover wiring.

### Important practical note

Do not memorize “same device = crossover” as an unconditional modern rule. Learn the historical pinout logic, then check whether the connected ports support Auto MDI-X. In modern networks, a correctly made straight-through cable often works for connections that historically required a crossover cable.

## 1. T568A and T568B pinouts

T568A and T568B are two recognized wiring schemes for terminating twisted-pair cabling. The orange and green pairs swap positions between the schemes; the blue and brown pairs stay in the same pin positions.

### Pin order (pins 1 to 8)

| Pin | T568A wire colour | T568B wire colour |
|---:|---|---|
| 1 | White/Green | White/Orange |
| 2 | Green | Orange |
| 3 | White/Orange | White/Green |
| 4 | Blue | Blue |
| 5 | White/Blue | White/Blue |
| 6 | Orange | Green |
| 7 | White/Brown | White/Brown |
| 8 | Brown | Brown |

These colours assume the usual North American T568A/T568B colour code. When reading a plug, make sure you orient it consistently and identify pin 1 correctly; reversing the view can make the sequence appear backwards.

### How they make a cable

- **T568A at both ends:** straight-through.
- **T568B at both ends:** straight-through.
- **T568A at one end and T568B at the other:** crossover.

A crossover does not mean the cable is damaged or randomly wired. It is intentionally terminated with a different standard at each end.

### Why does pair crossing matter?

For classic 10/100BASE-T Ethernet, one pair is used for transmit and another for receive on an MDI interface. A crossover cable connects the transmit pair of one end to the receive pair of the other. A straight-through cable preserves the same pin mapping.

## 1. MDI, MDIX and Auto MDI-X

These terms explain why the historical cable rules exist.

- **MDI (Medium Dependent Interface):** a traditional Ethernet interface pin assignment used by end devices such as PCs and routers.
- **MDIX (Medium Dependent Interface Crossover):** a traditional crossed interface pin assignment commonly associated with hubs and switches.
- **Auto MDI-X:** a feature that automatically detects/adjusts the transmit and receive pair mapping so that a link can work with either straight-through or crossover wiring, where supported.

### What should you remember for the quiz?

If two devices are connected using a cable type that would traditionally be considered wrong, but both ports support Auto MDI-X, the connection can still work normally. Do not assume every port has this feature—older equipment or specific configurations may not.

If a link does not come up, check the interface capabilities and configuration instead of assuming Auto MDI-X is present.

## 1. Gigabit Ethernet pin usage

A key distinction between 10/100BASE-T and 1000BASE-T:

- Traditional 10BASE-T and 100BASE-TX use two pairs for data.
- **1000BASE-T uses all four twisted pairs** for transmission and reception.
- The exact signaling is more sophisticated than simply dedicating one pair to transmit and one to receive; all four pairs participate in bidirectional communication.

This matters when making or troubleshooting a cable. A cable may appear to work at 100 Mbps but fail to establish Gigabit Ethernet if one or more pairs are damaged or terminated incorrectly. A cable tester and the device's negotiated link speed can help identify such issues.

**Do not confuse “uses all four pairs” with “has four wires.”** Four pairs means eight individual conductors.

## 1. Fiber-optic cabling

Fiber-optic cable carries information as **pulses of light**, rather than electrical signals through copper conductors.

A fiber link generally needs compatible optical interfaces or transceivers at both ends. On network switches and routers, these may be pluggable modules such as SFP or SFP+ modules, depending on the device and data rate. The connector, optical wavelength, fiber type and transceiver must be compatible.

### Why use fiber?

- Supports high data rates over appropriate distances.
- Has very low susceptibility to electromagnetic interference.
- Provides electrical isolation between the connected endpoints.
- Is useful for longer links and network backbone connections.

### Things to consider

- Fiber transceivers and ports can cost more than ordinary copper Ethernet ports.
- Fiber must be handled and installed carefully; dirty end faces, tight bends or physical damage can cause link problems.
- Do not look into the end of a fiber or optical transceiver. Invisible optical light may be present.
- A fiber patch cable must match the optical interface and fiber type being used.

## 1. Single-mode vs multimode fiber

The two major fiber categories discussed are **single-mode fiber (SMF)** and **multimode fiber (MMF)**.

### Single-mode fiber (SMF)

- Has a very small core.
- Supports one primary propagation mode.
- Commonly uses laser-based optics.
- Is suited to longer-distance links, including campus, metropolitan and provider links, when paired with the correct optics.
- Often costs more as a complete link because the optics and deployment requirements may be more expensive.

### Multimode fiber (MMF)

- Has a larger core than single-mode fiber.
- Allows multiple propagation modes.
- Commonly uses optics designed for shorter-distance links.
- Is widely used for shorter building or data-centre connections.
- Can be a cost-effective choice for short-to-medium distances with compatible equipment.

### Simple comparison

| Feature | Single-mode fiber | Multimode fiber |
|---|---|---|
| Core | Smaller | Larger |
| Light propagation | One primary mode | Multiple modes |
| Typical distance use | Longer links | Shorter links |
| Optics | Often laser optics designed for SMF | Optics designed for MMF |
| Main selection rule | Match the link's distance and optics | Match the link's distance and optics |

Distance is not determined by the fiber label alone. The supported rate and maximum reach depend on the exact Ethernet optical standard, transceiver, wavelength, fiber specification and link budget. Always verify the manufacturer's specifications.

## 1. Choosing copper or fiber

There is no single best medium for every network link. Choose based on endpoints, distance, rate, environment, cost and available ports.

| Consideration | UTP copper Ethernet | Fiber Ethernet |
|---|---|---|
| Common endpoint connection | PCs, printers, access points and ordinary switch ports | Switch uplinks, backbones, servers and longer links |
| Connector/interface | Commonly RJ-45-style port | Optical connector and compatible optical transceiver/port |
| Typical cost | Usually inexpensive for short endpoint links | Often higher transceiver and installation cost |
| Electromagnetic interference | More susceptible than fiber | Immune to electromagnetic interference in the optical medium |
| Distance | Common copper channel limit is often 100 m for supported structured cabling | Can support longer distances, depending on fiber and optics |
| Power over Ethernet | Can carry PoE when the equipment and cable support it | Standard fiber itself does not carry electrical PoE power |
| Compatibility | Must match port speed and cable category | Must match fiber type, connector, wavelength, transceiver and Ethernet standard |

### The final quiz scenario

If a company needs to connect many end hosts to a switch located in a wiring cabinet on the same office floor, the expected answer is **UTP copper Ethernet**. It is the standard practical choice for ordinary host-to-switch connections: end hosts commonly have RJ-45 Ethernet ports, switches have many RJ-45 access ports, and the cable is cost-effective for typical in-building distances.

Single-mode and multimode fiber are not the default answer for ordinary endpoint drops in this scenario, although fiber can be appropriate where distance, bandwidth, interference or network design requires it.

## 1. Practical Packet Tracer lab

**Goal:** practise identifying interfaces and selecting a cable. Packet Tracer's device models may simplify physical details, so use it to learn the connection workflow and link-state checks.

### Lab A — PC-to-switch-to-PC

1. Open Cisco Packet Tracer and start a blank workspace.
2. Add two PCs and one switch.
3. Connect PC0's FastEthernet/GigabitEthernet interface to a switch Ethernet port.
4. Connect PC1's Ethernet interface to another switch port.
5. Select a copper straight-through cable for each PC-to-switch connection.
6. Wait for the links to become active; the LEDs may take a little time to turn green.
7. Open each PC's **Desktop → IP Configuration** and assign addresses in the same subnet, for example:
   - PC0: `192.168.10.10`, mask `255.255.255.0`
   - PC1: `192.168.10.11`, mask `255.255.255.0`
8. From PC0, open **Desktop → Command Prompt** and run `ping 192.168.10.11`.
9. Confirm the replies. If the first ping times out, retry; initial ARP resolution can affect the first test.

### Lab B — Compare cable types

1. Save a copy of Lab A before changing anything.
2. Replace one PC-to-switch connection with a crossover cable.
3. Observe whether the link becomes active. A Packet Tracer switch model may support Auto MDI-X, so a crossover cable may work too.
4. Record the result and the exact device/interface models you used.
5. Explain why the traditional recommendation is straight-through for PC-to-switch even if the crossover also works on a modern/Auto MDI-X-enabled link.

### Lab C — Compare a direct PC connection

1. Place two PCs in the workspace.
2. Connect them directly using a copper crossover cable as the traditional choice.
3. Give them addresses in the same subnet, such as `192.168.20.10/24` and `192.168.20.11/24`.
4. Test with `ping`.
5. If testing a straight-through cable as well, note the behaviour of the Packet Tracer device models rather than assuming all real equipment behaves the same way.

### What to document in the repository

- A screenshot of each topology.
- The device names and interface names.
- Which cable type you chose and why.
- The IP addresses used for the ping test.
- The observed link state and ping result.
- Any troubleshooting steps needed.

## 1. Troubleshooting checklist

If an Ethernet link does not come up, check:

1. **Correct interfaces:** Are you connecting Ethernet to Ethernet rather than selecting a console or another interface?
2. **Cable and connector:** Is the cable type supported, correctly inserted and undamaged?
3. **Pair termination:** Are the wires terminated consistently, and is the correct pinout used?
4. **Port status:** Is the interface administratively disabled or shut down?
5. **Speed/duplex:** Are the ends compatible, and is auto-negotiation configured appropriately?
6. **Auto MDI-X:** Does the port actually support it? Do not assume.
7. **Fiber compatibility:** For fiber, verify SMF/MMF, transceiver type, wavelength, connector and supported rate.
8. **Physical distance and quality:** Is the cable within the standard's specified reach and installed correctly?
9. **Layer 3 settings:** If the link is up but ping fails, check IP addresses, subnet masks, firewall settings and whether the devices should be able to communicate.
10. **Test method:** Retry a first ping if address resolution may be needed; distinguish a physical-link problem from an IP configuration problem.

## 1. Exam revision sheet

- **Interface/port:** connection point on a device.
- **RJ-45 (common term):** familiar modular connector used for twisted-pair Ethernet.
- **Ethernet:** widely used wired LAN technology family; IEEE 802.3 standards.
- **Bit vs byte:** 8 bits = 1 byte; bps is bits per second, B/s is bytes per second.
- **Straight-through:** same wiring scheme at both ends (A–A or B–B).
- **Crossover:** T568A at one end and T568B at the other in the common Ethernet patch-cable example.
- **Traditional PC-to-switch:** straight-through.
- **Traditional switch-to-switch or PC-to-PC:** crossover in classic Ethernet scenarios.
- **Auto MDI-X:** can automatically adapt to straight-through or crossover wiring when supported.
- **100BASE-TX:** commonly uses two pairs.
- **1000BASE-T:** uses all four pairs.
- **UTP:** inexpensive, common copper medium for end-host connections.
- **Fiber:** light-based, EMI-immune medium; useful for longer links and uplinks.
- **SMF:** smaller core, typically longer-distance applications.
- **MMF:** larger core, typically shorter-distance applications.
- **Same office floor, many PCs to switch:** UTP is the expected choice for ordinary endpoint cabling.
- **Always match:** cable, connector, interface, transceiver and Ethernet standard.

## 1. Knowledge check and answers

Try answering before revealing the answers.

1. What is the difference between a port and a cable?
2. What does RJ-45 commonly refer to in Ethernet networking?
3. Why do networking vendors follow standards?
4. How many bits are in a byte?
5. Is 100 MB/s the same as 100 Mbps?
6. What is the difference between a straight-through and a crossover cable?
7. Which wiring schemes at the two ends produce a straight-through cable?
8. Which wiring schemes at the two ends produce a crossover cable?
9. What does Auto MDI-X do?
10. How many twisted pairs does a typical four-pair Ethernet cable contain, and how many conductors is that?
11. Which pairs are used by traditional 100BASE-TX, and how many pairs does 1000BASE-T use?
12. Why might a cable support a 100 Mbps link but fail to support Gigabit Ethernet?
13. What does UTP stand for?
14. What carries the signal through fiber-optic cable?
15. What is the general difference between single-mode and multimode fiber?
16. Why is fiber useful near sources of strong electromagnetic interference?
17. What should be checked when selecting an optical transceiver?
18. For many PCs connected to a switch in a same-floor wiring cabinet, what is the expected cable choice?
19. If a link light is on but ping fails, does that automatically mean the cable is faulty?
20. Why should you avoid assuming every modern or older Ethernet port supports Auto MDI-X?

### Answers

1. A port/interface is the connection point on a device; a cable is the medium connecting two interfaces.
2. The common name for the eight-position modular connector used with twisted-pair Ethernet.
3. To ensure compatible equipment, cables, connectors and communication methods can interoperate.
4. Eight.
5. No. MB/s means megabytes per second; Mbps means megabits per second. Ignoring overhead, 100 MB/s equals 800 Mbps.
6. Straight-through preserves the same pin mapping; crossover crosses the relevant transmit/receive pair mapping.
7. T568A at both ends or T568B at both ends.
8. T568A at one end and T568B at the other.
9. It automatically adjusts the transmit/receive mapping so either cable type can work on supported interfaces.
10. Four pairs and eight conductors.
11. 100BASE-TX commonly uses two pairs; 1000BASE-T uses all four.
12. One or more pairs may be damaged or incorrectly terminated, or the cable may not meet the required category/performance.
13. Unshielded Twisted Pair.
14. Pulses of light.
15. Single-mode has a smaller core and is typically used for longer reaches; multimode has a larger core and is typically used for shorter reaches.
16. The optical medium is not affected by electromagnetic interference like copper signaling.
17. Fiber type, connector, wavelength, data rate, supported distance and compatibility with both endpoint ports.
18. UTP copper Ethernet.
19. No. The link can be up while IP settings, firewall rules, endpoint state or other configuration prevents ping.
20. Support varies by equipment and configuration; verify port capabilities rather than relying on assumptions.

## 1. Glossary

| Term | Meaning |
|---|---|
| Ethernet | Wired LAN technology family standardized primarily through IEEE 802.3 |
| Interface / port | A connection point on a network device |
| RJ-45 | Common networking name for the modular connector used with twisted-pair Ethernet |
| NIC | Network Interface Card/controller |
| IEEE 802.3 | Ethernet standards family |
| bit | Binary digit, 0 or 1 |
| byte | Eight bits |
| bps | Bits per second |
| UTP | Unshielded Twisted Pair |
| STP | Common shorthand for shielded twisted-pair cabling |
| T568A / T568B | Standard conductor colour/pin assignment schemes |
| Straight-through | Same wiring scheme at both ends |
| Crossover | Different A/B wiring schemes at the two ends, crossing the relevant pairs |
| MDI | Medium Dependent Interface |
| MDIX | Medium Dependent Interface Crossover |
| Auto MDI-X | Automatic transmit/receive pair mapping adjustment, if supported |
| Full duplex | Simultaneous transmission and reception |
| Fiber-optic cable | Cable that carries information as light |
| SMF | Single-mode fiber |
| MMF | Multimode fiber |
| SFP / SFP+ | Common families of pluggable network transceiver form factors |
| Transceiver | Device/module that transmits and receives signals |
| EMI | Electromagnetic interference |

---

## Completion checklist

- [ ] I can identify Ethernet ports and RJ-45 connectors.
- [ ] I can explain why standards matter.
- [ ] I can convert between bits and bytes and distinguish Mbps from MB/s.
- [ ] I can explain straight-through vs crossover wiring.
- [ ] I can recognize T568A and T568B pinouts.
- [ ] I understand MDI, MDIX and Auto MDI-X.
- [ ] I know 1000BASE-T uses all four pairs.
- [ ] I can compare UTP copper with fiber.
- [ ] I can distinguish single-mode from multimode fiber.
- [ ] I completed the Packet Tracer lab and documented the results.
- [ ] I answered the knowledge-check questions without looking at the answer key.
