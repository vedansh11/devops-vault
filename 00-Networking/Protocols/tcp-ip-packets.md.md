# TCP/IP Packets! 
![[tcp-header-format 1.webp]]
DO- Data Offset
## What is a Packet?

In networking, a **packet** is a small unit of data sent over a network.  
Large data (images, files, videos, API responses) is split into packets, transmitted independently, and reassembled at the destination.

This enables:
- Efficient sharing of network links
- Parallel communication between many devices
- Reliability and error recovery

The Internet is a **packet-switched network**, meaning packets are routed independently and may take different paths to the same destination.

---

## Why Packets Are Used

Sending data as one continuous stream would:
- Block the network for other devices
- Fail completely if interrupted
- Be inefficient at scale

Packet-based communication allows:
- Multiple devices to share the same network
- Lost data to be retransmitted selectively
- Better congestion handling

---

## Packet Structure

A packet generally consists of:

- **Header** – control and routing information
- **Payload** – actual application data

Some protocols also add:
- **Trailer / Footer** – integrity or security data

---

## Packet Headers

Headers contain metadata required to process and deliver the packet correctly.

Typical information includes:
- Source and destination addresses
- Ordering information
- Error detection
- Control flags

Packets often have **multiple headers**, added by different protocol layers.

At minimum on the Internet:
- **IP header** (routing)
- **TCP or UDP header** (transport)

---

## What is an IP Packet?

An **IP packet** is a packet with an Internet Protocol header.

IP is responsible for:
- Logical addressing
- Routing between networks
- Fragmentation and reassembly

IP is **connectionless** and does not guarantee delivery or order.

---

## IPv4 Packet Header (Key Fields)

### Version
- Identifies IPv4 or IPv6

### Header Length
- Size of the IP header

### Type of Service (ToS)
- Traffic priority and QoS hints

### Total Length
- Entire packet size (header + payload)

### Identification
- Used to reassemble fragmented packets

### Flags
- Control fragmentation
- Includes “Don’t Fragment” flag

### Fragment Offset
- Position of fragment in original packet

### Time To Live (TTL)
- Prevents infinite routing loops
- Decremented at each hop

### Protocol
- Indicates next-layer protocol
- TCP = 6, UDP = 17

### Header Checksum
- Detects corruption in IP header

### Source IP Address
- Sender’s logical address

### Destination IP Address
- Receiver’s logical address

### Options
- Rarely used (routing, timestamps, security)

---

## TCP Packet (TCP Segment)

TCP provides **reliable, ordered, connection-oriented** communication.

---

## TCP Header Fields (Important Ones)

### Source Port
- Identifies sending application

### Destination Port
- Identifies receiving application

### Sequence Number
- Byte position of data in stream
- Enables ordering and retransmission

### Acknowledgment Number
- Confirms received data
- Indicates next expected byte

### Data Offset
- Size of TCP header

### Control Flags

- **SYN** – start connection
- **ACK** – acknowledge data
- **FIN** – graceful close
- **RST** – force reset connection
- **PSH** – push data immediately
- **URG** – urgent data present

### Window Size
- Flow control mechanism
- Limits how much data sender can transmit

### Checksum
- Detects corruption of header and data

### Urgent Pointer
- Marks urgent data position

### Options
- MSS (Maximum Segment Size)
- Window scaling
- Timestamps
- SACK (Selective Acknowledgment)

### Data Payload
- Actual application data

---

## TCP vs IP Responsibilities

| Layer | Responsibility |
|-----|---------------|
| IP | Addressing & routing |
| TCP | Reliability, order, flow control |

---

## Packets vs Datagrams

- **Datagram**: Independent unit of transmission
- **Packet**: Common term for network datagrams

In practice, the terms are often used interchangeably.

---

## Network Traffic

**Network traffic** is the flow of packets through a network.

Types:
- Legitimate traffic (apps, APIs, browsing)
- Malicious traffic (DDoS, scanning, exploits)

---

## Malicious Packet-Based Traffic

Examples:
- SYN floods (exhaust TCP state)
- Fragmentation attacks
- Malformed packets
- Reflection/amplification attacks

---

## Practical Observations (Very Important)

### SYN Flood
- Excessive SYN packets
- Exhausts server connection table

### TCP RST
- Abrupt connection termination
- Often caused by firewalls, timeouts, app crashes

### Zero Window
- Receiver cannot accept more data
- Indicates slow application or resource pressure

### Packet Loss
- Causes retransmissions
- Increases latency

---

## Practical Tools That Expose These Fields

- `tcpdump`
- `wireshark`
- Load balancer logs
- Firewall logs
- Cloud network flow logs

---

## Key Takeaway

You do not debug networks by memorizing bits.
You debug networks by understanding:
- Why packets exist
- What headers control
- How TCP/IP reacts under stress
