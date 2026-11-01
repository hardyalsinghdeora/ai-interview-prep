# Containers and Kubernetes Cheatsheet

Covers container images and Linux isolation, core Kubernetes objects, scaling, security, packaging, service mesh terms and everyday kubectl commands.

> Related: [Kubernetes skill](../skills/kubernetes.md) · [Docker and CI/CD skill](../skills/docker-and-ci-cd.md) · [Cloud, DevOps and integration](../topics/08-cloud-devops-integration/README.md) · [cloud-services.md](cloud-services.md) · [ci-cd-and-iac.md](ci-cd-and-iac.md) · [observability-and-reliability.md](observability-and-reliability.md)

## Images and layers

| Term | What it means |
|---|---|
| **Container** | A process isolated by kernel features, sharing the host kernel, so it starts faster and weighs less than a virtual machine. |
| **Image** | An immutable package of filesystem layers and config used to start containers, so the same artefact runs everywhere. |
| **Layer** | A read-only filesystem diff from a build step; layers are shared and cached, which speeds up builds and pulls. |
| **Dockerfile** | A text recipe of build instructions; ordering rarely changed steps first maximises layer cache hits. |
| **Multi-stage build** | A build using several stages and copying only outputs into the last, which keeps compilers out of production images. |
| **Tag** | A mutable, human-friendly image label such as `v1.2`; it can move to different content, so it is not a guarantee. |
| **Digest** | A content hash of an image (`sha256:...`); pinning to it guarantees exactly the same image every time. |
| **Registry** | A service that stores and distributes images, such as Docker Hub or a cloud provider's private registry. |
| **OCI** (Open Container Initiative) | The open standards for image format, runtime and distribution, which let tools from different vendors interoperate. |
| **OverlayFS** (Overlay Filesystem) | The Linux union filesystem that stacks image layers into a single view, with one writable layer on top. |

## Linux isolation

| Term | What it means |
|---|---|
| **Linux namespaces** | Kernel feature that limits what a process can see, such as PIDs, network, mounts, hostname, IPC and users. |
| **cgroups** (control groups) | Kernel feature that limits and accounts for what a process can use, such as CPU, memory and I/O. |
| **Capabilities** | Fine-grained slices of root privilege; dropping unused ones limits the damage a compromised container can do. |
| **seccomp** (secure computing mode) | A kernel filter restricting which system calls a process may make, which shrinks the kernel attack surface. |
| **containerd and runc** | containerd pulls images and manages containers; it calls the low-level OCI runtime runc to actually create them. |

## Cluster architecture

| Term | What it means |
|---|---|
| **Control plane** | Components that store desired state and make cluster decisions; if it fails, running Pods continue but nothing is rescheduled. |
| **kube-apiserver** | The front door for all cluster changes; it authenticates, authorises, validates and persists requests. |
| **etcd** | The consistent key-value store holding all cluster state, so losing it without a backup loses the cluster. |
| **kube-scheduler** | Assigns unscheduled Pods to nodes based on resources, affinity, taints and other constraints. |
| **kube-controller-manager** | Runs built-in controllers that reconcile actual state towards desired state, such as replica counts. |
| **Node** | A worker machine that runs Pods; it hosts the kubelet, a container runtime and usually kube-proxy. |
| **kubelet** | The node agent that starts Pods through the runtime, runs probes and reports status to the API server. |
| **kube-proxy** | Programs node networking rules so traffic to a Service reaches a backing Pod. |
| **Reconciliation loop** | Controllers repeatedly compare desired with actual state and correct differences, which makes Kubernetes self-healing. |
| **CNI** (Container Network Interface) | The plugin standard for Pod networking, implemented by projects such as Calico and Cilium. |

## Workloads

| Term | What it means |
|---|---|
| **Pod** | The smallest deployable unit: one or more containers sharing network and storage, treated as disposable. |
| **Init container** | Runs to completion before app containers start, useful for migrations or waiting on dependencies. |
| **Sidecar container** | A helper running alongside the app in a Pod, commonly for proxies or log shipping. |
| **ReplicaSet** | Keeps a set number of identical Pods running; usually managed for you by a Deployment. |
| **Deployment** | Manages stateless apps through ReplicaSets, providing declarative updates, rolling releases and rollbacks. |
| **StatefulSet** | Runs Pods with stable names, ordered rollout and per-Pod storage, which suits databases and clustered systems. |
| **DaemonSet** | Runs one Pod on each matching node, typically for log agents, monitoring or networking. |
| **Job** | Runs Pods until a set number finish successfully, which suits batch and one-off tasks. |
| **CronJob** | Creates Jobs on a schedule; set a concurrency policy so that slow runs do not overlap unexpectedly. |
| **`maxSurge` and `maxUnavailable`** | Rolling update settings controlling extra Pods allowed and Pods that may be down, trading speed against capacity. |

| Deployment vs StatefulSet | Deployment | StatefulSet |
|---|---|---|
| Pod identity | Random, interchangeable | Stable, ordinal (`db-0`, `db-1`) |
| Storage | Usually shared or none | One PersistentVolumeClaim per Pod |
| Rollout order | Any order, in parallel | Ordered by default |
| Typical use | Web and API services | Databases, Kafka, ZooKeeper |

## Config and metadata

| Term | What it means |
|---|---|
| **Kubernetes namespace** | A logical partition for names, quotas and access rules; it is not a strong security boundary alone. |
| **Label and annotation** | Key-value metadata: labels are used for selection, which is how Services find Pods; annotations carry non-identifying data for tools. |
| **ConfigMap** | Stores non-sensitive configuration separately from images, exposed to Pods as environment variables or files. |
| **Secret** | Stores sensitive values, base64-encoded rather than encrypted by default, so enable encryption at rest and restrict access. |

## Networking

| Term | What it means |
|---|---|
| **Service** | A stable virtual address and DNS name load-balancing to a changing set of Pods. |
| **ClusterIP** | The default Service type, reachable only inside the cluster. |
| **NodePort** | Exposes a Service on a static port on every node; simple, but rarely ideal for production traffic. |
| **LoadBalancer Service** | Asks the cloud provider for an external load balancer pointing at the Service. |
| **Headless Service** | A Service without a virtual IP (`clusterIP: None`), so DNS returns Pod addresses; commonly used with StatefulSets. |
| **Ingress** | Rules for routing external HTTP(S) traffic by host and path to Services; needs a separate controller to work. |
| **Ingress controller** | The proxy implementing Ingress rules; the community ingress-nginx project was retired in March 2026 *(verify)*. |
| **Gateway API** (Gateway Application Programming Interface) | The newer, role-oriented successor to Ingress, with richer routing and separate infrastructure and app ownership. |
| **NetworkPolicy** | Firewall rules between Pods, enforced only if the network plugin supports them. |

## Storage

| Term | What it means |
|---|---|
| **PV** (PersistentVolume) | A piece of cluster storage whose lifecycle is independent of any single Pod. |
| **PVC** (PersistentVolumeClaim) | A Pod's request for storage of a given size and access mode, bound to a PersistentVolume. |
| **StorageClass** | Defines a storage tier and provisioner, enabling volumes to be created dynamically on demand. |

## Scheduling and resources

| Term | What it means |
|---|---|
| **Resource request** | The CPU and memory reserved for scheduling, so it decides which node can fit the Pod. |
| **Resource limit** | The runtime ceiling: exceeding the CPU limit throttles the container, while exceeding the memory limit gets it OOM-killed. |
| **QoS class** (Quality of Service class) | Guaranteed, Burstable or BestEffort, derived from requests and limits; it influences eviction order under node pressure. |
| **Taint and toleration** | A taint repels Pods from a node unless they tolerate it, which reserves nodes for specific workloads. |
| **Node affinity and Pod anti-affinity** | Node affinity attracts Pods to labelled nodes; Pod anti-affinity keeps matching Pods apart to reduce shared failure. |
| **Topology spread constraints** | Distribute Pods evenly across zones or nodes, improving availability during a failure. |
| **OOMKilled** (Out Of Memory killed) | The container was killed for exceeding memory, shown as exit code 137. |
| **CrashLoopBackOff** | A container keeps crashing, and Kubernetes waits longer between restarts; check previous logs and events. |
| **ImagePullBackOff** | The image cannot be pulled, usually because of a wrong name, wrong tag or missing registry credentials. |

## Scaling, disruption and health

| Term | What it means |
|---|---|
| **HPA** (Horizontal Pod Autoscaler) | Changes the replica count from observed metrics such as CPU or custom signals; needs requests set for utilisation targets. |
| **VPA** (Vertical Pod Autoscaler) | Recommends or sets Pod requests from usage; avoid combining with HPA on the same CPU or memory metric. |
| **In-place Pod resize** | Changes a running Pod's CPU and memory, often without restarting; stable since Kubernetes 1.35 *(verify)*. |
| **Cluster Autoscaler** | Adds nodes when Pods cannot be scheduled and removes underused ones, usually through cloud node groups. |
| **Karpenter** | A node autoscaler that provisions suitable instances directly from pending Pod needs rather than fixed node groups. |
| **KEDA** (Kubernetes Event-Driven Autoscaling) | Scales workloads from event sources such as queue length, including down to zero. |
| **PDB** (PodDisruptionBudget) | Limits how many Pods voluntary evictions, such as node drains, may remove at once; it does not stop crashes. |
| **Liveness probe** | Checks whether a container should be restarted; failing it triggers a restart, so avoid checking external dependencies. |
| **Readiness probe** | Checks whether a Pod can take traffic; failing it removes the Pod from Service endpoints without restarting it. |
| **Startup probe** | Holds off the other probes until a slow-starting app is up, preventing premature restarts. |

| HPA vs VPA | HPA | VPA |
|---|---|---|
| Changes | Number of Pods | CPU and memory per Pod |
| Best for | Stateless, parallel services | Rightsizing, hard-to-scale apps |
| Limitation | App must scale out | May restart Pods to apply changes |

## Security and access

| Term | What it means |
|---|---|
| **RBAC** (Role-Based Access Control) | Grants verbs on API resources to users and service accounts, enabling least privilege. |
| **Role and ClusterRole** | Sets of permissions: a Role applies within one namespace, a ClusterRole is defined cluster-wide. |
| **RoleBinding and ClusterRoleBinding** | Attach roles to subjects; a RoleBinding grants within a namespace, a ClusterRoleBinding across the cluster. |
| **ServiceAccount** | An identity for workloads calling the API or, through federation, cloud services. |
| **Admission controller** | Intercepts API requests after authorisation to validate or change them, which is how policy gets enforced. |
| **Pod Security Standards** | Three built-in profiles (Privileged, Baseline, Restricted), enforced by the Pod Security Admission controller. |

## Packaging and extension

| Term | What it means |
|---|---|
| **Helm** | A package manager for Kubernetes that templates manifests and tracks versioned installs with rollback. |
| **Chart and release** | A chart packages templates and default values; a release is one installed instance with its own revision history. |
| **Kustomize** | Patches plain YAML through overlays rather than templates; built into kubectl with `-k`. |
| **CRD** (CustomResourceDefinition) | Adds a new resource type to the Kubernetes API, which is the basis for extending the platform. |
| **Operator** | A custom controller plus CRDs that encodes running expertise, such as database failover and backups. |

## Service mesh

| Term | What it means |
|---|---|
| **Service mesh** | Infrastructure handling service-to-service traffic, security and telemetry outside application code, at the cost of added complexity. |
| **Mesh data plane and control plane** | The data plane proxies carry traffic and apply rules; the control plane configures them and distributes policy and certificates. |
| **Envoy** | A widely used high-performance proxy, serving as the data plane for Istio and many gateways. |
| **Istio** | A feature-rich mesh offering traffic management, mTLS and observability, in sidecar or sidecar-less ambient modes. |
| **mTLS** (mutual Transport Layer Security) | Both sides present certificates, so services authenticate each other and traffic is encrypted. |
| **Traffic splitting** | Sends set percentages of requests to different versions, enabling canary releases without app changes. |

## Common kubectl commands

| Term | What it means |
|---|---|
| `kubectl get pods -o wide` | Lists Pods with their node and IP, a quick first look at placement and status. |
| `kubectl describe pod <name>` | Shows config, state and recent events, usually the fastest way to find why a Pod is stuck. |
| `kubectl logs <pod> --previous` | Prints logs from the last crashed container, which is essential for CrashLoopBackOff. |
| `kubectl exec -it <pod> -- sh` | Opens a shell inside a running container for live inspection. |
| `kubectl debug -it <pod> --image=busybox` | Attaches an ephemeral debug container, helpful when the image has no shell. |
| `kubectl get events --sort-by=.lastTimestamp` | Lists recent events in time order, showing scheduling and pull failures. |
| `kubectl apply -f app.yaml` | Creates or updates objects declaratively from a file. |
| `kubectl rollout undo deployment/<name>` | Rolls a Deployment back to its previous revision. |
| `kubectl port-forward svc/<name> 8080:80` | Tunnels a local port to a Service for testing without exposing it. |
| `kubectl top pods` | Shows live CPU and memory use; requires metrics-server. |
| `kubectl auth can-i <verb> <resource>` | Checks whether the current identity has a permission, which helps debug RBAC. |
| `kubectl drain <node> --ignore-daemonsets` | Evicts Pods for node maintenance, respecting PodDisruptionBudgets. |
