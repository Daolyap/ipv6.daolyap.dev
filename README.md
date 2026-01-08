# IPv6 Information Guide

A comprehensive guide to IPv6 addressing, ranges, and important concepts.

## IPv6 Address Ranges

### Link-Local Unicast
- **`fe80::/10`** - IPv6's built-in Link-local unicast range, which replaces IPv4 APIPA (169.254.0.0/16)
  - Automatically configured on all IPv6-enabled interfaces
  - Used for neighbor discovery and local network communication
  - Not routable beyond the local link
- **`fe80::/64`** - A specific subnet in the fe80::/10 block, as most hosts use /64 on a link
  - Most common configuration for link-local addresses
  - Interface identifier typically derived from MAC address (EUI-64) or randomly generated

### Unique Local Addresses (ULA)
- **`fc00::/7`** - Unique Local (routable within the LAN)
  - IPv6 equivalent of IPv4 private addresses (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16)
  - Subdivided into:
    - `fc00::/8` - Centrally assigned (not yet defined)
    - `fd00::/8` - Locally assigned (pseudo-random allocation)
  - Routable within an organization but not on the global Internet

### Global Unicast Addresses
- **`2000::/3`** - Global unicast (globally routable)
  - The main IPv6 address space for Internet-routable addresses
  - Equivalent to public IPv4 addresses
  - Allocated by IANA to Regional Internet Registries (RIRs)

### Multicast Addresses
- **`ff00::/8`** - The overall multicast address space for groups
  - Used for one-to-many communication
  - Replaces broadcast in IPv4
  - Second octet indicates scope and flags

#### Important Multicast Groups
- **`ff02::1`** - All-nodes multicast group (link-local scope)
  - Reaches all IPv6 nodes on the local link
  - Similar to IPv4 broadcast on local subnet
- **`ff02::2`** - All-routers multicast group (link-local scope)
  - Reaches all IPv6 routers on the local link
  - Used in router discovery and routing protocols
- **`ff02::1:ff00:0/104`** - Solicited-node multicast addresses
  - Used for Neighbor Discovery Protocol (NDP)
  - More efficient than broadcast for address resolution

## Special IPv6 Addresses

### Loopback and Unspecified
- **`::1/128`** - Loopback address
  - IPv6 equivalent of IPv4's 127.0.0.1
  - Used for local host communication
- **`::/128`** - Unspecified address
  - Indicates the absence of an address
  - Used before an address is assigned

### Documentation and Examples
- **`2001:db8::/32`** - Documentation prefix
  - Reserved for documentation and examples
  - Should never appear in production networks
  - IPv6 equivalent of IPv4's 192.0.2.0/24, 198.51.100.0/24, 203.0.113.0/24

### Other Notable Ranges
- **`::ffff:0:0/96`** - IPv4-mapped IPv6 addresses
  - Allows IPv6 applications to communicate with IPv4 endpoints
  - Example: `::ffff:192.0.2.1` represents IPv4 address 192.0.2.1
- **`64:ff9b::/96`** - Well-known prefix for IPv4/IPv6 translation
  - Used in NAT64 scenarios
- **`2001::/32`** - IETF Protocol Assignments
  - Reserved for IETF protocol use
  - Includes Teredo (2001::/32) and benchmarking addresses

## IPv6 vs IPv4 Comparison

| Feature | IPv4 | IPv6 |
|---------|------|------|
| Address Length | 32 bits | 128 bits |
| Address Format | Dotted decimal (e.g., 192.168.1.1) | Hexadecimal with colons (e.g., 2001:db8::1) |
| Address Space | ~4.3 billion addresses | ~340 undecillion addresses |
| Private Addresses | 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 | fc00::/7 (ULA) |
| Loopback | 127.0.0.1 | ::1 |
| Link-Local | 169.254.0.0/16 (APIPA) | fe80::/10 |
| Broadcast | Yes (e.g., 255.255.255.255) | No (replaced by multicast) |
| Default Subnet | Varies (/24 common) | /64 (standard) |
| Configuration | Manual or DHCP | SLAAC, DHCPv6, or manual |

## IPv6 Address Format

### Address Notation
- Full format: `2001:0db8:0000:0000:0001:0000:0000:0001`
- Compressed format: `2001:db8::1:0:0:1`
- Leading zeros can be omitted: `2001:db8:0:0:1:0:0:1`
- Double colon (::) can replace consecutive zero blocks (only once per address)

### Subnet Notation
- IPv6 uses CIDR notation: `2001:db8::/32`
- Standard subnet size is /64 (64-bit network prefix, 64-bit interface identifier)
- /64 allows for SLAAC (Stateless Address Autoconfiguration)

## Key IPv6 Concepts

### Stateless Address Autoconfiguration (SLAAC)
- Hosts can automatically configure their own IPv6 addresses
- Uses Router Advertisements (RA) from routers
- Combines network prefix with interface identifier (EUI-64 or random)
- No DHCP server required for basic connectivity

### Neighbor Discovery Protocol (NDP)
- Replaces ARP, ICMP Router Discovery, and ICMP Redirect
- Uses ICMPv6 messages
- Functions include:
  - Router discovery
  - Address resolution
  - Duplicate Address Detection (DAD)
  - Neighbor unreachability detection

### Multiple Addresses per Interface
- IPv6 interfaces typically have multiple addresses:
  - Link-local address (fe80::/10)
  - One or more global unicast addresses
  - Possibly unique local addresses (fc00::/7)
  - Multiple multicast group memberships

## Best Practices

1. **Use /64 for subnets** - Standard and enables SLAAC
2. **Enable IPv6 privacy extensions** - Prevents tracking via stable interface identifiers
3. **Use ULA for internal networks** - Provides stability even if global prefix changes
4. **Implement proper firewall rules** - IPv6 doesn't hide hosts behind NAT
5. **Disable IPv6 if not used** - Or ensure proper security configuration
6. **Plan address allocation** - Use hierarchical addressing for easier management
7. **Monitor both IPv4 and IPv6** - During transition period, monitor both protocols

## Additional Resources

- [RFC 4291](https://tools.ietf.org/html/rfc4291) - IPv6 Addressing Architecture
- [RFC 4862](https://tools.ietf.org/html/rfc4862) - IPv6 Stateless Address Autoconfiguration
- [RFC 4861](https://tools.ietf.org/html/rfc4861) - Neighbor Discovery for IPv6
- [IANA IPv6 Address Space](https://www.iana.org/assignments/ipv6-address-space/)

---

*This guide provides essential IPv6 information for network administrators, developers, and anyone interested in understanding IPv6 addressing.*