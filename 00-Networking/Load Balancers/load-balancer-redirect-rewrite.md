
# 🧠 Load Balancer Deep Notes — Full Definitions (Platform/SRE)

---
# 📌 CORE DEFINITIONS

## 🔹 Load Balancer
A load balancer distributes incoming traffic across multiple backend targets to improve availability, scalability, and fault tolerance.

---

## 🔹 Listener
A listener is a process on a load balancer that checks for connection requests using a specific protocol and port.

Example:
- HTTP :80
- HTTPS :443
- TCP :3306

It forwards traffic to a **target group** based on rules.

Interview one-liner:
> A listener defines how the load balancer accepts traffic.

---

## 🔹 Target Group
A logical group of backend resources (EC2, pods, IPs) that receives traffic from a load balancer.

Features:
- Health checks
- Connection draining
- Port mapping

---

## 🔹 Health Check
A periodic request sent by the load balancer to determine if a backend is healthy.

Unhealthy targets are removed from routing automatically.

---

# 🍪 SESSION AFFINITY (STICKY SESSIONS)

## Definition
Session affinity ensures that requests from the same client are routed to the same backend server.

### How ALB stickiness works
ALB uses a cookie (AWSALB) to bind a client to a target.

Flow:
Client → ALB → Pod A → Cookie set  
Next request → ALB reads cookie → Pod A

### Problems
- Uneven load
- Pod failure loses session
- Breaks autoscaling

### Solution
Use Redis or shared session store → stateless services.

Interview one-liner:
> Stickiness binds a client to a backend using cookies.

---

# 🔀 REDIRECT vs REWRITE

## 🔹 Redirect
A redirect tells the **client** to go to a different URL.

HTTP status codes:
- 301 (permanent)
- 302 (temporary)

Flow:
Client → ALB → response 301 → Client requests new URL

Example:
http://example.com → https://example.com

---

## 🔹 Rewrite
A rewrite changes the URL **internally** without telling the client.

Client still sees original URL.

Example:
Client requests /api  
ALB/Nginx internally forwards to /v1/api

---

## Difference

| Feature | Redirect | Rewrite |
|--------|---------|---------|
Client aware | Yes | No |
Extra round trip | Yes | No |
URL changes in browser | Yes | No |
Use case | HTTP→HTTPS | Internal routing |

Interview one-liner:
> Redirect changes the URL for the client; rewrite changes it internally.

---

# 🎧 LISTENERS IN DETAIL

A listener:
- Binds protocol + port
- Evaluates rules
- Forwards to target group

Example ALB listeners:

Listener 1:
HTTPS :443  
Rule: /api → TG-API  

Listener 2:
HTTPS :443  
Rule: /auth → TG-AUTH  

Default rule → TG-WEB

---

# 🧱 RULE EVALUATION ORDER (ALB)

1. Host-based rules
2. Path-based rules
3. Header rules
4. Default rule

First match wins.

---

# 🔐 TLS TERMINATION

TLS termination happens at the listener level.

Listener:
HTTPS :443 → decrypt → forward HTTP to backend

Benefits:
- Offloads CPU from backend
- Enables L7 routing
- Allows WAF inspection

---

# 🔁 FULL TRAFFIC FLOW (ALB)

Client  
→ DNS  
→ ALB listener :443  
→ Rule match (/api)  
→ Target group (API pods)  
→ Pod  

---

# ⚡ SESSION FLOW WITH REDIS (STATELESS)

Client → ALB → Pod A → Redis (store session)  
Next request → ALB → Pod B → Redis (fetch session)

No stickiness required.

---

# 🔵 SESSION FLOW WITH STICKINESS

Client → ALB → Pod A → Cookie set  
Next request → ALB → Pod A (forced)

Uneven load risk.

---

# 🧠 INTERVIEW Q&A

## Q: What is a listener?
A listener defines the protocol and port on which the load balancer accepts traffic and routes it using rules.

## Q: What is a target group?
A target group is a pool of backend instances that receive traffic and are monitored by health checks.

## Q: What is stickiness?
Stickiness binds a client to a specific backend using cookies.

## Q: Redirect vs rewrite?
Redirect is client-side URL change; rewrite is internal routing.

## Q: Why avoid sticky sessions?
They break autoscaling and cause uneven load.

## Q: How to make apps stateless?
Store sessions in Redis.

---

# 🏁 MEMORY MODEL

Listener → accepts traffic  
Rule → decides routing  
Target group → backend pool  
Health check → removes bad targets  
Stickiness → cookie binding  
Redis → stateless sessions  
Redirect → client URL change  
Rewrite → internal path change  

---
