# Network Packet Capture and Analysis -- Wireshark Traffic Examination

![Wireshark](https://img.shields.io/badge/Wireshark-1679A7?style=for-the-badge&logo=wireshark&logoColor=white)
![Network Security](https://img.shields.io/badge/Network_Security-2C2D72?style=for-the-badge&logo=cisco&logoColor=white)
![PCAP](https://img.shields.io/badge/PCAP_Analysis-005571?style=for-the-badge&logo=wireshark&logoColor=white)
![Traffic Analysis](https://img.shields.io/badge/Traffic_Analysis-4B0082?style=for-the-badge&logoColor=white)

---

## Table of Contents

1. [What This Project Is](#what-this-project-is)
2. [Why Packet Analysis Matters](#why-packet-analysis-matters)
3. [What Is in the Capture File](#what-is-in-the-capture-file)
4. [How to Open and Analyze the Capture](#how-to-open-and-analyze-the-capture)
5. [Key Wireshark Display Filters](#key-wireshark-display-filters)
6. [Analysis Techniques](#analysis-techniques)
7. [Tools Used](#tools-used)
8. [Author](#author)
9. [License](#license)

---

## What This Project Is

I captured live network traffic using Wireshark and analyzed it at the packet level. The capture file (`hoohoo.pcapng`) contains real traffic from a live network interface -- ARP broadcasts, DNS queries, TCP handshakes, HTTP/HTTPS requests, and more. Everything here can be opened and inspected in Wireshark.

---

## Why Packet Analysis Matters

Packet captures are the ground truth of what happened on a network. When alerts fire and logs are ambiguous, the pcap tells the real story. This is the kind of analysis SOC analysts and incident responders do daily -- examining traffic to trace attacks, find C2 channels, or diagnose network issues.

---

## What Is in the Capture File

### File Details

| Property       | Value                                                        |
|----------------|--------------------------------------------------------------|
| File name      | `hoohoo.pcapng`                                              |
| Format         | PCAPNG (Packet Capture Next Generation)                      |
| File size      | ~9 KB                                                        |
| Purpose        | Live network traffic capture for protocol analysis           |

### Protocols Present

- **ARP** -- Layer 2 broadcasts that resolve IP addresses to MAC addresses.
- **DNS** -- Queries and responses translating domain names to IP addresses.
- **TCP Handshakes** -- SYN, SYN-ACK, ACK sequences establishing connections between hosts.
- **HTTP/HTTPS** -- Web traffic, including unencrypted HTTP and encrypted HTTPS sessions.
- **TLS Handshakes** -- Negotiation for encrypted connections, revealing server names and cipher suites even without decrypting payload.
- **ICMP** -- Ping requests/replies and traceroute packets for reachability and path analysis.

---

## How to Open and Analyze the Capture

### Prerequisites

Install Wireshark from the official site: [https://www.wireshark.org/download.html](https://www.wireshark.org/download.html). Wireshark is available for Windows, macOS, and Linux at no cost.

### Step-by-Step Guide

**Step 1 -- Open the Capture File**

Launch Wireshark and open the capture file by navigating to `File > Open` and selecting `hoohoo.pcapng`. Wireshark will parse the file and display all captured packets in the main packet list pane.

**Step 2 -- Examine the Packet List**

The packet list pane shows each captured packet with the following columns by default: packet number, timestamp, source address, destination address, protocol, length, and a brief info summary. Scroll through the list to get an initial sense of the traffic volume and protocol distribution.

**Step 3 -- Inspect Individual Packets**

Click on any packet in the list to expand its details in the packet detail pane below. Wireshark dissects each packet layer by layer, starting from the physical frame and moving up through Ethernet, IP, transport (TCP/UDP), and application-layer protocols.

**Step 4 -- Apply Display Filters**

Use the display filter bar at the top of the Wireshark window to isolate specific traffic. Type a filter expression (see the table below) and press Enter. Only packets matching the filter will be displayed. The filter bar turns green when a valid filter is entered and red when the syntax is incorrect.

**Step 5 -- Follow TCP Streams**

Right-click on any TCP packet and select `Follow > TCP Stream`. This reconstructs the entire conversation between two endpoints in a single readable window, showing the data exchanged in both directions. This is particularly useful for reading HTTP request/response pairs and identifying transmitted content.

**Step 6 -- View Protocol Hierarchy**

Navigate to `Statistics > Protocol Hierarchy` to see a breakdown of all protocols present in the capture, including the percentage of traffic each protocol accounts for. This provides a high-level overview of what types of communication occurred.

**Step 7 -- Review Conversations**

Navigate to `Statistics > Conversations` to see a summary of all communication pairs, organized by Ethernet, IPv4, TCP, and UDP layers. This reveals which hosts communicated most frequently and how much data was transferred.

---

## Key Wireshark Display Filters

| Filter Expression            | Description                                                  |
|------------------------------|--------------------------------------------------------------|
| `tcp`                        | Show all TCP traffic                                         |
| `udp`                        | Show all UDP traffic                                         |
| `http`                       | Show all HTTP traffic (requests and responses)               |
| `dns`                        | Show all DNS queries and responses                           |
| `arp`                        | Show all ARP traffic (address resolution)                    |
| `icmp`                       | Show all ICMP traffic (ping, traceroute)                     |
| `tls`                        | Show all TLS/SSL traffic (encrypted sessions)                |
| `tls.handshake`              | Show only TLS handshake messages                             |
| `ip.addr == 192.168.1.1`     | Show all traffic to or from a specific IP address            |
| `ip.src == 10.0.0.5`         | Show traffic originating from a specific source IP           |
| `ip.dst == 10.0.0.1`         | Show traffic destined for a specific destination IP          |
| `tcp.port == 443`            | Show all traffic on TCP port 443 (HTTPS)                     |
| `tcp.port == 80`             | Show all traffic on TCP port 80 (HTTP)                       |
| `tcp.stream eq 0`            | Show all packets belonging to TCP stream number 0            |
| `tcp.flags.syn == 1`         | Show all TCP SYN packets (connection initiations)            |
| `tcp.flags.reset == 1`       | Show all TCP RST packets (connection resets)                 |
| `frame.protocols`            | Useful in column customization to see protocol stack          |
| `http.request.method == GET` | Show only HTTP GET requests                                  |
| `dns.qry.name contains "example"` | Show DNS queries for domains containing "example"      |
| `!(arp or dns)`              | Exclude ARP and DNS traffic to reduce noise                  |

Filters can be combined using logical operators: `and`, `or`, `not` (or `&&`, `||`, `!`).

---

## Analysis Techniques

### Protocol Hierarchy Analysis

Access via `Statistics > Protocol Hierarchy`. This gives you a tree-structured breakdown of every protocol in the capture with packet counts, traffic percentages, and byte totals. It is usually the first place I look when opening a new capture.

### TCP Stream Reconstruction

Right-click any TCP packet and select `Follow > TCP Stream`. Wireshark reassembles the segments into a continuous data stream, color-coding client and server traffic. Essential for reading HTTP transactions and understanding application-layer exchanges.

### Conversation and Endpoint Analysis

Access via `Statistics > Conversations` and `Statistics > Endpoints`. These views show every unique communication pair with packet counts, byte totals, and duration -- answering who is talking to whom and how much data moved.

### I/O Graphs

Access via `Statistics > I/O Graphs`. This plots packet or byte counts over time, making it easy to spot traffic bursts and correlate events with specific time windows.

### Export Objects

Access via `File > Export Objects > HTTP` (or other protocols). If the capture contains unencrypted file transfers, Wireshark can extract and save the transferred files -- images, documents, executables, or other content.

### Expert Information

Access via `Analyze > Expert Information`. Wireshark flags potential issues like TCP retransmissions, malformed packets, connection resets, and protocol errors. A quick way to find problems without inspecting every packet manually.

---

## Tools Used

| Tool        | Purpose                                              | Website                                      |
|-------------|------------------------------------------------------|----------------------------------------------|
| Wireshark   | Packet capture, protocol analysis, traffic inspection | [wireshark.org](https://www.wireshark.org)   |

Wireshark is the world's most widely used network protocol analyzer. It is free, open-source, and maintained by a global community of contributors.

---

## Author

**Chioma Iroka**
Computer Science Graduate | Cybersecurity Focus

- GitHub: [github.com/ChiomaIroka](https://github.com/ChiomaIroka)

---

## License

This project is licensed under the [MIT License](LICENSE).
