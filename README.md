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
7. [Skills Demonstrated](#skills-demonstrated)
8. [Tools Used](#tools-used)
9. [Author](#author)
10. [License](#license)

---

## What This Project Is

This repository contains a hands-on network traffic capture and analysis exercise performed using **Wireshark**, the industry-standard open-source packet analyzer. The project involved capturing real network traffic traversing a live network interface, saving the raw packet data to a capture file, and then systematically analyzing that data to understand the protocols in use, identify communicating hosts, examine data flows, and uncover the structure of network conversations.

Packet-level analysis is a core competency for anyone working in cybersecurity. This project replicates the type of work that **SOC analysts**, **incident responders**, and **network security engineers** perform on a daily basis when they need to understand exactly what is happening on the wire.

The capture file included in this repository -- `hoohoo.pcapng` -- preserves the raw packets exactly as they were observed, allowing for repeatable analysis and deeper investigation at any time.

---

## Why Packet Analysis Matters

Network traffic analysis is one of the most fundamental skills in security operations. Firewalls, intrusion detection systems, and SIEMs all generate alerts, but when an analyst needs to understand the ground truth of what actually occurred on the network, they turn to packet captures. There is no substitute for reading the raw data.

Packet analysis is critical in the following areas:

- **Incident Response** -- When a security incident is detected, responders examine packet captures to determine the scope of a compromise, identify command-and-control communications, trace lateral movement, and reconstruct the timeline of an attack. Packets do not lie; they record exactly what was transmitted.

- **Threat Hunting** -- Proactive threat hunters use packet analysis to search for anomalous traffic patterns, unusual DNS queries, unexpected outbound connections, and protocol violations that may indicate the presence of an adversary who has evaded automated detection.

- **Digital Forensics** -- In forensic investigations, packet captures serve as evidence. They can reveal exfiltrated data, unauthorized access attempts, and the specific techniques an attacker used to compromise a system.

- **Network Troubleshooting** -- Beyond security, packet analysis is essential for diagnosing network performance issues, identifying misconfigurations, and verifying that systems are communicating as expected.

- **Compliance and Auditing** -- Organizations subject to regulatory requirements may need to demonstrate that network communications meet specific security standards, such as verifying that sensitive data is encrypted in transit.

Every cybersecurity professional benefits from the ability to open a packet capture and read it fluently.

---

## What Is in the Capture File

### File Details

| Property       | Value                                                        |
|----------------|--------------------------------------------------------------|
| File name      | `hoohoo.pcapng`                                              |
| Format         | PCAPNG (Packet Capture Next Generation)                      |
| File size      | ~9 KB                                                        |
| Purpose        | Live network traffic capture for protocol analysis           |

### About the PCAPNG Format

The `.pcapng` (Packet Capture Next Generation) format is the modern successor to the legacy `.pcap` format. It offers several advantages over its predecessor, including support for multiple capture interfaces in a single file, enhanced metadata storage (such as interface descriptions, capture comments, and name resolution records), and improved extensibility. PCAPNG is the default capture format used by current versions of Wireshark.

### Types of Traffic Typically Present in a Network Capture

Depending on the network environment and duration of the capture, the following protocol traffic may be observed in `hoohoo.pcapng`:

- **ARP (Address Resolution Protocol)** -- Layer 2 broadcasts used to resolve IP addresses to MAC addresses. ARP traffic reveals the hosts active on the local network segment.

- **DNS (Domain Name System)** -- Queries and responses that translate domain names to IP addresses. DNS traffic can reveal which websites and services were accessed during the capture window.

- **TCP Handshakes** -- The three-way handshake (SYN, SYN-ACK, ACK) that establishes reliable connections between hosts. Examining handshakes reveals connection patterns and timing.

- **HTTP/HTTPS Requests** -- Web traffic, including unencrypted HTTP requests (where full URLs, headers, and content are visible) and encrypted HTTPS sessions initiated with TLS handshakes.

- **TLS Handshakes** -- The negotiation process that establishes encrypted connections. Even though the payload is encrypted, the handshake itself reveals the server name (via SNI), supported cipher suites, and certificate information.

- **ICMP (Internet Control Message Protocol)** -- Ping requests and replies, traceroute packets, and error messages that provide insight into network reachability and path analysis.

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

The following table lists commonly used display filters for analyzing network traffic in Wireshark. These filters can be typed directly into the display filter bar.

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

Access via `Statistics > Protocol Hierarchy`. This view presents a tree-structured breakdown of every protocol detected in the capture. It shows the total number of packets, the percentage of total traffic, and the byte count for each protocol. This is typically the first place to look when opening an unfamiliar capture, as it immediately reveals the composition of the traffic.

### TCP Stream Reconstruction

Right-click any TCP packet and select `Follow > TCP Stream`. Wireshark reassembles the segments of a TCP conversation into a continuous data stream, displaying client-sent data in one color and server-sent data in another. This is invaluable for reading HTTP transactions, examining transferred files, and understanding application-layer exchanges.

### Conversation and Endpoint Analysis

Access via `Statistics > Conversations` and `Statistics > Endpoints`. The conversations view shows every unique communication pair along with packet counts, byte totals, and duration. The endpoints view summarizes all observed addresses. Together, these views answer the questions: who is talking to whom, and how much data is being exchanged.

### I/O Graphs

Access via `Statistics > I/O Graphs`. This feature plots packet or byte counts over time, making it easy to visualize traffic patterns, identify bursts of activity, and correlate events with specific time windows. Multiple filters can be graphed simultaneously for comparison.

### Export Objects

Access via `File > Export Objects > HTTP` (or other protocols). If the capture contains unencrypted file transfers, Wireshark can extract and save the transferred files. This is useful for recovering images, documents, executables, or other content transmitted over HTTP or SMB.

### Expert Information

Access via `Analyze > Expert Information`. Wireshark automatically flags potential issues such as TCP retransmissions, malformed packets, connection resets, and protocol errors. This is a quick way to identify problems without manually inspecting every packet.

---

## Skills Demonstrated

This project demonstrates the following cybersecurity and networking competencies:

- **Live Packet Capture** -- Configuring a network interface for packet capture and collecting traffic from a live network environment using Wireshark.

- **Protocol Identification and Analysis** -- Recognizing and interpreting common network protocols at multiple layers of the OSI model, including Ethernet (Layer 2), IP (Layer 3), TCP/UDP (Layer 4), and application-layer protocols such as HTTP, DNS, and TLS.

- **Wireshark Proficiency** -- Navigating the Wireshark interface, applying display filters, following streams, using statistical analysis tools, and interpreting packet-level details.

- **Network Forensics Fundamentals** -- Preserving captured traffic in standard formats for later analysis, reconstructing network conversations, and extracting meaningful intelligence from raw packet data.

- **Traffic Pattern Recognition** -- Identifying normal versus anomalous traffic patterns, understanding connection establishment and teardown sequences, and recognizing common protocol behaviors.

- **Security Analysis Mindset** -- Approaching network data with the analytical perspective required for SOC operations, incident response, and threat detection.

---

## Tools Used

| Tool        | Purpose                                              | Website                                      |
|-------------|------------------------------------------------------|----------------------------------------------|
| Wireshark   | Packet capture, protocol analysis, traffic inspection | [wireshark.org](https://www.wireshark.org)   |

Wireshark is the world's most widely used network protocol analyzer. It is free, open-source, and maintained by a global community of contributors. It is an essential tool in the toolkit of any cybersecurity professional.

---

## Author

**Chioma Iroka**
Computer Science Graduate | Cybersecurity Professional

This project is part of a cybersecurity portfolio demonstrating practical, hands-on experience with the tools and techniques used in network security operations.

- GitHub: [github.com/ChiomaIroka](https://github.com/ChiomaIroka)

---

## License

This project is licensed under the [MIT License](LICENSE).

You are free to use this project for educational and reference purposes. The packet capture file contains network traffic captured in a controlled environment. No sensitive or private data is intentionally included.

---

> **Note**: This project is intended for educational and professional portfolio purposes. All network traffic was captured on networks where the author had authorization to perform packet capture activities. Always ensure you have proper authorization before capturing network traffic on any network.
