# IPv4 Subnet Mask — Quick Study Reference

A beginner-friendly reference explaining how an IPv4 network address, CIDR prefix, and subnet mask relate to one another.

## 1. Key concept

**If the network is written as `192.168.1.0/24`, its subnet mask is `255.255.255.0`.**

The `/24` is important: it tells us that the first 24 bits of the 32-bit IPv4 address are the network portion. The remaining 8 bits are the host portion.

> Important: `192.168.1.0` by itself does not uniquely specify a subnet mask. The prefix or subnet mask must also be known. In this example, we are using `/24`.

## 2. CIDR prefix and subnet mask

CIDR notation writes the number of network bits after a slash. A subnet mask represents those network bits as binary 1s and the host bits as binary 0s.

| CIDR prefix | Binary subnet mask | Dotted-decimal subnet mask |
|---|---|---|
| `/8` | `11111111.00000000.00000000.00000000` | `255.0.0.0` |
| `/16` | `11111111.11111111.00000000.00000000` | `255.255.0.0` |
| `/24` | `11111111.11111111.11111111.00000000` | `255.255.255.0` |

Each IPv4 address contains 32 bits, divided into four 8-bit octets. In a subnet mask, each 8-bit octet of all 1s equals 255, while an octet of all 0s equals 0.

## 3. Convert `/24` to a subnet mask

A `/24` prefix means 24 network bits followed by 8 host bits:

```text
11111111.11111111.11111111.00000000
```

Convert each octet from binary to decimal:

- `11111111` = 255
- `11111111` = 255
- `11111111` = 255
- `00000000` = 0

Therefore:

```text
/24 = 255.255.255.0
```

## 4. Example subnet: `192.168.1.0/24`

| Property | Value |
|---|---|
| Network address | `192.168.1.0` |
| CIDR prefix | `/24` |
| Subnet mask | `255.255.255.0` |
| Network bits | 24 |
| Host bits | 8 |
| Total addresses | 256 |
| Usable host addresses | 254 |
| First usable address | `192.168.1.1` |
| Last usable address | `192.168.1.254` |
| Broadcast address | `192.168.1.255` |

For a typical IPv4 subnet, the number of usable host addresses is:

```text
Usable hosts = 2^(number of host bits) - 2
             = 2^8 - 2
             = 254
```

The two normally excluded addresses are the network address and the subnet broadcast address. Special cases, such as `/31` point-to-point links, use different rules.

## 5. How the subnet mask separates network and host portions

For `192.168.1.25/24`:

- The network portion is `192.168.1`.
- The host portion is the final octet, `25`.
- The subnet's network address is `192.168.1.0`.
- The host address `192.168.1.25` is inside this subnet.

The subnet mask uses 1 bits to identify the network portion and 0 bits to identify the host portion.

## 6. Common mistakes to avoid

1. **Do not assume an IP address alone tells you its mask.** The prefix or mask must be specified or otherwise established by the network configuration.
2. **Do not confuse the network address with a host address.** In a typical `/24` subnet, `192.168.1.0` identifies the network; it is not assigned to an ordinary host.
3. **Do not confuse the broadcast address with the last usable address.** For this example, broadcast is `192.168.1.255`, while the last usable host address is `192.168.1.254`.
4. **Do not assume every `192.168.x.x` subnet uses `/24`.** CIDR allows different prefixes, such as `/16`, `/25`, or `/26`, depending on the network design.

## 7. Quick memory table

| Prefix | Subnet mask | Host bits | Usable hosts in a typical subnet |
|---|---|---:|---:|
| `/8` | `255.0.0.0` | 24 | 16,777,214 |
| `/16` | `255.255.0.0` | 16 | 65,534 |
| `/24` | `255.255.255.0` | 8 | 254 |

## 8. Final takeaway

Remember this example:

```text
Network:      192.168.1.0/24
Subnet mask:  255.255.255.0
Host range:   192.168.1.1 - 192.168.1.254
Broadcast:    192.168.1.255
```

The subnet mask is determined by the prefix, not by the network address alone.

## Further reading

- [Cisco: Configure IP Addresses and Unique Subnets for New Users](https://www.cisco.com/c/en/us/support/docs/ip/routing-information-protocol-rip/13788-3.html)
- [Cisco: Understand Host and Subnet Quantities](https://www.cisco.com/c/en/us/support/docs/ip/routing-information-protocol-rip/13790-8.html)
