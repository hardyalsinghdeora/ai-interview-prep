# AWS Cheatsheet

Covers core AWS services across compute, storage, databases, networking, messaging, security and operations, with tables for choosing between neighbouring services.

> Related: [AWS topic](../topics/16-aws/README.md) · [AWS skill](../skills/aws.md) · [Cloud, DevOps and integration](../topics/08-cloud-devops-integration/README.md) · [cloud-services.md](cloud-services.md) · [gcp-and-azure.md](gcp-and-azure.md) · [containers-and-kubernetes.md](containers-and-kubernetes.md) · [messaging-and-streaming.md](messaging-and-streaming.md) · [security-and-auth.md](security-and-auth.md) · [ci-cd-and-iac.md](ci-cd-and-iac.md)

## Compute

| Term | What it means |
|---|---|
| **EC2** (Elastic Compute Cloud) | Virtual machines with full operating system control, the flexible baseline when managed services do not fit. |
| **Instance family** | A hardware profile, such as general purpose (M), compute (C) or memory (R); choose by the workload's bottleneck. |
| **AMI** (Amazon Machine Image) | A template holding the operating system and software, so every launched instance starts the same. |
| **Launch template** | A versioned instance configuration, covering image, type, network and role, which Auto Scaling groups launch from. |
| **Auto Scaling group** | Keeps a fleet between minimum and maximum size, replaces unhealthy instances and spans Availability Zones. |
| **Target tracking policy** | Scales to hold a metric near a target, such as 50% CPU; the usual default policy. |
| **Lifecycle hook** | Pauses an instance while launching or terminating, so setup or connection draining can finish. |
| **Lambda** | Runs functions on events, billed per request and duration; the maximum run time is 15 minutes *(verify)*. |
| **Provisioned concurrency** | Keeps Lambda environments initialised, avoiding cold starts but costing money while idle. |
| **Reserved concurrency** | Sets aside and caps a Lambda function's concurrency, protecting downstream systems and other functions. |
| **SnapStart** | Resumes Lambda from a snapshot of initialised state, cutting cold starts for Java, Python and .NET *(verify)*. |
| **ECS** (Elastic Container Service) | AWS's own container orchestrator, simpler than Kubernetes and tightly integrated with other AWS services. |
| **Task definition** | An ECS blueprint listing images, CPU, memory, ports and roles for a group of containers. |
| **EKS** (Elastic Kubernetes Service) | A managed Kubernetes control plane, chosen for portability and ecosystem, with more operational work and a cluster fee. |
| **Fargate** | Serverless compute for ECS tasks and EKS pods: no nodes to manage, but a higher unit price. |

| Choose compute | Lambda | ECS on Fargate | EKS | EC2 |
|---|---|---|---|---|
| Best for | Short, event-driven work | Long-running containers with little operations work | Existing Kubernetes skills or portability | Full control or special hardware |
| You manage | Code | Image and task settings | Upgrades, add-ons, often nodes | Operating system, patching, scaling |
| Scale to zero | Yes | Not by default | Not by default | Not by default |
| Avoid when | Runs exceed 15 minutes *(verify)* | GPUs are needed *(verify)* | The team lacks Kubernetes skills | A managed option fits |

| EC2 purchase option | Commitment | Discount | Use for |
|---|---|---|---|
| On-Demand | None | None | Spiky or short-term work |
| Savings Plans | One or three years of hourly spend | Up to about 72% *(verify)* | Steady baseline, with flexibility |
| Reserved Instances | One or three years, tied to a configuration | Up to about 72% *(verify)* | Steady, predictable instances |
| Spot | None | Up to about 90% *(verify)* | Interruptible work; two-minute reclaim warning |

## Storage

| Term | What it means |
|---|---|
| **S3** (Simple Storage Service) | Object storage designed for eleven nines of durability, with strong read-after-write consistency; the default home for files. |
| **Bucket policy** | A resource policy on an S3 bucket, used for cross-account access and enforcing rules such as TLS. |
| **Block Public Access** | An account or bucket setting that overrides public grants, which is the main guard against accidental exposure. |
| **Lifecycle rule** | Moves objects to colder classes or deletes them by age, which controls storage cost automatically. |
| **S3 versioning** (Simple Storage Service versioning) | Keeps every version of an object, so overwrites and deletes can be undone, while old versions still cost money. |
| **S3 Object Lock** (Simple Storage Service Object Lock) | Write-once-read-many protection for versions, using retention periods and legal holds, which suits compliance and ransomware defence. |
| **Presigned URL** (presigned Uniform Resource Locator) | A time-limited link using the signer's permissions, letting clients upload or download without AWS credentials. |
| **EBS** (Elastic Block Store) | Network block volumes for one instance at a time, normally, and locked to one Availability Zone. |
| **gp3** (General Purpose Solid State Drive volume) | The usual EBS type, setting IOPS and throughput independently of size; use io2 for demanding databases *(verify)*. |
| **EBS snapshot** (Elastic Block Store snapshot) | An incremental, point-in-time backup held in AWS-managed storage, used to restore or move volumes between zones. |
| **EFS** (Elastic File System) | Managed NFS that grows automatically and is shared across zones by many Linux clients, at a higher per-GB price. |
| **Instance store** | Very fast local disk on the host; the data is lost when the instance stops or terminates. |

| Choose storage | S3 | EBS | EFS | Instance store |
|---|---|---|---|---|
| Type | Object, over HTTP | Block | File, over NFS | Local block |
| Shared by | Anything with permission | Normally one instance | Many instances | One instance |
| Scope | Region | One zone | Region | One host |
| Best for | Files, backups, data lakes | Boot volumes, databases | Shared Linux content | Scratch data and caches |

Minimum storage durations and retrieval times below are *(verify)*.

| S3 storage class | Use for | Retrieval | Minimum storage |
|---|---|---|---|
| Standard | Frequent access | Milliseconds | None |
| Intelligent-Tiering | Unknown or changing access | Milliseconds, unless optional archive tiers are on | None |
| Standard-IA (Infrequent Access) | Infrequent access | Milliseconds, with a per-GB fee | 30 days |
| One Zone-IA (Infrequent Access) | Re-creatable data in one zone | Milliseconds, with a per-GB fee | 30 days |
| Express One Zone | Very high request rates in one zone | Single-digit milliseconds | None |
| Glacier Instant Retrieval | Rare access, needed instantly | Milliseconds | 90 days |
| Glacier Flexible Retrieval | Archives | Minutes to hours | 90 days |
| Glacier Deep Archive | Long-term compliance archives | Hours | 180 days |

## Databases

| Term | What it means |
|---|---|
| **RDS** (Relational Database Service) | Managed relational engines such as PostgreSQL, MySQL, MariaDB, Oracle and SQL Server, handling backups, patching and failover. |
| **Multi-AZ** (Multi-Availability Zone) | A synchronous standby in another zone with automatic failover, for availability rather than read scaling. |
| **Read replica** | An asynchronous copy that serves reads and can cross regions; lag means reads may be stale. |
| **RDS Proxy** (Relational Database Service Proxy) | A managed connection pool that stops bursts of clients, such as Lambda, exhausting database connections. |
| **Aurora** | MySQL- and PostgreSQL-compatible engine with shared storage keeping six copies across three zones, giving fast failover and replicas. |
| **Aurora Serverless v2** | Aurora capacity that scales within a set range in small steps, suiting spiky or unpredictable workloads. |
| **Aurora Global Database** | Storage-level replication to other regions, typically under a second behind, for disaster recovery and local reads *(verify)*. |
| **DynamoDB** | A serverless key-value and document database with low latency at any scale, if access patterns are designed upfront. |
| **Partition key** | The DynamoDB attribute hashed to place an item; low-cardinality or skewed keys create hot partitions. |
| **Sort key** | Orders items sharing a partition key, which allows range queries and one-to-many modelling. |
| **GSI** (Global Secondary Index) | A DynamoDB index with a different partition key, added any time, and eventually consistent only. |
| **LSI** (Local Secondary Index) | A DynamoDB index with the same partition key but another sort key, defined only at table creation. |
| **DynamoDB Streams** | A time-ordered record of item changes kept for 24 hours, commonly used to trigger Lambda *(verify)*. |
| **ElastiCache** | Managed Valkey, Redis OSS or Memcached, used as a cache in front of slower data stores *(verify)*. |
| **Redshift** | A columnar, massively parallel data warehouse for analytics, available as provisioned clusters or serverless. |
| **Athena** | Serverless SQL over files in S3, priced by data scanned, so columnar formats and partitions cut costs. |

| RDS option | Multi-AZ standby | Read replica |
|---|---|---|
| Purpose | Availability | Read scaling |
| Replication | Synchronous | Asynchronous |
| Serves reads | No, in the single-standby deployment | Yes |
| Failover | Automatic | Manual promotion |

| Choose a database | RDS | Aurora | DynamoDB |
|---|---|---|---|
| Model | Relational | Relational | Key-value and document |
| Scale | Vertical, plus replicas | Vertical, more replicas, shared storage | Horizontal |
| Queries | Ad hoc SQL and joins | Ad hoc SQL and joins | Known access patterns |
| Choose for | Standard or commercial engines | Higher throughput, fast failover, global reads | Huge scale or serverless key lookups |

| DynamoDB capacity | On-demand | Provisioned |
|---|---|---|
| Billing | Per request | Per unit of capacity per hour |
| Planning | None | Set capacity, usually with auto scaling |
| Choose for | New, spiky or unknown traffic | Steady, predictable traffic |

| Choose analytics | Redshift | Athena |
|---|---|---|
| Data lives in | The warehouse, plus S3 through Redshift Spectrum | S3 |
| Cost driver | Nodes or serverless capacity | Data scanned |
| Choose for | Frequent BI with many concurrent users | Occasional queries on a data lake |

## Networking and content delivery

| Term | What it means |
|---|---|
| **VPC** (Virtual Private Cloud) | Your isolated private network in one region, with your own IP ranges, routing and firewalls. |
| **Subnet** | An IP range in one Availability Zone; it is public if its route table leads to an internet gateway. |
| **NAT gateway** (Network Address Translation gateway) | Lets private subnets start outbound internet connections without accepting inbound ones; per-GB charges can be large. |
| **Security group** | A stateful, allow-only firewall on network interfaces, which can reference other security groups instead of IP ranges. |
| **NACL** (Network Access Control List) | A stateless subnet firewall with ordered allow and deny rules, so return traffic needs its own rule. |
| **Gateway endpoint** | A free route-table entry giving private access to S3 and DynamoDB without a NAT gateway *(verify)*. |
| **Interface endpoint** | A private IP in your subnet for an AWS or partner service, powered by PrivateLink and charged hourly. |
| **Transit Gateway** | A regional hub connecting many VPCs and on-premises networks, replacing large meshes of peering links. |
| **ALB** (Application Load Balancer) | A Layer 7 load balancer that routes HTTP by host and path, and integrates with WAF and Lambda. |
| **NLB** (Network Load Balancer) | A Layer 4 load balancer for TCP and UDP with very low latency and static IP addresses per zone. |
| **Route 53** | AWS's DNS service, offering health checks and policies such as weighted, latency, failover and geolocation routing. |
| **Alias record** | A Route 53 record pointing at AWS resources, which unlike CNAME also works at the zone apex. |
| **CloudFront** | AWS's CDN, caching at edge locations and shielding origins such as S3 buckets and load balancers. |
| **OAC** (Origin Access Control) | Lets only a CloudFront distribution read a private S3 bucket, replacing the older origin access identity. |
| **Global Accelerator** | Static anycast IP addresses that carry TCP and UDP over AWS's backbone to regional endpoints, without caching. |
| **API Gateway** (Application Programming Interface Gateway) | Managed API front door handling throttling, authorisation and routing, in REST, HTTP and WebSocket forms. |

| Choose a load balancer | ALB | NLB |
|---|---|---|
| Layer | 7 (HTTP, gRPC, WebSocket) | 4 (TCP, UDP, TLS) |
| Routing | Host, path, header | Port |
| Static IP addresses | No | Yes, per zone |
| Choose for | Web apps and microservices | Non-HTTP traffic, extreme throughput, IP allow lists |

| Firewall | Security group | NACL |
|---|---|---|
| Attached to | Network interface | Subnet |
| State | Stateful | Stateless |
| Rules | Allow only | Allow and deny, in order |
| Typical use | Main access control | Coarse subnet-wide blocks |

| Choose an API front door | API Gateway REST API | API Gateway HTTP API | ALB |
|---|---|---|---|
| Strength | Most features: usage plans, validation, caching | Cheaper, lower latency, JWT authorisers | Cost at sustained high traffic |
| Billing | Per request | Per request | Per hour and capacity unit |
| Choose for | Public APIs with quotas | Simple proxies to Lambda or HTTP | Sustained traffic to containers or instances |

| Global traffic | CloudFront | Global Accelerator |
|---|---|---|
| Protocols | HTTP and HTTPS | TCP and UDP |
| Caching | Yes | No |
| Entry point | DNS name | Static anycast IP addresses |
| Choose for | Cacheable web content and APIs | Non-HTTP apps, fast regional failover |

## Messaging and integration

| Term | What it means |
|---|---|
| **SQS** (Simple Queue Service) | A managed pull-based queue that buffers work between services, with no brokers to run. |
| **SQS FIFO** (Simple Queue Service First-In, First-Out) | A queue type with ordering per message group and deduplication, at lower throughput than standard queues. |
| **Visibility timeout** | Hides a received message during processing; if it is not deleted in time, it reappears. |
| **DLQ** (Dead Letter Queue) | Receives messages after too many failed receives, so poison messages stop blocking work; alert on its depth. |
| **SNS** (Simple Notification Service) | Push-based publish and subscribe, delivering to SQS, Lambda, HTTP, email and mobile endpoints. |
| **SNS and SQS fan-out** (Simple Notification Service and Simple Queue Service fan-out) | One topic feeds many queues, giving each consumer durable, independently paced delivery. |
| **EventBridge** | An event bus that routes AWS, SaaS and custom events to targets using content-based rules. |
| **EventBridge Pipes** | Connects one source to one target, with optional filtering and enrichment, without glue code. |
| **EventBridge Scheduler** | Managed one-off and recurring schedules, which replaces cron servers and suits per-user timers. |
| **Kinesis Data Streams** | A managed, shard-based stream that keeps records for replay, with several independent consumers. |
| **Shard** | Kinesis's unit of order and capacity: 1 MB/s or 1,000 records/s in, 2 MB/s out *(verify)*. |
| **Data Firehose** | Loads streams into S3, Redshift or OpenSearch with buffering and no consumer code; not for replay. |
| **MSK** (Managed Streaming for Apache Kafka) | Managed Apache Kafka, offered as provisioned or serverless, for Kafka APIs and ecosystem compatibility *(verify)*. |
| **Step Functions** | State machines that coordinate services with retries, branching, parallelism and waits, keeping orchestration out of code. |

| Choose messaging | SQS | SNS | EventBridge | Kinesis Data Streams | MSK |
|---|---|---|---|---|---|
| Model | Queue, consumers pull | Topic, broker pushes | Bus with rules | Log of shards | Kafka log |
| Ordering | Per group on FIFO | Per group on FIFO topics | Not guaranteed | Per shard | Per partition |
| Replay | No | FIFO archive only *(verify)* | Archive and replay | Within retention | Within retention |
| Choose for | Work queues and buffering | Simple, high-volume fan-out | Rule-based routing, SaaS events | AWS-native streaming | Kafka compatibility |

| Step Functions | Standard | Express |
|---|---|---|
| Maximum duration | One year *(verify)* | Five minutes *(verify)* |
| Execution | Exactly once | At least once when asynchronous |
| Billing | Per state transition | Per request and duration |
| Choose for | Long or auditable workflows | High-volume, short event processing |

## Security and identity

| Term | What it means |
|---|---|
| **IAM** (Identity and Access Management) | Controls who can do what on which AWS resource, through identities and policies. |
| **IAM role** (Identity and Access Management role) | An identity assumed for temporary credentials, which avoids long-lived access keys in code. |
| **Resource policy** | A policy on the resource itself, such as an S3 bucket or KMS key, that names allowed principals. |
| **Policy evaluation** | Everything is denied by default, an explicit deny always wins, and an allow must pass every applicable guardrail. |
| **SCP** (Service Control Policy) | An organisation guardrail that caps permissions in member accounts; it never grants anything itself. |
| **STS** (Security Token Service) | Issues temporary credentials, most often through `AssumeRole`, for cross-account and federated access. |
| **IAM Identity Center** (Identity and Access Management Identity Center) | Workforce single sign-on across accounts, replacing long-lived IAM users for people. |
| **KMS** (Key Management Service) | Creates and controls encryption keys, with every use logged in CloudTrail and access set by key policies. |
| **Envelope encryption** | A data key encrypts the data and a KMS key encrypts the data key, which keeps bulk encryption fast. |
| **Secrets Manager** | Stores secrets with built-in rotation and cross-account access, priced per secret *(verify)*. |
| **Parameter Store** | Systems Manager's store for configuration and encrypted strings, with a free tier but no native rotation. |
| **WAF** (Web Application Firewall) | Filters HTTP requests at CloudFront, ALB or API Gateway using managed, custom and rate-based rules. |
| **Cognito user pool** | A user directory handling sign-up and sign-in, which issues JSON Web Tokens to your application. |
| **Cognito identity pool** | Exchanges a signed-in identity for temporary AWS credentials, so clients can call AWS directly. |

```json
{
  "Effect": "Allow",
  "Action": "s3:GetObject",
  "Resource": "arn:aws:s3:::reports/*",
  "Condition": { "Bool": { "aws:SecureTransport": "true" } }
}
```

| Choose secret storage | Secrets Manager | Parameter Store |
|---|---|---|
| Rotation | Built in | Build your own |
| Cost | Per secret and API call *(verify)* | Standard tier free *(verify)* |
| Choose for | Database credentials and API keys | Configuration and simple secrets |

## Operations

| Term | What it means |
|---|---|
| **CloudWatch** | AWS's metrics, logs and alarms service, and the default place to watch workload health. |
| **CloudWatch Logs Insights** | A query language over CloudWatch Logs for filtering and aggregating, charged by data scanned. |
| **CloudWatch alarm** | Watches a metric against a threshold, then notifies through SNS or triggers scaling and other actions. |
| **CloudTrail** | Records AWS API calls, showing who did what and when, which is essential for audits and incidents. |
| **AWS Config** | Records resource configuration over time and checks it against rules, showing compliance and what changed. |
| **X-Ray** | Traces requests across services, which helps find latency and errors in distributed calls *(verify)*. |
| **CloudFormation** | AWS's native infrastructure as code, deploying templates as stacks with automatic rollback on failure. |
| **Change set** | A preview of what a CloudFormation update will change, which catches unexpected replacements before they happen. |
| **Drift detection** | Compares a CloudFormation stack with its template, finding manual changes made outside the code. |
| **CDK** (Cloud Development Kit) | Defines infrastructure in general-purpose languages, then synthesises it into CloudFormation templates. |

| Which log or record | CloudWatch | CloudTrail | AWS Config |
|---|---|---|---|
| Shows | Workload health | Who called which API | Resource history and compliance |
| Data | Metrics, logs, alarms | API events | Configuration history |

| Choose infrastructure as code | CloudFormation | CDK | Terraform |
|---|---|---|---|
| Written in | YAML or JSON | TypeScript, Python, Java and others | HCL (HashiCorp Configuration Language) |
| State held by | AWS | AWS, through CloudFormation | Your state file or backend |
| Clouds | AWS | AWS | Many |
| Choose for | AWS-only, no extra tooling | AWS-only, developer-led teams | Multi-cloud or existing Terraform estates |
