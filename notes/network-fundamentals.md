# Network Fundamentals

These notes cover foundational networking concepts studied while working through the TryHackMe SOC Level 1 learning pathway. They provide a reference for understanding network traffic, protocols, addressing, and how security analysts investigate network activity.

## 1. Network Traffic Analysis Basics

### What Is Network Traffic Analysis?

Network traffic analysis is the process of examining network communications to understand how devices communicate, identify abnormal behavior, and investigate potentially malicious activity.

Security analysts use network traffic analysis to:
- Understand normal network communication patterns.
- Investigate suspicious connections and network activity.
- Identify potentially malicious domains, IP addresses, and protocols.
- Investigate possible data exfiltration or command-and-control (C2) communications.

### DNS Logs and Suspicious Activity

DNS (Domain Name System) translates domain names into IP addresses so systems can locate services on a network.

DNS logs may contain:
- **Query name:** The requested domain or hostname.
- **Query type:** The type of DNS record requested, such as A or AAAA.
- **Source/client IP:** The system making the query.
- **Destination/resolver IP:** The DNS server receiving the query.
- **Timestamp:** When the query occurred.
- **Response information:** The answer returned, if available.

DNS logs can help identify suspicious domains and unusual query patterns. However, DNS logs alone may not provide enough context to determine whether an attack occurred.

For example, attackers may encode information in DNS queries or use DNS to communicate with remote systems. Inspecting a packet capture (PCAP) can provide additional context about the queries, responses, and communication patterns.

### DNS Tunneling

DNS tunneling is a technique that uses DNS queries and responses to carry data or communicate with a remote system, potentially bypassing some network security controls.

Potential indicators include:
- Unusually long or frequently changing subdomains.
- High volumes of DNS queries to an unfamiliar domain.
- Unusual query patterns or encoded-looking subdomain strings.
- Repeated DNS requests that do not match normal browsing behavior.

These indicators are not proof of malicious activity by themselves and require further investigation.

### Checking Domain Reputation

[ VirusTotal](https://www.virustotal.com/gui/home/url) can be used to check a URL or domain against multiple security vendors and reputation sources.

**Important:** A clean reputation result does not guarantee that a domain is safe. A malicious or newly created domain may not yet have been detected. Reputation results should be considered alongside other evidence, such as DNS logs and packet captures.

## 2. The TCP/IP Model and Encapsulation

The TCP/IP model explains how data moves through four layers: the Application Layer, Transport Layer, Internet Layer, and Link Layer.

### 1. Application Layer
- Consists of an application header and application data.
- The application header contains information related to the application communication.

### 2. Transport Layer
- Receives the application header and application data.
- Segments and encapsulates the application information.
- Adds a transport header.

### 3. Internet Layer
- Receives the segment from the transport layer.
- Adds an Internet header.
- If the segment is larger than the Maximum Transmission Unit (MTU), it will be divided into fragments, and a header will be added to each fragment.

### 4. Link Layer
- Receives the data from the Internet layer.
- Adds a link-layer header containing additional addressing information.

### Key Takeaway
As data moves down through the TCP/IP layers, each layer adds its own header. This process is called **encapsulation**.

## 3. Corporate Network Traffic

### Intermediary Devices and Endpoints

**Intermediary devices** forward, route, inspect, or control network traffic. Examples include:
- Routers
- Switches
- Firewalls
- Intrusion Detection Systems (IDS)

**Endpoints** are devices that originate or receive communications. Examples include:
- Workstations
- Servers
- Laptops
- Mobile devices

Monitoring both endpoints and intermediary devices can help analysts understand what happened and where it occurred.

### North-South Traffic

North-south traffic moves between an organization's internal network and external networks, such as the internet.

- **Ingress:** Traffic entering the organization's network.
- **Egress:** Traffic leaving the organization's network.

For example, a workstation connecting to an external website generates outbound traffic, while the website's response travels back into the network.

### East-West Traffic

East-west traffic moves between systems within an organization's internal network.

Examples include:
- A workstation communicating with an internal server.
- A user device connecting to a domain controller.
- Internal systems exchanging application data.

Monitoring east-west traffic can help identify lateral movement, unauthorized access, and unusual communication between internal systems.

### Network Taps

A network tap is a device that copies network traffic so monitoring tools can inspect it without directly interrupting the communication being observed.

Network taps can provide visibility into traffic for security monitoring, troubleshooting, and network investigations.

## 4. IP Addressing and Networking Essentials

### Public and Private IP Addresses

An IP address identifies a network interface for communication using the Internet Protocol.

**Public IP addresses** are globally routable addresses used to communicate across the internet.

**Private IPv4 addresses** are reserved for use within private networks. The following ranges are defined by RFC 1918:

| Range | CIDR notation |
|---|---|
| `10.0.0.0` – `10.255.255.255` | `10.0.0.0/8` |
| `172.16.0.0` – `172.31.255.255` | `172.16.0.0/12` |
| `192.168.0.0` – `192.168.255.255` | `192.168.0.0/16` |

Private addresses are not directly routable across the public internet.

### NAT (Network Address Translation)

Network Address Translation (NAT) modifies network address information as traffic passes through a network device.

In many home networks, a router uses NAT to allow multiple devices with private IP addresses to share a public IPv4 address when accessing the internet.

NAT helps conserve public IPv4 addresses, which are limited in supply.

### IPv4 Address Structure

An IPv4 address consists of four decimal octets separated by periods, such as `192.168.1.10`.

Each octet ranges from `0` to `255`, because each represents an 8-bit value.

However, not every possible combination represents a valid, usable host address. For example, network and broadcast addresses in traditional IPv4 subnets generally cannot be assigned to ordinary hosts.

## 5. Encapsulation and Data Transmission

### What Is Encapsulation?

Encapsulation is the process of adding protocol-specific information to data as it moves down the networking layers before transmission.

When data is received, the corresponding headers and trailers are processed as the data moves back up the layers. This reverse process is called decapsulation.

### Encapsulation Steps

1. **Application data:** An application generates information to send, such as a message or a web request.
2. **Transport layer:** TCP adds its header to create a TCP segment. UDP adds its header to create a UDP datagram.
3. **Internet layer:** IP adds an IP header, creating an IP packet.
4. **Link layer:** Ethernet or Wi-Fi prepares the data for transmission over the local network, creating a frame with the appropriate link-layer information.

### Why Encapsulation Matters

Encapsulation helps explain why a single communication can contain multiple layers of protocol information.

When analyzing traffic in Wireshark, you can expand the packet details to inspect the different layers and understand how the communication is structured.

## 6. Networking Essentials and Core Protocols

### DHCP (Dynamic Host Configuration Protocol)

DHCP automatically provides network configuration to devices, including an IP address, subnet mask, default gateway, and DNS server information.

DHCP uses UDP for its standard IPv4 client-server exchange. Clients typically use UDP port `68`, while servers use UDP port `67`.

### The DHCP DORA Process

DHCP commonly follows four steps, known as DORA:

1. **Discover:** The client broadcasts a DHCPDISCOVER message to find a DHCP server.
2. **Offer:** The server responds with a DHCPOFFER containing an available IP address and configuration information.
3. **Request:** The client sends a DHCPREQUEST to indicate which offered configuration it wants to accept.
4. **Acknowledge:** The server sends a DHCPACK confirming the lease and configuration.

### ARP (Address Resolution Protocol)

ARP is used on IPv4 local networks to discover the MAC address associated with an IPv4 address.

The typical process is:

1. **ARP Request:** A device broadcasts a request asking which device owns a particular IPv4 address.
2. **ARP Reply:** The device with the matching address usually responds with its MAC address.

The Ethernet broadcast MAC address is:

`ff:ff:ff:ff:ff:ff`

ARP is useful when investigating local network communication and determining how devices resolve one another's addresses.

### ICMP (Internet Control Message Protocol)

ICMP supports network diagnostics and error reporting.

Two commonly used diagnostic tools are:

- **`ping`:** Typically uses ICMP Echo Request and Echo Reply messages to test reachability and measure round-trip time (RTT).
- **`traceroute` / `tracert`:** Attempts to discover the network path to a destination. Implementations may use ICMP, UDP, or TCP probes and rely on responses from intermediate routers.

A failed ping does not necessarily mean that a device is offline; firewalls or other network controls may block the relevant traffic.

### TELNET

TELNET is a protocol for interactive remote terminal access. Its client can also be used to test connectivity to a TCP service by attempting to connect to a specified host and port.

TELNET does not encrypt its communication by default, meaning credentials and other transmitted data can be exposed to network observers. Secure alternatives such as SSH are preferred for remote administration.

### FTP (File Transfer Protocol)

FTP is a protocol designed for transferring files between a client and a server.

Common FTP commands include:

- **`USER`:** Supplies the username.
- **`PASS`:** Supplies the password.
- **`RETR`:** Retrieves a file from the server.
- **`STOR`:** Uploads a file to the server.

Traditional FTP does not encrypt credentials or file contents by default. Secure alternatives include SFTP and FTPS, which are different protocols or protocol extensions.

## 7. DNS and Domain Information

### Common DNS Record Types

DNS records provide information about domain names and associated services.

| Record | Purpose |
|---|---|
| **A** | Maps a hostname to an IPv4 address. |
| **AAAA** | Maps a hostname to an IPv6 address. |
| **CNAME** | Maps one hostname to another canonical hostname. |
| **MX** | Identifies mail servers responsible for receiving email for a domain. |

For example, an A record can map a hostname to an IPv4 address, while an MX record identifies the mail servers for a domain.

### `nslookup`

`nslookup` is a command-line utility for querying DNS servers and retrieving DNS information.

Example:

```bash
nslookup example.com
```

It can help determine which IP address a domain resolves to and investigate DNS configuration or resolution issues.

### WHOIS

WHOIS is a protocol and associated lookup service used to retrieve available domain registration information.

Depending on the domain and registry, results may include:
- Registrar information.
- Registration and expiration dates.
- Nameservers.
- Registrant information, when available.

Privacy protections and registry policies may limit the information returned.

## 8. Key Takeaways

- Network traffic analysis helps security analysts understand communications and investigate suspicious activity.
- The TCP/IP model provides a framework for understanding how network protocols interact.
- Public and private IP addresses serve different purposes, and NAT commonly enables private IPv4 devices to access the internet.
- DHCP, ARP, ICMP, DNS, FTP, and TELNET each perform different networking functions.
- DNS logs and packet captures can provide complementary evidence during an investigation.
- Network indicators should be evaluated in context rather than treated as proof of an attack by themselves.
- Understanding encapsulation helps analysts interpret the different protocol layers visible in Wireshark.


