# GCP and Azure Cheatsheet

Covers the main Google Cloud Platform services and then the main Microsoft Azure services, with choosing tables for each; cross-provider equivalents are in the mapping table in [cloud-services.md](cloud-services.md).

> Related: [cloud-services.md](cloud-services.md) · [aws.md](aws.md) · [Google Cloud skill](../skills/google-cloud-gcp.md) · [Azure skill](../skills/azure.md) · [Bedrock and Vertex AI skill](../skills/bedrock-and-vertex-ai.md) · [Cloud, DevOps and integration](../topics/08-cloud-devops-integration/README.md) · [containers-and-kubernetes.md](containers-and-kubernetes.md) · [messaging-and-streaming.md](messaging-and-streaming.md)

## GCP: compute and containers

| Term | What it means |
|---|---|
| **Compute Engine** | Google Cloud's virtual machines, which can be live-migrated during host maintenance, usually without a reboot. |
| **Managed instance group** | Identical virtual machines created from a template, with autoscaling, autohealing and rolling updates. |
| **Spot VM** (Spot Virtual Machine) | Heavily discounted spare capacity that can be reclaimed at short notice, for fault-tolerant work. |
| **Cloud Run** | Runs stateless containers per request or as jobs, scaling to zero, with little to operate. |
| **Cloud Run functions** | Event-driven functions, formerly Cloud Functions, deployed from source and run on Cloud Run *(verify)*. |
| **GKE** (Google Kubernetes Engine) | Managed Kubernetes, for portability and advanced scheduling, with more operational work than Cloud Run. |
| **GKE Autopilot** (Google Kubernetes Engine Autopilot) | A GKE mode where Google manages nodes and you pay for pod resources, which reduces operations work *(verify)*. |
| **App Engine** | An older application platform that runs code without server management; new projects often prefer Cloud Run. |
| **Artifact Registry** | Stores container images and language packages, replacing the older Container Registry. |

| Choose compute | Cloud Run | GKE | Compute Engine |
|---|---|---|---|
| Unit | Container | Pod | Virtual machine |
| Scale to zero | Yes | Pods can, but cluster fees remain | No |
| You manage | Image and settings | Workloads and cluster settings | Operating system and up |
| Choose for | Stateless HTTP services and jobs | Kubernetes features and portability | Full control or legacy software |

## GCP: storage and databases

| Term | What it means |
|---|---|
| **Cloud Storage** | Object storage in regional, dual-region or multi-region locations, with strongly consistent object operations. |
| **Storage classes** | Standard, Nearline, Coldline and Archive, with 0, 30, 90 and 365 day minimums respectively *(verify)*. |
| **Persistent Disk** | Network block storage for virtual machines, available in zonal and regional forms. |
| **Cloud SQL** | Managed MySQL, PostgreSQL and SQL Server for standard relational workloads, scaling mainly vertically. |
| **AlloyDB** | PostgreSQL-compatible database with disaggregated storage and a columnar engine, for demanding transactional and analytical work. |
| **Spanner** | Horizontally scalable relational database with strong global consistency, using synchronised clocks called TrueTime. |
| **Firestore** | Serverless document database with real-time listeners and offline mobile sync, common for app backends. |
| **Bigtable** | Wide-column database with low latency at huge scale, keyed by row, and HBase-compatible. |
| **Memorystore** | Managed Valkey, Redis or Memcached, used as a low-latency cache *(verify)*. |

| Choose a database | Cloud SQL | AlloyDB | Spanner | Firestore | Bigtable |
|---|---|---|---|---|---|
| Model | Relational | PostgreSQL-compatible | Relational | Document | Wide-column |
| Write scaling | Vertical | Mostly vertical | Horizontal | Automatic | Horizontal |
| Choose for | Standard apps | Heavy PostgreSQL workloads | Global consistency at scale | Mobile and web apps | Time series, huge key lookups |

## GCP: analytics and messaging

| Term | What it means |
|---|---|
| **BigQuery** | Serverless columnar data warehouse that separates storage from compute, priced by data scanned or reserved slots. |
| **Slot** | BigQuery's unit of query compute, reserved through editions to make costs predictable *(verify)*. |
| **Partitioned table** | A BigQuery table divided by date or integer range, so queries scan and cost less. |
| **Dataflow** | Managed Apache Beam for batch and streaming pipelines, with autoscaling and exactly-once processing within the pipeline. |
| **Dataproc** | Managed Spark and Hadoop clusters, useful for migrating existing jobs with little change. |
| **Cloud Composer** | Managed Apache Airflow for scheduling and orchestrating data pipelines. |
| **Pub/Sub** | Global messaging where topics deliver to subscriptions; at-least-once by default, with no partitions to manage. |
| **Cloud Tasks** | A queue that dispatches tasks to a named handler, with rate limits, retries and scheduled delivery. |
| **Eventarc** | Routes events from Google services, audit logs and custom sources to targets such as Cloud Run. |
| **Workflows** | Serverless orchestration of services and APIs, written in YAML or JSON, with retries and branching. |

| Pub/Sub vs Cloud Tasks | Pub/Sub | Cloud Tasks |
|---|---|---|
| Publisher knows consumer | No | Yes, it names the handler |
| Delivery rate control | Limited | Per-queue rate limits |
| Scheduled delivery | No | Yes |
| Choose for | Event fan-out and decoupling | Calling a fragile API at a controlled rate |

## GCP: networking

| Term | What it means |
|---|---|
| **VPC** (Virtual Private Cloud) | A global private network whose subnets are regional, so one network can span regions. |
| **Shared VPC** (Shared Virtual Private Cloud) | A host project shares subnets with service projects, centralising network control across teams. |
| **VPC firewall rules** (Virtual Private Cloud firewall rules) | Stateful rules applied using network tags or service accounts rather than subnet membership. |
| **VPC Service Controls** (Virtual Private Cloud Service Controls) | A perimeter around managed services, which reduces the risk of data exfiltration even with valid credentials. |
| **Cloud Load Balancing** | Managed Layer 4 and Layer 7 balancing; global external types use one anycast IP across regions. |
| **Cloud Armor** | WAF and DDoS protection attached to external load balancers, with custom and preconfigured rules. |
| **Private Service Connect** | Reaches Google or third-party services through private IP addresses in your own VPC. |

## GCP: identity, operations and AI

| Term | What it means |
|---|---|
| **Resource hierarchy** | Organisation, folders, projects and resources; policies set high up are inherited below. |
| **Project** | The basic unit for resources, billing and permissions, commonly split per application and environment. |
| **Cloud IAM** (Cloud Identity and Access Management) | Grants principals roles on resources through allow policies; prefer predefined roles over broad basic roles. |
| **Service account** | An identity for workloads; avoid downloaded keys, which are long-lived and easy to leak. |
| **Workload Identity Federation** | Lets external workloads, such as CI jobs, get short-lived credentials without service account keys. |
| **Organization Policy** | Organisation-wide constraints, such as allowed regions, that apply regardless of IAM grants. |
| **Cloud KMS** (Cloud Key Management Service) | Creates and manages encryption keys, including customer-managed keys for other Google Cloud services. |
| **Secret Manager** | Stores versioned secrets with IAM access control and audit logging. |
| **Cloud Monitoring and Cloud Logging** | Google Cloud's metrics, alerts, dashboards and log storage, formerly known together as Stackdriver. |
| **Gemini Enterprise Agent Platform** | Google Cloud's AI platform for models, agents and machine learning, formerly Vertex AI *(verify)*. |

## Azure: organisation and compute

| Term | What it means |
|---|---|
| **Management group** | Groups subscriptions so policies and access apply across them from one place. |
| **Subscription** | A billing and quota boundary holding resource groups, often used to separate teams and environments. |
| **Resource group** | A container for resources sharing a lifecycle, which can be deployed, secured and deleted together. |
| **ARM** (Azure Resource Manager) | Azure's control plane, which handles every deployment and management request, whichever tool sent it. |
| **Bicep** | A concise language for Azure infrastructure as code that compiles to Azure Resource Manager templates. |
| **Azure Virtual Machines** | Azure's infrastructure as a service compute, used when you need full control of the operating system. |
| **Virtual Machine Scale Sets** | Manages a group of load-balanced virtual machines, with autoscaling and rolling updates. |
| **Azure App Service** | A managed platform for web apps and APIs, with built-in scaling, TLS and deployment integration. |
| **App Service plan** | The compute behind App Service apps; apps on one plan share it, along with its costs. |
| **Deployment slot** | A separate live instance of an App Service app, which can be swapped with production for near-zero downtime. |
| **Azure Functions** | Event-driven serverless functions using triggers and bindings, with several hosting plans *(verify)*. |
| **AKS** (Azure Kubernetes Service) | Managed Kubernetes, where you run workloads and pay mainly for nodes, plus optional paid control plane tiers *(verify)*. |
| **Azure Container Apps** | Serverless containers built on Kubernetes, KEDA and Dapr, scaling to zero without exposing the cluster *(verify)*. |

| Choose compute | App Service | Functions | Container Apps | AKS | Virtual Machines |
|---|---|---|---|---|---|
| Deploy | Code or container | Code | Container | Pods | Anything |
| Scale to zero | No | Yes, on consumption plans | Yes | Not by default | No |
| Choose for | Web apps and APIs | Short event handlers | Microservices without Kubernetes management | Full Kubernetes control | Legacy or special software |

## Azure: storage and databases

| Term | What it means |
|---|---|
| **Storage account** | A namespace for blobs, files, queues and tables, which sets redundancy and networking choices. |
| **Blob Storage** | Object storage with hot, cool, cold and archive access tiers; archive must be rehydrated before reading *(verify)*. |
| **Azure Managed Disks** | Block storage for virtual machines, offered in tiers from Standard HDD up to Ultra Disk *(verify)*. |
| **Azure Files** | Managed file shares over SMB and NFS, often used in lift-and-shift migrations. |
| **Azure SQL Database** | Fully managed SQL Server-based database as a service, with serverless and Hyperscale options *(verify)*. |
| **Azure SQL Managed Instance** | A near-complete SQL Server instance as a service, for migrations needing instance-level features. |
| **Azure Database for PostgreSQL** | Managed PostgreSQL, with its flexible server option giving more control over configuration and availability *(verify)*. |
| **Azure Cosmos DB** | Globally distributed multi-model database with multi-region replication and a choice of consistency levels. |
| **RU** (Request Unit) | Cosmos DB's normalised cost of an operation; poor partition keys and queries consume many. |
| **Azure Managed Redis** | Managed Redis service, succeeding Azure Cache for Redis, which is retiring *(verify)*. |

| Storage redundancy | Copies held | Survives |
|---|---|---|
| LRS (Locally Redundant Storage) | Three, in one data centre | Disk or rack failure |
| ZRS (Zone-Redundant Storage) | Three, across zones | A zone failure |
| GRS (Geo-Redundant Storage) | Six, across two regions | A regional outage |
| GZRS (Geo-Zone-Redundant Storage) | Six, across zones and a second region | A zone or regional outage |

| Cosmos DB consistency | Guarantee |
|---|---|
| Strong | Reads always see the latest committed write |
| Bounded staleness | Reads lag by no more than a set time or number of versions |
| Session | A client sees its own writes; the default level |
| Consistent prefix | Reads never see writes out of order |
| Eventual | No ordering guarantee, with the lowest latency and cost |

## Azure: data, messaging and integration

| Term | What it means |
|---|---|
| **Azure Data Factory** | Managed data integration service for building and scheduling pipelines that move and transform data. |
| **Azure Synapse Analytics** | Analytics service combining SQL data warehousing, Spark and pipelines *(verify)*. |
| **Microsoft Fabric** | A software as a service analytics platform combining engineering, warehousing and BI over one lake, called OneLake *(verify)*. |
| **Azure Service Bus** | An enterprise broker with queues, topics, sessions for ordering, transactions and dead-lettering. |
| **Azure Event Grid** | Pushes discrete events from Azure services and custom sources to subscribed handlers. |
| **Azure Event Hubs** | A partitioned, high-throughput event log with a Kafka-compatible endpoint, for telemetry and streaming. |
| **Queue Storage** | A simple, low-cost queue inside a storage account, with far fewer features than Service Bus. |
| **Azure Logic Apps** | Low-code workflows with many prebuilt connectors, which suit integrating SaaS and enterprise systems. |

| Choose messaging | Service Bus | Event Grid | Event Hubs |
|---|---|---|---|
| Carries | Commands and business messages | Notifications that something happened | Streams of telemetry and events |
| Delivery | Consumers receive and settle | Pushed to handlers | Consumers read by offset |
| Replay | No | No | Within retention |
| Choose for | Reliable, ordered workflows | Reacting to state changes | High-volume ingestion and analytics |

## Azure: networking

| Term | What it means |
|---|---|
| **VNet** (Virtual Network) | Azure's private network, regional in scope, connected to others through peering. |
| **NSG** (Network Security Group) | Stateful allow and deny rules with priorities, attached to subnets or network interfaces. |
| **Azure Load Balancer** | A regional Layer 4 load balancer for TCP and UDP traffic. |
| **Application Gateway** | A regional Layer 7 load balancer with path-based routing, TLS termination and optional WAF. |
| **Azure Front Door** | A global Layer 7 entry point combining CDN, acceleration, failover and optional WAF *(verify)*. |
| **Traffic Manager** | DNS-based global routing that sends clients to healthy endpoints without carrying the traffic itself. |
| **ExpressRoute** | A private dedicated connection from on-premises networks to Azure, bypassing the public internet. |
| **Azure Private Link** | Reaches Azure and partner services over a private IP address in your VNet, through private endpoints. |
| **Azure API Management** | A gateway and developer portal for publishing, securing and throttling APIs. |

| Choose traffic routing | Load Balancer | Application Gateway | Front Door | Traffic Manager |
|---|---|---|---|---|
| Scope | Regional | Regional | Global | Global |
| Layer | 4 | 7 | 7 | DNS |
| In the traffic path | Yes | Yes | Yes | No |
| Choose for | TCP and UDP inside a region | HTTP apps inside a region | Global web apps | DNS failover for any protocol |

## Azure: identity, operations and AI

| Term | What it means |
|---|---|
| **Microsoft Entra ID** | Microsoft's cloud identity service, formerly Azure Active Directory, used for sign-in and single sign-on. |
| **Azure RBAC** (Azure Role-Based Access Control) | Assigns a role to a principal at a scope, and that access is inherited by everything below. |
| **Service principal** | An application's identity in an Entra tenant, which usually relies on a secret or certificate. |
| **Managed identity** | An Azure-managed workload identity with no credentials to store; system-assigned or user-assigned. |
| **Azure Key Vault** | Stores secrets, encryption keys and certificates, with access controls and audit logs. |
| **Azure Policy** | Audits or enforces rules on resources at a scope, such as allowed regions or required tags. |
| **Microsoft Sentinel** | A cloud SIEM (Security Information and Event Management) tool for collecting, detecting and responding to threats. |
| **Azure Monitor** | Azure's platform for metrics, logs and alerts across resources and applications. |
| **Log Analytics workspace** | Where Azure Monitor logs are stored and queried, using KQL (Kusto Query Language). |
| **Application Insights** | The application performance monitoring part of Azure Monitor, covering requests, dependencies and traces. |
| **Microsoft Foundry** | Azure's platform for building with hosted models and agents, formerly Azure AI Foundry *(verify)*. |
| **Azure AI Search** | Managed keyword, vector and hybrid search, commonly used as the retrieval layer in RAG systems. |

| Azure identity | Service principal with secret | Managed identity |
|---|---|---|
| Credentials | Created, stored and rotated by you | Handled by Azure |
| Runs on | Anywhere | Azure resources |
| Choose for | Workloads outside Azure, ideally federated instead | Anything running in Azure |

```bash
# Azure RBAC: role + principal + scope
az role assignment create \
  --assignee "<principal-id>" \
  --role "Storage Blob Data Reader" \
  --scope "/subscriptions/<id>/resourceGroups/<group>"
```
