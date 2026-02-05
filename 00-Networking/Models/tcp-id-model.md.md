# TCP/IP Model

## What is TCP/IP?

TCP/IP (Transmission Control Protocol / Internet Protocol) is the fundamental networking model used to transmit data across interconnected networks, including the Internet.

It defines:
- how data is addressed
- how it is packaged into packets
- how it is transmitted
- how it is routed
- how it is verified and reassembled

TCP/IP is the **actual model implemented in operating systems and network devices**, unlike OSI which is mainly conceptual.

---

## IP and TCP Overview

### Internet Protocol (IP)

IP is responsible for:
- logical addressing
- packet delivery between source and destination
- routing packets across multiple networks

Characteristics:
- connectionless
- best-effort delivery
- no guarantee of order
- no retransmission

IP does **not** handle:
- packet ordering
- error correction
- retransmission

---

### Transmission Control Protocol (TCP)

TCP works **on top of IP** to provide reliability.

Responsibilities:
- connection establishment
- packet ordering
- error detection
- retransmission of lost data
- flow control

Analogy:
- IP delivers puzzle pieces
- TCP assembles them correctly

---

## TCP Three-Way Handshake

TCP establishes a connection using a three-step handshake:

1. **SYN** – Client requests connection
2. **SYN-ACK** – Server acknowledges and agrees
3. **ACK** – Client confirms

Only after this handshake does data transmission begin.

Why this matters:
- prevents half-open connections
- synchronizes sequence numbers
- negotiates flow control parameters

---

## TCP/IP Model Layers

TCP/IP follows a **4-layer architecture**:

| Layer | Purpose |
|------|--------|
| Application | User-facing network services |
| Transport | End-to-end communication |
| Internet | Addressing and routing |
| Network Access | Physical transmission |

---

## 1. Application Layer

![[application-layer.gif]]
### Responsibilities

- Provides network services to applications
- Defines communication protocols
- Handles data formatting and interpretation
- Often includes encryption and session management

This layer maps to OSI:
- Application
- Presentation
- Session

---

### Common Application Layer Protocols

| Protocol | Purpose |
|-------|--------|
| HTTP / HTTPS | Web communication |
| DNS | Domain name resolution |
| SMTP | Sending email |
| IMAP / POP3 | Receiving email |
| FTP / SFTP | File transfer |
| SSH | Secure remote access |
| NTP | Time synchronization |

---

### Practical Notes

Issues at this layer often appear as:
- failed API calls
- TLS handshake errors
- authentication failures
- malformed requests

---

## 2. Transport Layer

### Responsibilities

- Segmentation and reassembly
- Reliable or fast delivery
- Port-based multiplexing
- Flow control

---

### TCP (Transmission Control Protocol)

Features:
- connection-oriented
- reliable delivery
- ordered packets
- congestion control

Key mechanisms:
- sequence numbers
- acknowledgments
- retransmissions
- sliding window

Common use cases:
- web traffic (HTTP/HTTPS)
- SSH
- file transfers
- databases

---

### UDP (User Datagram Protocol)

Features:
- connectionless
- no retransmission
- no ordering
- low latency

Common use cases:
- DNS queries
- video streaming
- VoIP
- online gaming
- metrics and telemetry

---

## 3. Internet Layer

### Responsibilities

- logical addressing using IP
- packet routing
- fragmentation and reassembly
- inter-network communication

---

### Protocols at Internet Layer

| Protocol | Purpose |
|--------|--------|
| IP | Packet addressing and routing |
| ICMP | Error reporting and diagnostics |
| ARP | Mapping IP to MAC addresses |

---

### Practical Notes

- Routing loops and black holes occur here
- ICMP is used by tools like `ping` and `traceroute`
- MTU issues cause fragmentation problems

---

## 4. Network Access (Link) Layer

### Responsibilities

- physical data transmission
- framing
- MAC addressing
- error detection
- media access control

---

### Examples

- Ethernet
- Wi-Fi
- ARP
- VLANs

---

### Practical Notes

- Switches operate at this layer
- MAC address conflicts happen here
- Packet loss due to collisions or cable issues

---

## TCP Packet Structure (Important Fields)

### Key Fields to Understand

- **Source Port**
- **Destination Port**
- **Sequence Number**
- **Acknowledgment Number**
- **Flags**
  - SYN
  - ACK
  - FIN
  - RST
- **Window Size**
- **Checksum**

---

### Why These Fields Matter

- SYN flood attacks exploit handshake
- RST packets cause forced connection drops
- Zero window causes slow or stalled applications
- Sequence errors reveal packet loss or reordering

---

## TCP Flags Explained

| Flag | Meaning |
|----|--------|
| SYN | Start connection |
| ACK | Acknowledge data |
| FIN | Graceful close |
| RST | Forceful reset |

---

## Flow Control and Congestion Control

### Flow Control
- prevents sender from overwhelming receiver
- implemented via window size

### Congestion Control
- prevents network overload
- TCP adapts sending rate dynamically

---

## Practical Debugging Tools

- `tcpdump`
- `wireshark`
- `ss`
- `netstat`
- `ping`
- `traceroute`

Used to analyze:
- packet loss
- latency
- retransmissions
- handshake failures

---

## IPv4 vs IPv6

### IPv4
- 32-bit addressing
- limited address space

### IPv6
- 128-bit addressing
- massive address availability
- simplified routing
- improved multicast support

---

## Why TCP/IP Is Used Instead of OSI

![[osi-to-tcp.webp]]
- simpler structure
- protocol-driven design
- widely implemented
- practical and flexible
- foundation of the Internet

OSI is useful for learning concepts, but TCP/IP is used for real networking.

---

## Summary

TCP/IP defines how data:
- is created
- is transmitted
- is routed
- is verified
- reaches applications reliably

Understanding TCP/IP is essential for:
- debugging network issues
- designing scalable systems
- diagnosing performance problems
- securing network communication

Working of Tcp 

![[working-of-tcp.webp]]