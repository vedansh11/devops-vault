# AWS IAM (Identity and Access Management)

## Purpose of IAM
AWS IAM is the service responsible for **authentication** (who you are) and **authorization** (what you are allowed to do) inside an AWS account.

It prevents:
- Sharing root credentials
- Accidental or malicious deletion of resources
- Lack of auditability (who did what)

---

## Core Security Concepts

### Authentication
Verifies identity.
- Root user
- IAM users
- IAM roles (temporary)

### Authorization
Determines permissions.
- Controlled using **policies**
- Enforced at every AWS API call

---

## Root User (Critical Concept)
- Created when AWS account is created
- Has **unrestricted access**
- Should **never** be used for daily operations
- Best practice:
  - Enable MFA
  - Lock credentials
  - Use only for account-level tasks (billing, support, IAM bootstrap)

---

## IAM Users
Represents **humans** (developers, admins, QA, etc.)

### Characteristics
- Permanent identity
- Username + password (console)
- Access keys (CLI / SDK)
- Permissions granted via policies

### Best Practices
- Never attach permissions directly unless unavoidable
- Always assign users to groups
- Enforce password rotation
- Use least privilege

---

## IAM Policies
Policies define **authorization rules**

### Types
- AWS Managed Policies
- Customer Managed Policies
- Inline Policies (discouraged)

### Policy Structure (JSON)
- Version
- Statement
  - Effect (Allow / Deny)
  - Action
  - Resource
  - Condition (optional)

### Example (Conceptual)
- Allow read-only access to S3
- Deny delete operations
- Restrict access to specific resources

---

## IAM Groups
Logical collection of users.

### Why Groups Exist
- Reduce repetitive permission management
- Centralized access control
- Scales with organization growth

### Typical Groups
- Developers
- QA
- DevOps
- DB Admins
- Read-only users

### Best Practice
- Attach policies to groups, not users
- Add/remove users from groups as people join/leave

---

## IAM Roles
Used for **non-human access** and **temporary credentials**

### Common Use Cases
- EC2 accessing S3
- Lambda accessing DynamoDB
- CI/CD tools accessing AWS
- Cross-account access

### Key Differences from Users
- No permanent credentials
- Assumed temporarily
- Uses STS (Security Token Service)

### Why Roles Are Critical
- No credential leakage
- Automatic rotation
- Mandatory for production workloads

---

## IAM vs Real-World Analogy (Bank Example)
- Authentication = entering the bank
- Authorization = areas you are allowed to access
- Policies = written rules
- Groups = job roles
- Roles = temporary passes for systems

---

## Practical Flow (Real Life)
1. Create AWS account
2. Secure root user
3. Create IAM admin user
4. Create groups
5. Attach policies to groups
6. Add users to groups
7. Use roles for services & automation

---

## Security Best Practices
- Enable MFA everywhere
- Use least privilege
- Avoid inline policies
- Prefer roles over access keys
- Audit with CloudTrail
- Rotate credentials

---

## Interview-Grade Key Takeaways
- Root user should never be shared
- IAM users are for humans
- IAM roles are for services and automation
- Policies define permissions
- Groups simplify management
- IAM is the foundation of AWS security
