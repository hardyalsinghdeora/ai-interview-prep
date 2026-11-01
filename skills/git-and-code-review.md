# Git & Code Review

Git and code review show how an engineer works with other people: how safely changes reach production, how quickly mistakes are recovered, and how standards are raised without slowing a team down. Interviewers ask because these habits are hard to fake and show up within the first week on the job. Senior and Staff/Principal backend roles ask most, often inside leadership or live review rounds, and Forward Deployed Engineers need the same discipline when maintaining customer-specific branches and releases.

> Related: [Docker & CI/CD](docker-and-ci-cd.md) · [Technical leadership & behavioral](technical-leadership-and-behavioral.md) · [Debugging & troubleshooting](../topics/12-debugging-troubleshooting/README.md) · [Cloud, DevOps & Integration](../topics/08-cloud-devops-integration/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] What are a commit, a tree, a blob and a ref, and what is a branch actually?
- [ ] Merge vs. rebase: what does each do to history, and when do you choose each?
- [ ] Why should you not rebase a branch others have based work on, and what is the safe way to force-push?
- [ ] `git reset` soft vs. mixed vs. hard vs. `git revert` — what does each change, and which is safe on a shared branch?
- [ ] What is the reflog, and what kind of mistakes can it recover from?
- [ ] Fast-forward vs. merge commit vs. squash merge: how does each affect history and later debugging?
- [ ] What is `git bisect` for, and what must be true of the history for it to work well?
- [ ] What is the purpose of code review, beyond finding bugs?
- [ ] What makes a good commit message and a good pull request description?

## B. Working depth
- [ ] You ran `git reset --hard` on the wrong branch and lost three commits. Walk through the recovery.
- [ ] How does `git cherry-pick` work, what are its risks, and when is it the right tool for a hotfix?
- [ ] Trunk-based development vs. GitFlow: what does each assume about release cadence and team maturity?
- [ ] What do feature flags let you do, and how do they make trunk-based development practical?
- [ ] What types of flag exist (release, experiment, ops, permission), and how do you stop stale flags becoming debt?
- [ ] How do you run a release branch: cutting it, back-porting fixes, tagging and keeping it from drifting from main?
- [ ] What branch protection and CI checks would you require on main, and what would you deliberately leave out?
- [ ] How do you keep pull requests small, and how would you split a large change into a reviewable stack?
- [ ] As a reviewer, what do you look at first, and how do you separate blocking issues from preferences?
- [ ] How do you automate away review arguments with formatters, linters and CI, and what still needs a human?
- [ ] How do you remove a secret committed to a repository, and why is rewriting history not sufficient on its own?

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Monorepo vs. polyrepo: what are the trade-offs in tooling, ownership, dependency management and atomic changes?
- [ ] How do affected-only builds and test selection work in a monorepo (Nx, Turborepo, Bazel), and what makes them hard to trust? *(verify)*
- [ ] A large repository takes minutes for `git status` and clones. What do partial clone, shallow clone, sparse-checkout and commit-graph each help with? *(verify)*
- [ ] When would you use Git LFS, and what problems does it bring along with it? *(verify)*
- [ ] Rebase-and-merge vs. squash-only policies: how do they affect bisecting, reverts and audit requirements?
- [ ] How do merge queues solve the "green PR breaks main" problem, and when are they worth the added wait? *(verify)*
- [ ] How do you decouple deployment from release, and what are the failure modes of feature flags in production?
- [ ] How would you measure review health (time to first review, PR size, change failure rate), and how can those metrics be gamed?
- [ ] How do you move a team from long-lived branches to trunk-based development without stopping delivery?
- [ ] How do code ownership rules (CODEOWNERS) help and hurt, and how do you avoid review bottlenecks on a few senior people?
- [ ] AI-generated code raises PR volume: what should change in your review standards, testing and accountability?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] A regression shipped somewhere in the last 400 commits, and there is a flaky test. How do you use `bisect` to find it?
- [ ] A bad merge reached main an hour ago and others have pushed on top of it. How do you recover without losing their work?
- [ ] You receive a 3,000-line PR that is due today. What do you do, as a reviewer and with the author?
- [ ] Two senior engineers disagree in review and the thread has reached 40 comments. How do you resolve it and prevent a repeat?
- [ ] Design a branching and release strategy for a product that ships continuously as SaaS and also supports versioned on-premises customers.
- [ ] Plan a migration of 30 repositories into one monorepo, keeping history, CI times and team ownership under control.

## E. Hands-on checks (things to be able to DO)
- [ ] Clean up a messy feature branch with an interactive rebase and resolve the conflicts, without losing work.
- [ ] Recover a deleted branch and a dropped commit using `git reflog`.
- [ ] Automate `git bisect run` with a test script to find a commit that introduced a bug.
- [ ] Set up sparse-checkout and a partial clone on a large repository, and measure the improvement.
- [ ] Write a feature flag with a safe default, a kill switch and tests for both paths, and then remove it cleanly.
- [ ] Review a real pull request and write comments that separate blockers from suggestions and explain why.

---

**Good answers show:**
- A clear mental model of Git as a graph of commits and refs, rather than a list of memorised commands.
- Calm recovery, knowing what is reversible and checking the state before making it worse.
- Choosing a branching model from release cadence, risk and team size, rather than from habit.
- Reviews that are specific, kind and prioritised, focused on correctness, design and risk over style.
- Treating release safety as a system: small changes, automated checks, flags, and fast rollback.
- Awareness of scale, including repository performance, CI cost and how ownership affects review speed.

**Prove it:** Create a sample repository with a deliberately messy history, then rewrite it into a clean set of commits, recover "lost" work through the reflog and find a planted bug with `git bisect run`. Add a CI pipeline, branch protection, CODEOWNERS and a feature-flagged change that is released separately from its deployment. Write a short team guide covering your branching model, PR size expectations and review standards, with reasons for each.
