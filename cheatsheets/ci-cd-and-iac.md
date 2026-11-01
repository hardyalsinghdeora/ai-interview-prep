# CI/CD and Infrastructure as Code Cheatsheet

Covers build and release pipelines, artefacts, deployment strategies, feature flags, GitOps, Terraform and DORA metrics.

> Related: [Docker and CI/CD skill](../skills/docker-and-ci-cd.md) · [Terraform and IaC skill](../skills/terraform-and-iac.md) · [Cloud, DevOps and integration](../topics/08-cloud-devops-integration/README.md) · [containers-and-kubernetes.md](containers-and-kubernetes.md) · [cloud-services.md](cloud-services.md) · [observability-and-reliability.md](observability-and-reliability.md) · [CI/CD tools cheatsheet](ci-cd-tools.md)

## Pipelines

| Term | What it means |
|---|---|
| **CI** (Continuous Integration) | Merging small changes often, with automated build and tests on each one, so problems surface early. |
| **Continuous Delivery** | Every passing change is kept ready to release, with production release as a one-click human decision. |
| **Continuous Deployment** | Every passing change goes to production automatically, which demands strong tests and safe rollout tooling. |
| **Pipeline** | An automated sequence of build, test and deploy steps, making releases repeatable and auditable. |
| **Stage** | A group of pipeline jobs, such as test or deploy, with gates between stages. |
| **Job** | A unit of work in a pipeline that runs on a runner; jobs can run in parallel to save time. |
| **Runner** | The machine or container executing pipeline jobs; it often holds credentials, so it needs hardening. |
| **Trigger** | The event starting a pipeline, such as a push, pull request, tag or schedule. |
| **Quality gate** | A required check such as tests, coverage or a security scan that blocks promotion when it fails. |
| **Flaky test** | A test that passes and fails without code changes, which erodes trust in the pipeline. |
| **Build cache** | Reused dependencies or build outputs between runs, which shortens feedback time. |
| **Trunk-based development** | Short-lived branches merged often into main, which reduces merge pain and supports continuous delivery. |
| **Monorepo** | Many projects in one repository, simplifying shared changes but requiring selective builds. |

## Artefacts and supply chain

| Term | What it means |
|---|---|
| **Artefact** | The versioned output of a build, such as an image or package, stored for later deployment. |
| **Artefact repository** | Storage for versioned build outputs, such as a container registry, Artifactory or Nexus. |
| **Build once, deploy many** | The same artefact is promoted through environments, so what was tested is exactly what ships. |
| **Promotion** | Moving a tested artefact to the next environment rather than rebuilding it. |
| **Semantic versioning** | `MAJOR.MINOR.PATCH` numbering that signals breaking changes, new features and fixes. |
| **SBOM** (Software Bill of Materials) | An inventory of the components in a build, which speeds up response when a dependency has a vulnerability. |
| **SAST** (Static Application Security Testing) | Scans source code for security flaws without running it, catching issues early in the pipeline. |
| **DAST** (Dynamic Application Security Testing) | Tests a running application from outside, finding issues that only appear at runtime. |
| **SCA** (Software Composition Analysis) | Checks third-party dependencies for known vulnerabilities and licence problems. |
| **SLSA** (Supply-chain Levels for Software Artifacts) | A framework of increasing assurance levels against build and supply chain tampering. |
| **Artefact signing** | Cryptographically signing outputs, for example with Sigstore Cosign, so deployers can verify their origin. |

## Release strategies

| Term | What it means |
|---|---|
| **Rolling release** | Replaces instances gradually in batches, needing no spare environment, though two versions run together for a while. |
| **Blue-green deployment** | Runs two full environments and switches traffic at once, allowing instant rollback at double the capacity cost. |
| **Canary release** | Sends a small share of traffic to the new version, watches metrics, then widens, which limits blast radius. |
| **Shadow deployment** | Copies live traffic to a new version and discards its responses, testing it safely under real load. |
| **Rollback** | Returning to the previous version, which should be tested and quick rather than improvised. |
| **Roll forward** | Fixing a problem by shipping a new version, common when database changes make going back unsafe. |
| **Expand and contract** | Backward-compatible schema change: add new structure, migrate, then remove old, so old and new code can coexist. |
| **Deployment vs release** | Deployment puts code in production; release exposes it to users, and flags let you separate the two. |
| **Smoke test** | A quick post-deployment check of critical paths, which catches obvious breakage early. |

| Strategy | Rollback speed | Extra cost | Main risk |
|---|---|---|---|
| Rolling | Medium | Low | Mixed versions during rollout |
| Blue-green | Very fast | High | Database compatibility |
| Canary | Fast | Medium | Needs good metrics and routing |

## Feature flags

| Term | What it means |
|---|---|
| **Feature flag** | A runtime switch that turns code paths on or off, separating deployment from release. |
| **Release flag** | A short-lived flag hiding unfinished work, which should be removed after launch. |
| **Ops flag** | A kill switch that disables a feature quickly during an incident without a deployment. |
| **Experiment flag** | Routes users to variants for A/B testing and measures the effect. |
| **Percentage rollout** | Enables a feature for a growing share of users, usually with sticky assignment per user. |
| **Flag debt** | Stale flags left in code, which multiply code paths and raise the risk of mistakes. |

## GitOps

| Term | What it means |
|---|---|
| **GitOps** | Git holds the desired system state, and an agent keeps the environment matched to it, giving an audit trail. |
| **Pull-based deployment** | An agent in the cluster pulls changes, so CI needs no direct cluster credentials. |
| **Push-based deployment** | The pipeline applies changes directly to the target, which is simple but needs stored credentials. |
| **Argo CD** (Argo Continuous Delivery) | A Kubernetes GitOps controller with a UI that syncs manifests from Git and shows drift. |
| **Flux** | A Kubernetes GitOps toolkit that reconciles cluster state from Git and other sources. |
| **Self-healing sync** | The agent automatically reverts manual changes that differ from Git. |

## Terraform and IaC

| Term | What it means |
|---|---|
| **IaC** (Infrastructure as Code) | Defining infrastructure in version-controlled files, making environments reproducible and changes reviewable. |
| **Declarative** | You describe the desired end state and the tool works out the steps, as Terraform does. |
| **Terraform** | A declarative multi-cloud IaC tool using HCL; relicensed under the Business Source License in 2023 *(verify)*. |
| **OpenTofu** | An open-source fork of Terraform under the Linux Foundation, largely compatible with its workflow *(verify)*. |
| **HCL** (HashiCorp Configuration Language) | The readable configuration language for Terraform files. |
| **Provider** | A plugin that talks to a platform's API, such as AWS or Kubernetes, and defines its resources. |
| **Resource** | A single infrastructure object managed by Terraform, such as a VM or bucket. |
| **Data source** | A read-only lookup of something existing outside this configuration. |
| **Module** | A reusable, parameterised group of resources that standardises patterns across teams. |
| **State file** | Terraform's record mapping config to real resources; it can contain secrets, so store and encrypt it carefully. |
| **Remote backend** | Stores state in shared storage such as object storage, enabling team use. |
| **State locking** | Prevents two runs changing state at once, which avoids corruption and conflicting changes. |
| **`terraform plan`** | Previews proposed changes without applying them, which is the key review step. |
| **`terraform apply`** | Executes the changes in a plan to reach the desired state. |
| **`terraform import`** | Brings an existing resource under Terraform management without recreating it. |
| **Drift** | Real infrastructure differing from code, typically from manual changes; detect it through regular plans. |
| **Workspace** | Separate state instances from one configuration; many teams prefer separate directories for strict environment isolation. |
| **Idempotence** | Repeating an apply with no changes produces no further effect, making automation safe. |
| **Blast radius** | How much a bad change could break; smaller state files and modules reduce it. |
| **Policy as code** | Automated rules on infrastructure changes, using tools such as Open Policy Agent or Sentinel. |

## DORA metrics

The DORA report added rework rate as a fifth metric and now favours per-metric distributions over fixed performance tiers *(verify)*.

| Term | What it means |
|---|---|
| **DORA** (DevOps Research and Assessment) | A research programme whose metrics link software delivery performance to organisational outcomes. |
| **Deployment frequency** | How often code reaches production; higher usually indicates smaller, safer batches. |
| **Lead time for changes** | Time from commit to running in production, showing how fast the delivery system responds. |
| **Change failure rate** | Share of deployments that cause a failure needing remediation, measuring release quality. |
| **Failed deployment recovery time** | Time to recover from a failed deployment, replacing the earlier MTTR-style measure in DORA's framing. |
| **Rework rate** | Share of deployments that are unplanned and made to fix production problems *(verify)*. |
