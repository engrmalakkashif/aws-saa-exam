# Domain 1: Design Secure Architectures (30% of Exam)

**Content Domain 1 covers 30% of the exam with approximately 15 questions.**

---

## 📌 Overview

<cite index="1-9,1-10,1-11,1-12,1-13">The AWS Certified Solutions Architect - Associate exam is intended for individuals who perform a solutions architect role. The exam validates a candidate's ability to design solutions based on the AWS Well-Architected Framework. The exam also validates a candidate's ability to design solutions that incorporate AWS services to meet current business requirements and future projected needs, and to design architectures that are secure, resilient, high-performing, and cost-optimized.</cite>

This domain focuses on **security**, which is typically the highest priority in any architecture decision.

### Three Key Tasks

1. **Task 1.1**: Design secure access to AWS resources
2. **Task 1.2**: Design secure workloads and applications  
3. **Task 1.3**: Determine appropriate data security controls

---

## 🔒 Task 1.1: Design Secure Access to AWS Resources

### What You Need to Know

<cite index="1-40">Knowledge of access controls and management across multiple accounts, AWS federated access and identity services (for example, IAM, AWS IAM Identity Center), AWS global infrastructure (for example, Availability Zones, AWS Regions), AWS security best practices (for example, the principle of least privilege), and the AWS shared responsibility model.</cite>

### Key Concepts

#### 1. **AWS Shared Responsibility Model**
- **AWS Responsibility**: Infrastructure, foundation services, hardware, AWS global network, managed services
- **Your Responsibility**: Access management, application code, operating system patches, network config, encryption usage
- **Shared**: Patch management, configuration management, awareness and training

#### 2. **IAM (Identity and Access Management)**
- **Users**: Represent individual people or applications
- **Roles**: Temporary credentials for services or cross-account access
- **Groups**: Collection of users for easier permission management
- **Policies**: Define permissions in JSON format

**Best Practice**: Never share root account credentials. Always use IAM users/roles.

#### 3. **Principal of Least Privilege**
- Grant minimum permissions needed to perform job
- Use conditions to restrict by IP, time, MFA requirement
- Regularly review and remove unused permissions
- Example: Database access only from app servers, not from internet

#### 4. **Multi-Factor Authentication (MFA)**
- Adds second factor beyond password
- Types: Virtual (Google Authenticator), U2F Security Keys, Hardware tokens
- Essential for: Root account, IAM users with console access, sensitive operations

#### 5. **Access Controls Across Multiple Accounts**
- **Cross-Account Roles**: Trust relationship between accounts
- **External ID**: Prevents confused deputy problem
- **Service Control Policies (SCPs)**: Organization-wide permission boundaries
- **AWS Control Tower**: Automated multi-account setup with guardrails

#### 6. **AWS IAM Identity Center (Successor to SSO)**
- Single point of identity management
- Multi-account access
- Application assignments
- Federated access (Active Directory, Okta, etc.)

#### 7. **Federation & External Identity Providers**
- **SAML 2.0**: Enterprise SSO integration
- **Web Identity Federation**: Social login, OpenID Connect
- **AWS STS (Security Token Service)**: Generate temporary credentials
  - `AssumeRole`: Assume cross-account role
  - `GetCallerIdentity`: Verify identity
  - Temporary credentials have expiration

### Skills to Master

<cite index="1-40">Skills in applying AWS security best practices to IAM users and root users (for example, multi-factor authentication [MFA]), designing a flexible authorization model that includes IAM users, groups, roles, and policies, designing a role-based access control strategy (for example, AWS STS, role switching, cross-account access), designing a security strategy for multiple AWS accounts (for example, AWS Control Tower, service control policies [SCPs]), determining the appropriate use of resource policies for AWS services, and determining when to federate a directory service with IAM roles.</cite>

**Key Skills:**
- Write IAM policies that are specific and least-privileged
- Design cross-account access with assume role
- Implement MFA for sensitive accounts
- Use Federation for existing directory services
- Apply SCPs for compliance enforcement
- Design role assumption workflow

### Real-World Scenarios

**Scenario 1: Multi-Account Organization**
- **Challenge**: 10 accounts, need centralized access management
- **Solution**: 
  - IAM Identity Center in master account
  - Roles in each account with trust relationship
  - SCPs to enforce encryption, restrict regions

**Scenario 2: Third-Party Contractor**
- **Challenge**: Grant temporary access to specific S3 bucket
- **Solution**:
  - Create role in AWS account
  - External ID for security
  - STS AssumeRole to get temporary credentials
  - CloudTrail logs all access

**Scenario 3: Hybrid Environment**
- **Challenge**: Existing Active Directory, want AWS access
- **Solution**:
  - IAM Identity Center with AD connector
  - SAML assertion to temporary AWS credentials
  - No AWS password management needed

### Practice Questions Hints

**Q: When should you use cross-account roles instead of IAM users?**
- A: When accessing resources in another account, for better audit trail and temporary credentials

**Q: What's the most secure way to authenticate to AWS?**
- A: IAM user with MFA enabled, using temporary credentials from STS

**Q: How do you prevent a user from deleting critical resources?**
- A: Explicit Deny in IAM policy + Permission Boundary + Resource-based policy

---

## 🔐 Task 1.2: Design Secure Workloads and Applications

### What You Need to Know

<cite index="1-40,1-41">Knowledge of application configuration and credentials security, AWS service endpoints, control ports, protocols, and network traffic on AWS, secure application access, and security services with appropriate use cases (for example, Amazon Cognito, Amazon GuardDuty, Amazon Macie), and threat vectors external to AWS (for example, DDoS, SQL injection).</cite>

### Key Concepts

#### 1. **VPC Architecture with Security Components**

<cite index="1-41">Skills in designing VPC architectures with security components (for example, security groups, route tables, network ACLs, NAT gateways), determining network segmentation strategies (for example, using public subnets and private subnets), integrating AWS services to secure applications (for example, AWS Shield, AWS WAF, IAM Identity Center, AWS Secrets Manager), and securing external network connections to and from the AWS Cloud (for example, VPN, AWS Direct Connect).</cite>

**Multi-Tier VPC Design:**
```
Public Subnet (Web Tier):
  - Internet Gateway for inbound traffic
  - Security group: Allow 80, 443 from anywhere
  - ALB/NLB for load balancing
  - NAT Gateway for outbound traffic

Private Subnet (App Tier):
  - No internet access (except via NAT)
  - Security group: Allow traffic from web tier only
  - EC2 instances for application logic
  - Access logs to CloudWatch

Private Subnet (Data Tier):
  - No internet access
  - Security group: Allow traffic from app tier only
  - RDS database with encryption
  - Backups to encrypted S3
```

#### 2. **Security Groups vs. Network ACLs**

| Aspect | Security Groups | Network ACLs |
|--------|-----------------|--------------|
| **Layer** | Instance level | Subnet level |
| **Stateful** | Yes (remember connections) | No (check both ways) |
| **Deny** | Support explicit deny | Support explicit deny |
| **Default** | Deny all inbound | Allow all |
| **Use Case** | Application-level access | Subnet-level rules |

**Best Practice**: Use security groups for most cases, NACLs for edge cases (block specific IPs, ports).

#### 3. **Network Segmentation Strategies**

- **Public Subnets**: Resources needing internet access (web servers)
- **Private Subnets**: Internal resources (databases, app servers)
- **DMZ Pattern**: 3-tier with bastion host for access
- **Compliance Zones**: Separate VPCs for PCI-DSS, HIPAA workloads

#### 4. **AWS Shield & AWS WAF**

**AWS Shield** (DDoS Protection):
- **Standard**: Automatic, no cost, protection at edge
- **Advanced**: $3,000/month, 24/7 support, more sophisticated attacks
- Protection against volumetric attacks (UDP floods, DNS floods)

**AWS WAF** (Web Application Firewall):
- Protects against OWASP Top 10
- SQL injection, XSS, path traversal
- Rules: IP whitelists, geographic blocking, regex patterns
- Can attach to CloudFront, ALB, API Gateway

#### 5. **Application Secrets & Credentials**

<cite index="1-41">Integrating AWS services to secure applications (for example, AWS Shield, AWS WAF, IAM Identity Center, AWS Secrets Manager).</cite>

**AWS Secrets Manager**:
- Encrypt secrets (passwords, API keys, DB credentials)
- Automatic rotation
- Audit trail in CloudTrail
- Cross-region replication
- Java, Python, Node.js SDKs for retrieval

**AWS Systems Manager Parameter Store**:
- Simpler, free tier
- String parameters or secure strings
- No automatic rotation
- Good for configuration, not secrets

**Never**:
- Hardcode credentials in code
- Store secrets in environment variables
- Commit secrets to Git
- Log sensitive information

#### 6. **Service Endpoints & Private Connectivity**

**VPC Endpoints** (Avoid internet for AWS services):
- **Gateway Endpoints**: S3, DynamoDB (no charge, added to route table)
- **Interface Endpoints**: Most AWS services (hourly + data transfer charges)
- Prevents data from leaving AWS network
- Reduces attack surface

**AWS Direct Connect** (Private connectivity):
- Dedicated network connection
- More consistent than internet
- Hybrid cloud connectivity
- Compliance requirement

#### 7. **Threat Detection Services**

**Amazon GuardDuty**:
- Intelligent threat detection
- Analyzes VPC Flow Logs, CloudTrail, DNS logs
- Identifies compromised instances, credential abuse
- Automated response via Lambda

**Amazon Macie**:
- Discovers and protects sensitive data
- Identifies PII, financial data, PHI
- S3 bucket analysis
- Integrates with CloudWatch for alerts

**Amazon Detective**:
- Investigates security findings
- Root cause analysis
- Graph analysis of relationships
- Works with GuardDuty and Security Hub

#### 8. **External Threat Vectors**

**DDoS Attacks**:
- Volumetric (UDP floods, DNS amplification)
- Protocol attacks (SYN floods, Ping floods)
- Application layer (HTTP floods)
- **Defense**: AWS Shield, WAF, CloudFront caching

**SQL Injection**:
- Malicious SQL in user input
- Can access/modify database
- **Defense**: Parameterized queries, WAF, input validation

**Cross-Site Scripting (XSS)**:
- Malicious JavaScript in web pages
- Steals cookies, credentials
- **Defense**: WAF, Content Security Policy, input sanitization

### Practice Questions Hints

**Q: How do you prevent SQL injection attacks?**
- A: Use parameterized queries, WAF with SQL injection rules, input validation

**Q: When should you use VPC Endpoints vs. NAT Gateway?**
- A: VPC Endpoints for AWS services (cheaper), NAT Gateway for internet traffic

**Q: What's the best way to protect against DDoS?**
- A: CloudFront + AWS Shield + WAF + Auto Scaling

---

## 🔑 Task 1.3: Determine Appropriate Data Security Controls

### What You Need to Know

<cite index="1-41">Knowledge of data access and governance, data recovery, data retention and classification, and encryption and appropriate key management. Skills in aligning AWS technologies to meet compliance requirements, encrypting data at rest (for example, AWS KMS), encrypting data in transit (for example, AWS Certificate Manager [ACM] using TLS), implementing access policies for encryption keys, implementing data backups and replications, implementing policies for data access, lifecycle, and protection, and rotating encryption keys and renewing certificates.</cite>

### Key Concepts

#### 1. **AWS KMS (Key Management Service)**

**Types of Keys**:
- **AWS Managed Keys**: Free, automatic rotation, less control
- **Customer Managed Keys**: Full control, audit, charges apply, manual rotation possible
- **Imported Keys**: Your own key material, maximum control

**Key Operations**:
```
- Create: Generate new CMK
- Encrypt: Encrypt data up to 4KB
- GenerateDataKey: For envelope encryption
- Decrypt: Decrypt ciphertext
- ReEncrypt: Change customer master key
- ScheduleKeyDeletion: Delete after 7-30 day waiting period
```

**Envelope Encryption**:
- GenerateDataKey returns plaintext key + encrypted key
- Encrypt data with plaintext key
- Delete plaintext key
- Store encrypted key with data
- On decrypt: Use CMK to decrypt key, then decrypt data
- Benefits: Encrypt large files efficiently, CloudTrail doesn't log data keys

**Key Policies**:
- JSON policies controlling who can use key
- Different from IAM policies
- Can allow cross-account access
- Example: Allow Lambda role to decrypt

#### 2. **Encryption at Rest**

**S3 Encryption Options**:
- **SSE-S3**: AWS manages keys (default), free
- **SSE-KMS**: AWS KMS manages keys, audit trail, charges
- **SSE-C**: Customer provides key (not recommended usually)
- **Client-Side**: Encrypt before uploading

**EBS Encryption**:
- Encrypt volumes at creation
- Encrypted snapshots remain encrypted
- No performance impact
- KMS key used for encryption
- Cannot decrypt existing unencrypted volumes

**RDS Encryption**:
- Enable at creation time
- EBS volumes encrypted
- Automated backups encrypted
- Read replicas encrypted
- Cannot modify after creation (create snapshot → restore encrypted)

**DynamoDB Encryption**:
- Default: AWS managed encryption
- Option: AWS KMS for more control
- Encryption in transit: Always HTTPS

#### 3. **Encryption in Transit**

**TLS/SSL with ACM**:
- AWS Certificate Manager provides free public certificates
- Auto-renewal
- Easy integration with CloudFront, ALB, API Gateway
- Private certificates for internal services

**Common Scenarios**:
- HTTPS between clients and ALB
- HTTPS between ALB and EC2 (internal)
- HTTPS to RDS database
- VPN encryption between offices

#### 4. **Data Classification & Retention**

**Data Classification Levels**:
- **Public**: No sensitivity (marketing materials)
- **Internal**: Company use only (internal documentation)
- **Confidential**: Restricted access (financial data)
- **Restricted**: Highest sensitivity (credit cards, SSNs)

**Retention Policies**:
- Legal hold for litigation
- Regulatory requirements (HIPAA 6 years, PCI-DSS 1 year)
- Operational needs (keep 90 days for troubleshooting)
- Archive after retention period

#### 5. **S3 Lifecycle Policies**

**Tiering Strategy**:
```
Day 0-30:   S3 Standard (frequent access)
Day 31-90:  S3 Standard-IA (infrequent access, cheaper)
Day 91-180: S3 One Zone-IA (infrequent, single AZ)
Day 181+:   S3 Glacier (archive, slow retrieval)
After 365:  S3 Deep Archive (7-10 hour retrieval)
```

**Example Policy**:
- Keep current version in Standard 30 days
- Transition to Glacier 90 days
- Delete after 7 years (compliance)
- Delete incomplete multipart uploads after 7 days

#### 6. **Backup & Disaster Recovery**

**AWS Backup Service**:
- Centralized backup management
- Backup policies across services (EC2, RDS, EFS, DynamoDB)
- Copy to other region
- Compliance compliance (retention policies)
- Automated lifecycle (transition to cold storage)

**RDS Backups**:
- Automated backups: 1-35 day retention
- Manual snapshots: Retained until deleted
- Point-in-time recovery: Within backup window
- Cross-region snapshots for DR

#### 7. **Data Access Controls**

**Resource-Based Policies**:
- S3 bucket policies (who can access bucket)
- SQS queue policies (who can send/receive messages)
- SNS topic policies (who can publish/subscribe)
- KMS key policies (who can use key)

**IAM Policies**:
- User-based access control
- Role-based access control
- Conditions (IP, time, MFA)
- Service-specific permissions

**Attribute-Based Access Control (ABAC)**:
- Tags determine access
- Example: Cost-center-tag = finance
- More flexible, scales with growth

#### 8. **Compliance Alignment**

<cite index="1-41">Skills in aligning AWS technologies to meet compliance requirements.</cite>

**Common Requirements**:
- **HIPAA** (healthcare): Encryption at rest/transit, audit logs, access controls
- **PCI-DSS** (credit cards): Encryption, network segmentation, key management
- **SOC 2** (service providers): Monitoring, incident response, backup
- **GDPR** (privacy): Data minimization, right to deletion, data portability

**AWS Services for Compliance**:
- AWS Config: Track resource configurations
- CloudTrail: Audit API calls
- CloudWatch: Monitor logs and events
- AWS Artifact: Compliance reports and certifications

### Practice Questions Hints

**Q: How do you ensure data deleted from S3 cannot be recovered?**
- A: Enable versioning then delete all versions, or use Object Lock in WORM mode

**Q: What's the most cost-effective way to store data for long-term archival?**
- A: S3 Glacier for infrequent access, S3 Deep Archive for compliance (7-10 year holds)

**Q: How should you encrypt database passwords for an application?**
- A: Store in Secrets Manager or Parameter Store with KMS encryption

**Q: What's the advantage of using customer-managed KMS keys?**
- A: Audit trail, key rotation control, cross-account access, compliance requirements

---

## 🎯 Domain 1 Key Takeaways

### Security Principles
1. **Least Privilege**: Grant minimum necessary permissions
2. **Defense in Depth**: Multiple layers of security
3. **Shared Responsibility**: Understand who does what
4. **Assume Breach**: Design for breach scenario

### Top Services for Domain 1
| Service | Purpose | Key Feature |
|---------|---------|-------------|
| IAM | Access management | Policies, roles, users |
| KMS | Encryption key management | Audit trail, rotation |
| Secrets Manager | Sensitive data storage | Auto rotation |
| VPC | Network isolation | Public/private subnets |
| Security Groups | Instance firewalls | Stateful rules |
| WAF | Web protection | OWASP Top 10 rules |
| GuardDuty | Threat detection | ML-based analysis |
| CloudTrail | Audit logging | All API calls |

### Common Exam Patterns
- **Access Control Question**: Identify least-privileged option
- **Encryption Question**: Know when to use KMS, S3 encryption, TLS
- **Network Security**: Design 3-tier VPC with security groups/NACLs
- **Compliance**: Match requirement to AWS service
- **Threat**: Identify attack type and AWS defense

---

## 📚 Recommended Deep Dives

1. <cite index="1-69">AWS Certificate Manager (ACM)</cite> for certificate management
2. <cite index="1-69">AWS Artifact</cite> for compliance documentation
3. <cite index="1-69">AWS Resource Access Manager (AWS RAM)</cite> for sharing resources
4. <cite index="1-68,1-69">AWS Config for configuration tracking and AWS Control Tower for multi-account governance</cite>

---

*Study Time: 8-10 hours | Practice: 30+ questions | Hands-On: Create secure VPC with all components*

