# AWS EC2 (Elastic Compute Cloud)

## Definition
Amazon EC2 (Elastic Compute Cloud) is a service that provides resizable compute capacity in the cloud. It allows you to provision virtual machines (instances) with configurable CPU, memory, storage, and networking.

In simple terms, **EC2 = Virtual Servers in AWS**.

---

## Why EC2 Exists (The Problem It Solves)

### Traditional (On‑Prem) Challenges
- Buying physical servers is expensive
- Capacity planning is hard (over‑provisioning vs under‑provisioning)
- Manual maintenance (patching, upgrades, failures)
- Scaling requires hardware purchase
- High operational overhead

### EC2 Advantages
- No hardware management
- Pay‑as‑you‑go pricing
- Elastic scaling (up/down)
- Global availability
- High availability via AZs
- Fast provisioning (minutes, not weeks)

---

## What Does “Elastic” Mean?
Elastic means:
- You can increase/decrease CPU, RAM, storage
- You can start/stop instances
- You can scale horizontally or vertically
- You only pay for what you use

---

## EC2 = Compute
An EC2 instance is a combination of:
- CPU
- Memory (RAM)
- Storage (EBS)
- Network interface
- Operating system

---

## Virtualization Model
AWS uses a hypervisor to:
- Run multiple virtual machines on physical servers
- Isolate tenants securely
- Share resources efficiently

You never interact with physical hardware.

---

## EC2 Instance Types

### General Purpose
Balanced CPU and memory  
Use cases: Web apps, APIs, dev/test  
Examples: t2, t3, t4g

### Compute Optimized
High CPU-to-memory ratio  
Use cases: Gaming servers, ML inference  
Examples: c5, c6g

### Memory Optimized
Large RAM capacity  
Use cases: Databases, in-memory caching  
Examples: r5, x1

### Storage Optimized
High IOPS  
Use cases: Data warehouses, logs  
Examples: i3, d2

### Accelerated Computing
GPU / FPGA based  
Use cases: ML training, rendering  
Examples: p3, g4

---

## Regions
A region is a geographical location with AWS data centers.

Examples:
- us-east-1
- eu-central-1
- ap-south-1

Reasons:
- Latency
- Compliance
- Disaster recovery

---

## Availability Zones (AZs)
- Isolated data centers within a region
- Used for high availability
- Example: ap-south-1a, ap-south-1b

---

## High Availability
- Deploy across multiple AZs
- Use load balancers
- Prevent single point of failure

---

## AMI (OS Selection)
- Amazon Linux
- Ubuntu
- RedHat
- Windows

---

## Instance Type
Defines CPU, RAM, network performance  
Free tier: t2.micro (1 vCPU, 1 GB RAM)

---

## Key Pair
- Used for SSH
- Private key (.pem) is sensitive
- No password login by default

---

## Security Group
- Virtual firewall
- Inbound and outbound rules

---

## Connecting to EC2

```bash
chmod 600 key.pem
ssh -i key.pem ubuntu@<public-ip>
```

---

## Application Deployment Example (Jenkins)

1. Install Java
```bash
sudo apt install openjdk-11-jdk -y
```

2. Install Jenkins
3. Start Jenkins
```bash
sudo systemctl start jenkins
```

4. Open port 8080 in security group

---

## Access Application
```
http://<public-ip>:8080
```

---

## Best Practices
- Do not use root user
- Use IAM roles
- Stop unused instances
- Least privilege
