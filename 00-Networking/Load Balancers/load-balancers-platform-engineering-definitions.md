
# Load Balancers, Sessions, Redis & Conntrack — Interview Revision Notes

---
#  CORE DEFINITIONS (Say this in interviews)

## 🔹 Load Balancer
A load balancer distributes incoming traffic across multiple backend servers to ensure high availability, scalability, and fault tolerance.

One-liner:
> A load balancer prevents a single server from becoming a bottleneck by distributing traffic.

---

## 🔹 ALB (Application Load Balancer)
An L7 load balancer that makes routing decisions based on HTTP attributes like path, host, headers, and cookies.

Key points:
- HTTP aware
- TLS termination
- Path-based routing
- Used for microservices

One-liner:
> ALB is used when routing decisions depend on HTTP data.

---

## 🔹 NLB (Network Load Balancer)
An L4 load balancer that routes traffic based on IP and port using a flow hash. It does not inspect HTTP.

Key points:
- Ultra low latency
- TCP/UDP
- Static IP
- Long-lived connections

One-liner:
> NLB is used for high-performance TCP/UDP traffic.

---

## 🔹 GWLB (Gateway Load Balancer)
Used to route traffic to security appliances like firewalls, IDS/IPS, and VPNs.

One-liner:
> GWLB is for security appliance load balancing.

---

# TLS TERMINATION (ALB)

Definition:
TLS termination means the load balancer decrypts HTTPS traffic, inspects it, and forwards it to backend servers.

Why?
- Read HTTP headers
- Path routing
- Attach WAF
- Authentication

Flow:
Client → HTTPS → ALB → HTTP/HTTPS → Backend

---

# STICKY SESSIONS

Definition:
Sticky sessions force a user to always connect to the same backend server.

Problem:
- Uneven load
- Breaks autoscaling
- Pod failure = session loss

One-liner:
> Stickiness binds a user to one server, which hurts scalability.

---

#  STATELESS ARCHITECTURE

Definition:
Stateless services store session data outside the application (e.g., Redis) so any pod can handle any request.

One-liner:
> Stateless apps scale horizontally because session state is externalized.

---

# REDIS

Definition:
Redis is an in-memory key–value data store used for caching, session storage, queues, and rate limiting.

Why Redis for sessions?
Without Redis:
User → Pod A → session stored in memory → Pod B → session missing ❌

With Redis:
User → Any pod → Redis → session retrieved ✅

Key properties:
- In-memory (fast)
- TTL support
- Shared across pods
- Enables stateless services

Interview one-liner:
> Redis externalizes session state so any pod can serve any request.

---

# FLOW TABLE (CONNTRACK)

Definition:
A flow table tracks active TCP/UDP connections so return packets go to the correct backend.

Stores:
Client IP:Port → Load balancer → Backend IP:Port

Used by:
- NLB
- NAT gateway
- Firewalls
- kube-proxy

Linux commands:
conntrack -L
/proc/net/nf_conntrack

One-liner:
> Conntrack is the kernel flow table that tracks TCP connections.

---

#  NLB STICKINESS (CONNECTION LEVEL)

NLB uses a flow hash based on the TCP 5-tuple:
- Source IP
- Source port
- Destination IP
- Destination port
- Protocol

All packets of the same connection → same backend.

---

# CONNTRACK FULL ISSUE

Error:
nf_conntrack: table full, dropping packet

Impact:
- New connections fail
- Timeouts
- 502/504 errors

Fix:
sysctl -w net.netfilter.nf_conntrack_max=262144

---

#  REDIS vs FLOW TABLE

| Feature | Redis | Flow table |
|--------|-------|-----------|
Layer | Application | Network |
Purpose | Session storage | Connection tracking |
Shared | Yes | No |
TTL | App-controlled | Kernel timeout |
Scales | Horizontally | Limited by kernel memory |

Memory trick:
Redis = user session memory  
Conntrack = TCP connection memory  

---

# 🔗 REVERSE PROXY

Definition:
A reverse proxy sits in front of backend servers and forwards client requests to them.

Used for:
- TLS termination
- Caching
- Compression
- Auth
- HTTP routing

Examples:
Nginx, Envoy, HAProxy

One-liner:
> Reverse proxy is software L7 routing in front of apps.

---

# TRAFFIC FLOW (PRODUCTION)

Client  
→ DNS  
→ ALB/NLB  
→ Target group  
→ Pod/EC2  
→ Redis (session lookup)

---

# ⚖️ ALB vs NLB DECISION TABLE

| Requirement | Use |
|------------|-----|
Path routing | ALB |
Host routing | ALB |
WAF | ALB |
gRPC | ALB |
Static IP | NLB |
TCP/UDP | NLB |
Low latency | NLB |
Streaming/games | NLB |
Security appliances | GWLB |

---

#  LOAD BALANCING ALGORITHMS

- Round robin
- Weighted round robin
- Least connections
- IP hash

ALB → HTTP aware routing  
NLB → Flow hash routing  

---

#  TARGET GROUP

Definition:
A logical group of backend servers that a load balancer routes traffic to.

Also used for:
- Health checks
- Connection draining

---

# 🏁 FINAL MENTAL MODEL

ALB → HTTP brain (smart routing)  
NLB → TCP pipe (fast routing)  
GWLB → Security chain  
Redis → Session memory  
Conntrack → Connection memory  

---

#  RAPID INTERVIEW Q&A

Q: ALB vs NLB?  
ALB = L7 HTTP routing, NLB = L4 TCP/UDP high performance.

Q: Why TLS termination at ALB?  
To inspect HTTP and apply routing/WAF.

Q: How to avoid sticky sessions?  
Use Redis or shared session store.

Q: What is conntrack?  
Linux flow table tracking TCP connections.

Q: Why NLB for WebSockets/games?  
Long-lived TCP + low latency.

Q: What happens when conntrack is full?  
Packets dropped → connection failures.

Q: What is Redis used for?  
Caching, sessions, queues, rate limiting.

Q: What is a stateless service?  
A service that stores no session data locally.

Q: What is a target group?  
Backend pool used by load balancer.

---
