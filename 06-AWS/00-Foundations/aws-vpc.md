 
# AWS VPC – Virtual Private Cloud (Foundations)

## What is a VPC
A **Virtual Private Cloud (VPC)** is a logically isolated virtual network within AWS where you can launch AWS resources in a controlled and secure manner.  
It provides full control over networking, including IP addressing, routing, security, and traffic flow.

Think of a VPC as **your own private data center network inside AWS**, isolated from other customers at the network layer.

---

## Why VPC is Required
Without a VPC:
- Resources from different customers could share flat networking
- Isolation and security would be weak
- Traffic control would be limited
- Compliance requirements could not be met

With a VPC:
- Strong network isolation
- Fine-grained traffic control
- Secure application design
- Compliance-ready architectures
- Predictable IP management

VPC is **mandatory for production-grade AWS workloads**.

---

## Core Building Blocks of a VPC

### 1. CIDR Block (IP Address Range)
- Defines the size of the VPC
- Example: `172.16.0.0/16`
- Determines how many private IPs are available

Key points:
- Cannot overlap with other VPCs if peered
- Chosen at creation time
- Impacts subnet design and scalability

---

### 2. Subnets
A **subnet** is a subdivision of a VPC’s CIDR block.

Example:
- VPC: `172.16.0.0/16`
- Subnet A: `172.16.1.0/24`
- Subnet B: `172.16.2.0/24`
- Subnet C: `172.16.3.0/24`

Characteristics:
- Tied to **one Availability Zone**
- Used to logically separate workloads
- Can be **public** or **private**

---

### 3. Public Subnet
A subnet is public when:
- It has a route to an **Internet Gateway**
- Resources inside can be accessed from the internet

Common resources:
- Load Balancers
- Bastion hosts
- NAT Gateways

---

### 4. Private Subnet
A subnet is private when:
- It has **no direct route to the Internet Gateway**
- Resources cannot be directly accessed from the internet

Common resources:
- Application servers
- Databases
- Internal services

Private subnets are the **default choice for secure architectures**.

---

### 5. Internet Gateway (IGW)
- Enables internet access for the VPC
- Horizontally scaled and managed by AWS
- Required for inbound and outbound internet traffic

Rules:
- One IGW per VPC
- Attached at the VPC level
- Used by public subnets

---

### 6. Route Tables
Route tables define **how traffic is routed**.

Examples:
- `0.0.0.0/0 → Internet Gateway` (public access)
- `0.0.0.0/0 → NAT Gateway` (private outbound access)

Each subnet:
- Must be associated with one route table
- Uses longest-prefix match

---

### 7. Load Balancers
Placed in **public subnets**, they:
- Accept traffic from the internet
- Forward requests to private targets

Types:
- Application Load Balancer (Layer 7)
- Network Load Balancer (Layer 4)

Load balancers **never sit inside private subnets only** for internet-facing use cases.

---

### 8. Target Groups
- Define backend resources for a load balancer
- Can include EC2 instances, IPs, or containers
- Health checks are mandatory

Target groups decouple traffic routing from instances.

---

### 9. Security Groups
Stateful virtual firewalls attached to:
- EC2 instances
- Load balancers
- ENIs

Characteristics:
- Allow rules only (implicit deny)
- Return traffic automatically allowed
- Evaluated at instance level

Used for **fine-grained application security**.

---

### 10. Network ACLs (NACLs)
Stateless firewalls at subnet level.

Characteristics:
- Allow and deny rules
- Evaluated in order
- Apply to all resources in a subnet

Used for **coarse-grained network protection**.

---

### 11. NAT Gateway
Allows **private subnets to access the internet** securely.

Key points:
- Placed in a public subnet
- Uses Elastic IP
- Prevents exposure of private IPs
- Supports outbound traffic only

Essential for:
- OS updates
- Package downloads
- External API access

---

### 12. Elastic IP (EIP)
- Static public IPv4 address
- Used by NAT Gateways and public services
- Remains constant across restarts

---

### 13. VPC Flow Logs
- Capture network traffic metadata
- Used for debugging and security analysis
- Can be sent to CloudWatch or S3

Important:
- Chargeable
- Does not capture payloads

---

## End-to-End Traffic Flow (Internet to Private EC2)

1. User sends request from internet
2. Request hits Internet Gateway
3. Enters public subnet
4. Reaches Load Balancer
5. Routed using route tables
6. Forwarded to private subnet
7. Allowed by Security Group
8. Processed by EC2 application

---

## Outbound Traffic from Private Subnet

1. EC2 sends request to internet
2. Routed to NAT Gateway
3. NAT Gateway masks private IP
4. Internet receives request from public IP
5. Response returns via NAT Gateway

---

## Availability Zones & High Availability
- Each subnet belongs to one AZ
- Production VPCs use **multiple AZs**
- Load balancers span AZs
- Failure of one AZ does not cause outage

---

## Design Best Practices
- Use private subnets for workloads
- Expose only load balancers
- Minimize public IPs
- Separate tiers
- Plan CIDR carefully
- Enable flow logs for visibility

---

## Summary
VPC is the **foundation of AWS networking** and critical for secure, scalable cloud architectures.
