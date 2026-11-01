# AI-assisted engineering

Coding assistants are now part of everyday engineering work, so interviewers have moved on from "do you use them?" to how you keep quality, security and understanding intact when code arrives faster than it can be reviewed. Staff engineers, tech leads and Engineering Managers are asked most, because they set the review standards, the policies and the rollout, and they have to answer when leadership asks what the tools have really delivered.

> Related: [Git & code review](git-and-code-review.md) · [Technical leadership & behavioral](technical-leadership-and-behavioral.md) · [Security & compliance](security-and-compliance.md) · [LLM evaluation](llm-evaluation.md) · [Behavioral](../topics/10-behavioral/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] Inline completion vs. chat vs. an agent that edits files and runs commands: what changes in what the tool can see and do, and where does each fit?
- [ ] Compare how Copilot, Cursor and Claude Code work: where each runs, how it gets context from your repository, and what it can do by itself. *(verify)*
- [ ] What context does a coding assistant actually have when it answers, and why do context limits affect the quality of its work on a large repository?
- [ ] Which tasks do assistants speed up most in a mature production codebase, and where have they been a net loss in your experience?
- [ ] Why does code that compiles and passes its tests still need careful review? Give three concrete ways AI-generated code goes wrong.
- [ ] An assistant suggests a package or API that does not exist. How do you catch that, and why is an invented package name a supply-chain risk?
- [ ] Who is accountable when AI-generated code causes an outage, and what should a review sign-off mean when the author did not type the code?
- [ ] Before anyone uses a coding assistant on proprietary code, what would you check about data retention, training use, data location and contract terms? *(verify)*
- [ ] What belongs in a project instruction file such as `CLAUDE.md`, Cursor rules or Copilot instructions, and what should stay out? *(verify)*

## B. Working depth
- [ ] You are asking an assistant to change a large legacy service. How do you frame the task: goal, constraints, relevant files, acceptance checks and what must not be touched?
- [ ] When do you ask for a plan before any edits, and how do you keep changes small enough to review?
- [ ] How do you manage context across a long task: when do you start a fresh session, what do you pull in deliberately, and what signs show the session has degraded?
- [ ] What do you look for when reviewing AI-generated code that you would not check as closely in human-written code (invented abstractions, duplicated helpers, broad exception handling, unrequested changes)?
- [ ] The assistant wrote both the code and the tests. How do you find out whether the tests mean anything? Consider tests that mirror the implementation, mutation testing and writing tests first.
- [ ] How do you use an assistant to debug a production issue without it patching the symptom? What evidence do you give it, and how do you check its theory?
- [ ] Which security flaws do assistants commonly introduce (injection, missing authorisation checks, insecure defaults, weak crypto), and which automated checks would catch them?
- [ ] An agent reads a README, an issue or a tool response that contains hidden instructions. How does prompt injection work here, and what limits the damage? *(verify)*
- [ ] What should an agent be allowed to do without asking? Set the boundaries for shell commands, network access, credentials, production systems and pushing code. *(verify)*
- [ ] How should engineers keep secrets, customer data and regulated data out of prompts, and which technical controls back up the written rule? *(verify)*
- [ ] What are the licence and intellectual property questions around generated code, and how would you decide with legal what the team should do about them? *(verify)*
- [ ] How do you use assistants well for a large refactor or a test backfill while keeping the change verifiable, and when do codemods or scripts serve you better?

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Why are lines of code, suggestion acceptance rate and PR count misleading? What would you measure instead, using DORA or SPACE-style signals alongside quality measures?
- [ ] Published studies disagree on whether these tools speed developers up, and self-reported gains can differ from measured ones. How would you design a fair internal measurement? *(verify)*
- [ ] Code output has risen, but reviewers are now the bottleneck. What do you change in PR size, review ownership, automated checks and expectations of authors?
- [ ] Generated code can pile up faster than anyone understands it. How do you detect comprehension debt, and how do you keep incident response and onboarding from suffering?
- [ ] How would you prevent quality drift from duplication and churn, using lint rules, architecture tests and static analysis rather than relying on reviewers to notice?
- [ ] Write the outline of a team policy on AI tools: approved tools, data classes, disclosure, review rules and restricted areas. How do you keep it short enough to be followed?
- [ ] How would a tech lead or EM roll these tools out to 200 engineers? Cover the pilot group, training, shared practices, guardrails, success criteria and what makes you stop.
- [ ] How do you forecast and control spend on these tools as usage shifts from seats to consumption, and who should see the numbers? *(verify)*
- [ ] What changes in a regulated environment such as SOC 2, GDPR or financial services? Consider audit trails, data residency, private deployment options and vendor review. *(verify)*
- [ ] How do you make sure junior engineers still build judgement and debugging skill, and how would you change your team's hiring loop now that assistants can complete most take-home exercises?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] A senior engineer opens a 2,000-line AI-generated PR. Tests pass, but nobody on the team can explain parts of it, including the author. What do you do?
- [ ] An agent ran a destructive command against a shared staging database. Run the review: what failed, and what guardrails go in place?
- [ ] An outage is traced to a retry loop written by an assistant, and the author says "the AI wrote it". How do you run the postmortem, and what changes afterwards?
- [ ] Leadership asks you to report a 30% productivity gain and suggests reducing headcount accordingly. How do you respond as an EM, and what evidence do you bring?
- [ ] Half of your team refuses to use these tools, while the other half ships code they do not fully understand. How do you bring the team to a shared standard?
- [ ] An engineer pasted customer data and a production credential into an unapproved assistant. Walk through the first day of your response and the lasting changes. *(verify)*
- [ ] You need to choose between two coding assistants for your organisation, knowing both products change often. Design an evaluation on your own repositories, with tasks, scoring, reviewers and security checks, and say what should stay portable if you switch later.

## E. Hands-on checks (things to be able to DO)
- [ ] Fix a real issue in an unfamiliar open-source repository using an assistant, and keep a log of your prompts, what you rejected and why.
- [ ] Write a project instruction file for a real codebase, run the same task with and without it, and compare the diffs. *(verify)*
- [ ] Review an AI-generated PR that has seeded defects (missing authorisation, swallowed errors, a fake dependency, a test that cannot fail), and explain how you found each one.
- [ ] Add characterisation tests around a legacy function, let an assistant refactor it, and use mutation testing to show whether the tests would catch a regression.
- [ ] Run an agent with sandboxed, restricted permissions, plant a prompt injection in a repository file, and show what it tried and what was blocked. *(verify)*
- [ ] Build a small report from repository data showing cycle time, review time, rework rate and change failure rate for a period before and after adopting a tool.

---

**Good answers show:**
- Specific examples of where the tools helped and where they failed, rather than enthusiasm or blanket scepticism.
- Clear ownership: the engineer who merges the code understands it and answers for it, whoever or whatever drafted it.
- Deliberate context and task framing, with small diffs and verification built into how the work is done.
- Security thinking that covers both generated code and the tool itself: secrets, prompt injection, agent permissions and data handling.
- Measurement that looks past output, combining delivery, quality and developer experience data with an honest view of the limits of each.
- Leadership that treats rollout as change management: a pilot, training, a short policy, a review process that scales and room for dissent.

**Prove it:** Pick a small production-style service and add one meaningful feature with a coding assistant under written rules: an instruction file, restricted permissions, tests first and a review checklist. Repeat a comparable feature without the tool, then compare time taken, review findings, defects and mutation score. Turn the results into a one-page team policy and rollout proposal that a lead or EM could use, stating clearly what the small sample cannot prove.
