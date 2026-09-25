# AWS SAA-C03: 30-Day Study Plan - Daily Breakdown

## 📅 Study Calendar Overview

```
WEEK 1: Foundation & Secure Architectures (Days 1-7)
WEEK 2: Secure Architectures Deep Dive (Days 8-14)
WEEK 3: Resilient Architectures (Days 15-21)
WEEK 4: High-Performance & Cost-Optimized (Days 22-30)
```

---

## 📍 WEEK 1: Foundation & Secure Architectures (Days 1-7)

### **Day 1: AWS Fundamentals & Global Infrastructure**

**Learning Objectives:**
- Understand AWS shared responsibility model
- Learn AWS global infrastructure (Regions, AZs, Edge locations)
- Understand AWS Well-Architected Framework

**Key Topics:**
- ✅ AWS Shared Responsibility Model
  - What AWS manages (infrastructure, security of the cloud)
  - What customers manage (applications, access management)
- ✅ AWS Global Infrastructure
  - Regions (geographic locations)
  - Availability Zones (isolated data centers)
  - Edge Locations (content delivery)
  - Local Zones (extend regions)
- ✅ AWS Well-Architected Framework (5 Pillars)
  - Operational Excellence
  - Security
  - Reliability
  - Performance Efficiency
  - Cost Optimization

**Study Tasks:**
- [ ] Read: AWS Shared Responsibility Model (15 min)
- [ ] Watch: AWS Global Infrastructure overview (10 min)
- [ ] Review: 5 Well-Architected Pillars (15 min)
- [ ] Create: Mind map of global infrastructure
- [ ] Practice: Identify regions for compliance requirements (10 min)

**Key Takeaways:**
- Security is shared responsibility
- Multi-AZ provides high availability
- Multi-region provides disaster recovery
- Each region is independent

---

### **Day 2: IAM Fundamentals & AWS Account Security**

**Learning Objectives:**
- Master IAM core concepts
- Understand root account vs. IAM users
- Learn about MFA and best practices

**Key Topics:**
- ✅ IAM (Identity and Access Management)
  - Root account (avoid using)
  - IAM Users (for people)
  - IAM Roles (for services)
  - IAM Groups (for organizing users)
- ✅ Authentication & Authorization
  - Passwords and Access Keys
  - Multi-Factor Authentication (MFA)
  - Temporary security credentials
- ✅ IAM Best Practices
  - Principle of Least Privilege
  - Use roles instead of keys
  - Enable MFA for all accounts
  - Rotate credentials regularly

**Study Tasks:**
- [ ] Deep dive: IAM Users, Roles, Groups (20 min)
- [ ] Study: MFA options and setup (10 min)
- [ ] Review: IAM best practices checklist (10 min)
- [ ] Create: IAM policy examples document
- [ ] Practice: Write custom IAM policies (15 min)

**Key Takeaways:**
- Always use IAM users, never share root account
- Require MFA on all production accounts
- Use roles for cross-account and service access
- Follow least privilege principle

---

### **Day 3: IAM Policies, Permissions & Access Control**

**Learning Objectives:**
- Understand policy structure and permissions
- Learn how to create and manage policies
- Master access control strategies

**Key Topics:**
- ✅ IAM Policies
  - Policy structure (JSON format)
  - Principal, Action, Resource, Effect
  - Inline policies vs. managed policies
- ✅ Permission Types
  - User permissions (attached to users)
  - Role permissions (attached to roles)
  - Resource-based policies (S3 bucket policies)
  - Conditions and wildcards
- ✅ Cross-Account Access
  - Trust relationships
  - AssumeRole mechanism
  - External ID for security

**Study Tasks:**
- [ ] Study: IAM policy structure and JSON (20 min)
- [ ] Review: Managed vs. inline policies (10 min)
- [ ] Practice: Create sample IAM policies (20 min)
- [ ] Create: Policy comparison chart
- [ ] Exercise: Build cross-account access scenario (15 min)

**Key Takeaways:**
- Policies are written in JSON format
- Always specify actions and resources explicitly
- Use AWS managed policies as base, customize with inline
- Cross-account roles enable delegation safely

---

### **Day 4: Secure Access - Federation & Identity Services**

**Learning Objectives:**
- Understand federated access
- Learn about AWS IAM Identity Center
- Master identity federation strategies

**Key Topics:**
- ✅ Federation
  - SAML 2.0 integration
  - Web identity federation
  - OpenID Connect (OIDC)
- ✅ AWS IAM Identity Center
  - Centralized identity management
  - Multi-account access
  - Application assignments
- ✅ Service Control Policies (SCPs)
  - Organization-level permissions
  - Preventive controls
  - Permission boundaries

**Study Tasks:**
- [ ] Study: Federation concepts (15 min)
- [ ] Review: IAM Identity Center features (10 min)
- [ ] Learn: SCPs for compliance (10 min)
- [ ] Create: Federation architecture diagram
- [ ] Practice: SCP examples (15 min)

**Key Takeaways:**
- Federation reduces password management burden
- IAM Identity Center enables SSO across accounts
- SCPs enforce organization-wide security policies
- External ID prevents confused deputy problem

---

### **Day 5: VPC Security - Network Architecture**

**Learning Objectives:**
- Understand VPC structure and design
- Learn about subnets and network segmentation
- Master security groups and NACLs

**Key Topics:**
- ✅ VPC (Virtual Private Cloud)
  - VPC creation and configuration
  - CIDR blocks and IP addressing
  - Public vs. Private subnets
- ✅ Network Segmentation
  - Multi-tier architecture
  - Public subnets (web tier)
  - Private subnets (application/data tier)
  - Bastion hosts for access
- ✅ Security Controls
  - Security Groups (stateful firewall)
  - Network ACLs (stateless firewall)
  - NACLs for subnet-level control
  - Default rules and custom rules

**Study Tasks:**
- [ ] Study: VPC architecture (20 min)
- [ ] Deep dive: Subnet design (10 min)
- [ ] Review: Security Groups vs. NACLs (15 min)
- [ ] Create: VPC architecture diagram
- [ ] Practice: Design 3-tier VPC (20 min)

**Key Takeaways:**
- VPC is your network boundary
- Security Groups are stateful, NACLs are stateless
- Private subnets for databases and internal services
- NAT Gateway for outbound internet from private subnets

---

### **Day 6: Secure Workloads - Applications & Networking**

**Learning Objectives:**
- Learn to secure applications and workloads
- Understand network security options
- Master external connection security

**Key Topics:**
- ✅ Application Security
  - Secure application design
  - Credentials and secrets management
  - AWS Secrets Manager
  - AWS Systems Manager Parameter Store
- ✅ Network Security Services
  - AWS Shield (DDoS protection)
  - AWS WAF (Web Application Firewall)
  - AWS Network Firewall
- ✅ VPN & Direct Connect
  - Site-to-Site VPN for hybrid
  - Client VPN for remote access
  - AWS Direct Connect for dedicated connection
  - PrivateLink for private connectivity

**Study Tasks:**
- [ ] Study: Secrets management (15 min)
- [ ] Review: WAF and Shield (10 min)
- [ ] Learn: VPN vs. Direct Connect (10 min)
- [ ] Create: Network security checklist
- [ ] Practice: Design hybrid network (15 min)

**Key Takeaways:**
- Never hardcode secrets in code
- Use Secrets Manager for rotation and audit
- WAF protects against OWASP top 10
- Site-to-Site VPN for quick hybrid setup
- Direct Connect for consistent performance

---

### **Day 7: Data Security - Encryption & Compliance**

**Learning Objectives:**
- Master encryption strategies
- Learn about data classification
- Understand compliance requirements

**Key Topics:**
- ✅ Encryption at Rest
  - AWS KMS (Key Management Service)
  - Customer-managed keys vs. AWS managed
  - S3 Server-Side Encryption
  - EBS volume encryption
- ✅ Encryption in Transit
  - TLS/SSL (HTTPS)
  - AWS Certificate Manager (ACM)
  - VPN encryption
- ✅ Data Classification & Governance
  - Data retention policies
  - Data lifecycle management
  - Amazon Macie for data discovery
  - GuardDuty for threat detection

**Study Tasks:**
- [ ] Study: KMS and key management (20 min)
- [ ] Review: Encryption at rest vs. in transit (10 min)
- [ ] Learn: Data classification strategies (10 min)
- [ ] Create: Encryption architecture diagram
- [ ] Practice: Design encryption solution (15 min)

**Key Takeaways:**
- Always encrypt sensitive data at rest and in transit
- Use AWS KMS for centralized key management
- Key rotation improves security
- Data lifecycle reduces compliance burden

**End of Week 1 Check:**
- [ ] Understand AWS global infrastructure
- [ ] Master IAM concepts and policies
- [ ] Design secure VPCs and networks
- [ ] Explain encryption strategies
- [ ] Score 75%+ on Domain 1 practice test

---

## 📍 WEEK 2: Secure Architectures Deep Dive (Days 8-14)

### **Day 8: KMS & Secrets Management Deep Dive**

**Learning Objectives:**
- Master AWS KMS operations
- Understand key policies and permissions
- Learn advanced encryption scenarios

**Key Topics:**
- ✅ AWS KMS Architecture
  - Customer Master Keys (CMK)
  - Key hierarchy and envelope encryption
  - Key policies vs. IAM policies
  - Key rotation and lifecycle
- ✅ KMS Operations
  - GenerateDataKey for application-level encryption
  - Encrypt/Decrypt operations
  - Grant mechanisms
  - CloudTrail audit logs
- ✅ Secrets Manager
  - Automatic rotation
  - Cross-region replication
  - Cost vs. Parameter Store

**Study Tasks:**
- [ ] Study: KMS key types and lifecycle (20 min)
- [ ] Review: Envelope encryption (15 min)
- [ ] Practice: Write KMS key policies (20 min)
- [ ] Create: Secrets rotation strategy diagram
- [ ] Exercise: Design encryption for compliance (15 min)

**Key Takeaways:**
- CMK provides control and audit trail
- Envelope encryption optimizes performance
- Key grants for temporary access
- Secrets Manager > Parameter Store for secrets

---

### **Day 9: S3 Security & Access Control**

**Learning Objectives:**
- Master S3 security features
- Learn bucket policies and ACLs
- Understand encryption options

**Key Topics:**
- ✅ S3 Access Control
  - Bucket policies (resource-based)
  - IAM policies (user-based)
  - ACLs (legacy, avoid if possible)
  - Public access blocks
- ✅ S3 Encryption
  - Server-Side Encryption with S3 (SSE-S3)
  - Server-Side Encryption with KMS (SSE-KMS)
  - Client-Side Encryption
  - Default encryption bucket setting
- ✅ S3 Security Features
  - Block Public Access settings
  - MFA Delete protection
  - Versioning for data protection
  - CloudTrail logging

**Study Tasks:**
- [ ] Study: S3 bucket policies vs. IAM (20 min)
- [ ] Review: S3 encryption options (15 min)
- [ ] Practice: Write S3 bucket policies (20 min)
- [ ] Create: S3 security best practices checklist
- [ ] Exercise: Design secure S3 architecture (15 min)

**Key Takeaways:**
- Enable Block Public Access for safety
- Use bucket policies for external access control
- SSE-KMS provides better audit trail
- Enable versioning on critical buckets

---

### **Day 10: Database Security & Compliance**

**Learning Objectives:**
- Secure RDS databases
- Implement encryption and access control
- Master compliance requirements

**Key Topics:**
- ✅ RDS Security
  - DB security groups vs. EC2 security groups
  - Encryption at rest (EBS encryption)
  - Encryption in transit (SSL/TLS)
  - IAM database authentication
- ✅ Backup & Recovery
  - Automated backups retention
  - Manual snapshots
  - Cross-region snapshots
  - Point-in-time recovery (PITR)
- ✅ Compliance Features
  - Enhanced monitoring with CloudWatch
  - Audit logs for compliance
  - Performance Insights
  - AWS Config for compliance tracking

**Study Tasks:**
- [ ] Study: RDS security architecture (20 min)
- [ ] Review: DB authentication methods (10 min)
- [ ] Learn: Backup and recovery strategies (15 min)
- [ ] Create: RDS security and compliance diagram
- [ ] Practice: Design secure RDS setup (15 min)

**Key Takeaways:**
- Enable encryption both at rest and in transit
- Use IAM auth for dynamic credentials
- Automate backups with retention policies
- Test recovery procedures regularly

---

### **Day 11: VPC Advanced Security & Segmentation**

**Learning Objectives:**
- Implement advanced network security
- Design network segmentation
- Master network monitoring

**Key Topics:**
- ✅ Advanced VPC Design
  - VPC Flow Logs for monitoring
  - VPC Endpoints (Gateway and Interface)
  - Private link for safe access
  - Transit Gateway for multi-account
- ✅ Network Segmentation Strategies
  - Application isolation
  - Compliance zones
  - DMZ pattern with bastion hosts
  - Service-oriented architecture
- ✅ Monitoring & Detection
  - VPC Flow Logs analysis
  - CloudWatch for network metrics
  - GuardDuty for threat detection
  - Network logs for compliance

**Study Tasks:**
- [ ] Study: VPC Endpoints and PrivateLink (15 min)
- [ ] Review: Transit Gateway for connectivity (10 min)
- [ ] Learn: Network segmentation patterns (15 min)
- [ ] Create: Multi-account network diagram
- [ ] Practice: Design isolated security zones (15 min)

**Key Takeaways:**
- VPC Endpoints prevent internet gateway exposure
- Flow Logs critical for security investigations
- Transit Gateway simplifies multi-account networks
- GuardDuty detects anomalous behavior

---

### **Day 12: Compliance, Audit & Governance**

**Learning Objectives:**
- Understand compliance frameworks
- Implement audit logging
- Master governance tools

**Key Topics:**
- ✅ Compliance Frameworks
  - HIPAA, PCI-DSS, SOC 2
  - GDPR and data residency
  - FedRAMP for government
- ✅ AWS Audit & Compliance Tools
  - CloudTrail for API logging
  - Config for resource tracking
  - CloudWatch Logs for application logs
  - S3 for centralized logging
- ✅ Governance Services
  - AWS CloudFormation for IaC
  - AWS Organizations for governance
  - Service Control Policies (SCPs)
  - Resource Access Manager (RAM)

**Study Tasks:**
- [ ] Study: CloudTrail and Config (20 min)
- [ ] Review: Compliance frameworks (15 min)
- [ ] Learn: Logging best practices (10 min)
- [ ] Create: Audit and compliance strategy
- [ ] Practice: Design logging architecture (15 min)

**Key Takeaways:**
- CloudTrail is essential for compliance audits
- Config tracks resource configuration changes
- Centralized logging enables analysis
- Organizations enforces compliance across accounts

---

### **Day 13: AWS Security Best Practices & Architecture Patterns**

**Learning Objectives:**
- Review security best practices
- Understand common attack vectors
- Learn defensive architecture patterns

**Key Topics:**
- ✅ Security Best Practices
  - Defense in depth (layered security)
  - Principle of least privilege
  - Assume breach mentality
  - Continuous monitoring
- ✅ Common Attack Scenarios
  - DDoS attacks prevention (Shield, WAF)
  - SQL injection prevention (WAF)
  - Privilege escalation prevention
  - Lateral movement prevention
- ✅ Secure Architecture Patterns
  - Zero-trust network architecture
  - Immutable infrastructure
  - Security-first design
  - Automated remediation

**Study Tasks:**
- [ ] Study: Defense in depth strategy (15 min)
- [ ] Review: Common attacks and prevention (15 min)
- [ ] Learn: Zero-trust architecture (10 min)
- [ ] Create: Security architecture review checklist
- [ ] Practice: Design defense against attack scenario (20 min)

**Key Takeaways:**
- Assume accounts will be compromised
- Multiple layers of security necessary
- Automation speeds detection and response
- Continuous improvement mindset

---

### **Day 14: Domain 1 Review & Practice**

**Learning Objectives:**
- Review all Domain 1 concepts
- Take practice tests
- Identify weak areas

**Key Topics:**
- ✅ Review: Task 1.1 - Secure Access
  - IAM users, roles, groups
  - Cross-account access
  - MFA implementation
- ✅ Review: Task 1.2 - Secure Workloads
  - VPC architecture
  - Security groups and NACLs
  - WAF and Shield
- ✅ Review: Task 1.3 - Data Security
  - Encryption at rest and in transit
  - KMS and key management
  - Compliance and audit

**Study Tasks:**
- [ ] Review all Domain 1 study notes (30 min)
- [ ] Take: Full Domain 1 practice test (45 min)
- [ ] Analyze: Weak areas (15 min)
- [ ] Study: Incorrect answers deep dive (20 min)
- [ ] Create: Domain 1 quick reference guide

**Expected Performance:**
- Target score: 85%+ on practice test
- Review weak topics before moving on
- Document tricky questions

**End of Week 2 Check:**
- [ ] Master KMS and secrets management
- [ ] Understand S3 and database security
- [ ] Design secure VPCs
- [ ] Explain compliance and audit
- [ ] Score 80%+ on Domain 1 practice test

---

## 📍 WEEK 3: Resilient Architectures (Days 15-21)

### **Day 15: Auto Scaling & Load Balancing Fundamentals**

**Learning Objectives:**
- Understand Auto Scaling concepts
- Master load balancer types
- Learn scaling strategies

**Key Topics:**
- ✅ Auto Scaling
  - Launch configurations/templates
  - Auto Scaling groups
  - Scaling policies (target tracking, simple, step)
  - Lifecycle hooks
- ✅ Load Balancers (ELB/ALB/NLB)
  - Application Load Balancer (Layer 7)
  - Network Load Balancer (Layer 4)
  - Classic Load Balancer (legacy)
  - Target groups and health checks
- ✅ Scaling Strategies
  - Horizontal scaling (add instances)
  - Vertical scaling (larger instances)
  - Predictive scaling with ML
  - Scheduled scaling

**Study Tasks:**
- [ ] Study: Auto Scaling concepts (20 min)
- [ ] Review: Load balancer types and use cases (15 min)
- [ ] Learn: Scaling policies and triggers (15 min)
- [ ] Create: Auto Scaling architecture diagram
- [ ] Practice: Design scaling strategy (15 min)

**Key Takeaways:**
- ALB for web applications (HTTP/HTTPS)
- NLB for ultra-high performance (TCP/UDP)
- Auto Scaling maintains availability and cost
- Lifecycle hooks enable graceful shutdown

---

### **Day 16: High Availability & Multi-AZ Architecture**

**Learning Objectives:**
- Design highly available systems
- Understand multi-AZ deployment
- Master failover strategies

**Key Topics:**
- ✅ High Availability Design
  - Redundancy across AZs
  - Eliminating single points of failure
  - Active-active vs. active-passive
  - Health checks and recovery
- ✅ Multi-AZ Deployments
  - RDS Multi-AZ (synchronous replication)
  - Elastic File System (EFS) across AZs
  - Auto Scaling across AZs
  - Load balancer distribution
- ✅ Failover Mechanisms
  - Route 53 health checks
  - Automatic failover
  - DNS failover policies
  - Application-level failover

**Study Tasks:**
- [ ] Study: Multi-AZ architecture patterns (20 min)
- [ ] Review: RDS Multi-AZ failover (10 min)
- [ ] Learn: Route 53 failover (15 min)
- [ ] Create: HA architecture comparison chart
- [ ] Practice: Design HA system for e-commerce (15 min)

**Key Takeaways:**
- Always design for at least 2 AZs
- RDS Multi-AZ is automatic and transparent
- Route 53 enables intelligent failover
- Test failover procedures regularly

---

### **Day 17: Disaster Recovery & Resilience Patterns**

**Learning Objectives:**
- Understand disaster recovery strategies
- Learn RTO/RPO concepts
- Master resilient architecture patterns

**Key Topics:**
- ✅ Disaster Recovery Strategies
  - Backup and Restore (high RPO/RTO)
  - Pilot Light (moderate RPO/RTO)
  - Warm Standby (low RPO/RTO)
  - Active-Active (zero downtime)
- ✅ RTO & RPO Metrics
  - RTO (Recovery Time Objective) - target recovery time
  - RPO (Recovery Point Objective) - data loss tolerance
  - Balancing cost and requirements
- ✅ Resilience Patterns
  - Idempotent operations
  - Retry logic with exponential backoff
  - Circuit breakers
  - Bulkheads for fault isolation
- ✅ Data Resilience
  - Cross-region replication
  - Backup retention policies
  - Point-in-time recovery
  - Immutable backups

**Study Tasks:**
- [ ] Study: DR strategies and RTO/RPO (20 min)
- [ ] Review: Comparing DR approaches (15 min)
- [ ] Learn: Resilience patterns (10 min)
- [ ] Create: DR strategy comparison table
- [ ] Practice: Design DR for critical system (20 min)

**Key Takeaways:**
- Define RTO and RPO before architecture
- Multi-region for zero downtime
- Backup strategy depends on RPO needs
- Test DR regularly (DR drills)

---

### **Day 18: Microservices & Loose Coupling**

**Learning Objectives:**
- Understand microservices architecture
- Master loose coupling patterns
- Learn service integration

**Key Topics:**
- ✅ Microservices Architecture
  - Benefits and challenges
  - Service boundaries
  - Independent deployment
  - Technology flexibility
- ✅ Loose Coupling Patterns
  - Asynchronous messaging (SQS, SNS)
  - Event-driven architecture (EventBridge)
  - API Gateway for decoupling
  - Managed services for scaling
- ✅ Service Communication
  - Synchronous (REST, gRPC)
  - Asynchronous (queues, topics)
  - Request/response patterns
  - Event notification patterns

**Study Tasks:**
- [ ] Study: Microservices principles (20 min)
- [ ] Review: Coupling vs. cohesion (10 min)
- [ ] Learn: Message queue patterns (15 min)
- [ ] Create: Microservices architecture diagram
- [ ] Practice: Design loosely coupled system (15 min)

**Key Takeaways:**
- SQS for reliable async processing
- SNS for publish/subscribe patterns
- Decoupling improves resilience
- Each service should have own database

---

### **Day 19: Serverless & Event-Driven Architecture**

**Learning Objectives:**
- Master serverless computing
- Understand event-driven patterns
- Learn serverless scaling

**Key Topics:**
- ✅ Serverless Computing
  - AWS Lambda (functions)
  - AWS Fargate (containers)
  - Managed services elimination
  - Auto-scaling from zero
- ✅ Event-Driven Architecture
  - EventBridge for event routing
  - Lambda triggers and events
  - S3 event notifications
  - DynamoDB streams
  - Kinesis for real-time processing
- ✅ Serverless Resilience
  - Dead-letter queues (DLQ)
  - Automatic retries
  - Lambda concurrent execution limits
  - Reserved concurrency for critical functions

**Study Tasks:**
- [ ] Study: Lambda and Fargate concepts (20 min)
- [ ] Review: Event-driven patterns (15 min)
- [ ] Learn: EventBridge routing rules (10 min)
- [ ] Create: Event-driven architecture diagram
- [ ] Practice: Design serverless data pipeline (15 min)

**Key Takeaways:**
- Lambda scales automatically to demand
- Fargate for container workloads
- EventBridge enables event-driven design
- DLQ for failed message handling

---

### **Day 20: Managed Services for Resilience**

**Learning Objectives:**
- Understand managed service benefits
- Learn to evaluate services
- Master data stores for resilience

**Key Topics:**
- ✅ Managed Databases
  - Amazon RDS for relational
  - DynamoDB for NoSQL
  - Aurora for high performance
  - ElastiCache for caching
- ✅ Message Queues & Streaming
  - SQS Standard vs. FIFO
  - SNS for pub/sub
  - Kinesis for streaming data
  - MSK for managed Kafka
- ✅ Caching Strategies
  - ElastiCache (Redis/Memcached)
  - CloudFront for content caching
  - DAX for DynamoDB
  - Application-level caching

**Study Tasks:**
- [ ] Study: Managed service options (20 min)
- [ ] Review: Choosing right database (15 min)
- [ ] Learn: Caching strategies (10 min)
- [ ] Create: Service selection decision tree
- [ ] Practice: Recommend services for use case (15 min)

**Key Takeaways:**
- Managed services reduce operational burden
- Aurora combines MySQL/PostgreSQL reliability with DynamoDB scale
- Caching critical for performance
- Queue depth for auto scaling trigger

---

### **Day 21: Domain 2 Review & Practice**

**Learning Objectives:**
- Review all Domain 2 concepts
- Take practice tests
- Identify weak areas

**Key Topics:**
- ✅ Review: Task 2.1 - Scalable & Loose Coupling
  - Auto Scaling and load balancing
  - Microservices and event-driven
  - Managed services
- ✅ Review: Task 2.2 - High Availability
  - Multi-AZ design
  - Disaster recovery strategies
  - Failover mechanisms
  - Resilience patterns

**Study Tasks:**
- [ ] Review all Domain 2 study notes (30 min)
- [ ] Take: Full Domain 2 practice test (45 min)
- [ ] Analyze: Weak areas (15 min)
- [ ] Study: Incorrect answers deep dive (20 min)
- [ ] Create: Domain 2 quick reference guide

**Expected Performance:**
- Target score: 80%+ on practice test
- Review weak topics before moving on
- Focus on architecture design questions

**End of Week 3 Check:**
- [ ] Understand Auto Scaling and load balancing
- [ ] Design highly available systems
- [ ] Implement DR strategies
- [ ] Understand serverless and event-driven
- [ ] Score 80%+ on Domain 2 practice test

---

## 📍 WEEK 4: High-Performance & Cost-Optimized (Days 22-30)

### **Day 22: Storage Solutions for Performance**

**Learning Objectives:**
- Understand storage types and characteristics
- Choose appropriate storage for workload
- Master storage optimization

**Key Topics:**
- ✅ Storage Types
  - Object Storage (S3) - unlimited scale
  - File Storage (EFS, FSx) - shared access
  - Block Storage (EBS) - instance volumes
  - Database Storage (RDS, DynamoDB)
- ✅ S3 Optimization
  - S3 Standard for frequently accessed
  - S3 Intelligent-Tiering for variable access
  - S3 Glacier for archival
  - Multi-part upload for large objects
  - Byte-range fetches for efficiency
- ✅ EBS & EFS Performance
  - EBS volume types (gp3, io2, st1, sc1)
  - Provisioned IOPS vs. baseline
  - EFS performance characteristics
  - Throughput optimization

**Study Tasks:**
- [ ] Study: Storage type comparison (20 min)
- [ ] Review: S3 storage classes (10 min)
- [ ] Learn: EBS performance tuning (15 min)
- [ ] Create: Storage selection matrix
- [ ] Practice: Design storage for database (15 min)

**Key Takeaways:**
- S3 for unstructured data (photos, logs)
- EFS for shared file access
- EBS for database volumes (provisioned IOPS)
- Storage class selection critical for performance

---

### **Day 23: Compute Solutions for Performance**

**Learning Objectives:**
- Design high-performing compute
- Understand compute options
- Master instance selection

**Key Topics:**
- ✅ Compute Options
  - EC2 instance types (general, compute, memory optimized)
  - Lambda for serverless
  - Fargate for containers
  - Batch for batch processing
  - EMR for big data
- ✅ Instance Selection
  - vCPU, memory, network characteristics
  - Burstable performance (t3/t4)
  - Dedicated instances/hosts for compliance
  - Placement groups for performance
- ✅ Auto Scaling Performance
  - Warm pools for faster scaling
  - Predictive scaling using ML
  - Target tracking for consistency
  - Scheduled scaling for known patterns

**Study Tasks:**
- [ ] Study: EC2 instance families (20 min)
- [ ] Review: Compute option comparison (15 min)
- [ ] Learn: Instance selection best practices (10 min)
- [ ] Create: Compute recommendation guide
- [ ] Practice: Select instances for workload (15 min)

**Key Takeaways:**
- m5/m6 for general purpose
- c5/c6 for compute intensive
- r5/r6 for memory intensive
- Lambda for variable workloads
- Fargate for consistent container load

---

### **Day 24: Database Solutions for Performance**

**Learning Objectives:**
- Choose appropriate database engine
- Understand database scaling strategies
- Master query optimization

**Key Topics:**
- ✅ Database Engines
  - MySQL/PostgreSQL for relational
  - DynamoDB for NoSQL
  - Aurora for high performance
  - Redshift for analytics
  - ElastiCache for caching
- ✅ Scaling Strategies
  - Read replicas for read scaling
  - Database clustering
  - Partitioning and sharding
  - Connection pooling with RDS Proxy
- ✅ Performance Optimization
  - Indexing strategies
  - Query optimization
  - Connection limits
  - Query result caching
  - Provisioned concurrency

**Study Tasks:**
- [ ] Study: Database engine comparison (20 min)
- [ ] Review: Scaling relational databases (15 min)
- [ ] Learn: DynamoDB performance (10 min)
- [ ] Create: Database selection guide
- [ ] Practice: Design database for high traffic (15 min)

**Key Takeaways:**
- Aurora: MySQL/PostgreSQL + DynamoDB scale
- DynamoDB: NoSQL with automatic scaling
- Read replicas for read-heavy workloads
- RDS Proxy for connection management
- ElastiCache for query result caching

---

### **Day 25: Network Architecture for Performance**

**Learning Objectives:**
- Design high-performing networks
- Master CDN and edge services
- Understand network optimization

**Key Topics:**
- ✅ Network Performance
  - CloudFront CDN for content
  - AWS Global Accelerator for applications
  - Edge locations for latency reduction
  - Transfer acceleration for S3
- ✅ Network Design
  - VPC flow design
  - Subnet sizing
  - Route optimization
  - Cross-AZ latency
  - VPC endpoints for private access
- ✅ Application Integration
  - API Gateway for APIs
  - Network Load Balancer for performance
  - Elastic IP for failover
  - Route 53 routing policies

**Study Tasks:**
- [ ] Study: CloudFront and CDN concepts (20 min)
- [ ] Review: Global Accelerator (10 min)
- [ ] Learn: Network optimization (10 min)
- [ ] Create: Network architecture diagram
- [ ] Practice: Design global application (15 min)

**Key Takeaways:**
- CloudFront for static and dynamic content
- Global Accelerator for latency-sensitive apps
- VPC endpoints avoid internet gateway
- Route 53 geolocation routing for optimization

---

### **Day 26: Data Ingestion & Transformation**

**Learning Objectives:**
- Design high-performing data pipelines
- Master data transformation services
- Learn streaming patterns

**Key Topics:**
- ✅ Data Ingestion Patterns
  - Batch ingestion (EMR, Glue)
  - Real-time streaming (Kinesis)
  - Near real-time (Firehose)
  - Database migration (DMS)
- ✅ Data Transformation
  - AWS Glue for ETL
  - Lambda for lightweight transforms
  - EMR for distributed processing
  - Kinesis for stream processing
- ✅ Data Lake Architecture
  - AWS Lake Formation for setup
  - Athena for querying
  - Redshift for analytics
  - QuickSight for visualization

**Study Tasks:**
- [ ] Study: Ingestion patterns (20 min)
- [ ] Review: Transformation services (15 min)
- [ ] Learn: Data lake architecture (10 min)
- [ ] Create: Data pipeline diagram
- [ ] Practice: Design data platform (15 min)

**Key Takeaways:**
- Kinesis for real-time streaming
- Glue for batch ETL jobs
- Lake Formation simplifies data lake setup
- Partition data for query performance

---

### **Day 27: Cost Optimization - Foundations**

**Learning Objectives:**
- Understand AWS cost models
- Learn cost optimization strategies
- Master cost tools

**Key Topics:**
- ✅ Cost Models
  - Pay-as-you-go pricing
  - Regional differences
  - Data transfer costs
  - Reserved Capacity options
- ✅ Cost Optimization Tools
  - AWS Cost Explorer
  - AWS Budgets for alerts
  - AWS Cost Anomaly Detection
  - AWS Trusted Advisor
  - Compute Optimizer
- ✅ Purchasing Options
  - On-Demand (flexible, highest cost)
  - Reserved Instances (1-3 year, up to 72% savings)
  - Savings Plans (1-3 year, compute flexibility)
  - Spot Instances (up to 90% savings, interruptible)

**Study Tasks:**
- [ ] Study: AWS pricing models (20 min)
- [ ] Review: Cost management tools (15 min)
- [ ] Learn: Purchasing options (10 min)
- [ ] Create: Cost optimization checklist
- [ ] Practice: Analyze cost scenario (15 min)

**Key Takeaways:**
- Reserved Instances for stable workloads
- Spot Instances for flexible workloads
- Savings Plans for compute flexibility
- Cost anomaly detection for monitoring

---

### **Day 28: Cost Optimization - Across Services**

**Learning Objectives:**
- Optimize costs in each service category
- Master waste elimination
- Learn right-sizing

**Key Topics:**
- ✅ Compute Cost Optimization
  - Right-sizing instances
  - Unused resources elimination
  - Hibernation for batch jobs
  - EC2 Fleet for diversification
- ✅ Storage Cost Optimization
  - S3 lifecycle policies
  - S3 Intelligent-Tiering
  - Glacier for archival
  - Delete unused snapshots
- ✅ Database Cost Optimization
  - Aurora (cost-effective scaling)
  - DynamoDB on-demand vs. provisioned
  - ElastiCache for read optimization
  - RDS reserved capacity
- ✅ Network Cost Optimization
  - NAT Gateway vs. Instance
  - VPC Endpoints for private access
  - Direct Connect for large transfers
  - Data transfer minimization

**Study Tasks:**
- [ ] Study: Service-specific cost optimization (25 min)
- [ ] Review: Waste elimination strategies (10 min)
- [ ] Learn: Right-sizing approaches (10 min)
- [ ] Create: Cost optimization guide per service
- [ ] Practice: Design cost-optimized solution (15 min)

**Key Takeaways:**
- Aurora cheaper than RDS at scale
- S3 Intelligent-Tiering for variable patterns
- NAT Gateway more cost-effective at scale
- Compute Optimizer for right-sizing

---

### **Day 29: Domain 3 & 4 Review & Practice**

**Learning Objectives:**
- Review Domains 3 and 4 concepts
- Take comprehensive practice tests
- Identify remaining weak areas

**Key Topics:**
- ✅ Review: Domain 3 - High-Performing
  - Storage, compute, database solutions
  - Network architectures
  - Data ingestion and transformation
- ✅ Review: Domain 4 - Cost-Optimized
  - Storage optimization
  - Compute optimization
  - Database optimization
  - Network optimization

**Study Tasks:**
- [ ] Review all Domain 3 and 4 study notes (40 min)
- [ ] Take: Domain 3 practice test (45 min)
- [ ] Take: Domain 4 practice test (45 min)
- [ ] Analyze: Weak areas across domains (20 min)
- [ ] Study: Incorrect answers deep dive (30 min)

**Expected Performance:**
- Domain 3 target: 75%+
- Domain 4 target: 75%+
- Focus on architecture trade-offs

---

### **Day 30: Final Review & Full Practice Exam**

**Learning Objectives:**
- Comprehensive exam preparation
- Identify last-minute study areas
- Build exam confidence

**Study Tasks:**
- [ ] Take: Full practice exam (130 min)
  - 50 questions timed
  - Simulated exam conditions
  - No breaks or references
- [ ] Score analysis by domain (20 min)
  - Domain 1 performance
  - Domain 2 performance
  - Domain 3 performance
  - Domain 4 performance
- [ ] Quick review of weak areas (30 min)
- [ ] Final tips preparation (10 min)
- [ ] Rest and prepare (relaxation)

**Full Practice Exam Score:**
- Target: 720+/1000 (72%+)
- Domain breakdown:
  - Domain 1: 15/15 (100%) = 30 points
  - Domain 2: 11/13 (85%) = 22 points
  - Domain 3: 9/12 (75%) = 18 points
  - Domain 4: 7/10 (70%) = 14 points

**Last-Minute Tips:**
- [ ] Review AWS Well-Architected Framework
- [ ] Review Domain 1 weak areas
- [ ] Practice reading questions carefully
- [ ] Understand trade-offs in solutions
- [ ] Get good sleep night before exam
- [ ] Eat well morning of exam
- [ ] Arrive early to exam center

---

## 🏁 Exam Day Preparation

### **Before Exam (1 week)**
- Finalize last weak topics
- Do light review, not heavy studying
- Get regular sleep (7-9 hours)
- Exercise and manage stress
- Plan exam day logistics

### **Exam Day Morning**
- Eat healthy breakfast
- Arrive 15 minutes early
- Bring required ID
- Minimize stress
- Clear your mind

### **During Exam**
- Read questions carefully
- Watch for negatives in questions
- Flag difficult questions
- Don't second-guess answers
- Manage time (2 min/question)
- Review flagged questions at end

### **After Exam**
- Celebrate if you pass!
- If fail, review weak areas and reschedule
- Get official exam report
- Schedule retake if needed

---

## 📊 Progress Tracking Template

```
WEEK 1: Foundation & Secure Architectures
Day 1: ☐ Complete  Score: __/100
Day 2: ☐ Complete  Score: __/100
Day 3: ☐ Complete  Score: __/100
Day 4: ☐ Complete  Score: __/100
Day 5: ☐ Complete  Score: __/100
Day 6: ☐ Complete  Score: __/100
Day 7: ☐ Complete  Score: __/100
Weekly Test: ☐ Complete  Score: __/100

WEEK 2: Secure Architectures Deep Dive
Day 8: ☐ Complete  Score: __/100
Day 9: ☐ Complete  Score: __/100
Day 10: ☐ Complete  Score: __/100
Day 11: ☐ Complete  Score: __/100
Day 12: ☐ Complete  Score: __/100
Day 13: ☐ Complete  Score: __/100
Day 14: ☐ Complete  Score: __/100
Domain 1 Test: ☐ Complete  Score: __/100

WEEK 3: Resilient Architectures
Day 15: ☐ Complete  Score: __/100
Day 16: ☐ Complete  Score: __/100
Day 17: ☐ Complete  Score: __/100
Day 18: ☐ Complete  Score: __/100
Day 19: ☐ Complete  Score: __/100
Day 20: ☐ Complete  Score: __/100
Day 21: ☐ Complete  Score: __/100
Domain 2 Test: ☐ Complete  Score: __/100

WEEK 4: Performance & Cost
Day 22: ☐ Complete  Score: __/100
Day 23: ☐ Complete  Score: __/100
Day 24: ☐ Complete  Score: __/100
Day 25: ☐ Complete  Score: __/100
Day 26: ☐ Complete  Score: __/100
Day 27: ☐ Complete  Score: __/100
Day 28: ☐ Complete  Score: __/100
Day 29: ☐ Complete  Score: __/100
Day 30: ☐ Complete  Score: __/1000

FINAL GOAL: 720+/1000 ✓
```

---

## 🎯 Success Criteria

| Metric | Target | Status |
|--------|--------|--------|
| Domain 1 Score | 85%+ | ☐ |
| Domain 2 Score | 80%+ | ☐ |
| Domain 3 Score | 75%+ | ☐ |
| Domain 4 Score | 75%+ | ☐ |
| Overall Exam Score | 720+/1000 | ☐ |
| Study Time | 120+ hours | ☐ |
| Practice Tests | 5+ | ☐ |
| Hands-on Labs | 10+ | ☐ |

---

*Good luck with your AWS SAA-C03 exam preparation!* 🚀

