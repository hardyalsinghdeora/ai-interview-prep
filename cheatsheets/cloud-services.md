# Cloud Services Cheatsheet

Covers core cloud terms and maps equivalent AWS, Google Cloud and Azure services across compute, storage, databases, messaging, networking, identity and AI.

> Related: [AWS skill](../skills/aws.md) · [Google Cloud skill](../skills/google-cloud-gcp.md) · [Azure skill](../skills/azure.md) · [AWS topic](../topics/16-aws/README.md) · [Cloud, DevOps and integration](../topics/08-cloud-devops-integration/README.md) · [containers-and-kubernetes.md](containers-and-kubernetes.md) · [ci-cd-and-iac.md](ci-cd-and-iac.md) · [security-and-auth.md](security-and-auth.md)

## Service models

| Term | What it means |
|---|---|
| **IaaS** (Infrastructure as a Service) | Rented virtual machines, disks and networks; you manage the operating system upwards, keeping control but also the patching. |
| **PaaS** (Platform as a Service) | You deploy code and the provider runs the platform, trading some flexibility for much less operations work. |
| **SaaS** (Software as a Service) | Finished software delivered online, where the vendor runs everything and you only manage users and data. |
| **FaaS** (Function as a Service) | Event-triggered functions billed per use, with no servers to manage, but with limits on runtime and state. |
| **Serverless** | Capacity is managed and scaled for you, often to zero, so you pay for use rather than idle servers. |
| **Cold start** | Extra latency when a serverless platform must start a new instance, which matters for user-facing paths. |
| **Managed service** | The provider handles patching, backups and failover, freeing time but reducing control and adding some lock-in. |
| **Shared responsibility model** | The provider secures the cloud itself while you secure your data, identities and configuration; the split varies by service. |

## Geography and availability

| Term | What it means |
|---|---|
| **Region** | A geographic area of data centres; the choice affects latency, cost, available services and data residency. |
| **AZ** (Availability Zone) | One or more isolated data centres within a region, used to survive a single-site failure. |
| **Edge location** | A site close to users that serves CDN and DNS traffic, reducing latency. |
| **Multi-AZ** (Multi-Availability Zone) | Running across zones in one region, the usual baseline for high availability. |
| **Multi-region** | Running across regions for disaster recovery or global latency, at much higher cost and data-consistency complexity. |
| **Availability** | The share of time a service works and responds; often stated in nines, such as 99.9%. |
| **Durability** | The likelihood stored data is not lost; high durability does not guarantee the data is reachable right now. |
| **Quota** | A limit on resources per account or region; many can be raised, but they should be checked before launches. |
| **Data residency** | A requirement that data stays in a set location, which often constrains region and service choices. |

## Networking

| Term | What it means |
|---|---|
| **VPC** (Virtual Private Cloud) | A logically isolated private network in the cloud where you control IP ranges, routing and firewall rules. |
| **Subnet** | A range of IP addresses inside a VPC, used to separate tiers and control routing. |
| **Public subnet** | A subnet with a route to the internet, typically for load balancers. |
| **Private subnet** | A subnet with no direct inbound internet route, typically for application servers and databases. |
| **CIDR** (Classless Inter-Domain Routing) | Notation for IP ranges, such as `10.0.0.0/16`; overlapping ranges block later network connections. |
| **Security group** | An AWS stateful virtual firewall attached to instances, so return traffic is allowed automatically; other clouds have close equivalents. |
| **NACL** (Network Access Control List) | A stateless, subnet-level AWS firewall, so inbound and outbound rules are both needed. |
| **NAT gateway** (Network Address Translation gateway) | Lets private resources make outbound internet calls without accepting inbound ones; data charges can be surprisingly large. |
| **VPC peering** (Virtual Private Cloud peering) | A private link between two networks; it is not transitive, so large estates tend to use a hub instead. |
| **Private endpoint** | Reaches a managed service over private IP addresses, keeping traffic off the public internet. |
| **Layer 4 load balancer** | Balances by IP and port, offering high throughput and low latency, but with no view of HTTP. |
| **Layer 7 load balancer** | Balances by HTTP details such as host and path, enabling smarter routing and TLS termination. |
| **Egress** | Data leaving a cloud or region, usually charged, which makes chatty cross-region designs expensive. |

## Storage and compute options

| Term | What it means |
|---|---|
| **Object storage** | Stores whole files by key over HTTP, offering huge scale and low cost, but no in-place edits. |
| **Block storage** | A virtual disk attached to a machine, offering low latency for databases and boot volumes. |
| **File storage** | A shared network filesystem mounted by many machines, suiting lift-and-shift apps. |
| **Storage class** | A price and access tier within object storage; colder tiers cost less to hold but more to read. |
| **Lifecycle policy** | Rules that move or delete objects by age, which controls storage costs automatically. |
| **Autoscaling** | Adds and removes capacity from demand or schedules, so you pay for less idle capacity. |
| **On-demand pricing** | Pay per use with no commitment, the most flexible and usually most expensive compute option. |
| **Reserved capacity and committed use** | A one or three year spend or usage commitment in exchange for a discount, suited to steady workloads. |
| **Spot capacity** | Heavily discounted spare capacity that can be reclaimed at short notice, so workloads must tolerate interruption. |

## Identity and governance

| Term | What it means |
|---|---|
| **IAM** (Identity and Access Management) | The system deciding who can do what to which resource; most cloud breaches involve it or misconfiguration. |
| **Principal** | The user, group or workload making a request, which is what permissions get attached to. |
| **Role** | An identity with permissions that is assumed temporarily, avoiding long-lived credentials in code. |
| **Policy** | A document of allow or deny rules attached to identities or resources. |
| **Least privilege** | Granting only the permissions actually needed, which limits the blast radius of any compromise. |
| **Workload identity federation** | Lets external workloads, such as CI jobs, get short-lived cloud credentials without stored keys. |
| **Landing zone** | A pre-built, governed multi-account setup with networking, logging and guardrails, used as a safe starting point. |
| **Tagging** | Key-value labels on resources, which make cost allocation and automation possible. |

## Cost and architecture

| Term | What it means |
|---|---|
| **FinOps** (Financial Operations) | The practice of engineering and finance sharing ownership of cloud spend, making cost a design input. |
| **Rightsizing** | Matching resource size to actual use, usually the quickest way to reduce waste. |
| **Well-Architected Framework** | Provider guidance for reviewing designs against pillars such as security, reliability and cost. |
| **Vendor lock-in** | Dependence on one provider's proprietary services, making moving costly; worth weighing against speed gained. |
| **Hybrid cloud** | Running across on-premises and cloud environments, often because of latency, regulation or existing investment. |
| **Multicloud** | Using more than one cloud provider, adding resilience or choice but also real operational complexity. |

## Service mapping

Rough equivalents, not exact matches: limits, pricing and features differ. Names change often, so treat every row as *(verify)*.

### Compute

| Need | AWS | Google Cloud | Azure |
|---|---|---|---|
| Virtual machines | Amazon EC2 (Elastic Compute Cloud) | Compute Engine | Azure Virtual Machines |
| Auto-scaled machine groups | EC2 Auto Scaling groups | Managed instance groups | Virtual Machine Scale Sets |
| Functions | AWS Lambda | Cloud Run functions | Azure Functions |
| Serverless containers | Amazon ECS (Elastic Container Service) on AWS Fargate | Cloud Run | Azure Container Apps |
| Managed Kubernetes | Amazon EKS (Elastic Kubernetes Service) | GKE (Google Kubernetes Engine) | AKS (Azure Kubernetes Service) |
| Application platform | AWS Elastic Beanstalk | App Engine | Azure App Service |
| Batch jobs | AWS Batch | Batch | Azure Batch |
| Container images | Amazon ECR (Elastic Container Registry) | Artifact Registry | Azure Container Registry |

### Storage

| Need | AWS | Google Cloud | Azure |
|---|---|---|---|
| Object storage | Amazon S3 (Simple Storage Service) | Cloud Storage | Azure Blob Storage |
| Block storage | Amazon EBS (Elastic Block Store) | Persistent Disk, Hyperdisk | Azure Managed Disks |
| Shared files | Amazon EFS (Elastic File System) | Filestore | Azure Files |
| Archive | S3 Glacier storage classes | Cloud Storage Archive class | Blob Storage Archive tier |
| Online data transfer | AWS DataSync | Storage Transfer Service | Azure Storage Mover |

### Databases

| Need | AWS | Google Cloud | Azure |
|---|---|---|---|
| Managed relational | Amazon RDS (Relational Database Service), Aurora | Cloud SQL, AlloyDB | Azure SQL Database, Azure Database for PostgreSQL |
| Globally distributed SQL | Aurora DSQL | Spanner | No direct equal; Azure Cosmos DB is closest |
| Key-value and document | Amazon DynamoDB | Firestore | Azure Cosmos DB |
| Wide-column | Amazon Keyspaces | Bigtable | Azure Managed Instance for Apache Cassandra |
| In-memory cache | Amazon ElastiCache, MemoryDB | Memorystore | Azure Managed Redis (Azure Cache for Redis is retiring) |
| Data warehouse | Amazon Redshift | BigQuery | Microsoft Fabric, Azure Synapse Analytics |
| Database migration | AWS DMS (Database Migration Service) | Database Migration Service | Azure Database Migration Service |

### Messaging

| Need | AWS | Google Cloud | Azure |
|---|---|---|---|
| Message queue | Amazon SQS (Simple Queue Service) | Pub/Sub, Cloud Tasks | Azure Service Bus queues, Queue Storage |
| Publish and subscribe | Amazon SNS (Simple Notification Service) | Pub/Sub | Azure Service Bus topics |
| Event routing | Amazon EventBridge | Eventarc | Azure Event Grid |
| Event streaming | Amazon Kinesis Data Streams | Pub/Sub | Azure Event Hubs |
| Managed Kafka | Amazon MSK (Managed Streaming for Apache Kafka) | Google Cloud Managed Service for Apache Kafka | Azure Event Hubs (Kafka-compatible endpoint) |
| Stream processing | Amazon Managed Service for Apache Flink | Dataflow | Azure Stream Analytics |
| Workflow orchestration | AWS Step Functions | Workflows | Azure Logic Apps, Durable Functions |

### Networking

| Need | AWS | Google Cloud | Azure |
|---|---|---|---|
| Private network | Amazon VPC | VPC (global by default) | Azure Virtual Network |
| Layer 7 load balancing | ALB (Application Load Balancer) | Application Load Balancer | Azure Application Gateway |
| Layer 4 load balancing | NLB (Network Load Balancer) | Network Load Balancer | Azure Load Balancer |
| DNS (Domain Name System) | Amazon Route 53 | Cloud DNS | Azure DNS |
| CDN (Content Delivery Network) | Amazon CloudFront | Cloud CDN | Azure Front Door |
| Dedicated private link | AWS Direct Connect | Cloud Interconnect | Azure ExpressRoute |
| Site-to-site VPN (Virtual Private Network) | AWS Site-to-Site VPN | Cloud VPN | Azure VPN Gateway |
| Network hub | AWS Transit Gateway | Network Connectivity Center | Azure Virtual WAN |
| Private service access | AWS PrivateLink | Private Service Connect | Azure Private Link |
| Outbound NAT (Network Address Translation) | NAT Gateway | Cloud NAT | Azure NAT Gateway |
| WAF (Web Application Firewall) | AWS WAF | Cloud Armor | Azure Web Application Firewall |
| API (Application Programming Interface) management | Amazon API Gateway | Apigee, API Gateway | Azure API Management |

### Identity

| Need | AWS | Google Cloud | Azure |
|---|---|---|---|
| Resource permissions | AWS IAM | Cloud IAM | Azure RBAC (Role-Based Access Control) |
| Workforce sign-on | IAM Identity Center | Cloud Identity | Microsoft Entra ID |
| Customer sign-in | Amazon Cognito | Identity Platform | Microsoft Entra External ID |
| Workload identity | IAM roles | Service accounts, Workload Identity Federation | Managed identities |
| Encryption keys | AWS KMS (Key Management Service) | Cloud KMS | Azure Key Vault |
| Secrets | AWS Secrets Manager | Secret Manager | Azure Key Vault |
| Organisation guardrails | AWS Organizations service control policies | Organization Policy | Azure Policy |
| Audit logs | AWS CloudTrail | Cloud Audit Logs | Azure Monitor activity log |

### AI

| Need | AWS | Google Cloud | Azure |
|---|---|---|---|
| Hosted foundation models | Amazon Bedrock | Gemini Enterprise Agent Platform (formerly Vertex AI) | Microsoft Foundry (formerly Azure AI Foundry) |
| Custom machine learning platform | Amazon SageMaker AI | Gemini Enterprise Agent Platform (formerly Vertex AI) | Azure Machine Learning |
| Managed retrieval for RAG (Retrieval-Augmented Generation) | Amazon Bedrock Knowledge Bases | Agent Search (formerly Vertex AI Search) | Azure AI Search |
| Speech to text | Amazon Transcribe | Speech-to-Text | Azure AI Speech |
| Image analysis | Amazon Rekognition | Cloud Vision API | Azure AI Vision |
| Document extraction | Amazon Textract | Document AI | Azure Document Intelligence |
| Translation | Amazon Translate | Cloud Translation | Azure Translator |
