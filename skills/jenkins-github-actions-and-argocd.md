# Jenkins, GitHub Actions & Argo CD

Most teams inherit a delivery toolchain rather than choose one, so interviewers want to know whether you can run it in depth: agents and runners, GitOps sync behaviour, idempotent automation, and credentials that a malicious pull request cannot reach. Platform, DevOps, Staff backend and Engineering Manager roles ask most, often through a slow pipeline, a leaked secret or a migration away from an ageing Jenkins server.

> Related: [Docker and CI/CD](docker-and-ci-cd.md) · [Kubernetes](kubernetes.md) · [Terraform & IaC](terraform-and-iac.md) · [Security & compliance](security-and-compliance.md) · [Cloud, DevOps & Integration](../topics/08-cloud-devops-integration/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] Jenkins controller vs. agent vs. executor: what runs where, and why should the controller have no executors of its own?
- [ ] Declarative vs. scripted Jenkins pipelines: what does each allow, and which would you default to in a new Jenkinsfile?
- [ ] GitHub Actions: how do events, workflows, jobs, steps and actions relate, and how does one job pass data to another?
- [ ] GitHub-hosted vs. self-hosted runners: what do you gain and give up, and why are self-hosted runners risky on a public repository? *(verify)*
- [ ] GitLab CI: stages vs. `needs`, what `rules` decide, and how the shell, Docker and Kubernetes executors differ. *(verify)*
- [ ] What makes a workflow GitOps rather than a CI job that runs `kubectl apply`?
- [ ] Argo CD: what does an Application define, and how do sync status and health status differ?
- [ ] Ansible: how do inventories, playbooks, roles and handlers fit together, and what does running agentless over SSH mean for scale and setup?
- [ ] What does idempotency mean for an Ansible task, and why are `command` and `shell` the usual ways to break it?

## B. Working depth
- [ ] Jenkins shared libraries: what belongs in `vars` and `src`, how do you version them, and how do you stop one library change from breaking every team's build?
- [ ] Ephemeral agents using the Jenkins Kubernetes plugin: what do pod templates give you over static agents, and what happens to build caches? *(verify)*
- [ ] How do `stash`, `parallel`, `when` and `input` behave in a Jenkins pipeline, and what happens to a running build when the controller restarts?
- [ ] GitHub Actions: reusable workflows vs. composite actions. What can each do, how do secrets and permissions pass through, and when would you use each? *(verify)*
- [ ] What does `GITHUB_TOKEN` get by default, how would you narrow it with `permissions`, and how do `pull_request` and `pull_request_target` differ for a fork? *(verify)*
- [ ] GitLab CI: how do `include`, parent-child pipelines, protected variables and merge trains help a large organisation, and what do they cost in complexity? *(verify)*
- [ ] Argo CD: what do automated sync, self-heal and prune each do, and how would sync waves and hooks order a database migration before an application rollout? *(verify)*
- [ ] App of Apps vs. ApplicationSet: how does each scale to many services and clusters, and how do AppProjects limit what a team can deploy? *(verify)*
- [ ] Ansible: how do `changed_when`, `failed_when`, `creates` and `--check --diff` help you write and verify idempotent tasks, and what breaks in check mode?
- [ ] Ansible rolling changes: what do `serial`, `max_fail_percentage` and handlers do during an update across a fleet, and when would you use a dynamic inventory?
- [ ] Artefact repositories: what are local, proxy and virtual repositories in tools like Artifactory or Nexus, and how do you set retention without deleting what is still running? *(verify)*
- [ ] OIDC federation from a pipeline to a cloud provider vs. stored access keys: how does it work, and which token claims would you restrict on? *(verify)*
- [ ] Log masking is on, yet a secret leaks. Name the ways that happens (encoding, `set -x`, build arguments, uploaded artefacts, debug output) and how you would close each.

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Jenkins at scale: how do you deal with plugin sprawl, a single controller and upgrades, what does Configuration as Code solve, and when is leaving Jenkins worth the cost? *(verify)*
- [ ] Poisoned pipeline execution: how can a changed pipeline file or a PR title interpolated into `run:` execute attacker code, and what stops each?
- [ ] Third-party actions and plugins are code running with your credentials. Compare pinning to a commit SHA, allowlists and internal forks, and say what signing and provenance add. *(verify)*
- [ ] Design a self-hosted runner fleet: ephemeral runners, autoscaling with Actions Runner Controller or similar, network isolation and cost. What must never persist between jobs? *(verify)*
- [ ] Argo CD vs. Flux, and one Argo CD managing many clusters vs. one per cluster: compare blast radius, credentials, permissions and operational overhead. *(verify)*
- [ ] Secrets in GitOps: compare Sealed Secrets, SOPS and the External Secrets Operator on rotation, audit and what a repository compromise exposes. Where does Ansible Vault fit? *(verify)*
- [ ] GitOps promotion: branches per environment vs. directories with Kustomize overlays vs. automated image updates. How does each affect review, rollback and drift? *(verify)*
- [ ] Ansible, Terraform and Argo CD overlap at the edges. Where would you draw the boundaries, and is Ansible still worthwhile when you bake immutable images?
- [ ] Pipeline speed at scale: how do you separate queue time from run time, weigh dependency caches against artefacts and remote build caches, and stop flaky tests cancelling out your gains?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Migrate 200 Jenkins jobs to GitHub Actions or GitLab CI. How do you inventory them, replace shared libraries and credentials, run old and new side by side, and decide when to cut over?
- [ ] A long-lived cloud access key appears in a public build log. Run the first hour, then describe the lasting fix.
- [ ] Each build takes eight minutes, but developers wait 30 minutes every morning before theirs starts. How do you diagnose it, and what would you change?
- [ ] During an incident an engineer patches a Deployment with `kubectl`, and Argo CD reverts it seconds later. What should the emergency change process be, and what would you configure differently?
- [ ] An Argo CD Application stays `OutOfSync` even though nobody has changed it. How do you find out why, and when is `ignoreDifferences` the right answer rather than a cover-up? *(verify)*
- [ ] A playbook reports `changed` on every run, and last week it restarted production services needlessly. Find the cause and show how you would prevent a repeat.
- [ ] Auditors require separation of duties and a trace from commit to production. Design that for GitOps across 40 services in three regions without a manual ticket gate on every change.

## E. Hands-on checks (things to be able to DO)
- [ ] Write a declarative Jenkinsfile with parallel stages, a container or Kubernetes agent, `withCredentials`, a `post` block and one shared-library step.
- [ ] Write a reusable GitHub Actions workflow with a matrix, dependency caching, least-privilege `permissions`, OIDC to a cloud provider and a deploy job gated by an environment. *(verify)*
- [ ] Port that pipeline to `.gitlab-ci.yml` using `rules` and `needs`, and publish a versioned artefact to a repository that refuses to overwrite a release.
- [ ] Install Argo CD on a kind cluster, deploy a Helm chart from Git, show drift being self-healed, and roll back with a Git revert.
- [ ] Write an Ansible role that installs and configures nginx, show `changed=0` on the second run, and test it with `--check --diff` and Molecule. *(verify)*
- [ ] Take a slow pipeline, record queue time, stage durations and cache hit rate, then cut the total and publish the before and after numbers.

---

**Good answers show:**
- Knowing where code runs and which credentials it can reach at every step, for builds from forks as well as trusted branches.
- Short-lived, narrowly scoped identity as the default, with a practised response for when a secret does leak.
- GitOps explained through reconciliation: what Git declares, what the controller changes, and how emergencies and drift are handled.
- Idempotency demonstrated rather than assumed, with a second run and check mode showing that nothing changes.
- Pipeline speed treated as a measured quantity, separating queue time, run time, cache hit rate and flakiness.
- Pragmatism about tools: a Jenkins migration or a new GitOps controller justified by the problem it solves and the cost of moving.

**Prove it:** Build one small service through GitHub Actions with OIDC and a repository that holds immutable artefacts, deploy it with Argo CD to dev and prod overlays on a local cluster, and configure its host or database dependency with an idempotent Ansible role. Then attack your own setup with a fork PR and an injected branch name, fix what succeeds, and write up the results along with pipeline times before and after tuning.
