
# Load Balancers, Flows & Sessions — Platform Engineering Notes

##  Why Load Balancers Exist
Single server → cannot handle scale → downtime/latency.

Client → Load Balancer → Multiple servers

Benefits:
- High availability  
- Horizontal scaling  
- Fault tolerance  
- Zero-downtime deployments  

---

#  OSI Layer Relevance for Load Balancers

ALB = Layer 7 (HTTP aware)  
NLB = Layer 4 (TCP/UDP)

---

# AWS Load Balancer Types

## ALB — Application Load Balancer (L7)
- Path-based routing  
- Host-based routing  
- TLS termination  
- WAF integration  
- Microservices behind one domain  

## NLB — Network Load Balancer (L4)
- Ultra-low latency  
- TCP/UDP  
- Static IP  
- Connection-level stickiness  
- Millions of connections  

## GWLB — Gateway Load Balancer
Used for:
- Firewalls  
- IDS/IPS  
- VPN appliances  

---

# Traffic Flow

Client  
→ DNS  
→ ALB/NLB  
→ Target group  
→ Pod/EC2  
→ Redis (session lookup)

---

#  Sticky Sessions Problem

User → Pod A → session stored  
Next request → Pod B → session missing ❌

### Fix
Store session in Redis:

User → any pod → Redis → session retrieved ✅

---

#  NLB Stickiness

Based on TCP 5‑tuple:
- srcIP
- srcPort
- dstIP
- dstPort
- protocol

All packets of same connection → same backend.

---

#  Flow Table (Conntrack)

Stores:
Client TCP connection → Target instance

Used by:
- NLB  
- NAT  
- Firewalls  
- kube‑proxy  

Linux commands:
conntrack -L  
/proc/net/nf_conntrack

Conntrack full error:
nf_conntrack: table full, dropping packet

Fix:
sysctl -w net.netfilter.nf_conntrack_max=262144

---

#  Redis vs Flow Table

Redis:
- App layer
- Stores sessions/cache
- TTL based
- Shared across pods

Flow table:
- Network layer
- Stores TCP connection state
- Timeout based
- Used for packet routing

---

#  TLS Termination (ALB)

Client → HTTPS → ALB  
ALB → HTTP/HTTPS → backend

Why?
- Inspect headers  
- Path routing  
- Attach WAF  
- Auth  

---

# 🚀 Microservices Behind One Domain

/api → service A  
/auth → service B  
/cart → service C  

---

#  ALB vs NLB Quick Matrix

Path routing → ALB  
Host routing → ALB  
WAF → ALB  
gRPC → ALB  

Static IP → NLB  
TCP/UDP → NLB  
Low latency → NLB  
Streaming/games → NLB  

Security appliances → GWLB

---

# ❓ Interview Q&A

Q: ALB vs NLB?  
ALB = L7 HTTP smart routing, NLB = L4 high‑performance TCP/UDP.

Q: Why TLS termination at ALB?  
To inspect HTTP and apply routing/WAF.

Q: How to avoid sticky sessions?  
Use Redis or shared session store.

Q: What is conntrack?  
Linux flow table tracking TCP connections.

Q: Why NLB for WebSockets/games?  
Long‑lived TCP + low latency.

Q: What happens when conntrack is full?  
Packets dropped → connection failures.

Q: What is a target group?  
Backend pool for routing + health checks.

Q: How does NLB choose target?  
Flow hash of 5‑tuple.

Q: GWLB use case?  
Firewall/VPN traffic inspection.

---

# Production Pattern

Client  
→ CloudFront (optional)  
→ ALB  
→ Kubernetes Service  
→ Pods  
→ Redis
