# Day 03 — The TCP/IP Model and OSI Reference Model

> **Course:** Jeremy's IT Lab — Free CCNA 200-301  
> **Video:** [Free CCNA | How the TCP/IP Model Actually Works | Day 3](https://youtu.be/yM-XNq9ADlI)  
> **Purpose:** Understand how network communication is divided into layers, how data moves through those layers, and how the TCP/IP and OSI models help explain and troubleshoot communication.

These notes are written as a beginner-friendly study guide for learning, revision, and practical networking. They follow the lesson's core concepts and add clarifications and examples where useful.

## Contents

1. [Learning objectives](#1-learning-objectives)
2. [Protocols and standards](#2-protocols-and-standards)
3. [A short history of TCPIP](#3-a-short-history-of-tcpip)
4. [Organizations that develop standards](#4-organizations-that-develop-standards)
5. [Why networking uses layers](#5-why-networking-uses-layers)
6. [The five-layer TCPIP model](#6-the-five-layer-tcpip-model)
7. [Layer 1 — Physical](#7-layer-1--physical)
8. [Layer 2 — Local network or Data Link](#8-layer-2--local-network-or-data-link)
9. [Layer 3 — Internet or Network](#9-layer-3--internet-or-network)
10. [Layer 4 — Transport](#10-layer-4--transport)
11. [Layer 5 — Application](#11-layer-5--application)
12. [A client-to-server example](#12-a-client-to-server-example)
13. [Encapsulation and decapsulation](#13-encapsulation-and-decapsulation)
14. [Protocol Data Units (PDUs)](#14-protocol-data-units-pdus)
15. [Adjacent-layer and same-layer interaction](#15-adjacent-layer-and-same-layer-interaction)
16. [The seven-layer OSI model](#16-the-seven-layer-osi-model)
17. [TCPIP vs OSI](#17-tcpip-vs-osi)
18. [Troubleshooting with layers](#18-troubleshooting-with-layers)
19. [Common mistakes](#19-common-mistakes)
20. [Revision sheet](#20-revision-sheet)
21. [Knowledge check](#21-knowledge-check)
22. [Glossary](#22-glossary)

---

## 1. Learning objectives

By the end of this lesson, you should be able to:

- Explain what a protocol and a standard are.
- Identify the roles of IEEE and IETF.
- Name the five layers in the model used in this lesson.
- Describe the main responsibility of each layer.
- Distinguish MAC addresses, IP addresses, and transport port numbers.
- Explain encapsulation, decapsulation, payloads, and PDUs.
- Describe adjacent-layer and same-layer interaction.
- Map the five-layer TCP/IP model to the seven-layer OSI model.
- Use layers as a starting framework for troubleshooting.

## 2. Protocols and standards

### What is a protocol?

A **protocol** is a set of rules that allows devices or software to communicate. A protocol can define message formats, addressing, message order, and how a device should respond to particular events.

Examples:

| Protocol | Main purpose |
|---|---|
| HTTP | Defines web request and response communication |
| HTTPS | HTTP secured with TLS |
| TCP | Reliable, connection-oriented transport |
| UDP | Connectionless transport with low protocol overhead |
| IP | Addresses and forwards packets between networks |
| Ethernet | Defines communication over Ethernet LANs |
| Wi-Fi | Defines wireless LAN communication |
| DNS | Resolves domain names to information such as IP addresses |
| ICMP | Network-layer control and diagnostic messages |

### What is a standard?

A **standard** is an agreed specification for a technology or protocol. Standards allow devices from different manufacturers to interoperate when they implement compatible specifications.

Without shared standards, a computer from one manufacturer might not communicate reliably with a switch or server from another manufacturer.

### Open and proprietary specifications

- **Open standard:** A published specification that different vendors can implement.
- **Proprietary technology:** Technology controlled by a particular vendor or organization, sometimes with implementation restrictions.

Open standards have been important to the growth of interoperable networks and the Internet.

## 3. A short history of TCP/IP

The lesson briefly introduces networking history to explain how TCP/IP became central to modern networking.

- **1960s:** Research into computer networking expanded, including work supported by the US Advanced Research Projects Agency (ARPA).
- **1969:** ARPANET connected computers at research institutions. Its early host protocol was NCP (Network Control Program).
- **1970s:** Vint Cerf and Bob Kahn contributed to the development of internetworking concepts and TCP/IP.
- **January 1, 1983:** ARPANET transitioned to TCP/IP, a major milestone in the adoption of the protocol suite.

TCP/IP became widely used because it could connect different networks and operate over different underlying link technologies.

## 4. Organizations that develop standards

### IEEE — Institute of Electrical and Electronics Engineers

IEEE develops standards for many electrical, electronic, and networking technologies.

Two important examples for CCNA are:

- **IEEE 802.3:** Ethernet.
- **IEEE 802.11:** Wireless LAN (Wi-Fi).

### IETF — Internet Engineering Task Force

The IETF is an open community that develops many Internet protocols and related specifications. These specifications are commonly published as **RFCs**, or *Requests for Comments*.

Examples of technologies documented through IETF standards include IP, TCP, UDP, and many application protocols.

**Memory aid:** IEEE → Ethernet and Wi-Fi standards. IETF → Internet protocols and RFCs.

## 5. Why networking uses layers

Sending data over a network requires many tasks: creating application data, identifying the destination application, addressing the destination host, delivering data over local links, and transmitting signals.

A **layered model** separates these tasks into manageable groups. Each layer has defined responsibilities and uses services from the layer below it.

Benefits of layering:

- **Modularity:** Each layer focuses on a specific set of tasks.
- **Interoperability:** Devices can communicate when they follow compatible protocols.
- **Flexibility:** A technology at one layer can change without necessarily requiring changes to every other layer.
- **Troubleshooting:** Engineers can organize checks by function.
- **Learning and design:** Complex communication becomes easier to describe.

### Postal-service analogy

Imagine mailing a letter:

- The message inside is the information you want to communicate.
- The intended recipient is like the application or service that should receive the data.
- The destination address identifies the destination location.
- Delivery services move the letter between locations.
- Roads and vehicles provide the physical means of transport.

Likewise, networking layers divide application communication, transport, logical addressing, local-link delivery, and physical transmission into separate responsibilities. The analogy is useful for understanding the concept, though it is not a literal description of networking.

## 6. The five-layer TCP/IP model

This lesson uses a five-layer model. Learn the layers from **Layer 1 (bottom) to Layer 5 (top)**:

| Layer | Name used in this lesson | Common equivalent name | Main responsibility |
|---:|---|---|---|
| 5 | Application | Application | Application protocols and network services |
| 4 | Transport | Transport | Communication between application processes |
| 3 | Internet | Network | IP addressing and forwarding between networks |
| 2 | Local network | Data Link | Frames and delivery over a local link |
| 1 | Physical | Physical | Transmission of bits as physical signals |

Some textbooks call Layer 3 the **Network layer** and Layer 2 the **Data Link layer**. These are common names for the corresponding responsibilities.

A simple way to remember the main jobs:

- **Application:** What network service or message is being used?
- **Transport:** Which application process should receive the data?
- **Internet/Network:** Which host/network is the destination?
- **Local network/Data Link:** How does data get across this local link?
- **Physical:** How are the bits transmitted?

## 7. Layer 1 — Physical

The **Physical layer** transmits and receives bits over a physical medium.

It deals with the physical representation of bits and the means used to transmit them, such as:

- Electrical signals on copper cables.
- Optical signals on fiber-optic cables.
- Radio signals over Wi-Fi.
- Physical media, connectors, and signal characteristics.
- Transmission rates and other physical characteristics.

A **NIC (Network Interface Card)** provides a device with a network interface. It may be integrated into the computer or added as a separate adapter.

**Example:** On a wired Ethernet connection, bits are transmitted as electrical signals over copper or as optical signals over fiber. On Wi-Fi, data is transmitted using radio signals.

**Think:** Bits, signals, cables, fiber, radio, and physical connectivity.

## 8. Layer 2 — Local network or Data Link

The **Local Network/Data Link layer** handles delivery across a local network link. Ethernet and Wi-Fi are key examples of link-layer technologies.

Its responsibilities include:

- Encapsulating a network-layer packet inside a frame.
- Using link-layer addressing, such as MAC addresses.
- Delivering frames across the local link.
- Supporting communication between directly connected interfaces.

### MAC addresses

MAC stands for **Media Access Control**. In typical Ethernet and Wi-Fi networking, a MAC address is a link-layer address commonly displayed as six hexadecimal byte values, for example:

`00:1A:2B:3C:4D:5E`

MAC addresses and IP addresses serve different purposes:

- **MAC address:** Used for delivery on the current local link.
- **IP address:** Used for logical addressing and forwarding between networks.

### Switches and routers

- An Ethernet switch primarily forwards frames using MAC address information.
- A router forwards IP packets between networks and uses link-layer framing for each outgoing link.

### What is a hop?

A **Layer 3 hop** usually means moving from one host or router to the next router/host along an IP path. Switches forward frames within a LAN but are not normally counted as separate Layer 3 hops.

For example:

`PC1 → Switch → Router 1 → Router 2 → Switch → Server`

The Layer 3 hops are typically:

1. PC1 to Router 1.
2. Router 1 to Router 2.
3. Router 2 to the server.

The switches are part of the local Layer 2 delivery, not separate IP-router hops.

**Think:** Ethernet frames, MAC addresses, and local-link delivery.

## 9. Layer 3 — Internet or Network

The **Internet/Network layer** enables communication between hosts across different networks.

Its main responsibilities include:

- Logical addressing using IP addresses.
- Packet delivery across interconnected networks.
- Forwarding decisions made by routers.
- Identifying source and destination hosts at the IP layer.

### IP addresses

An IP address is a logical address used at Layer 3.

- **IPv4 example:** `192.168.1.10`
- **IPv6 example:** `2001:db8::10`

IPv4 and IPv6 are different versions of the Internet Protocol.

### What does a router do?

A router examines Layer 3 information, especially the destination IP address, and consults its routing table to decide where to forward a packet.

If a PC sends traffic to a server on another network, the packet may travel through several routers. Each router forwards it toward the destination using the routing information available to it.

### ICMP

**ICMP** (Internet Control Message Protocol) carries network-layer control and diagnostic messages. The `ping` utility commonly uses ICMP Echo Request and Echo Reply messages to test reachability.

**Think:** IP addresses, packets, routing, and inter-network communication.

## 10. Layer 4 — Transport

The **Transport layer** provides communication between application processes on hosts. It uses port numbers to help deliver data to the correct service or process.

A server can run several services at once. An IP address identifies the host, while the transport protocol and port help identify the intended application endpoint.

### Transport port numbers

A **transport port number** is a logical number used by TCP or UDP. It is not the same as a physical Ethernet port or connector.

Common service ports include:

| Service | Common transport port |
|---|---:|
| HTTP | TCP 80 |
| HTTPS | TCP 443 |
| FTP control | TCP 21 |
| DNS | UDP 53 or TCP 53, depending on use |

These are common defaults; services can be configured differently.

### TCP and UDP

**TCP — Transmission Control Protocol**
- Connection-oriented.
- Provides reliable, ordered delivery to applications.
- Uses mechanisms such as acknowledgments and retransmissions.

**UDP — User Datagram Protocol**
- Connectionless.
- Has less built-in protocol overhead.
- Does not inherently guarantee delivery, ordering, or retransmission.

TCP and UDP are both transport-layer protocols, but they offer different services.

**Think:** Ports, application-to-application delivery, TCP, and UDP.

## 11. Layer 5 — Application

The **Application layer** contains protocols that applications use to communicate over a network. It is the interface between application software and network services.

Examples:

- **HTTP/HTTPS:** Web communication.
- **DNS:** Domain-name resolution.
- **FTP/TFTP:** File transfer.
- **SMTP, IMAP, POP3:** Email-related communication protocols.

For example, a browser can create an HTTP request, send it through the lower layers, and process the response returned by a web server.

The application layer is not simply a list of apps installed on a computer. It describes the networking protocols those applications use.

**Think:** Application protocols, requests, responses, and network services.

## 12. A client-to-server example

Imagine a PC opens a web page from a server on a different IP network.

Example values:

- Client IP: `192.168.1.10`
- Server IP: `10.1.1.1`
- Example service: HTTP on TCP port `80`

The addresses and port are illustrative.

1. **Application:** The browser creates an HTTP request.
2. **Transport:** TCP adds transport information, including source and destination ports.
3. **Internet:** IP adds source and destination IP addresses.
4. **Local network:** Ethernet or Wi-Fi encapsulates the IP packet in a frame for delivery over the current link.
5. **Physical:** The frame is transmitted as bits represented by electrical, optical, or radio signals.

The receiving server processes the information upward through the stack and delivers the request to the appropriate service. A response follows the reverse logical process.

### What changes at a router?

A router receives a frame on one link, processes the Layer 2 information, examines the IP packet, and forwards the packet in a new frame suitable for the outgoing link.

The Layer 2 source and destination MAC addresses generally change at each routed hop. The source and destination IP addresses normally remain the same end-to-end, except where mechanisms such as Network Address Translation (NAT) modify them. Other IP fields, such as the TTL in IPv4 or Hop Limit in IPv6, change as the packet is forwarded.

## 13. Encapsulation and decapsulation

### Encapsulation

**Encapsulation** is the process of adding protocol information as data moves down the sending device's protocol stack.

A simplified sequence:

1. The Application layer produces data.
2. The Transport layer adds a TCP or UDP header.
3. The Internet layer adds an IP header.
4. The Local Network layer adds a frame header and trailer.
5. The Physical layer transmits the bits.

Conceptually:

`[Application data]`

`[TCP/UDP header | Application data]`

`[IP header | TCP/UDP header | Application data]`

`[Frame header | IP packet | Frame trailer]`

The precise headers and trailers depend on the protocols in use.

### Decapsulation

**Decapsulation** is the receiving host's process of interpreting and removing the relevant encapsulation as data moves up the stack.

1. The Physical layer receives signals and recovers bits.
2. The Local Network layer processes the frame and its link-layer information.
3. The Internet layer processes the IP packet.
4. The Transport layer processes TCP or UDP information and uses the port information.
5. The Application layer receives the application data.

**Key idea:** Encapsulation generally moves down the sender's stack; decapsulation generally moves up the receiver's stack.

Intermediate devices also process relevant headers. A router does not normally remove every protocol header all the way to the application data.

## 14. Protocol Data Units (PDUs)

A **Protocol Data Unit (PDU)** is the name for data at a particular layer.

| Layer | PDU name | Simplified description |
|---|---|---|
| Application | Data | Application information |
| Transport with TCP | Segment | TCP header plus data |
| Transport with UDP | Datagram | UDP header plus data |
| Internet/Network | Packet | IP header plus payload |
| Local Network/Data Link | Frame | Link-layer header, payload, and usually a trailer |
| Physical | Bits | Data represented by physical signals |

### What is a payload?

A **payload** is the information carried inside a particular protocol data unit, excluding that PDU's own header and trailer.

For example:

- Application data is carried in a TCP segment.
- A TCP segment is carried in an IP packet.
- An IP packet is carried in an Ethernet frame.
- The frame is transmitted as bits.

A piece of data can be a payload from one layer's perspective and part of a larger PDU from another layer's perspective.

**Exam reminder:** TCP = segment; UDP = datagram; IP = packet; Ethernet = frame; Physical = bits.

## 15. Adjacent-layer and same-layer interaction

### Adjacent-layer interaction

**Adjacent-layer interaction** is cooperation between neighboring layers on the same device.

Examples:

- The Application layer uses the Transport layer to communicate with a remote application process.
- The Transport layer uses the Internet layer to send data toward the destination host.
- The Internet layer uses the Local Network layer to send a packet across the current link.
- The Local Network layer relies on the Physical layer to transmit a frame.

Each layer uses the services provided by the layer immediately below it.

### Same-layer interaction

**Same-layer interaction** is logical communication between corresponding layers on different devices.

Examples:

- Application protocols on the client and server follow the same message rules.
- TCP on one host exchanges TCP information with TCP on the other host.
- IP provides source and destination addressing for packets.
- Ethernet frames are exchanged between interfaces across individual Ethernet links.

Same-layer interaction does not mean each layer has its own separate physical cable or direct connection. It means corresponding protocols cooperate through the encapsulated information and behavior defined by the protocol.

| Adjacent-layer interaction | Same-layer interaction |
|---|---|
| Neighboring layers on one device | Corresponding layers on different devices |
| Describes cooperation inside the stack | Describes logical protocol communication across the network |
| Example: Transport passes data to Internet | Example: TCP on one host communicates logically with TCP on another |

## 16. The seven-layer OSI model

**OSI** stands for Open Systems Interconnection. The OSI reference model was developed by the International Organization for Standardization (ISO).

It is a seven-layer model commonly used to explain networking functions and organize troubleshooting.

| Layer | Name | Main responsibility |
|---:|---|---|
| 7 | Application | Network services used by applications |
| 6 | Presentation | Data representation, encoding, compression, and related functions |
| 5 | Session | Establishing, managing, and ending logical sessions |
| 4 | Transport | Transport between application processes |
| 3 | Network | Logical addressing and routing |
| 2 | Data Link | Framing and local-link delivery |
| 1 | Physical | Transmitting bits as signals |

### OSI mnemonic

From Layer 7 down to Layer 1:

**A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing.

Use this as a memory aid, but learn each layer's function as well.

### Why are Presentation and Session separate in OSI?

The OSI model separates some functions that are commonly grouped into the TCP/IP Application layer. Presentation concerns data representation and related transformations; Session concerns logical communication sessions.

In real-world TCP/IP stacks, these responsibilities may be handled by applications, libraries, or protocols rather than by distinct standalone layers.

## 17. TCP/IP vs OSI

The video uses a five-layer TCP/IP model. Other references may show a four-layer TCP/IP model by combining the Physical and Data Link functions into a Network Access/Link layer.

These are different ways of organizing the model, not necessarily disagreements about how networking works.

### Mapping the five-layer model to OSI

| Five-layer model used in this lesson | OSI layer(s) |
|---|---|
| Application | Application, Presentation, Session (Layers 7–5) |
| Transport | Transport (Layer 4) |
| Internet | Network (Layer 3) |
| Local Network | Data Link (Layer 2) |
| Physical | Physical (Layer 1) |

### Main differences

- **TCP/IP** describes the protocol suite and the practical layered architecture used by Internet networking.
- **OSI** is a seven-layer reference model for describing networking functions.
- TCP/IP became the dominant deployed protocol suite.
- The OSI model remains useful for learning, documentation, and structured troubleshooting.

**Important:** The model is a tool for understanding and diagnosing communication. Real protocols and devices do not always fit perfectly into one conceptual layer.

## 18. Troubleshooting with layers

When a user cannot reach a server, a layered model gives you a structured way to investigate.

| Layer | Example questions |
|---|---|
| Physical | Is the cable connected? Is the interface up? Is Wi-Fi available? |
| Local Network/Data Link | Is the local link working? Can the host reach the local gateway at Layer 2? |
| Internet/Network | Is the IP address and subnet configuration valid? Is there a route to the destination? |
| Transport | Can the required TCP connection be established? Is the destination service listening on the expected port? |
| Application | Is the application protocol working? Is the server returning an error? Is DNS resolving the name correctly? |

This is a starting checklist, not a rule that every issue must be solved strictly from Layer 1 upward. For example, DNS problems may involve application-layer configuration even when the network connection is healthy.

Useful beginner commands include:

- Windows: `ipconfig`, `ping`, `tracert`, `nslookup`
- Linux: `ip addr`, `ip route`, `ping`, `traceroute`, `dig`

Use them in your own lab or on networks where you have permission.

## 19. Common mistakes

1. **Confusing MAC and IP addresses.** MAC addresses are used for local-link delivery; IP addresses provide logical addressing across networks.
2. **Confusing physical ports with transport ports.** An Ethernet connector is physical; TCP port 443 is a logical port number.
3. **Assuming the same Ethernet frame travels end-to-end.** Routers forward packets using new link-layer framing on the outgoing link.
4. **Treating TCP and UDP as identical.** TCP provides reliable, ordered transport; UDP does not inherently guarantee delivery or ordering.
5. **Thinking a router and a switch do the same thing.** A Layer 2 switch primarily forwards frames using MAC addresses; a router forwards IP packets between networks.
6. **Thinking encapsulation only means encryption.** Encapsulation means wrapping data with protocol information; it is not the same as encryption.
7. **Memorizing layer names without understanding their jobs.** Be able to explain the role, addressing, and PDU of each layer.
8. **Assuming every TCP/IP diagram has the same number of layers.** Four- and five-layer presentations exist.

## 20. Revision sheet

| Question | Quick answer |
|---|---|
| What is a protocol? | Rules that define communication |
| What is a standard? | An agreed specification for a technology or protocol |
| Who develops IEEE 802.3? | IEEE |
| What does IEEE 802.3 describe? | Ethernet |
| What does IEEE 802.11 describe? | Wireless LAN standards (Wi-Fi) |
| What does IETF publish? | Many Internet protocol specifications as RFCs |
| Which layer transmits bits? | Physical |
| Which layer uses MAC addresses? | Local Network/Data Link |
| Which layer uses IP addresses? | Internet/Network |
| Which layer uses TCP/UDP ports? | Transport |
| Which layer includes HTTP and DNS? | Application |
| What is encapsulation? | Adding protocol information as data moves down the stack |
| What is decapsulation? | Processing/removing encapsulation as data moves up the stack |
| What does PDU stand for? | Protocol Data Unit |
| TCP PDU? | Segment |
| UDP PDU? | Datagram |
| IP PDU? | Packet |
| Ethernet PDU? | Frame |
| Physical-layer representation? | Bits/signals |
| Adjacent-layer interaction? | Neighboring layers on one device cooperate |
| Same-layer interaction? | Corresponding layers on different devices communicate logically |
| How many OSI layers? | Seven |
| How many layers are used in this lesson's TCP/IP model? | Five |

## 21. Knowledge check

Try to answer before checking the key.

1. Which layer uses IP addresses to forward traffic between networks?
2. What is the difference between a MAC address and an IP address?
3. What is the PDU name for TCP, UDP, IP, and Ethernet?
4. What does encapsulation do?
5. What is the difference between adjacent-layer and same-layer interaction?
6. Which two organizations are associated with Ethernet/Wi-Fi standards and Internet protocol specifications?
7. Why can the Ethernet frame change as a packet travels through routers?
8. How does the five-layer model map to the seven-layer OSI model?
9. What is the difference between a transport port number and a physical network port?
10. Name one troubleshooting question for each of the five layers.

### Answer key

1. The Internet/Network layer (Layer 3).
2. MAC addresses support local-link delivery; IP addresses provide logical addressing between hosts/networks.
3. TCP segment; UDP datagram; IP packet; Ethernet frame.
4. It adds protocol headers and, where applicable, trailers as data moves down the sending stack.
5. Adjacent-layer interaction is between neighboring layers on one device; same-layer interaction is logical communication between corresponding layers on different devices.
6. IEEE is associated with standards such as Ethernet and Wi-Fi; IETF develops many Internet protocols and publishes RFCs.
7. Each outgoing link needs its own link-layer framing and source/destination MAC addresses.
8. TCP/IP Application maps to OSI Application, Presentation, and Session; the other four layers map to OSI Transport, Network, Data Link, and Physical.
9. A physical port is a connector/interface; a transport port is a logical TCP/UDP number used to identify an application endpoint.
10. Example answers: Physical—Is the interface up? Local Network—Is the local link working? Internet—Is there a valid IP route? Transport—Is the service listening? Application—Is the application protocol or name resolution working?

## 22. Glossary

- **Application data:** Information created or interpreted by an application.
- **Encapsulation:** Adding protocol information around data as it moves down the stack.
- **Decapsulation:** Processing and removing protocol encapsulation at the receiver.
- **Frame:** Layer 2 PDU.
- **Hop:** A step between hosts/routers in a routed path; usually counted at Layer 3.
- **IETF:** Internet Engineering Task Force.
- **IEEE:** Institute of Electrical and Electronics Engineers.
- **IP address:** Logical address used by the Internet Protocol.
- **MAC address:** Link-layer address used for local-link communication.
- **NIC:** Network Interface Card.
- **OSI:** Open Systems Interconnection reference model.
- **Packet:** Layer 3 PDU.
- **Payload:** Data carried inside a protocol data unit, excluding that PDU's own header/trailer.
- **PDU:** Protocol Data Unit, the name for data at a particular layer.
- **Protocol:** Rules that define communication between devices or software.
- **RFC:** Request for Comments; a document series containing Internet specifications and related information.
- **Segment:** TCP PDU.
- **Datagram:** UDP PDU; the term is also used in other networking contexts.
- **TCP:** Transmission Control Protocol.
- **UDP:** User Datagram Protocol.

---

## Final study checklist

Before moving to the next lesson, confirm that you can:

- [ ] Name the five layers in order.
- [ ] Explain the purpose of each layer in one or two sentences.
- [ ] Distinguish MAC addresses, IP addresses, and port numbers.
- [ ] Explain encapsulation and decapsulation using an example.
- [ ] Recall the PDU names: segment, datagram, packet, frame, and bits.
- [ ] Explain adjacent-layer and same-layer interaction.
- [ ] Map the five-layer model to the seven-layer OSI model.
- [ ] Use the model to structure basic network troubleshooting.

**Recommended study method:** Read each section, draw the five layers from memory, explain a web request out loud, then answer the knowledge check without looking at the notes.

*Companion lesson: Jeremy's IT Lab — [Day 3: How the TCP/IP Model Actually Works](https://youtu.be/yM-XNq9ADlI).* 
