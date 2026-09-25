# Domain 3: Design High-Performing Architectures (24% of Exam)

**Content Domain 3 covers 24% of the exam with approximately 12 questions.**

---

## 📌 Overview

(cite index="1-44,1-45,1-46">This domain focuses on determining high-performing and/or scalable storage solutions, designing high-performing and elastic compute solutions, determining high-performing database solutions, determining high-performing and/or scalable network architectures, and determining high-performing data ingestion and transformation solutions.</cite>

**Performance = Achieving desired speed, throughput, and response times efficiently.**

### Five Key Tasks

1. **Task 3.1**: Determine high-performing and/or scalable storage solutions
2. **Task 3.2**: Design high-performing and elastic compute solutions
3. **Task 3.3**: Determine high-performing database solutions
4. **Task 3.4**: Determine high-performing and/or scalable network architectures
5. **Task 3.5**: Determine high-performing data ingestion and transformation solutions

---

## 💾 Task 3.1: Storage Solutions for Performance

### Storage Type Comparison

| Type | Service | Use Case | Throughput | Cost |
|------|---------|----------|-----------|------|
| **Object** | S3 | Files, images, logs | Unlimited | Low |
| **File** | EFS, FSx | Shared access, NFS | Medium | Medium |
| **Block** | EBS | Database, high IOPS | High | Medium |
| **Database** | RDS, DynamoDB | Structured data | Varies | Varies |

### S3 Performance Optimization

(cite index="1-44">Storage services with appropriate use cases (for example, Amazon S3, Amazon EFS, Amazon EBS).</cite>

**S3 Request Rate**:
- Previously: 3,500 PUT/COPY/POST/DELETE per second per prefix
- Now: 5,500 requests per second (any operation, any prefix)
- Multi-part upload for large files (recommended for >100MB)
- Byte-range fetches for parallel downloads

**S3 Intelligent-Tiering**:
- Automatically moves data between access tiers
- No retrieval fees if object accessed
- Cost-effective for unknown access patterns
- Moves between: Frequent, Infrequent, Archive Instant, Deep Archive

### EBS Performance

**Volume Types**:
- **gp3**: General purpose (5.3 K IOPS baseline, 125 MB/s)
- **io2**: Provisioned IOPS (up to 64 K IOPS, 1 GB/s)
- **st1**: Throughput optimized (big data, streaming)
- **sc1**: Cold storage (infrequently accessed)

**Performance Tuning**:
- Provisioned IOPS: Pay for guaranteed performance
- Baseline vs. burst: gp2 can burst, gp3 has consistent performance
- I/O Credits: gp2 accumulates credits at rest, bursts when needed

### EFS Performance

- Throughput modes: Bursting vs. Provisioned
- Performance mode: General purpose vs. Max IO
- Scales automatically from KB to EB
- Shared across multiple instances in region

---

## 🖥️ Task 3.2: Compute Solutions for Performance

### EC2 Instance Families

(cite index="1-45">AWS compute services with appropriate use cases (for example, AWS Batch, Amazon EMR, AWS Fargate).</cite>

| Family | Optimized For | Examples | Use Case |
|--------|---------------|----------|----------|
| **General** | Balanced | m5, m6i | Web apps, small DBs |
| **Compute** | CPU | c5, c6i | Batch processing, compiling |
| **Memory** | RAM | r5, r6i | In-memory DBs, caching |
| **Storage** | I/O | i3, i4i | NoSQL, data warehousing |
| **GPU** | Graphics | p3, p4 | ML, rendering |
| **Accelerated** | Specialized | f1, g4 | Video encoding, ML |

### Lambda for Performance

**Cold Starts**:
- Provisioned concurrency: Pre-warmed instances
- Memory allocation: Higher memory = faster CPU (15 seconds max)
- Languages: Compiled faster (Go, Java with optimization)

**Performance Tips**:
- Connection pooling: Reuse database connections
- Environment variables: Don't recalculate
- Bundling: Minimize package size
- Warm pools: Pre-launch instances in Auto Scaling

### Fargate vs. EC2

| Aspect | Fargate | EC2 |
|--------|---------|-----|
| **Management** | Serverless | Manage instances |
| **Scaling** | Automatic | Auto Scaling groups |
| **Cost** | Per task second | Instance hour |
| **Control** | Less | More |
| **Performance** | Consistent | Burstable (t3) |

---

## 🗄️ Task 3.3: Database Solutions for Performance

### Database Engine Selection

(cite index="1-45,1-46">Database engines with appropriate use cases (for example, heterogeneous migrations, homogeneous migrations), and database types and services (for example, serverless, relational compared with non-relational, in-memory).</cite>

**Relational Databases**:
- **MySQL**: Open source, web applications
- **PostgreSQL**: Advanced features, JSON support
- **MariaDB**: MySQL compatible, better performance
- **Oracle**: Enterprise features, expensive

**NoSQL Databases**:
- **DynamoDB**: Managed NoSQL, auto-scaling, serverless
- **DocumentDB**: MongoDB compatible, AWS managed
- **Neptune**: Graph database for relationships
- **Keyspaces**: Cassandra compatible, multi-region

**Data Warehouse**:
- **Redshift**: Columnar, analytics, massive scale

### Performance Optimization

**Read Replicas**:
- Async replication from primary
- Read-only endpoints for distribution
- Scales read throughput
- Cross-region for DR

**Database Proxy**:
- RDS Proxy reduces connection overhead
- Connection pooling at database level
- Transparent to application
- Improved failover time

**Caching Layer**:
(cite index="1-45,1-46">Caching strategies and services (for example, Amazon ElastiCache).</cite>

- **ElastiCache Redis**: Data structures, persistence, pub/sub
- **ElastiCache Memcached**: Simple key-value, fastest
- **DAX**: DynamoDB Accelerator for millisecond latency

**Query Optimization**:
- Indexes: Fast lookups but slower writes
- Partitioning: Distribute load across shards
- Denormalization: Redundant data for faster reads

---

## 🌐 Task 3.4: Network Architectures for Performance

### Content Delivery Network (CDN)

**Amazon CloudFront**:
- 500+ edge locations worldwide
- Cache static and dynamic content
- Regional caches for frequently accessed
- Origin can be S3, ALB, EC2, on-premises

**Performance Benefits**:
- Reduced latency (users get from nearest edge)
- Reduced origin load (edges cache content)
- DDoS protection (Shield included)
- Accelerated uploads (multiple paths to origin)

### AWS Global Accelerator

- Optimizes path selection to origin
- Anycast IPs: Route to nearest healthy endpoint
- Ultra-low latency (tens of milliseconds)
- Bypass public internet (use AWS global network)
- Good for non-HTTP protocols (gaming, IoT)

### Network Load Balancer (NLB)

(cite index="1-46">Edge networking services with appropriate use cases (for example, Amazon CloudFront, AWS Global Accelerator).</cite>

- Layer 4 (transport layer)
- 1 million requests per second
- Ultra-low latency (microseconds)
- Use for: Gaming, IoT, non-HTTP protocols

### VPC Design for Performance

- Placement groups: Cluster instances for low latency
- Enhanced networking: SR-IOV for higher throughput
- VPC endpoints: No internet gateway overhead
- Route 53 geolocation routing: Users to nearest region

---

## 📊 Task 3.5: Data Ingestion & Transformation

### Data Ingestion Patterns

(cite index="1-46,1-47">Data ingestion patterns (for example, frequency), data transfer services with appropriate use cases (for example, AWS DataSync, AWS Storage Gateway), data transformation services with appropriate use cases (for example, AWS Glue), and streaming data services with appropriate use cases (for example, Amazon Kinesis).</cite>

**Batch vs. Stream**:
- **Batch**: Process data in chunks (ETL jobs)
- **Stream**: Process continuously (real-time analytics)

### AWS Glue

- Fully managed ETL service
- Crawlers discover data schema
- Visual job designer
- 0.44 USD per DPU-hour
- Good for batch transformations

### Amazon Kinesis

**Types**:
- **Streams**: Real-time, shards for throughput (expensive)
- **Firehose**: Near real-time, auto-scales, cheaper (writes to S3/Redshift)
- **Analytics**: SQL queries on stream

**Use Cases**:
- Real-time dashboards
- Anomaly detection
- IoT data ingestion

### AWS DataSync

- Transfer large data volumes quickly
- Automated verification and monitoring
- Bandwidth control
- 1/10th the cost of direct transfer

### Data Lake Architecture

1. **Ingest**: S3 raw data zone
2. **Process**: Glue, Lambda, EMR transformation
3. **Analyze**: Athena queries, Redshift
4. **Visualize**: QuickSight dashboards

**AWS Lake Formation**:
- Simplifies data lake setup
- Centralized permissions (Lake Formation tags)
- Data catalog for metadata
- Automated data cleansing

---

## 🎯 Domain 3 Key Takeaways

### Performance Principles
1. **Caching**: Reduce database load
2. **Asynchronous**: Don't wait for slow operations
3. **Distribution**: Spread across regions and AZs
4. **Specialization**: Use right tool for job

### Service Selection Matrix

| Requirement | Services |
|------------|----------|
| **Huge scale, unstructured** | S3 + CloudFront |
| **High IOPS, structured** | RDS + ElastiCache |
| **Auto-scaling DB** | DynamoDB, Aurora |
| **Real-time analytics** | Kinesis + Redshift |
| **Batch processing** | EMR, Batch, Glue |
| **Low latency globally** | Global Accelerator, CloudFront |

### Common Exam Patterns
- **Storage**: Right type for workload
- **Compute**: Right instance family for task
- **Database**: Right engine for data model
- **Network**: CloudFront for static, Global Accelerator for dynamic
- **Data**: Batch vs. stream ingestion

---

*Study Time: 7-9 hours | Practice: 25+ questions | Hands-On: Build high-performance application with caching and CDN*

