
---
#  CORE DEFINITIONS

##  Reverse Proxy
A reverse proxy is a server that sits in front of backend servers and forwards client requests to them.

It hides backend servers and provides:
- TLS termination
- Authentication
- Caching
- Compression
- HTTP routing
- Rate limiting

Examples:
- Nginx
- Envoy
- HAProxy
- Traefik

One-liner:
> A reverse proxy is an L7 traffic manager in front of applications.

---

##  Load Balancer
A load balancer distributes incoming traffic across multiple backend servers to ensure high availability and scalability.

One-liner:
> A load balancer spreads traffic across multiple instances.

---

#  KEY RELATIONSHIP

An ALB is:
- A managed L7 reverse proxy
- With built-in load balancing
- With cloud-native scaling

A reverse proxy:
- May or may not load balance
- Is self-managed

Interview one-liner:
> ALB is a managed L7 reverse proxy with load balancing and auto scaling.

---

# ⚖️ REVERSE PROXY vs ALB

| Feature | Reverse Proxy (Nginx) | ALB |
|--------|----------------------|-----|
Managed by cloud | ❌ | ✔ |
Auto scaling | ❌ | ✔ |
Multi-AZ | ❌ | ✔ |
Load balancing | Optional | Always |
Health checks | Manual config | Built-in |
WAF integration | Manual | Native |
TLS termination | ✔ | ✔ |
Cost model | EC2 cost | Pay per use |
Ops overhead | High | Low |

---

#  TLS TERMINATION FLOW

Client → HTTPS → ALB/Nginx → HTTP/HTTPS → Backend

Why terminate TLS at proxy?
- Read headers
- Path routing
- Attach WAF
- Offload CPU from backend

---

#  TRAFFIC FLOW COMPARISON

## Reverse Proxy (Self-managed)

Client  
→ Nginx (EC2/VM)  
→ App servers  

Scaling:
- You must scale Nginx manually
- Add another proxy and use DNS/ELB in front

---

## ALB (Managed)

Client  
→ ALB (multi-AZ, auto scaled)  
→ Target group  
→ Pods/EC2  

No proxy management required.

---

#  MICROservices BEHIND SINGLE DOMAIN

ALB:

api.example.com/users → user service  
api.example.com/orders → order service  
api.example.com/payments → payment service  

This is **path-based routing**.

Reverse proxy (Nginx) can do the same but you must:
- Configure routes
- Reload config
- Handle scaling

---

# REAL-LIFE ARCHITECTURES

## Small Startup

Client → Nginx → App  

Reason:
- Cheap
- Simple
- Single region

---

## Production Kubernetes

Client → CloudFront → ALB → Ingress → Service → Pods  

Reason:
- Auto scaling
- HA across AZ
- Managed TLS
- WAF

---

#  WHEN TO USE REVERSE PROXY

Use Nginx/Envoy when:
- Inside Kubernetes (Ingress controller)
- Need custom routing logic
- Need caching at edge
- Need service mesh sidecar

---

#  WHEN TO USE ALB

Use ALB when:
- Internet-facing apps
- Need WAF
- Need auto scaling
- Need multi-AZ HA
- Don’t want to manage proxies

---

#  PERFORMANCE DIFFERENCE

Nginx:
- Very fast
- But single node unless scaled manually

ALB:
- Slightly higher latency than local Nginx
- But horizontally scaled and HA

---

#  COMMON CONFUSION

❓ Is reverse proxy same as load balancer?

Answer:
- Reverse proxy = request forwarder
- Load balancer = distributes across multiple backends

They overlap but are not identical.

---

#  CAN NGINX DO LOAD BALANCING?

Yes.

Nginx can:
- Round robin
- Least connections
- IP hash

But:
- You manage scaling
- You handle failover
- No multi-AZ by default

---

ALB → External entry point (managed L7 LB)  
Ingress (Nginx/Envoy) → Internal reverse proxy inside cluster  
Service → L4 load balancing  
Pod → Application  

---

# ❓ INTERVIEW Q&A

Q: Is ALB a reverse proxy?
Yes. ALB is a managed L7 reverse proxy with load balancing.

Q: Can reverse proxy exist without load balancing?
Yes. Nginx in front of one app is still a reverse proxy.

Q: Can reverse proxy do load balancing?
Yes, but you must manage scaling and HA.

Q: Why use ALB instead of Nginx?
Managed, auto scaling, multi-AZ, WAF, no ops overhead.

Q: Where do we use Nginx then?
Inside Kubernetes as Ingress or for internal routing/caching.

---

# 🏁 FINAL MEMORY TRICK

Reverse proxy → software traffic controller  
ALB → managed cloud reverse proxy + load balancer  

Nginx = you manage  
ALB = AWS manages  

---
