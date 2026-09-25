# Domain 2: Design Resilient Architectures (26% of Exam)

**Content Domain 2 covers 26% of the exam with approximately 13 questions.**

---

## 📌 Overview

<cite index="1-42">This domain focuses on designing scalable and loosely coupled architectures, and designing highly available and/or fault-tolerant architectures.</cite>

**Resilience = The ability to recover quickly from failures and adapt to changing demands.**

### Two Key Tasks

1. **Task 2.1**: Design scalable and loosely coupled architectures
2. **Task 2.2**: Design highly available and/or fault-tolerant architectures

---

## 📈 Task 2.1: Design Scalable and Loosely Coupled Architectures

### What You Need to Know

<cite index="1-42">Knowledge of API creation and management (for example, Amazon API Gateway, REST API), AWS managed services with appropriate use cases (for example, AWS Transfer Family, Amazon SQS, AWS Secrets Manager), caching strategies, design principles for microservices (for example, stateless workloads compared with stateful workloads), event-driven architectures, horizontal scaling and vertical scaling, how to appropriately use edge accelerators (for example, content delivery network [CDN]), how to migrate applications into containers, load balancing concepts (for example, Application Load Balancer [ALB]), multi-tier architectures, queuing and messaging concepts (for example, publish/subscribe), serverless technologies and patterns (for example, AWS Fargate, AWS Lambda), storage types with associated characteristics (for example, object, file, block), the orchestration of containers (for example, Amazon ECS, Amazon EKS), when to use read replicas, and workflow orchestration (for example, AWS Step Functions).</cite>

### Key Concepts

#### 1. **Horizontal vs. Vertical Scaling**

**Horizontal Scaling** (Scale Out):
- Add more instances instead of bigger ones
- Better resilience (one failure doesn't take down system)
- Works with cloud architecture
- Cost-effective with spot instances
- Example: Add 10 small servers instead of 1 huge server

**Vertical Scaling** (Scale Up):
- Bigger instance type, more CPU/memory
- Limit to largest available instance
- Downtime during scale up
- Better for stateful workloads
- Example: t3.medium → t3.xlarge

**Best Practice**: Prefer horizontal with auto scaling for cloud workloads.

#### 2. **Auto Scaling Architecture**

**Components**:
```
Launch Template:
  - Define instance configuration (AMI, instance type, security groups)
  - Version control for updates

Auto Scaling Group:
  - Min size: 2 (availability)
  - Desired capacity: 4 (normal load)
  - Max size: 10 (peak load)

Scaling Policies:
  - Target tracking: CPU at 70%
  - Step scaling: Different actions at different thresholds
  - Scheduled: 9 AM ramp up, 6 PM ramp down

Lifecycle Hooks:
  - Custom actions on launch/termination
  - Draining connections, cleanup tasks
```

**Scaling Speed**:
- Warm pools: Pre-launched instances for faster scaling
- Predictive scaling: ML predicts demand
- Termination protection: Don't scale down critical

#### 3. **Load Balancing**

<cite index="1-42">Load balancing concepts (for example, Application Load Balancer [ALB]).</cite>

| Load Balancer | Layer | Use Case | Protocol |
|---------------|-------|----------|----------|
| **ALB** | 7 (Application) | Web apps, microservices, multiple domains | HTTP/HTTPS |
| **NLB** | 4 (Transport) | Ultra-high performance, gaming, IoT | TCP/UDP |
| **CLB** | 4/7 (Classic) | Legacy, EC2-Classic | HTTP/HTTPS/TCP |
| **GLB** | 3-7 (Gateway) | Inline appliances, third-party | IP protocol |

**ALB Features**:
- Path-based routing (/api → api-server, /images → image-server)
- Host-based routing (api.example.com → api-server)
- Hostname routing (secure.example.com → SSL-server)
- Request attribute routing

**Health Checks**:
- HTTP status 200-399 = healthy
- Healthy threshold: 2 consecutive (default)
- Unhealthy threshold: 2 consecutive (default)
- Interval: 30 seconds (default)
- Timeout: 5 seconds (default)

#### 4. **Microservices & Loose Coupling**

**Microservices Benefits**:
- Independent scaling (API server up 10x without scaling database)
- Technology flexibility (Node for API, Python for processing)
- Independent deployment (deploy authentication service without redeploying web)
- Fault isolation (broken service doesn't crash entire system)

**Loose Coupling Patterns**:
- **Asynchronous**: SQS, SNS (don't wait for response)
- **Synchronous**: REST APIs, gRPC (immediate response)
- **Event-driven**: EventBridge, Lambda (react to events)

**Challenges of Microservices**:
- Network latency and failures
- Distributed debugging
- Data consistency across services
- Operational complexity

#### 5. **Event-Driven Architecture**

**Components**:
```
Event Source:
  - S3 bucket (file upload)
  - DynamoDB Streams (database change)
  - Kinesis (real-time data)
  - Custom application

Event Router:
  - EventBridge: Powerful routing with rules
  - SNS: Simple publish/subscribe
  - SQS: Reliable queuing

Event Target:
  - Lambda function
  - EC2, ECS tasks
  - API endpoints
  - Data stores (S3, DynamoDB)
```

**EventBridge Routing Example**:
```
Rule: source = "myapp" AND detail-type = "order"
Target 1: Payment Lambda
Target 2: Inventory SQS
Target 3: Analytics Firehose

Benefits:
- One event triggers multiple actions
- No code changes for new consumers
- Audit trail of all events
```

#### 6. **Queuing & Messaging**

**Amazon SQS** (Simple Queue Service):
- **Standard**: Unlimited throughput, at-least-once delivery, best effort ordering
- **FIFO**: Exactly-once, ordered processing, limited throughput (300 msg/sec)
- Message retention: 1 minute to 14 days
- Visibility timeout: Prevent other workers processing message
- Dead Letter Queue (DLQ): Capture failed messages

**Use Cases**:
- Decouple web app from backend workers
- Batch processing
- Rate limiting (queue caps processing speed)
- Durability (messages persisted across failures)

**Amazon SNS** (Simple Notification Service):
- Publish/Subscribe model
- Multiple subscribers to one topic
- Protocol: SQS, Lambda, HTTP, email, SMS
- Push-based (messages sent immediately)

**Amazon Kinesis**:
- Real-time data streaming
- Multiple consumers from same stream
- Shards for throughput (more shards = more throughput)
- Record retention: 24 hours (default) to 365 days

#### 7. **Containerization & Orchestration**

<cite index="1-42">The orchestration of containers (for example, Amazon ECS, Amazon EKS).</cite>

**Amazon ECS** (Elastic Container Service):
- AWS native container orchestration
- Tasks: Docker container definition
- Services: Running instances of tasks with auto scaling
- Launch types: EC2 or Fargate
- Easy integration with AWS services

**AWS Fargate**:
- Serverless container compute
- Don't manage EC2 instances
- Pay per vCPU-hour + memory-hour
- Simpler, but less control

**Amazon EKS** (Elastic Kubernetes Service):
- Kubernetes on AWS
- Complex orchestration, larger feature set
- Control plane managed by AWS
- Community standard, portable to other clouds

**When to Use**:
- ECS: AWS-first, integrated services, simpler
- EKS: Kubernetes needed, multi-cloud, complex workloads

#### 8. **Serverless Patterns**

<cite index="1-42">Serverless technologies and patterns (for example, AWS Fargate, AWS Lambda).</cite>

**AWS Lambda**:
- Functions triggered by events
- Automatic scaling from 0 to thousands
- Pay only for execution time
- Supported languages: Python, Node.js, Java, Go, Ruby, C#, PowerShell
- Duration: 15 minutes max (use Step Functions for longer)

**Lambda Scaling**:
- Concurrent executions: 1000 (default, soft limit)
- Reserved concurrency: Guarantee capacity for critical functions
- Provisioned concurrency: Pre-warmed instances

**Step Functions** (Workflow Orchestration):
- Coordinate multiple Lambda functions/services
- States: Task (execute), Choice (if/else), Wait, Parallel
- Visual workflow editor
- Automatic retries and error handling

#### 9. **Caching Strategies**

**Multi-Level Caching**:
```
User Request
    ↓
CloudFront (edge caching)
    ↓
ALB (connection multiplexing)
    ↓
Application (Redis in-memory)
    ↓
Database Query Result Cache
    ↓
Database (expensive computation)
```

**ElastiCache** (In-Memory Cache):
- **Memcached**: Simple key-value, no persistence, good for sessions
- **Redis**: Persistence, pub/sub, data structures, transactions

**Cache Invalidation Patterns**:
- TTL: Automatic expiration (90 seconds)
- Event-based: Invalidate on database update
- LRU: Least Recently Used items evicted
- Write-through: Update cache and database together

#### 10. **API Gateway for Integration**

- Request/response transformation
- Authorization and authentication
- Rate limiting and throttling
- CORS handling
- API versioning
- Request validation

### Practice Questions Hints

**Q: How do you make architecture more scalable?**
- A: Horizontal scaling with auto scaling, stateless design, caching, async processing

**Q: When should you use SQS vs. SNS?**
- A: SQS for guaranteed processing (queue), SNS for broadcast (fan-out)

**Q: What's the benefit of microservices over monolithic?**
- A: Independent scaling, deployment, technology choice; better resilience

**Q: How do you handle spiky traffic?**
- A: Auto scaling + SQS queue + caching + CDN

---

## 🚀 Task 2.2: Design Highly Available and/or Fault-Tolerant Architectures

### What You Need to Know

<cite index="1-43">Knowledge of AWS global infrastructure (for example, Availability Zones, AWS Regions, Amazon Route 53), AWS Managed Services (AMS) with appropriate use cases (for example, Amazon Comprehend, Amazon Polly), basic networking concepts (for example, route tables), disaster recovery (DR) strategies (for example, backup and restore, pilot light, warm standby, active-active failover, recovery point objective [RPO], recovery time objective [RTO]), distributed design patterns, failover strategies, immutable infrastructure, load balancing concepts (for example, ALB), proxy concepts (for example, Amazon RDS Proxy), service quotas and throttling (for example, how to configure the service quotas for a workload in a standby environment), storage options and characteristics (for example, durability, replication), and workload visibility (for example, AWS X-Ray).</cite>

### Key Concepts

#### 1. **High Availability vs. Fault Tolerance**

**High Availability**:
- System continues operating despite failures
- Automatic failover to backup
- Minimal downtime (but some downtime possible)
- Example: ALB switches traffic if instance fails
- Cost: Medium (multiple instances/regions)

**Fault Tolerance**:
- System continues operating with no downtime
- Multiple redundant components
- Requires synchronous replication
- Example: RDS Multi-AZ with automatic failover
- Cost: High (real-time sync across zones)

**Choosing**: Most systems need HA, few need full fault tolerance.

#### 2. **Multi-AZ Architecture**

**Availability Zones**:
- Physically separate data centers within region
- Hundreds of kilometers apart
- Independent infrastructure (power, cooling, networking)
- Low-latency network between zones

**Multi-AZ Deployment**:
```
Users
  ↓
Route 53 (DNS)
  ↓
ALB (distributes traffic)
  ├─ AZ-1: Web Server
  ├─ AZ-2: Web Server  
  └─ AZ-3: Web Server
  ↓
RDS Multi-AZ (synchronous replication)
  ├─ AZ-1: Primary Database
  └─ AZ-2: Standby (not used, failover only)
```

**RDS Multi-AZ Failover**:
- Automatic failover to standby (1-2 minutes)
- DNS updated automatically
- Transparent to application
- Recommended for production databases

#### 3. **Disaster Recovery Strategies**

<cite index="1-43">Disaster recovery (DR) strategies (for example, backup and restore, pilot light, warm standby, active-active failover, recovery point objective [RPO], recovery time objective [RTO]).</cite>

**RTO (Recovery Time Objective)**:
- How quickly must system be restored?
- Shorter RTO = Higher cost
- Example: RTO = 1 hour (must restore within 1 hour)

**RPO (Recovery Point Objective)**:
- How much data can you lose?
- Shorter RPO = More frequent backups = Higher cost
- Example: RPO = 15 minutes (acceptable data loss = 15 min)

**DR Strategies** (from cheapest to most expensive):

1. **Backup & Restore**
   - RTO: 24 hours (or more)
   - RPO: 24 hours
   - Cost: Very low
   - Use Case: Non-critical systems, dev/test
   - Implementation: Daily snapshots to S3

2. **Pilot Light**
   - RTO: 4-12 hours
   - RPO: 1 hour
   - Cost: Low to medium
   - Use Case: Acceptable brief outage
   - Implementation: Minimal version of app in second region, scale up on disaster

3. **Warm Standby**
   - RTO: 1-4 hours
   - RPO: 15-60 minutes
   - Cost: Medium to high
   - Use Case: Important systems
   - Implementation: Reduced capacity in second region, scale up on failover

4. **Active-Active (Multi-Region)**
   - RTO: Minutes
   - RPO: 0-15 minutes (near real-time)
   - Cost: Very high
   - Use Case: Business-critical systems
   - Implementation: Full deployment in multiple regions with traffic split

**Choosing Strategy**:
```
Business Requirement → RTO/RPO → DR Strategy → Cost

Example:
- Ecommerce: RTO 1 hour, RPO 15 min → Warm Standby
- Banking: RTO 5 min, RPO 5 min → Active-Active
- Internal tool: RTO 8 hours, RPO 24 hours → Backup & Restore
```

#### 4. **Failover Strategies**

**Route 53 Health Checks**:
- Check endpoint health (HTTP, TCP, CloudWatch)
- Failover record: Primary (primary region), Secondary (DR region)
- Automatic DNS failover when primary unhealthy
- Multi-value answer routing: Route 53 returns all healthy IPs

**Example**:
```
Route 53 Failover Routing:
  Primary: www.example.com → us-east-1 ALB
  Health check: GET /health (200 = OK)
  
  If health check fails:
    Failover to Secondary → eu-west-1 ALB
    
  Application doesn't need to know about failover
```

**Application-Level Failover**:
- Retry logic with exponential backoff
- Circuit breaker: Stop retrying after repeated failures
- Graceful degradation: Reduce features, not availability

#### 5. **Distributed Design Patterns**

**Stateless Design**:
- No session data stored on servers
- Session data in external store (ElastiCache, DynamoDB)
- Any server can handle request
- Enables horizontal scaling

**Example - E-commerce checkout**:
```
Request 1: browser → server-1 (sets session in ElastiCache)
Request 2: browser → server-2 (reads session from ElastiCache)
        ↓ (server-1 crashes)
Request 3: browser → server-3 (session still available)
```

**Idempotent Operations**:
- Repeating operation = same result
- Safe to retry without side effects
- Example: PUT /api/users/123 (create/update)
- Bad: POST /api/transfer (could duplicate transaction)

**Database Synchronization**:
- **Read Replicas**: Async replication, read-only
- **Multi-AZ**: Sync replication, one standby
- **Multi-region**: Async cross-region, for DR

#### 6. **RDS Proxy for Connection Management**

**Problem Without RDS Proxy**:
- Thousands of connections from app servers
- Database has connection limit
- Connection pooling: Expensive

**RDS Proxy Solution**:
```
App Servers
    ↓ (many connections)
RDS Proxy (connection pooling)
    ↓ (few connections)
RDS Database (manageable connections)

Benefits:
- Improves database performance 100x
- Shorter failover time (transparently switches to read replica)
- IAM authentication for apps
```

#### 7. **Storage Durability & Replication**

**S3 Durability & Availability**:
- **Durability**: 99.999999999% (11 9's) = 1 loss per 10 billion objects
- **Availability**: 99.99% = 52 minutes downtime per year
- Cross-AZ replication built-in

**EBS Durability**:
- Automatically replicated within AZ
- Snapshots stored in S3 for durability
- Not automatically replicated to other AZs
- Use RDS Multi-AZ for cross-AZ database durability

**EFS (Elastic File System)**:
- Automatically replicated across AZs
- Shared across EC2 instances
- Consistent performance
- Good for distributed applications

#### 8. **Immutable Infrastructure**

**Benefits**:
- Golden AMI: Pre-configured OS + all software
- Launch instances consistently
- Reduces configuration drift
- Easier rollback (terminate + launch old AMI)

**Pipeline**:
```
Build:
  Create EC2 instance
  Install software
  Configure settings
  Run tests

Package:
  Create AMI from instance
  Store in image repository
  Version/tag AMI

Deploy:
  Auto Scaling group uses AMI
  No post-launch configuration
  Consistent across all instances

Rollback:
  Change ASG to use old AMI
  Instant consistency
```

#### 9. **Service Quotas & Throttling**

**Service Quotas** (Limits):
- Account limits on resources
- Examples: 5 VPCs per region, 20 RDS instances
- Can request increase via AWS Support
- Important in DR: Request quota increase in backup region

**Throttling**:
- API rate limits to prevent abuse
- Example: 100 requests/second to API Gateway
- Applications must handle gracefully (exponential backoff)
- Auto Scaling respects limits

#### 10. **Observability for Resilience**

**AWS X-Ray** (Distributed Tracing):
- Track requests across microservices
- Identify bottlenecks and failures
- Service map shows dependencies
- Latency analysis

**CloudWatch**:
- Metrics: CPU, memory, network
- Logs: Application and system logs
- Alarms: Trigger on thresholds
- Dashboards: Visualize health

**Combining for Resilience**:
```
X-Ray shows service B is slow
  ↓
CloudWatch metrics show B's CPU 99%
  ↓
Auto Scaling triggered, B scales out
  ↓
New instances brought online
  ↓
Latency returns to normal
  ↓
Auto Scaling scales back down
```

### Practice Questions Hints

**Q: What's the minimum number of AZs for HA?**
- A: 2 (spread load, failover capability)

**Q: How long does RDS Multi-AZ failover take?**
- A: 1-2 minutes (automatic and transparent)

**Q: When should you use warm standby vs. active-active?**
- A: Warm standby if some downtime acceptable, active-active for zero-downtime requirement

**Q: How do you achieve RTO < 5 minutes for critical database?**
- A: Read replicas in multiple regions, automated failover, or DynamoDB for horizontal scale

---

## 🎯 Domain 2 Key Takeaways

### Resilience Principles
1. **Redundancy**: Multiple instances of critical components
2. **Automation**: Automatic failover, scaling, recovery
3. **Stateless Design**: Services independent of state
4. **Distributed**: Spread across AZs and regions

### Top Services for Domain 2
| Service | Purpose | Key Strength |
|---------|---------|--------------|
| Auto Scaling | Scale capacity automatically | Cost efficiency |
| ALB/NLB | Distribute load | High availability |
| RDS Multi-AZ | Database HA | Automatic failover |
| Route 53 | DNS failover | Geographic routing |
| SQS | Async processing | Decoupling |
| SNS | Pub/Sub messaging | Broadcast |
| Lambda | Event-driven compute | Auto-scaling to 0 |
| ECS/Fargate | Containers | Managed orchestration |

### Common Exam Patterns
- **HA Question**: Multi-AZ with Auto Scaling
- **DR Question**: Identify RTO/RPO and suggest strategy
- **Failover**: Route 53 health checks + secondary region
- **Decoupling**: SQS for async, SNS for broadcast
- **Scaling**: Auto Scaling group with ALB

---

## 📚 Advanced Topics

1. <cite index="1-43">AWS Managed Services (AMS) with appropriate use cases (for example, Amazon Comprehend, Amazon Polly)</cite> - Managed services reduce operational burden
2. Cross-region replication strategies for disaster recovery
3. Lambda concurrent execution limits and reserved concurrency
4. Database replication lag and consistency models

---

*Study Time: 8-10 hours | Practice: 30+ questions | Hands-On: Design multi-AZ application with failover*

