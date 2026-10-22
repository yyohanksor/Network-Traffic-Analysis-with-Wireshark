# Wireshark: The Basics

## 1. Wireshark Interface

Wireshark provides tools for capturing, filtering, examining, and processing network traffic.

| Interface Component | Description |
|---|---|
| **Toolbar** | Contains menus and shortcuts for filtering, sorting, summarizing, exporting, and merging packet captures. |
| **Display Filter Bar** | The main section for entering display filters to find specific packets. |
| **Recent Files** | Lists recently investigated files. Double-click a file to reopen it. |
| **Capture Filters and Interfaces** | Provides capture filters and available network interfaces for capturing traffic. A network interface is a connection point between a computer and a network. Examples include `lo`, `eth0`, and `ens33`. |
| **Status Bar** | Displays tool status, profile information, and packet statistics. |

### Starting and Stopping a Capture

- **Blue shark button:** Starts capturing network traffic.
- **Red button:** Stops the capture.
- **Green button:** Restarts the capture process.

## 2. Packet Analysis Interface

When you open a packet capture file, Wireshark displays three main panes.

| Pane | Description |
|---|---|
| **Packet List Pane** | Summarizes each packet, including its source, destination, protocol, and other packet information. Select a packet to investigate it further. |
| **Packet Details Panel** | Displays a detailed breakdown of the selected packet's protocols and fields. |
| **Packet Bytes Pane** | Shows the packet's hexadecimal and decoded ASCII representations. Selecting a field in the details panel highlights the corresponding bytes. |

## 3. Packet Layers and Details

Wireshark displays different types of information about a packet, including its frame, MAC addresses, IP addresses, protocol details, protocol errors, application protocol, and application data.

### Frame (Layer 1)

Displays information about the frame or packet being examined, including details associated with the Physical layer of the OSI model.

### Source and Destination MAC Addresses (Layer 2)

Displays the source and destination MAC addresses associated with the Data Link layer of the OSI model.

### Source and Destination IP Addresses (Layer 3)

Displays the source and destination IPv4 addresses associated with the Network layer of the OSI model.

### Protocol (Layer 4)

Displays transport protocol information, such as TCP or UDP, including source and destination ports.

### Application Protocol

Displays details about the application protocol being used, such as HTTP, FTP, and SMB.

## 4. Finding Packets

Wireshark provides multiple ways to locate packets.

- **Go:** Navigate to a particular packet by its number.
- **Edit → Find Packet:** Search for a specific string in packet data.

## 5. Merging Capture Files

Wireshark can combine two PCAP files into a single file.

1. Select **File → Merge**.
2. Choose the second PCAP file.
3. Review the total number of packets displayed for the selected file.
4. Click **Open** to merge the selected file with the current capture.
5. Save the merged PCAP file before continuing to work with it.

## 6. Capture File Properties

Knowing the details of a capture file is useful when working with multiple PCAP files. These details can help identify, classify, and prioritize files for investigation.

To view the details, select:

**Statistics → Capture File Properties**

The information includes:

- File hash
- Capture time
- Capture file comments
- Interface information
- Statistics

## 7. Extracting Files from Network Traffic

Wireshark can extract files transferred over the network. Security analysts can save discovered files for further investigation.

Exporting objects is available for selected protocols, including:

- DICOM
- HTTP
- IMF
- SMB
- TFTP

## 8. Filtering Packets

### Apply as Filter

While investigating a capture file, select a packet field and use the right-click menu or **Analyze → Apply as Filter**.

This creates a filter based on the selected field value, allowing you to focus on packets containing that value.

The status bar displays the total number of packets and the number currently displayed.

### Conversation Filter

A Conversation Filter helps isolate related packets by focusing on a conversation, such as traffic associated with particular IP addresses and port numbers.

You can access it through the right-click menu or **Analyze → Conversation Filter**.

Unlike filtering a single field value, this option helps display the related packets while hiding unrelated traffic.

### Colourise Conversation

This option highlights packets associated with a conversation without applying a display filter or reducing the number of displayed packets.

You can access it through the right-click menu or:

**View → Colourise Conversation**

To undo the highlighting, select:

**View → Colourise Conversation → Reset Colourisation**

Colourisation works with Wireshark's colouring rules to visually distinguish related packets.

### Prepare a Filter

This option is similar to **Apply as Filter**, but it prepares a display filter without immediately applying it.

You can access it through the right-click menu. The filter query is added to the display filter bar, where you can execute it by pressing Enter or modify it using the available **... and/or ...** options.

## 9. Following Network Streams

Wireshark normally displays traffic in individual packets. Following a stream allows you to reconstruct the communication and view the traffic as it appears at the application level.

This helps analysts understand the event being investigated. Unencrypted protocol traffic may expose transferred information, including usernames, passwords, and other data.

To follow a stream, use the right-click menu or:

**Analyze → Follow TCP/UDP/HTTP Stream**

The stream appears in a separate dialogue box. In the stream view described in the course, server-originating packets are highlighted in blue and client-originating packets are highlighted in red.

## 10. Basic Display Filter Syntax

### Filter by TCP Port

```text
tcp.port == <port number>
```

### Filter by UDP Port

```text
udp.port == <port number>
```

### Filter by IP Address

```text
ip.addr == <IP address>
```

### Filter by Protocol Name

Type the protocol name into the display filter bar.

## 11. OpenSSH Overview

OpenSSH provides several security benefits:

- **Secure authentication:** Supports password-based authentication, public-key authentication, and two-factor authentication.
- **Confidentiality:** Provides end-to-end encryption to protect against eavesdropping. It also notifies users of new server keys to help protect against man-in-the-middle attacks.
- **Integrity:** Cryptography helps protect the integrity of exchanged data.
- **Tunneling:** Can create a secure tunnel through which other protocols are routed, providing a VPN-like connection.
- **X11 Forwarding:** Allows graphical applications on Unix-like systems to be used over a network connection.


