# Domain 4: Design Cost-Optimized Architectures (20% of Exam)

**Content Domain 4 covers 20% of the exam with approximately 10 questions.**

---

## 📌 Overview

(cite index="1-47,1-48,1-49,1-50">This domain focuses on designing cost-optimized storage solutions, designing cost-optimized compute solutions, designing cost-optimized database solutions, and designing cost-optimized network architectures.</cite>

**Cost Optimization = Getting the required results at the lowest price.**

### Four Key Tasks

1. **Task 4.1**: Design cost-optimized storage solutions
2. **Task 4.2**: Design cost-optimized compute solutions
3. **Task 4.3**: Design cost-optimized database solutions
4. **Task 4.4**: Design cost-optimized network architectures

---

## 💰 Task 4.1: Cost-Optimized Storage

### Storage Cost Management

(cite index="1-47,1-48">AWS cost management service features (for example, cost allocation tags, multi-account billing), AWS cost management tools with appropriate use cases (for example, AWS Cost Explorer, AWS Budgets, AWS Cost and Usage Report).</cite>

**Cost Tools**:
- **AWS Cost Explorer**: Visualize spending trends
- **AWS Budgets**: Set alerts at thresholds
- **Compute Optimizer**: Right-sizing recommendations
- **Trusted Advisor**: Best practices (limited free tier)

### S3 Cost Optimization

**Storage Classes** (by cost):
1. **S3 Standard**: $0.023/GB (frequent access)
2. **S3 Standard-IA**: $0.0125/GB (infrequent, 30-day minimum)
3. **S3 One Zone-IA**: $0.01/GB (non-critical, single AZ)
4. **S3 Glacier Instant**: $0.004/GB (millisecond retrieval)
5. **S3 Glacier Flexible**: $0.0036/GB (3-5 hour retrieval)
6. **S3 Deep Archive**: $0.00099/GB (7-10 hour retrieval)

**Lifecycle Policies**:
```
Day 0:     Standard       ($0.023/GB)
Day 30:    Standard-IA    ($0.0125/GB) = 46% savings
Day 90:    Glacier        ($0.0036/GB) = 84% savings
Day 365:   Deep Archive   ($0.00099/GB) = 96% savings
Day 2555:  DELETE         = 100% savings
```

**Cost Savings**:
- Lifecycle policies: 50-80% savings
- Compression: Reduce size 2-10x
- Deduplication: Remove duplicates
- Intelligent-Tiering: Auto-moves, no retrieval charges if accessed

### EBS Cost Optimization

(cite index="1-48">Block storage options (for example, hard disk drive [HDD] volume types, solid state drive [SSD] volume types).</cite>

| Volume | Cost | IOPS | Use Case |
|--------|------|------|----------|
| **gp3** | $0.10/GB | 3,000 | Default |
| **gp2** | $0.10/GB | 3,000 | Legacy |
| **io2** | $0.125/GB | 64,000 | High performance |
| **st1** | $0.045/GB | - | Throughput |
| **sc1** | $0.015/GB | - | Infrequent |

**Optimization**:
- Use gp3 instead of gp2 (same cost, better performance)
- sc1 for infrequently accessed volumes (saves 85%)
- Right-size volumes (don't over-provision)
- Delete unattached snapshots

### EFS Cost Optimization

- Pay per GB stored
- Standard: $0.30/GB/month
- Infrequent: $0.025/GB/month (EFS-IA)
- Move to Standard-IA if < 50% access frequency

---

## 💾 Task 4.2: Cost-Optimized Compute

### EC2 Purchasing Options

(cite index="1-49">AWS purchasing options (for example, Spot Instances, Reserved Instances, Savings Plans).</cite>

**On-Demand**: 
- $0.096/hour (example: m5.large)
- Most expensive
- No commitment

**Reserved Instances** (1 or 3 year term):
- 1-year: 40% savings
- 3-year: 60% savings (upfront payment)
- All Upfront > Partial > No Upfront
- Unused RIs can be sold on marketplace

**Savings Plans**:
- Compute Savings Plans: 30-50% savings, flexibility across instance types/sizes/regions
- EC2 Instance Savings Plans: 30-40% savings, locked to instance type
- Better than RI for variable workloads

**Spot Instances**:
- 70-90% discount
- Interruption risk (2-minute notice)
- Good for: Batch jobs, stateless workloads, testing
- Spot Fleet diversifies availability zones/instance types

**Choosing Strategy**:
```
Stable 24/7 workload:
  → Reserved Instances or Savings Plan (60% savings)

Variable workload with flexibility:
  → Savings Plan (40% savings) + Spot (80% savings for flexible)

Development/testing:
  → Spot Instances (90% savings)

Guaranteed capacity needed:
  → Reserved + Spot mix (On-Demand too high)
```

### Auto Scaling Cost Optimization

- Terminate instances during off-hours (save 8 hours/day)
- Spot Fleet for dev/test (70% savings)
- Right-size instances (measure actual usage)
- Use burstable (t3) for variable workloads

### Lambda Cost Optimization

- Duration: 100ms increments (round up)
- Memory: Higher memory = faster execution
- Provisioned concurrency: Only for predictable demand
- Reserved concurrency: Waste if unused

**Calculation**:
```
1 million requests × 300ms × 512 MB memory
= 1,000,000 × 0.0003 × 512/1024 GB
= $0.0278 (approximately)

Dedicated server for same: $1,000+/month
```

---

## 🗃️ Task 4.3: Cost-Optimized Databases

### Database Engine Selection

(cite index="1-50">Database engines with appropriate use cases (for example, heterogeneous migrations, homogeneous migrations), and database types and services (for example, relational compared with non-relational, Amazon Aurora, Amazon DynamoDB).</cite>

**Cost Comparison** (1 TB, 1 year):
- RDS MySQL: $2,000-3,000
- Aurora MySQL: $1,500-2,000 (better value)
- DynamoDB provisioned: $10,000+ (high cost)
- DynamoDB on-demand: $300-500 (if sparse usage)

**Choosing**:
- **RDS**: Fixed workload, consistent demand
- **Aurora**: Scales automatically, high performance needed
- **DynamoDB**: Unpredictable demand, global scale

### Cost Optimization Strategies

**Capacity Planning**:
- Provisioned: Pay regardless of usage
- On-demand: Pay for what you use
- Choose provisioned if usage >70% of capacity

**Multi-AZ Costs**:
- Double compute + storage
- Worth it if downtime cost > 2x compute
- RDS Multi-AZ failover is automatic

**Read Replicas**:
- Same cost as primary (compute + storage)
- But distributes reads (can downsize primary)
- Cross-region: Add data transfer cost

### Aurora Cost Optimization

- Serverless: Pay per ACU (auto-scaling)
- On-demand: Pay per second
- Reserved: Upfront discount

**When Serverless Saves**:
- Intermittent workload (dev, test)
- Unpredictable spikes
- Example: Save 70% with serverless for batch jobs

---

## 🌍 Task 4.4: Cost-Optimized Network

### Data Transfer Costs

**Pricing** (approximate):
- **Inbound**: Free
- **Same AZ**: Free
- **Cross-AZ**: $0.02/GB
- **Cross-Region**: $0.02/GB (outbound)
- **To internet**: $0.09/GB (most expensive)

### Network Optimization Strategies

**VPC Endpoints**:
- Avoid data transfer charges
- Gateway endpoints: Free (S3, DynamoDB)
- Interface endpoints: $0.01/hour + $0.01/GB

**NAT Gateway Cost**:
- $45/month per NAT Gateway
- $0.045/GB processed
- Alternative: NAT Instance (cheaper for sparse usage)

**Direct Connect**:
- One-time: $0.30/hour
- Data transfer: $0.02/GB (cheaper than internet)
- Good for: Large, consistent data flows

**Content Delivery**:
- **CloudFront**: $0.085/GB (first 10TB) cheaper than direct
- **S3 Transfer Acceleration**: $0.04/GB (rarely cost-effective)
- Cache more at edges = lower costs

### Multi-Region Cost Optimization

(cite index="1-50,1-51">Configuring appropriate network routes to minimize network transfer costs (for example, Region to Region, Availability Zone to Availability Zone, private to public, AWS Global Accelerator, VPC endpoints).</cite>

- Replicate data only to regions with active users
- Use CloudFront for global static content
- S3 replication rules: Only needed regions
- Data residency: Keep data close to users

---

## 🔍 Cross-Service Cost Optimization

### Right-Sizing Methodology

1. **Measure**: CloudWatch metrics (CPU, memory, network)
2. **Analyze**: Identify underutilized resources
3. **Right-size**: Smaller instance type or delete
4. **Monitor**: Continue measuring post-change

**Common Findings**:
- 30% of instances consistently <10% CPU
- Databases provisioned for peak never reached
- Unattached EBS volumes
- Outdated snapshots

### Tagging Strategy for Cost Allocation

```
Tags enable cost allocation:
- Environment: production, staging, development
- CostCenter: finance, engineering, marketing
- Owner: john.doe@company.com
- Project: customer-portal, analytics

AWS Cost Explorer can:
- Filter by tags
- Break down cost by department
- Chargeback by owner
- Track trends over time
```

### Governance Best Practices

**Preventive Controls**:
- SCPs to restrict expensive regions
- IAM policies to prevent expensive operations
- Budget alerts for unusual spending

**Detective Controls**:
- Cost Anomaly Detection (ML-based)
- Trusted Advisor findings
- AWS Config for unused resources

**Responsive Controls**:
- Automated Lambda to shut down non-production
- Reserved Capacity planner
- Spot Fleet to replace expensive instances

---

## 💡 Cost Optimization Scenarios

### Scenario 1: Migrate RDS to Aurora
```
Current: RDS MySQL Multi-AZ = $5,000/month
Problem: 20% CPU utilization, needs scale

Solution:
- Migrate to Aurora MySQL
- Use auto-scaling (pay per use)
- Reduce read replica costs

Result: 40% savings = $2,000/month saved
```

### Scenario 2: Development Environment Costs
```
Current: Continuous instances = $3,000/month
Problem: Used only 9-5 weekdays

Solution:
- Spot Instances for non-production
- Scheduled scaling (shut down 5 PM - 9 AM)
- Downsize instances (t3.micro for dev)

Result: 80% savings = $2,400/month saved
```

### Scenario 3: Data Transfer Costs
```
Current: 1 TB/day cross-region = $2,700/month
Problem: Expensive data transfer

Solution:
- Move compute to data (process in source region)
- Use VPC Endpoints (free for S3, DynamoDB)
- CloudFront for static content

Result: 70% savings = $1,890/month saved
```

---

## 🎯 Domain 4 Key Takeaways

### Cost Optimization Principles
1. **Right-size**: Match capacity to demand
2. **Automate**: Schedule scaling, shutdown
3. **Choose wisely**: Reserved vs. on-demand vs. spot
4. **Monitor**: Continuous improvement

### Top Cost-Saving Opportunities
| Opportunity | Potential Savings |
|-------------|-------------------|
| Lifecycle storage | 50-80% |
| Reserved Instances | 30-60% |
| Spot Instances | 70-90% |
| Data transfer optimization | 50-70% |
| Right-sizing | 30-50% |

### Service Cost Multipliers
- **RDS Multi-AZ**: 2x (compute + storage)
- **Cross-region replication**: 1.3x (data transfer)
- **CloudFront**: Saves 80-90% vs. direct
- **Reserved RI**: 0.4-0.6x on-demand

---

## 📚 AWS Cost Optimization Tools

| Tool | Purpose | Cost |
|------|---------|------|
| Cost Explorer | Visualize spending | Free |
| Budgets | Alerts at thresholds | Free |
| Compute Optimizer | Right-sizing recommendations | Free (limited) |
| Trusted Advisor | Best practices | Free (7 checks) |
| Cost Anomaly Detection | Unusual spending | Free (beta) |

---

## 🏁 Exam Strategy for Domain 4

**Question Types**:
1. Right-sizing (identify underutilized resources)
2. Purchasing options (when to use Reserved vs. Spot)
3. Service selection (cheaper alternative)
4. Optimization strategy (reduce costs for scenario)

**Approach**:
1. Understand the workload characteristics
2. Identify constraints (availability, performance)
3. Choose least expensive option that meets requirements
4. Justify with specific cost savings

---

*Study Time: 6-8 hours | Practice: 20+ questions | Hands-On: Analyze AWS bill and identify 50% cost reduction opportunity*

