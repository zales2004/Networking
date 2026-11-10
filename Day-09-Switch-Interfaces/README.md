# Day 09 — Switch Interfaces

## Overview

This lesson explains how Ethernet switch interfaces operate, how to inspect and configure them, and how to identify common interface problems. It covers speed, duplex, autonegotiation, interface descriptions, unused ports, collision handling, and interface error counters.

## 1. Switch Interfaces

A switch interface (port) is the physical connection used to connect end devices, other switches, routers, and network equipment.

Common interface names include:

| Interface type | Typical capability |
|---|---|
| FastEthernet (Fa) | 10/100 Mbps |
| GigabitEthernet (Gi) | Up to 1 Gbps |
| TenGigabitEthernet (Te) | Up to 10 Gbps |

Actual capabilities depend on the switch model and interface hardware.

Ethernet switching is primarily a Layer 2 function. A Layer 2 switch forwards Ethernet frames using destination MAC addresses. Speed and duplex are physical-link characteristics associated with Layer 1.

A normal Layer 2 switch access port does not need an IP address to forward frames. A switch management IP address is commonly configured on a VLAN interface (SVI), separately from physical switch ports.

## 2. Interface Speed

Speed is the rate at which an interface transmits data, measured in bits per second (bps).

| Speed | Equivalent |
|---|---:|
| 10 Mbps | 10 million bits per second |
| 100 Mbps | 100 million bits per second |
| 1 Gbps | 1,000 Mbps |
| 10 Gbps | 10,000 Mbps |

The operational speed must be supported by both connected interfaces and the physical medium.

## 3. Duplex Modes

Duplex determines whether an Ethernet interface can transmit and receive data simultaneously.

### Half-duplex

- An interface can transmit or receive at a given time, but not both simultaneously.
- Devices share access to the communication medium.
- Collisions can occur when multiple devices transmit at the same time.
- Traditional Ethernet hubs operate in half-duplex environments.

### Full-duplex

- An interface can transmit and receive simultaneously.
- A dedicated switched Ethernet link normally operates in full-duplex.
- Normal Ethernet collisions do not occur on a properly operating full-duplex link.

**Remember:** Speed describes how fast a link operates. Duplex describes whether transmission and reception can happen simultaneously.

## 4. Interface Status

Cisco IOS commands report the operational condition of interfaces.

| Status | Meaning |
|---|---|
| up/up | Physical link and line protocol are operational |
| down/down | The interface or physical link is not operational |
| administratively down/down | The interface has been disabled with the `shutdown` command |

Switch ports are generally enabled by default. If no active device or cable is connected, the port commonly reports a down state. Router interfaces are commonly administratively disabled by default and must be enabled with `no shutdown`.

The exact output and available states can vary by device and interface type.

## 5. Checking Switch Interfaces

Use privileged EXEC mode (`Switch#`) to run verification commands.

| Command | Purpose |
|---|---|
| `show interfaces status` | Quick summary of ports, status, VLAN, duplex, speed, and type |
| `show ip interface brief` | Brief interface status and IP address summary |
| `show interfaces` | Detailed interface statistics and counters |
| `show interfaces fa0/1` | Detailed information for a specific interface |
| `show running-config` | Displays the current device configuration |

Example:

```text
Switch> enable
Switch# show interfaces status
Switch# show ip interface brief
Switch# show interfaces fa0/1
```

### Common fields in `show interfaces status`

- **Port:** Interface identifier, such as Fa0/1.
- **Name:** Configured interface description.
- **Status:** Whether the port is connected, not connected, disabled, or in another state.
- **VLAN:** Access VLAN or trunk status.
- **Duplex:** Current or configured duplex mode.
- **Speed:** Current or configured link speed.
- **Type:** Interface or media type.

A `connected` status generally indicates an active link. `notconnect` generally means that no active physical link is detected. A `disabled` status commonly indicates that the port has been administratively shut down.

## 6. Configuring Speed and Duplex

Autonegotiation is normally recommended. Manual configuration may be required for a specific compatibility or troubleshooting situation.

Example: configure FastEthernet0/1 for 100 Mbps and full-duplex.

```text
Switch> enable
Switch# configure terminal
Switch(config)# interface fa0/1
Switch(config-if)# speed 100
Switch(config-if)# duplex full
Switch(config-if)# description Connected to R1
Switch(config-if)# end
Switch# show interfaces status
```

To restore automatic negotiation:

```text
Switch# configure terminal
Switch(config)# interface fa0/1
Switch(config-if)# speed auto
Switch(config-if)# duplex auto
Switch(config-if)# end
```

**Important:** If settings are configured manually, the connected device must use compatible settings. A speed or duplex mismatch can cause performance problems and interface errors.

## 7. Interface Descriptions

An interface description is an administrative label that documents what is connected to a port. It helps network administrators understand and troubleshoot a network.

Example:

```text
Switch(config)# interface fa0/1
Switch(config-if)# description Connected to R1
```

Descriptions do not change frame forwarding behavior. They are used for documentation and easier identification.

## 8. Disabling Unused Switch Ports

Unused switch ports should be disabled as a basic security practice. This reduces the chance that an unauthorized device can connect through an unused port.

Example: disable ports Fa0/5 through Fa0/12.

```text
Switch# configure terminal
Switch(config)# interface range fa0/5 - 12
Switch(config-if-range)# description UNUSED - DISABLED
Switch(config-if-range)# shutdown
Switch(config-if-range)# end
```

To enable the ports later:

```text
Switch# configure terminal
Switch(config)# interface range fa0/5 - 12
Switch(config-if-range)# no shutdown
Switch(config-if-range)# end
```

An interface range can also include separate groups of ports:

```text
Switch(config)# interface range fa0/5 - 6, fa0/9 - 12
```

Check the actual interface numbering on the switch before applying a range.

## 9. Hubs, Collision Domains, and CSMA/CD

### Ethernet hubs

A hub is a Layer 1 device. It repeats incoming signals out of its other ports rather than learning MAC addresses and selectively forwarding frames.

Devices connected to a traditional hub share the same communication medium and collision domain.

### Collision domains

A collision domain is a network segment in which simultaneous transmissions can interfere with each other when half-duplex Ethernet is used.

- A traditional hub creates one shared collision domain for its connected devices.
- Each switch port creates a separate collision domain.
- A properly operating full-duplex switched link does not experience normal Ethernet collisions.

### CSMA/CD

CSMA/CD stands for **Carrier Sense Multiple Access with Collision Detection**. It is used by traditional shared, half-duplex Ethernet to manage access to the communication medium.

The process is:

1. **Carrier Sense:** A device listens to check whether the medium is idle.
2. **Multiple Access:** Multiple devices share the same medium.
3. **Collision Detection:** If two devices transmit at the same time, a collision is detected.
4. **Jamming and Backoff:** The devices stop transmitting and wait for a random backoff period.
5. **Retransmission:** A device tries again when the medium is available.

CSMA/CD is associated with traditional half-duplex Ethernet. It is not normally needed on modern full-duplex switched Ethernet links.

CSMA/CA (Carrier Sense Multiple Access with Collision Avoidance) is a different mechanism associated with wireless networking.

## 10. Autonegotiation

Autonegotiation allows connected Ethernet interfaces to advertise their supported capabilities and select compatible link settings, including speed and duplex where supported.

For example, if both interfaces support 10/100/1000 Mbps, they can negotiate a compatible speed based on their capabilities and the link.

### Duplex mismatch

A duplex mismatch can occur when the two ends of a link operate with different duplex settings.

Example:

- Switch A: 100 Mbps, full-duplex.
- Switch B: 100 Mbps, half-duplex.

The half-duplex side follows collision-detection rules, while the full-duplex side does not. This can lead to collisions on the half-duplex side, late collisions, errors, and poor performance.

If autonegotiation is disabled on one side, the other side may be unable to determine the remote duplex setting correctly. On traditional Cisco Ethernet implementations, this can result in a half-duplex assumption at 10 or 100 Mbps.

**Best practice:** Keep autonegotiation enabled on both ends unless a specific requirement makes manual configuration necessary. If settings must be configured manually, configure both ends consistently.

## 11. Interface Counters and Errors

The `show interfaces` command displays interface statistics that help identify link and frame problems.

| Counter | Meaning |
|---|---|
| Input packets / bytes | Packets and bytes received by the interface |
| Runts | Frames smaller than the minimum Ethernet frame size of 64 bytes |
| Giants | Frames larger than the normal maximum untagged Ethernet frame size |
| CRC | Frames that failed the cyclic redundancy check |
| Frame | Frames with an invalid format or alignment |
| Input errors | A summary counter for various receive-side errors |
| Output errors | Errors encountered while transmitting frames |

The normal maximum size for an untagged Ethernet frame is 1518 bytes, including the Ethernet header and FCS. VLAN tags and jumbo-frame configurations can change the applicable frame-size limits.

### CRC and FCS

- **CRC (Cyclic Redundancy Check):** An error-detection calculation.
- **FCS (Frame Check Sequence):** The Ethernet frame field that carries the CRC value.

The receiving device calculates the CRC and compares it with the value in the received FCS. If the values do not match, the frame is considered corrupted and is discarded.

Increasing CRC or other error counters may indicate a cabling problem, interference, a faulty interface, hardware problems, or a duplex mismatch. Counters should be interpreted together with the link status and other evidence rather than assuming that one counter always identifies a single cause.

Useful commands:

```text
Switch# show interfaces
Switch# show interfaces fa0/1
Switch# show interfaces status
Switch# show ip interface brief
```

## 12. Practical Verification Workflow

When investigating a switch port:

1. Check the port state with `show interfaces status`.
2. Inspect detailed information with `show interfaces fa0/1`.
3. Verify the configured speed and duplex settings.
4. Check whether both ends use compatible settings.
5. Inspect error counters such as CRC, runts, giants, and input errors.
6. Check the cable, connected device, and physical port if the link is down or errors continue to increase.
7. Document the connection using an interface description.
8. Disable ports that are not in use.

## 13. Quick Command Reference

| Task | Command |
|---|---|
| Enter privileged EXEC mode | `enable` |
| Enter global configuration mode | `configure terminal` |
| Select an interface | `interface fa0/1` |
| Configure speed | `speed 100` |
| Configure full-duplex | `duplex full` |
| Set speed to automatic | `speed auto` |
| Set duplex to automatic | `duplex auto` |
| Add interface description | `description Connected to R1` |
| Disable an interface | `shutdown` |
| Enable an interface | `no shutdown` |
| Select multiple interfaces | `interface range fa0/5 - 12` |
| Check port summary | `show interfaces status` |
| Check detailed interface statistics | `show interfaces fa0/1` |
| Check brief interface status | `show ip interface brief` |
| View current configuration | `show running-config` |

## 14. Key Takeaways

- Switch interfaces connect end devices and other network equipment.
- Speed is the link data rate; duplex determines whether transmission and reception can occur simultaneously.
- Half-duplex Ethernet can experience collisions; full-duplex switched Ethernet normally does not.
- Hubs repeat signals and create a shared collision domain.
- Switch ports separate collision domains.
- CSMA/CD manages access to traditional half-duplex Ethernet.
- Autonegotiation helps connected interfaces select compatible speed and duplex settings.
- A duplex mismatch can cause errors and poor performance.
- `show interfaces status` provides a quick port overview; `show interfaces` provides detailed statistics.
- Use interface descriptions to document connections.
- Disable unused switch ports to reduce unauthorized access opportunities.

---

**Learning reference:** Jeremy's IT Lab — CCNA Day 09: Switch Interfaces.

**Study approach:** Understand the concept, identify the relevant IOS command, verify the interface state, and interpret the output before making configuration changes.
