# CI/CD Tools Cheatsheet

Covers the tools rather than the concepts: Jenkins, GitHub Actions, GitLab CI, Argo CD, Ansible, SonarQube and artefact registries.

> Related: [Docker and CI/CD skill](../skills/docker-and-ci-cd.md) · [Git and code review skill](../skills/git-and-code-review.md) · [Cloud, DevOps and integration](../topics/08-cloud-devops-integration/README.md) · [ci-cd-and-iac.md](ci-cd-and-iac.md) · [containers-and-kubernetes.md](containers-and-kubernetes.md) · [monitoring-tools.md](monitoring-tools.md) · [Jenkins, GitHub Actions & Argo CD skill](../skills/jenkins-github-actions-and-argocd.md)

## Tools at a glance

| Tool | Main job | Configuration | Runs on |
|---|---|---|---|
| Jenkins | General CI/CD server | `Jenkinsfile` in the repository | Self-hosted controller and agents |
| GitHub Actions | CI/CD inside GitHub | `.github/workflows/*.yml` | GitHub-hosted or self-hosted runners |
| GitLab CI | CI/CD inside GitLab | `.gitlab-ci.yml` | GitLab-hosted or self-managed runners |
| Argo CD | GitOps delivery to Kubernetes | Manifests in Git plus `Application` resources | Inside a Kubernetes cluster |
| Ansible | Configuration and task automation | YAML playbooks | Control node, over SSH, no agents |
| SonarQube | Static code analysis and quality gates | Scanner properties and server settings | Self-hosted or cloud |

## Jenkins

| Term | What it means |
|---|---|
| **Jenkins** | An open-source, self-hosted automation server extended by plugins; extremely flexible, but the team owns its upkeep. |
| **Pipeline** | A build and delivery process defined as code, which makes it reviewable and repeatable. |
| **`Jenkinsfile`** | The text file in a repository that defines its pipeline, versioned alongside the code. |
| **Declarative Pipeline** | The structured `pipeline { }` syntax with fixed sections; easier to read and validate, so it is the usual default. |
| **Scripted Pipeline** | The older `node { }` form written in Groovy; more flexible but harder to maintain. |
| **Controller** | The central Jenkins server, formerly called master, which schedules work and holds configuration; avoid running builds on it. |
| **Agent** | A machine or container that executes pipeline work, which spreads load and isolates build environments. |
| **Executor** | A slot on an agent that runs one task at a time, so executors set concurrency. |
| **Stage** | A named phase such as Build or Test, which structures the pipeline and its progress view. |
| **Step** | A single action inside a stage, such as `sh` to run a shell command. |
| **`post` block** | Actions that run according to outcome, such as `always` or `failure`; used for cleanup and notifications. |
| **Multibranch Pipeline** | Discovers branches and pull requests containing a `Jenkinsfile`, then creates a pipeline for each automatically. |
| **Shared library** | Reusable pipeline code in its own Git repository, which removes duplication across many `Jenkinsfile`s. |
| **Credentials binding** | Exposes stored secrets to steps as masked variables, keeping them out of source code. |
| **JCasC** (Jenkins Configuration as Code) | Defines Jenkins server settings in YAML, so a controller can be rebuilt consistently. |

```groovy
pipeline {
    agent any
    stages {
        stage('Test') { steps { sh 'make test' } }
        stage('Deploy') {
            when { branch 'main' }
            steps { sh 'make deploy' }
        }
    }
    post { always { junit 'reports/**/*.xml' } }
}
```

## GitHub Actions

| Term | What it means |
|---|---|
| **Workflow** | An automated process in a YAML file under `.github/workflows`, started by events. |
| **Event** | What triggers a workflow, such as `push`, `pull_request`, `schedule` or manual `workflow_dispatch`. |
| **Job** | A set of steps on one runner; jobs run in parallel unless ordered with `needs`. |
| **Step** | One item in a job, either a shell command through `run` or an action through `uses`. |
| **Action** | A reusable packaged unit of work; pin third-party actions to a full commit hash to reduce supply-chain risk. |
| **Runner** | The machine that executes a job; each GitHub-hosted runner is a fresh virtual machine. |
| **Self-hosted runner** | A machine under team control, useful for private networks or special hardware; risky on public repositories because forks can run code. |
| **Matrix** | Expands one job across combinations such as language versions and operating systems. |
| **Secrets** | Encrypted values exposed to workflows and masked in logs; normally withheld from pull requests opened from forks. |
| **`GITHUB_TOKEN`** | An automatic, short-lived token for each job; restrict it with `permissions` for least privilege. |
| **OIDC** (OpenID Connect) | Lets a workflow exchange its identity for short-lived cloud credentials, so no long-lived keys are stored. |
| **Reusable workflow** | A whole workflow called from others through `workflow_call`, which centralises standard pipelines. |
| **Composite action** | Bundles several steps into a single action, for reuse at step level rather than workflow level. |
| **Environment** | A named deployment target with its own secrets and protection rules, such as required reviewers. |
| **`pull_request_target`** | Runs in the base repository's context with secrets; dangerous if it also checks out untrusted fork code. |
| **`concurrency`** | Limits simultaneous runs in a named group, optionally cancelling older ones, which stops deployments clashing. |

```yaml
on:
  push:
    branches: [main]
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node: [20, 22]
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node }}
      - run: npm ci && npm test
```

Action and runtime versions above are examples *(verify)*.

## GitLab CI

| Term | What it means |
|---|---|
| **`.gitlab-ci.yml`** | The file in a repository's root that defines its pipeline by default. |
| **GitLab pipeline** | The stages and jobs that run for an event such as a push or merge request. |
| **GitLab stage** | A group of jobs that run in parallel; the next stage starts after the group succeeds. |
| **GitLab job** | A script that a runner executes, the basic unit of a pipeline. |
| **GitLab Runner** | The application that picks up and runs jobs, on shared or self-managed infrastructure. |
| **GitLab executor** | How a runner runs a job, such as Docker, Kubernetes or plain shell; this sets the isolation level. |
| **`rules`** | Conditions deciding whether a job runs; the preferred replacement for `only` and `except`. |
| **`needs`** | Declares job dependencies, forming a directed acyclic graph that can start jobs before their stage's turn. |
| **`include`** | Imports configuration from other files or projects, which standardises pipelines across teams. |
| **Parent-child pipeline** | A pipeline triggers separate child pipelines, which keeps large or monorepo configuration manageable. |
| **Review app** | A temporary deployment for a merge request, so changes can be seen before merging. |
| **Protected variable** | A CI/CD variable available only on protected branches or tags, which guards production secrets. |

| `artifacts` vs `cache` | `artifacts` | `cache` |
|---|---|---|
| Purpose | Pass build outputs to later jobs | Speed up repeated work |
| Reliability | Expected to be present | Best effort, may be missing |
| Downloadable afterwards | Yes | No |
| Typical content | Binaries, test reports | Dependency folders |

## Argo CD

| Term | What it means |
|---|---|
| **Argo CD** | A GitOps controller that keeps a Kubernetes cluster matching what is declared in Git. |
| **`Application`** | The custom resource linking a Git source and path to a destination cluster and namespace. |
| **`AppProject`** | Groups applications and restricts allowed repositories, clusters and resources, which provides multi-team guardrails. |
| **Sync** | Applying the desired state from Git to the cluster. |
| **Sync status** | `Synced` or `OutOfSync`, showing whether the live cluster matches Git. |
| **Health status** | Whether resources are actually working, such as `Healthy`, `Progressing` or `Degraded`; separate from sync status. |
| **Automated sync** | Applies Git changes without a manual step, so a merge can become a deployment. |
| **Self-heal** | Reverts manual changes made directly in the cluster, which keeps Git the source of truth. |
| **Prune** | Deletes cluster resources removed from Git; off by default for automated sync, as a safeguard. |
| **Sync hook** | A resource run at a point in a sync, such as `PreSync` for a database migration. |
| **Sync wave** | An annotation ordering resources within a sync, so dependencies are applied first. |
| **`ApplicationSet`** | Generates many applications from one template, which suits multiple clusters or environments. |
| **App of Apps** | A parent application whose manifests define child applications, used to bootstrap whole clusters. |
| **Argo Rollouts** | A separate controller adding canary and blue-green strategies to Kubernetes deployments. |

| Push CD vs pull GitOps | Push CD | Pull GitOps |
|---|---|---|
| Who applies changes | The CI pipeline | An agent in the cluster |
| Cluster credentials | Held by the pipeline | Stay inside the cluster |
| Drift detection | Usually none | Continuous |
| Example | Jenkins running `kubectl apply` | Argo CD |

## Ansible

| Term | What it means |
|---|---|
| **Ansible** | Agentless automation that configures machines over SSH using YAML, so nothing permanent needs installing on targets. |
| **Control node** | The machine where Ansible runs and from which tasks are sent. |
| **Managed node** | A target machine; Linux hosts typically need only SSH and Python. |
| **Inventory** | The list of managed hosts and groups, which playbooks target by name. |
| **Dynamic inventory** | Builds the host list from a source such as a cloud API, which suits changing fleets. |
| **Playbook** | A YAML file of ordered plays, and the main way to describe an automation. |
| **Play** | Maps a group of hosts to the tasks to run on them. |
| **Task** | One call to a module, run in order on every targeted host. |
| **Module** | The unit of work, such as `apt` or `copy`; most check current state before changing anything. |
| **Role** | A standard folder structure of tasks, handlers, templates and defaults, which makes automation reusable. |
| **Handler** | A task that runs only when notified of a change, typically to restart a service once. |
| **Idempotence** | Running again changes nothing already correct; `shell` and `command` tasks need extra care to achieve it. |
| **Facts** | Details gathered from each host, such as operating system or addresses, usable in conditions. |
| **Ansible Vault** | Encrypts secrets in files, so they can safely live in version control. |
| **Collection** | A distributable bundle of modules, roles and plugins, often shared through Ansible Galaxy. |
| **Check mode** | A dry run with `--check` that reports likely changes without making them. |

```yaml
- hosts: web
  become: true
  tasks:
    - name: Install nginx
      ansible.builtin.apt:
        name: nginx
        state: present
      notify: Restart nginx
  handlers:
    - name: Restart nginx
      ansible.builtin.service:
        name: nginx
        state: restarted
```

| Ansible vs Terraform | Ansible | Terraform |
|---|---|---|
| Best at | Configuring machines and running tasks | Provisioning infrastructure |
| Style | Mostly procedural, ordered tasks | Declarative desired state |
| State | No state file | Tracks resources in a state file |
| Typical pairing | Configures what Terraform creates | Creates what Ansible configures |

## SonarQube

| Term | What it means |
|---|---|
| **SonarQube** | A static analysis platform reporting bugs, vulnerabilities and maintainability issues; product names and editions change *(verify)*. |
| **SonarScanner** | The client that runs analysis in CI and sends results to the server. |
| **Quality gate** | Pass or fail conditions on analysis results, which can stop a pipeline from merging poor code. |
| **Quality profile** | The set of rules applied to a language, which lets teams tune what gets flagged. |
| **Code smell** | A maintainability problem that is not a bug but makes future changes harder. |
| **Security hotspot** | Security-sensitive code needing human review; unlike a vulnerability, it is not a confirmed problem. |
| **New code period** | Defines which code counts as new, so gates can focus on recent changes rather than legacy debt. |
| **Coverage import** | SonarQube does not run tests; it reads reports produced by tools such as JaCoCo. |

## Artefact registries

| Term | What it means |
|---|---|
| **Artefact registry** | Versioned storage for build outputs, so the same tested package moves through environments. |
| **JFrog Artifactory** | A commercial universal repository manager supporting many package formats, with some free editions *(verify)*. |
| **Sonatype Nexus Repository** | A repository manager often used for Maven, npm and containers, with free and paid editions *(verify)*. |
| **GitHub Packages** | Package and container hosting tied to GitHub repositories and permissions. |
| **Docker Hub** | The default public container registry; pull rate limits can break builds, so mirrors are common *(verify)*. |
| **ECR** (Elastic Container Registry) | AWS's private container registry, with IAM-based access and image scanning. |
| **Harbor** | An open-source, self-hosted container registry offering access control, scanning and replication. |
| **Proxy repository** | Caches an upstream source such as Maven Central, which improves speed and survives upstream outages. |
| **Image digest** | A `sha256` content hash that identifies an exact image, unlike a tag, which can move. |
| **Immutable tag** | A registry setting preventing a tag from being overwritten, which keeps deployments traceable. |
| **Snapshot vs release** | Snapshots are mutable development builds, while releases are fixed versions; a Maven distinction with wide relevance. |
| **Artefact promotion** | Moving one built artefact from test to production repositories rather than rebuilding, so what shipped is what was tested. |
