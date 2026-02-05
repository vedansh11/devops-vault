
# OSI Model

## What is the OSI Model?

The OSI (Open Systems Interconnection) model is a **conceptual framework** that describes how data moves through a network using **seven logical layers**.

It was defined by the International Organization for Standardization (ISO) to allow different systems, vendors, and technologies to communicate using common rules.

The OSI model is not an implementation guide like TCP/IP — it is a **diagnostic and learning model**.

---

## Why the OSI Model Matters

The OSI model helps to:
- understand how networking works step by step
- isolate and troubleshoot network issues
- communicate problems clearly between teams
- reason about security attacks and failures

Instead of debugging “the network”, you debug **one layer at a time**.

Example:
- DNS not resolving → Layer 7
- TLS handshake failing → Layer 6
- Connection drops → Layer 4
- Routing issue → Layer 3
- ARP/MAC issue → Layer 2
- Cable unplugged → Layer 1

---

## OSI Model Layers (Top to Bottom)
![[osi_model_7_layers.png]]

| Layer | Name |
|-----|------|
| 7 | Application |
| 6 | Presentation |
| 5 | Session |
| 4 | Transport |
| 3 | Network |
| 2 | Data Link |
| 1 | Physical |

---

## Layer 7: Application Layer

### Responsibility
- Defines **network services used by applications**
- Handles request/response logic
- Defines how data is requested and returned

Important:  
Applications (browser, mail client) are **not** the layer — the **protocols** are.

---

### Common Protocols
- HTTP / HTTPS
- DNS
- SMTP
- FTP / SFTP
- SSH
- DHCP

---

### Practical Examples
- API returning 500 errors
- DNS returning NXDOMAIN
- Authentication failures
- Malformed HTTP requests

---

## Layer 6: Presentation Layer

### Responsibility
- Data translation
- Encryption / decryption
- Compression / decompression

This layer ensures data is **readable and secure** for applications.

---

### Examples
- TLS / SSL
- Data encoding (UTF-8, ASCII)
- Image formats (JPEG, PNG)
- Media formats (MP3, MPEG)

---

### Practical Examples
- TLS handshake failures
- Certificate mismatch
- Unsupported cipher suites
- Data corruption due to encoding mismatch

---

## Layer 5: Session Layer

### Responsibility
- Session establishment
- Session maintenance
- Session termination
- Synchronization checkpoints

---

### Key Concepts
- Session lifetime
- Checkpoints for resumable transfers
- Dialog control (half-duplex / full-duplex)

---

### Practical Examples
- Dropped SSH sessions
- Database session timeouts
- Incomplete file transfers
- Re-authentication loops

---

## Layer 4: Transport Layer

### Responsibility
- End-to-end communication
- Segmentation and reassembly
- Flow control
- Error control

---

### Protocols
- TCP (reliable, ordered)
- UDP (fast, unreliable)

---

### Key Concepts
- Source port / destination port
- Sequence numbers
- Acknowledgments
- Retransmissions
- Window size

---

### Practical Examples
- Connection timeouts
- Packet retransmissions
- Slow applications due to small window size
- SYN flood attacks
- RST-based connection drops

---

## Layer 3: Network Layer

### Responsibility
- Logical addressing
- Routing between networks
- Packet forwarding

---

### Protocols
- IP (IPv4 / IPv6)
- ICMP
- IPsec
- OSPF, BGP (routing protocols)

---

### Practical Examples
- Incorrect routing tables
- Subnet misconfiguration
- ICMP unreachable errors
- Traffic going to wrong network

---

## Layer 2: Data Link Layer

### Responsibility
- Node-to-node delivery
- Framing
- MAC addressing
- Error detection
- Media access control

---

### Sublayers
- LLC (Logical Link Control)
- MAC (Media Access Control)

---

### Devices
- Switches
- Bridges
- Network Interface Cards (NICs)

---

### Practical Examples
- ARP failures
- MAC address conflicts
- VLAN misconfiguration
- Packet loss within same subnet

---

## Layer 1: Physical Layer

### Responsibility
- Physical transmission of bits
- Electrical / optical signaling
- Physical media specification

---

### Examples
- Ethernet cables
- Fiber optics
- Wi-Fi radio signals
- Voltage levels

---

### Practical Examples
- Cable unplugged
- Bad NIC
- Signal interference
- Hardware failure

---

## How Data Flows Through OSI

### Sending Side
1. Application creates data
2. Presentation formats/encrypts
3. Session establishes connection
4. Transport segments data
5. Network creates packets
6. Data Link frames packets
7. Physical transmits bits

### Receiving Side
Same process in **reverse order**.

---

## OSI vs TCP/IP Mapping

| OSI Layer | TCP/IP Equivalent |
|---------|------------------|
| Application | Application |
| Presentation | Application |
| Session | Application |
| Transport | Transport |
| Network | Internet |
| Data Link | Network Access |
| Physical | Network Access |

---

## OSI Model Usage in Troubleshooting

Typical approach:
1. Check Layer 1 (link up?)
2. Check Layer 2 (ARP, MAC, VLAN)
3. Check Layer 3 (IP, routing)
4. Check Layer 4 (ports, TCP state)
5. Check Layer 7 (application errors)



---

## Summary

- OSI is a **conceptual model**
- TCP/IP is the **implemented model**
- OSI helps isolate problems
- Each layer has a clear responsibility
- Real-world debugging maps cleanly to OSI layers
