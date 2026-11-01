# Tech Lead

A senior engineer who is accountable for how a team builds and delivers software: turning goals into a technical plan, making or guiding the hard design decisions, keeping quality and reliability healthy, and raising the output of the engineers around them, while still writing code.

## The role

**Day to day.** Breaking product goals into milestones and work the team can pick up; writing and reviewing design documents; reviewing code and pairing on the hardest parts; tracking risks and dependencies across teams; running technical discussions until they reach a decision; working out plans and dates with the product manager and engineering manager; mentoring engineers; leading incident response and following up on what it reveals; and writing code, usually on enabling or high-risk work rather than on everything.

**Common titles.** Tech Lead (TL), Technical Lead, Team Lead, Lead Engineer, Senior Software Engineer (with a lead remit), and Tech Lead Manager (TLM), which adds people management. At many companies "tech lead" is a responsibility held by a senior or staff engineer, not a job title, so the same work may appear under different names.

**Who hires for it.** Product companies, SaaS vendors, banks and fintechs, consulting and services firms, and growing startups. Startups may expect a "lead" to manage people too, while services firms may expect more client-facing delivery work. Read the job description for the balance.

**How it differs from neighbouring roles.**

| | Tech Lead | [Backend SDE](../18-software-development-engineer/README.md) | Staff Engineer | [Engineering Manager](../20-engineering-manager/README.md) |
|---|---|---|---|---|
| Main accountability | One team's technical direction and delivery | Own features and services | Technical direction across several teams | People, team health and results |
| Measured by | Team outcomes and system health | Own output and quality | Cross-team technical results | Team results, retention and growth |
| People management | Usually none: mentors, without formal authority | None | None | Hiring, reviews, pay, performance |
| Coding share | Roughly a third to a half | Most of the week | Varies; often prototypes and critical changes | Little or none |
| Main tools | Design docs, reviews, plans, pairing | Code | Strategy docs, cross-team influence | 1:1s, hiring, planning |

Titles and split of duties vary between companies. Asking "who owns what here?" early in the loop is a good sign, and interviewers usually welcome it. See the [role comparison](../../cheatsheets/engineering-management.md#tech-lead-vs-em-vs-staff-engineer) in the cheatsheet.

**Who it suits.** Strong engineers who enjoy helping others succeed, are comfortable making decisions with incomplete information, and will trade some personal coding time for a larger effect through the team.

**Skills mix.**

| Skill | Weight |
|---|---|
| System design and technical judgement | High |
| Planning, estimation and delivery | High |
| Influence and communication | High |
| Mentoring and team leadership | Med to High |
| Hands-on coding | Med (High in some loops) |
| Reliability and operations | Med to High |
| People management | Low to Med |

**Career paths.** Staff or Principal Engineer on the individual contributor track, [Engineering Manager](../20-engineering-manager/README.md) on the management track, or Software Architect. Many people move between tracks more than once, so the choice is rarely final.

## Typical interview loop

Typical reported shape; varies by company and level. Some companies interview tech leads through their senior engineer loop and add a leadership round.

| Round | What it tests | Prepare with |
|---|---|---|
| Recruiter screen | Background, team size led, leadership or individual contributor preference | [01-recruiter-and-motivation](../../topics/01-recruiter-and-motivation/ANSWERS.md) |
| Hiring manager screen | Scope of past leadership, how you work with product and managers | This file + [Hiring-manager round topics](../../topics/10-behavioral/ANSWERS.md#hiring-manager-round-topics) |
| Coding (one or two rounds) | Practical coding and code quality, usually less puzzle-heavy than junior loops, though large companies may still use DS&A problems | [02-coding](../../topics/02-coding/ANSWERS.md), [SDE guide](../18-software-development-engineer/README.md#coding-round-1) |
| System design | Requirements, trade-offs, failure modes, migration and team-level decisions | This file + [03-system-design](../../topics/03-system-design/ANSWERS.md) |
| Past-project deep dive | Technical depth, your decisions, how you led others through them | This file |
| Technical leadership or delivery | Planning, estimation, risk, tech debt, incidents, working across teams | This file |
| Behavioral / people | Mentoring, conflict, influence without authority, failure | This file + [10-behavioral](../../topics/10-behavioral/ANSWERS.md) |
| Cross-functional (product manager or engineering manager) | Prioritisation, pushing back, explaining technical risk | This file |

## Common rounds — use the shared sections

The shared sections were written with customer-facing roles in mind. The technical and behavioral content applies directly; replace "customer" with "stakeholder" where that fits, and skip questions about travel and on-site work.

- [03 System design](../../topics/03-system-design/ANSWERS.md): [classic prompts](../../topics/03-system-design/ANSWERS.md#c-classic-system-design-still-asked) and [follow-up questions](../../topics/03-system-design/ANSWERS.md#d-follow-up-questions-to-expect).
- [04 Decomposition](../../topics/04-decomposition/ANSWERS.md): useful practice for breaking vague goals into a plan. See [follow-ups and push-backs](../../topics/04-decomposition/ANSWERS.md#f-typical-interviewer-follow-ups--push-backs).
- [10 Behavioral](../../topics/10-behavioral/ANSWERS.md): [ownership](../../topics/10-behavioral/ANSWERS.md#ownership--impact), [ambiguity](../../topics/10-behavioral/ANSWERS.md#ambiguity--bias-for-action), [conflict](../../topics/10-behavioral/ANSWERS.md#conflict--collaboration), [failure](../../topics/10-behavioral/ANSWERS.md#failure--learning) and [prioritisation](../../topics/10-behavioral/ANSWERS.md#prioritisation--pressure).
- [12 Debugging](../../topics/12-debugging-troubleshooting/ANSWERS.md): [production troubleshooting scenarios](../../topics/12-debugging-troubleshooting/ANSWERS.md#b-production-troubleshooting-scenarios) and [debugging process questions](../../topics/12-debugging-troubleshooting/ANSWERS.md#d-debugging-process-questions).
- [13 CS fundamentals](../../topics/13-cs-fundamentals/ANSWERS.md): [distributed systems](../../topics/13-cs-fundamentals/ANSWERS.md#distributed-systems) and [software engineering practices](../../topics/13-cs-fundamentals/ANSWERS.md#software-engineering-practices).
- [14 Questions to ask](../../topics/14-questions-to-ask/ANSWERS.md): [about the team and product](../../topics/14-questions-to-ask/ANSWERS.md#about-the-team--product) and [to the hiring manager](../../topics/14-questions-to-ask/ANSWERS.md#to-the-hiring-manager).
- [SDE guide](../18-software-development-engineer/README.md): for the API, database, caching and messaging questions that also appear in tech lead loops.

## Question lists and cheatsheets

Use the [Tech Lead row of the skills table](../../skills/README.md#what-to-prepare-by-level) to choose an order, then add the tools named in the job description.

| Area | Question lists | Cheatsheets |
|---|---|---|
| Leadership and delivery | [Tech lead & agile delivery](../../skills/tech-lead-and-agile-delivery.md), [Technical leadership & behavioral](../../skills/technical-leadership-and-behavioral.md), [Engineering management](../../skills/engineering-management.md) (for role boundaries) | [Engineering management](../../cheatsheets/engineering-management.md), [Interview frameworks](../../cheatsheets/interview-frameworks.md) |
| Design and architecture | [System design](../../skills/system-design.md), [Large-scale distributed systems](../../skills/large-scale-distributed-systems.md), [API design & microservices](../../skills/api-design-and-microservices.md), [Data migration & legacy modernisation](../../skills/data-migration-and-legacy-modernisation.md), [Multi-tenant SaaS](../../skills/multi-tenant-saas.md) | [System design](../../cheatsheets/system-design.md), [Distributed systems](../../cheatsheets/distributed-systems.md), [API design](../../cheatsheets/api-design.md) |
| Quality | [Git & code review](../../skills/git-and-code-review.md), [Testing & quality](../../skills/testing-and-quality.md), [OOP, design patterns & LLD](../../skills/oop-design-patterns-and-lld.md), [AI-assisted engineering](../../skills/ai-assisted-engineering.md) | [Testing](../../cheatsheets/testing.md), [OOP & design patterns](../../cheatsheets/oop-and-design-patterns.md) |
| Reliability and delivery tooling | [Observability & reliability](../../skills/observability-and-reliability.md), [Docker & CI/CD](../../skills/docker-and-ci-cd.md), [Security & compliance](../../skills/security-and-compliance.md) | [Observability & reliability](../../cheatsheets/observability-and-reliability.md), [CI/CD & infrastructure as code](../../cheatsheets/ci-cd-and-iac.md), [Security & auth](../../cheatsheets/security-and-auth.md) |
| Platform tools, if in the job description | [Kubernetes](../../skills/kubernetes.md), [Kafka & event streaming](../../skills/kafka-and-event-streaming.md), [RabbitMQ & other brokers](../../skills/rabbitmq-and-other-brokers.md), [Elasticsearch & OpenSearch](../../skills/elasticsearch-and-opensearch.md), [AWS](../../skills/aws.md) | [Containers & Kubernetes](../../cheatsheets/containers-and-kubernetes.md), [Messaging & streaming](../../cheatsheets/messaging-and-streaming.md), [Elastic stack](../../cheatsheets/elastic-stack.md), [AWS](../../cheatsheets/aws.md) |

## Role-specific questions

### The role and its boundaries
- [ ] How do the tech lead, engineering manager and staff engineer roles differ, and where do they overlap?
- [ ] How do you know you are doing a good job as a tech lead?
- [ ] How do you stay hands-on without becoming a bottleneck?
- [ ] You join an existing team as tech lead. What do you do in your first 90 days?

### Planning, estimation and delivery
- [ ] How do you turn a vague goal into a plan a team can deliver?
- [ ] Product asks for a firm date on a six-month migration with many unknowns. How do you respond?
- [ ] How can past throughput be used to forecast a delivery range?
- [ ] Your team regularly carries 30% of its sprint into the next one. What do you do?
- [ ] When do you choose Scrum, Kanban or neither?
- [ ] Plan a feature that spans web, mobile and two backend teams, with a fixed external launch date.
- [ ] Your project is slipping. How and when do you tell people?

### Technical decisions
- [ ] When do you write an RFC and when an ADR, and how do you stop an RFC from stalling?
- [ ] Two senior engineers strongly disagree about an architecture. How do you resolve it?
- [ ] How do you run a design review that improves the design rather than turning into a debate?
- [ ] Monolith, modular monolith or microservices: how do you choose?
- [ ] When would you rewrite a component rather than refactor it?
- [ ] How do you decide whether to build or buy?

### System design at tech lead level
- [ ] How does a tech lead's system design interview differ from a mid-level engineer's?
- [ ] Move a billing module out of a monolith into its own service, with no downtime and no billing errors.
- [ ] Design a shared notification platform that several product teams will use.

### Quality and tech debt
- [ ] What counts as tech debt, and how do you prioritise it against features?
- [ ] Which quality standards do you automate, and which do you leave to reviewers?
- [ ] Pull requests wait three days for review on your team. How do you fix that?
- [ ] How do you introduce a new standard into a large legacy codebase?
- [ ] Your test suite is slow and flaky, and the team has stopped trusting it. What do you do?
- [ ] What should a team's policy on AI coding tools cover?

### Reliability and operations
- [ ] How do you introduce SLOs and error budgets to a team?
- [ ] How do you run a major incident and the review that follows?
- [ ] A team spends half its time on production support and keeps missing sprint goals. How do you redesign its work?
- [ ] On release day, one of five merged features has a serious defect. How do you decide what to do?
- [ ] Which delivery metrics do you track, and how do you avoid misusing them?

### Leading people without authority
- [ ] How do you mentor a junior engineer differently from a senior one, and how do you delegate work you could do faster yourself?
- [ ] An engineer on your team is underperforming, but you are not their manager. What do you do?
- [ ] One engineer's pull requests keep failing review, while another approves everything within minutes. How do you handle each of them?
- [ ] Your most experienced engineer resigns two weeks before a major release. What do you do?
- [ ] How do you push back when product or a senior leader asks for something that you think is wrong?

### Behavioral
- [ ] Tell me about a time you influenced a decision without having authority.
- [ ] Tell me about a project you led through heavy uncertainty.
- [ ] Tell me about a technical decision you got wrong.
- [ ] Tell me about a failing project you took over.

## Model answers

### The role and its boundaries

#### How do the tech lead, engineering manager and staff engineer roles differ, and where do they overlap?

- **Tech lead:** owns technical direction and delivery for one team. Decides, or guides decisions, about how the team builds things.
- **Engineering manager (EM):** owns people and team results: hiring, feedback, performance, career growth, capacity and team health.
- **Staff engineer:** owns technical problems and direction across several teams, often without being attached to one team's day-to-day work.

They overlap on delivery, mentoring and hiring. A useful test is to ask who answers each question:

| Question | Usually answered by |
|---|---|
| What should we build and why? | Product manager, with engineering input |
| How should we build it? | Tech lead |
| Who works on it, and are they growing? | EM, with tech lead input |
| Will it be delivered, and what are the risks? | Tech lead and EM together |
| How does this fit across teams in the long term? | Staff engineer |

Agree the split explicitly at the start, because gaps cause more harm than overlaps. On small teams one person may hold several of these roles. If so, say which work you would drop first and how you would know.

#### How do you know you are doing a good job as a tech lead?

Look at the team, not your own output:

- **Delivery:** the team hits its commitments or warns early; surprises become rarer.
- **Design quality:** decisions are written down, and few have to be reversed.
- **System health:** incidents, change failure rate and time to recover are stable or improving.
- **Independence:** engineers make routine decisions without you, and the team copes when you are on holiday.
- **Growth:** engineers take on larger scope, and someone could step into your role.
- **Trust:** product and the EM get straight answers about risk, and engineers raise concerns early.

Warning signs: every review waits on you, you write the hard parts every time, or you find out about problems last.

#### How do you stay hands-on without becoming a bottleneck?

- **Pick your code carefully:** spikes, risky integrations, tooling that helps everyone, and small bug fixes that keep you close to the codebase. Avoid being the only owner of critical, deadline-bound features.
- **Protect focus time:** for example, two blocks a week without meetings.
- **Pair rather than take over:** your context spreads, and the other engineer owns the result.
- **Write it down:** principles and examples in design docs let others decide without asking you.
- **Delegate decisions:** define the types of decision that do not need you, usually those that are cheap to reverse.
- **Watch your own queue:** if pull requests or questions wait on you, treat that as a bug and fix the process.
- **Stop some work as the team grows:** review every pull request at four people, but not at ten.

Use "can this happen without me?" as a regular test. If you cannot take a week's leave safely, the team has a risk that you need to fix.

#### You join an existing team as tech lead. What do you do in your first 90 days?

**Days 1–30, listen and learn:**
- Meet each engineer, the product manager and the EM. Ask what works, what hurts and what they would change.
- Read recent design documents, incident reviews and the roadmap.
- Ship something small, join on-call as a shadow and go through the deployment process yourself.
- Ask the EM and your manager what success looks like and which decisions are yours.

**Days 31–60, find the priorities:**
- Share what you have heard, including patterns, and test it with the team.
- Fix one or two things that people find painful, such as a flaky pipeline. This builds trust and costs little.
- Start leading a design or a project.

**Days 61–90, set direction:**
- Agree a short technical roadmap with the EM and product.
- Establish the working agreements the team lacks, such as design reviews or an on-call handover.
- Set a few measures to show progress.

Respect existing decisions until you understand why they were made. Avoid proposing a rewrite in your first month.

### Planning, estimation and delivery

#### How do you turn a vague goal into a plan a team can deliver?

1. **Clarify the outcome:** what changes for whom, how will it be measured, what date or constraint is real, and what is out of scope?
2. **Identify the unknowns,** and run short, time-boxed spikes on the riskiest ones first.
3. **Sketch the design** at enough depth to reveal the main components, dependencies and data changes.
4. **Slice vertically:** milestones that each deliver something working end to end, rather than a database layer, then an API layer, then a UI.
5. **Order by risk and value:** tackle the riskiest integration early, so that bad news comes while there is still time to act.
6. **Map dependencies and owners,** and agree them with other teams early.
7. **Estimate as a range,** with assumptions stated.
8. **Define how you will track it:** a demo at each milestone, a short weekly update and a risk list.
9. **Decide what you would cut** before you have to.

An example first milestone is a thin version running in production for internal users. It proves the deployment path, the data flow and the integrations. See also [04 Decomposition](../../topics/04-decomposition/ANSWERS.md).

#### Product asks for a firm date on a six-month migration with many unknowns. How do you respond?

Do not refuse, and do not give a single number too early.

1. **Understand the need:** why is a date needed? Contract, marketing, budget, or a dependency? This shows how much flexibility exists and which partial results might satisfy it.
2. **Offer a dated learning plan:** "In three weeks, after these two spikes, the range will be much narrower." Time-box the spikes.
3. **Give a range with assumptions:** "Likely between five and eight months. The main drivers are the data volume and whether the old reports must move too."
4. **Narrow it at each milestone:** a real cone of uncertainty, with a date each time for the next update.
5. **Find what can move:** scope, sequence, team size or the method of cutover. Migrate the highest-value parts first so that some benefit arrives early.
6. **State the risks and triggers:** for example, "If the data audit finds more than 5% bad rows, add four weeks."
7. **If a commitment is required,** commit to a date at a high confidence level, and say what it costs in scope or spare capacity.

Update as you learn, and report bad news when you find it, not at the deadline.

#### How can past throughput be used to forecast a delivery range?

Instead of estimating each ticket, use how many items the team actually finished per week. A Monte Carlo simulation samples from that history to produce a range:

```python
import random

history = [3, 5, 2, 4, 6, 3, 1, 4]   # items finished per week, recent weeks
remaining = 40                       # items left in the backlog


def weeks_to_finish():
    done, weeks = 0, 0
    while done < remaining:
        done += random.choice(history)
        weeks += 1
    return weeks


runs = sorted(weeks_to_finish() for _ in range(10_000))
print("50% likely by week", runs[5_000])
print("85% likely by week", runs[8_500])
```

Report the result as: "50% likely by week 12, 85% likely by week 13" (exact figures can vary slightly by run).

Conditions and limits:
- History must resemble future work: same team and similar item size. Items need not be identical, but the mix should be similar.
- Backlogs grow, so include an allowance for new scope, or count it and rerun.
- It does not help with completely new work. Use spikes there first.
- Rerun it weekly, because the forecast improves as the work proceeds.

Its advantages are that it needs no estimation meetings, and that it expresses uncertainty honestly.

#### Your team regularly carries 30% of its sprint into the next one. What do you do?

Find the cause before changing the process. Look at the data for several sprints and ask the team.

| Possible cause | Evidence | Response |
|---|---|---|
| Too much committed | Spillover persists even in calm sprints | Plan from actual throughput, leaving spare capacity |
| Stories too large | Spillover items are large, and finish late in the sprint | Split work into vertical slices of a few days at most |
| Unplanned work | Interrupts and additions after planning | Track it, reserve capacity or set up a rota for interrupts |
| Waiting | Tickets sit in review or wait on other teams | Limit work in progress, and flag dependencies at planning |
| Unclear requirements | Rework, and questions during the sprint | Better refinement and a clear Definition of Ready |
| Too many items in flight | Everyone has several tickets open at once | Work-in-progress limits, and finishing before starting |
| Late testing | Items are "done" apart from testing | Test as part of the work, not at the end |

Also ask whether spillover is really a problem. If stakeholders get what they need and the team is calm, a fixed sprint boundary might be the wrong model, and Kanban could suit the team better. Change one thing at a time, and check the effect after two or three sprints.

#### When do you choose Scrum, Kanban or neither?

- **Scrum:** suits product work that can be planned in short cycles, when a team benefits from a regular rhythm of planning, review and retrospective.
- **Kanban:** suits a continuous flow of varied work, with interrupts and changing priorities, such as platform, support or operations teams. It limits work in progress and focuses on cycle time.
- **Lighter or mixed approaches:** a small, experienced team may need only a weekly planning meeting and a board. Many teams combine the two, keeping a regular review and retrospective with continuous flow.

Keep what each practice is for, and drop rituals that no longer serve a purpose. Whatever the method, the team needs visible work, clear priorities, a limit on parallel work and a regular chance to improve. Scrum's current definition is in the [Scrum Guide](https://scrumguides.org/) *(verify)*. See [Scrum vs Kanban](../../cheatsheets/engineering-management.md#scrum-vs-kanban).

#### Plan a feature that spans web, mobile and two backend teams, with a fixed external launch date.

Work back from the date, and assume something will slip.

1. **Single owner:** name one technical owner across the teams, with a named contact in each team.
2. **Fix the interface first:** agree API contracts early, and publish mocks so that the clients can start while the backend is under construction. Use contract tests.
3. **Build backwards from launch:**
   - Launch date, minus an app store review allowance for mobile *(verify current review times)*.
   - Code freeze and a period for final testing.
   - End-to-end test environment working several weeks before that.
4. **Ship early behind flags:** backend first, then clients, all turned off. Backends must stay compatible with old mobile versions, which can stay in use for a long time.
5. **Integrate early:** run the thinnest possible flow end to end across all teams in the first few weeks, because that is where the hidden problems are.
6. **Define scope tiers:** must have, should have and can follow later, agreed with product in advance.
7. **Track risk:** a shared risk list, a short weekly sync and one dashboard or summary for everyone.
8. **Launch plan:** gradual rollout, a go or no-go meeting with clear criteria, rollback steps, on-call cover and support briefing.

Since the date is fixed, scope is what gives way. Say this at the start, not at the end.

#### Your project is slipping. How and when do you tell people?

Tell people as soon as you have reason to believe the date is at risk, even if you are not yet certain. Early warnings leave options open; late ones only deliver bad news.

1. **Understand it first, but quickly:** spend hours or a day or two, not weeks. How large is the slip, what caused it, and how confident are you?
2. **Tell the EM and product first,** then agree who else needs to know.
3. **Bring options:**
   - Cut scope: what gets dropped, and what is the effect?
   - Move the date: to when?
   - Add people: who, and is there still enough time for this to help?
   - Reduce quality or take on risk: spell out what the risk is.
4. **Give a recommendation,** and let the people who own the trade-off decide.
5. **Follow up:** update the plan, set a date for the next update, and record why it slipped.

A useful status message has this shape: status, what changed, impact, options, recommendation, decision needed by. See also [Your team has missed three deadlines](../../skills/technical-leadership-and-behavioral.md#d-scenario--design-questions-whiteboard--take-home-style) in the leadership question list.

### Technical decisions

#### When do you write an RFC and when an ADR, and how do you stop an RFC from stalling?

- **RFC (request for comments):** a proposal written before a decision, to gather input. Use it for changes that are costly to reverse, affect several teams or set a standard.
- **ADR (architecture decision record):** a short record of a decision made, including context, options and consequences. It explains the reasoning later on.

They work together: an RFC leads to a decision, and an ADR records it. Do not write either one for a change that is small and easily reversed. See [RFC vs ADR](../../cheatsheets/engineering-management.md#rfc-vs-adr).

To stop an RFC from stalling:
- **Name a decision-maker and a deadline** at the top: "Comments close on Friday; the tech lead decides by Tuesday."
- **Say what you want from readers,** and name required reviewers.
- **Keep it short,** and include real alternatives, including doing nothing.
- **Talk to the people most affected first,** so that the document holds no surprises.
- **Move long comment threads to a meeting,** then write down the result.
- **Separate blocking concerns from preferences.**
- **Treat silence as agreement** once the deadline has passed.

#### Two senior engineers strongly disagree about an architecture. How do you resolve it?

1. **Listen to each one separately first,** then restate their positions until each agrees you have them right.
2. **Find the real disagreement:** different facts, different predictions, or different priorities? Each needs a different response.
3. **Agree the decision criteria** before comparing options, such as cost, delivery time, operating load, or how easily it can be changed later.
4. **Replace opinion with evidence:** a short prototype, a load test or a written comparison.
5. **Check how reversible it is:** if the decision is cheap to change, choose quickly, and try one option for a set period.
6. **Decide:** if they still disagree, you decide (or escalate if the decision is above your scope). Explain your reasoning, and acknowledge what is lost.
7. **Commit together:** record it in an ADR, with the conditions under which you would revisit it.
8. **Look after both people:** credit the person whose option was not chosen, and give them a real part in the outcome.

If the disagreement is personal rather than technical, work with the EM.

#### How do you run a design review that improves the design rather than turning into a debate?

**Before:**
- Share a short written design in advance, covering the problem, requirements, options, the proposal, risks and rollout.
- Invite only the people who can contribute or are affected, including someone from operations or security when relevant.
- State the review's purpose: approval, or feedback on an early idea?

**During:**
- Begin with silent reading or a brief summary, not a full walkthrough.
- Start with requirements. Many arguments about design are really disagreements about requirements.
- Focus on failure modes, data, migration and operations.
- Critique the design and not the author, using questions such as "What happens if this queue backs up?"
- Move style preferences and side topics elsewhere.

**After:**
- Record decisions, open questions and owners.
- Update the document, and add an ADR if it is warranted.

Make this routine, including reviewing your own designs, so that people see reviews as help rather than judgement. Encourage junior engineers to ask questions and, when they are ready, to lead reviews.

#### Monolith, modular monolith or microservices: how do you choose?

| | Monolith | Modular monolith | Microservices |
|---|---|---|---|
| Deployment | One unit | One unit | Many independent units |
| Boundaries | Often weak | Enforced in code | Enforced by the network |
| Data | Shared | Owned per module, often in one database | Owned per service |
| Transactions | Simple | Simple | Distributed: sagas and eventual consistency |
| Team fit | Small teams | Small to medium teams | Many teams needing independent releases |
| Operations | Simple | Simple | Heavy: tracing, service discovery, platform team |

Start with a well-structured monolith or a modular monolith unless there is a specific reason not to. Good reasons to extract a service include:
- Teams are blocked by each other's releases.
- One component has very different scaling, reliability or security needs.
- A different technology is really needed for one part.

Weak reasons include fashion, or the belief that microservices automatically give clean code. A poorly structured monolith split across a network becomes a distributed monolith, which has the problems of both approaches.

Team structure matters too. Conway's law says systems tend to mirror the communication structure of the organisation that builds them. If boundaries are unclear, get them right inside the monolith first.

#### When would you rewrite a component rather than refactor it?

Default to incremental change. Rewrites often take longer than expected, because the old code contains many unwritten rules, and the business keeps changing while the rewrite is under way.

**A rewrite may be justified when:**
- The technology is truly end of life, or insecure, or nobody can be hired to work on it.
- The architecture cannot support an important requirement, and you have shown this.
- The component is small and well understood, with clear tests or contracts.
- The cost of change is so high that you can show incremental work will not pay off.

**How to contain it:**
- **Strangler fig:** route traffic through a facade, and move one piece at a time to the new code.
- **Define the scope:** feature parity for agreed behaviour only, with new features held back, and a written list of what is not being moved.
- **Characterisation tests:** capture existing behaviour before changing it.
- **Shadow traffic:** run both versions on real requests and compare outputs.
- **Deliver value early,** every few weeks, with a checkpoint where the work can stop.
- **Set a removal date** for the old system. Otherwise you will be running two.

#### How do you decide whether to build or buy?

Ask first: is this a part of the product that sets us apart, or something every company needs? Buy or adopt what does not differentiate you, and build what does.

Compare the **total cost over several years**, not the initial price:

| Factor | Build | Buy or adopt |
|---|---|---|
| Up-front cost | Engineering time | Licence and integration |
| Ongoing cost | Maintenance, on-call, security, upgrades | Subscription, usage growth, renewal price rises |
| Fit | Exact | Partial: you adapt to it |
| Time to value | Slower | Faster |
| Risk | Key people leaving, under-investment | Vendor lock-in, vendor failure, outages outside your control |
| Security and compliance | You own it all | Vendor review, data handling, certifications |

Also consider open source that you run yourself, which is a third option with low licence cost but real operating cost.

Reduce risk by putting a thin interface around the vendor, checking data export, testing the most important use case in a short pilot, and defining your exit plan. Revisit the decision when circumstances change: a bought tool can become expensive at scale, and a custom tool can stop being worth maintaining.

### System design at tech lead level

#### How does a tech lead's system design interview differ from a mid-level engineer's?

The method is the same, but expect to be judged on more than the diagram:

- **You drive:** you set the scope, choose where to go deep and manage the time, with few hints.
- **Requirements lead to decisions:** you identify which requirements actually shape the design, and turn vague ones into numbers.
- **Trade-offs are explicit:** say what you are giving up, and when you would choose differently.
- **Failure and operations are included:** retries, partial failures, data repair, monitoring and on-call load.
- **Migration:** how to get there from today's system without stopping the business.
- **Team and delivery:** what to build first, who could build it, how to split work among teams, and how the team's skills affect the choice. A simple design that your team can run may beat a clever one.
- **Cost:** a rough estimate, and which component dominates.

A useful closing statement: the main risks, the first milestone and what you would validate first.

#### Move a billing module out of a monolith into its own service, with no downtime and no billing errors.

Clarify: why move it (team autonomy, scaling or compliance), how much data, which modules read billing tables directly, and how reversible must each step be?

**Phases:**

1. **Prepare inside the monolith:**
   - Find everything that touches billing tables, and replace direct access with a billing interface.
   - Add tests that capture existing behaviour, especially tax and rounding.
   - Add a reconciliation report that compares invoices to an independent source.
2. **Build the service and copy the data:**
   - Create the new service with its own database.
   - Backfill historical data, then keep it up to date with change data capture.
3. **Shadow reads:** send reads to both, return the monolith's result and log any difference. Fix differences until they are explained or gone.
4. **Move reads:** gradually move reads to the service, by tenant or percentage, behind a flag.
5. **Move writes, one owner at a time:**
   - Avoid long-term dual writes in application code, since a failure between them creates differences.
   - Move a small group of low-risk tenants to the new write path, with one system as the source of truth per tenant.
   - Sync changes back to the old tables for as long as other parts of the monolith still read them, and to allow rollback.
6. **Finish:** remove the sync once all readers have moved, then archive the old tables and delete the old code.

**Billing-specific protections:**
- Idempotency keys on every charge and invoice operation.
- A double-entry ledger or an audit trail that cannot be changed.
- Avoid cutting over during the billing run, and move each tenant between cycles.
- Daily reconciliation, with an alert on any mismatch.
- A limit on the value affected at each stage.

**Team and delivery:** assign one owner for the migration, put milestones with exit criteria in place, involve finance early, and agree who decides to continue or roll back.

#### Design a shared notification platform that several product teams will use.

Clarify: channels (email, SMS, push), volume, how urgent messages are, who owns templates, and how many teams will use it.

**Design:**
- **API:** `POST /notifications` with a template ID, recipient, data and an idempotency key. Asynchronous, so it returns `202`.
- **Pipeline:** API → durable queue → workers per channel → providers. Separate queues by priority, so that a marketing batch cannot delay a password reset.
- **Preferences and compliance:** central opt-out and quiet hours, with legal rules for unsubscribing and consent handled once rather than by every team *(verify rules per region)*.
- **Templates:** versioned, owned by each team, with localisation and a preview.
- **Providers:** an adapter per provider, with failover and circuit breakers.
- **Delivery tracking:** record the state of each message, and accept provider webhooks for delivered, bounced and complained.
- **Isolation:** per-team quotas and rate limits, so that one team's mistake cannot exhaust the provider limit for all.

See also [Notification system](../../topics/03-system-design/ANSWERS.md#notification-system-emailsmspush).

**What makes this a platform, not just a service:**
- **Self-service onboarding,** documentation and client libraries.
- **Clear contract:** SLOs by priority, and a versioning policy.
- **Per-team cost and usage reports.**
- **Support model:** who gets paged, and how teams can debug their own messages.
- **Adoption:** start with one or two teams, migrate their real use cases and build what they need. Teams that adopt a platform because it is easier need less persuading than teams told to use it.

### Quality and tech debt

#### What counts as tech debt, and how do you prioritise it against features?

Tech debt is a past decision that now makes change slower or riskier. Separate:
- **Deliberate debt:** a shortcut taken knowingly for a good reason.
- **Accidental debt:** a design that no longer fits because requirements or understanding changed.
- **Not debt:** code that is old or that you dislike, but which works and rarely changes.

**Prioritise by cost, not by how bad it looks.** Debt matters most where the code changes often or where failure hurts.

For each item in a register, record the effect (slower delivery, incidents or risk), how often it is felt, the cost to fix, and what happens if nothing is done. A simple ordering is impact × frequency ÷ effort.

**Speak in business terms:**
- Weak: "We need to refactor the payments module."
- Strong: "Each new payment method takes six weeks, and four of those weeks are caused by this module. Two engineer-months would bring that down to about two weeks, and the three methods on the roadmap would then arrive sooner."

**Methods of funding it:**
- Include the improvement as part of the feature work that touches the area.
- Reserve a share of capacity, often 10–20%, agreed with product. Adjust based on system health.
- Plan larger items as projects with their own results, on the roadmap.

Track outcomes so that product trusts the investment next time.

#### Which quality standards do you automate, and which do you leave to reviewers?

**Automate anything a machine can check reliably:**
- Formatting and lint rules.
- Type checks and compile errors.
- Unit and integration tests, with coverage shown on the changed code.
- Dependency vulnerability and secret scanning, plus static security analysis.
- Unsafe database migrations.
- API compatibility and contract checks.
- Architecture rules, such as which modules can import which.

**Leave to people:**
- Is this the right design and the right place for this code?
- Does the behaviour match the actual need?
- Are the names and abstractions understandable?
- Do the tests test what matters?
- What operational risks does it add?

Automation frees reviewers to discuss these questions. Keep CI fast and reliable, make the checks run locally too, and apply new rules gradually. If a rule often has to be overridden, reconsider it.

#### Pull requests wait three days for review on your team. How do you fix that?

First measure time to first review, review rounds, pull request size and who does the reviewing. Then address the actual cause:

- **Large pull requests:** set a size target, and split work using flags or stacked changes. Small pull requests are reviewed faster and better.
- **Review is seen as secondary:** agree a target such as first response within one working day. Encourage people to review before starting new work, and limit work in progress.
- **Too few reviewers:** if one or two people review everything, widen `CODEOWNERS` groups, and use pairing to spread knowledge. If you are the bottleneck, step back.
- **Unclear pull requests:** use a template that covers why, what, how it was tested, and risk.
- **Time zones:** set aside overlap time, or choose a nearby reviewer.
- **Slow CI:** reviewers wait for the pipeline, so speed it up.
- **Long debates:** move design discussions earlier, so that reviews do not have to redo the design. After a few rounds of comments, talk in person.

Keep quality in place by still using automated gates and by looking at escaped defects as well as speed. Check the metric again after a few weeks, and do not use it to rank individuals. Pair programming can replace some reviews where company policy allows.

#### How do you introduce a new standard into a large legacy codebase?

Do not block every pull request or attempt one huge clean-up.

1. **Agree why:** tie the standard to a real problem, and get the team's input. Write it down with examples.
2. **Trial it:** apply it to one component, and see whether it helps.
3. **Set a ratchet:** the rule applies to new or changed code, and existing violations are recorded in a baseline. CI fails only if the total increases. Many linters support this directly *(verify)*.
4. **Automate fixes:** use codemods and auto-formatters for mechanical changes, in separate commits. List bulk-formatting commits in `.git-blame-ignore-revs` so that blame stays useful.
5. **Prioritise by use:** clean up frequently changed code first, and leave stable code alone.
6. **Make the right way easy:** templates, examples and libraries.
7. **Track progress:** show the count falling, and celebrate milestones.
8. **Plan for removal or exceptions:** record exceptions with a reason, and retire the standard if it stops helping.

#### Your test suite is slow and flaky, and the team has stopped trusting it. What do you do?

Treat trust as the problem, because people who ignore failures will let real bugs through.

1. **Measure:** duration by stage and by test, flake rate per test, and how often reruns are used.
2. **Stop the bleeding:**
   - Quarantine known flaky tests in a separate job that does not block merges.
   - Give each one an owner and a deadline, so quarantine does not become deletion by neglect.
   - Reject new flaky tests by rerunning new tests several times in CI.
3. **Fix common causes of flakiness:**
   - Fixed sleeps: wait for a condition instead.
   - Shared state: isolate data per test.
   - Test order dependence: randomise the order to reveal it.
   - Real time and network calls: inject a clock, and use local fakes.
4. **Speed it up:**
   - Move checks down the pyramid: fewer end-to-end tests, more unit and integration tests.
   - Run tests in parallel, cache dependencies, and split by timing data.
   - Run only affected tests on pull requests, and everything before merging or nightly.
5. **Set targets:** for example, pull request checks under 10 minutes and a flake rate below 1%. Show these on a dashboard.
6. **Assign ownership,** and include the work in planning with product's agreement.

Delete tests that add no value. A smaller suite that people trust catches more bugs in practice than a large one they ignore.

#### What should a team's policy on AI coding tools cover?

- **Approved tools and data:** which tools can be used, under what contract, and what code or customer data may be sent to them. Secrets and personal data need clear rules. Check with legal and security.
- **Accountability:** the author owns the code and must understand it. "The assistant wrote it" is not an excuse.
- **Review:** generated code gets the same review, with attention on invented APIs and dependencies, insecure defaults, and tests that merely repeat the implementation.
- **Licensing:** the position on generated code and its similarity to existing code, as set by legal *(verify)*.
- **Size:** larger generated changes are harder to review, so keep pull requests small.
- **Autonomous agents:** which permissions they can have, sandboxing, and which actions need human approval, such as anything touching production.
- **Learning:** make sure junior engineers still build their own understanding, for example by explaining their changes.
- **Measurement:** assess effects with lead time, change failure rate, review load and defect rates, not lines generated or acceptance rates.
- **Shared practice:** keep project instructions in the repo and share what works.

Review the policy regularly, because the tools change quickly.

### Reliability and operations

#### How do you introduce SLOs and error budgets to a team?

1. **Start with users:** choose two or three journeys that matter, such as checkout or login.
2. **Define SLIs:** measurable signals such as the proportion of successful requests, and the proportion served within 300 ms.
3. **Set SLOs from real data:** use the current baseline and what users need, rather than aiming for as many nines as possible. Each extra nine usually costs much more.
4. **Calculate the budget:** 99.9% over 30 days allows about 43 minutes of downtime, or one bad request in a thousand.
5. **Agree the policy in advance with product:** for example, when the budget is used up, risky releases wait, and reliability work comes first until it recovers.
6. **Alert on burn rate,** not on individual errors, so that pages reflect real user impact.
7. **Review monthly,** and adjust targets that do not reflect user experience.

Common pitfalls: too many SLOs, targets no one will act on, and internal targets confused with contractual SLAs. See [SLI vs SLO vs SLA](../../cheatsheets/engineering-management.md#sli-vs-slo-vs-sla).

#### How do you run a major incident and the review that follows?

**During:**
- **Declare early,** with a severity level and one channel. It is easy to stand down later.
- **Separate roles:** an incident commander coordinates and does not debug; there are also investigators and a communications lead.
- **Mitigate first:** roll back, fail over, turn off a flag or shed load. Find the root cause later.
- **Keep a timeline** in the channel, recording decisions and actions.
- **Communicate on a schedule:** even "no change, next update in 30 minutes" is useful. Write for customers and support in plain language.
- **Hand over** if it runs long, because tired people make mistakes.

**After (within a few days):**
- **A blameless review:** assume people acted sensibly given what they knew. Ask how the system allowed it, and not who did it.
- **Cover:** impact, timeline, how it was detected, contributing factors, what went well, and where you got lucky.
- **Actions:** a small number of specific items with owners and dates, tracked in the normal backlog.
- **Share widely,** and check later that actions were finished and recurring patterns are being tackled.

As tech lead, you may not act as commander in every incident. Make sure several people can, and practise with game days.

#### A team spends half its time on production support and keeps missing sprint goals. How do you redesign its work?

1. **Measure the load:** categorise a month of tickets and pages by source, cause and effort. A few causes usually dominate.
2. **Be honest in planning:** plan on the real remaining capacity. Delivery promises should reflect the support load.
3. **Contain the interruption:** a weekly rota in which one or two engineers handle all support work, and the rest are protected. The person on the rota uses spare time on fixing causes.
4. **Set up intake:** one channel, severity rules, and a clear route so that requests do not come through direct messages.
5. **Remove the top causes:** reserve capacity to automate or fix the three largest sources, and show the reduction.
6. **Push work to the right place:** self-service tools, documentation and runbooks for common requests. Move misrouted tickets to their proper owners.
7. **Reconsider the method:** Kanban may suit this team better than sprints.
8. **Manage the people aspects:** make rotas fair, recognise support work in reviews and keep an eye on burnout.

Report trends to product and the EM, such as support hours per week and tickets per customer. If the load remains high after this work, the case is for more people or less scope.

#### On release day, one of five merged features has a serious defect. How do you decide what to do?

1. **Assess severity:** who is affected, can it corrupt data, is there a security aspect, and does a workaround exist?
2. **Choose the least risky option:**
   - **Turn it off** with a feature flag, if it has one. This is the best case, and shows why flags matter.
   - **Revert** that feature's commits, then rerun the tests. Check for dependencies between it and the other features.
   - **Fix forward,** if the fix is small, well understood and testable.
   - **Delay the release,** if the risk cannot be isolated quickly.
3. **Decide with the right people:** use agreed go or no-go criteria, with one person making the decision. Involve product on the business effect, particularly if there is an external commitment.
4. **Consider data:** if any data was written with the defect, plan the repair.
5. **Communicate:** to stakeholders and support, with what is shipping, what is not and when it will follow.
6. **Afterwards:** find out why the defect got that far, and consider what would have made this easier, such as flags on all risky features, smaller and more frequent releases, and better integration tests.

If release day is often stressful, release more often. Smaller batches make it easier to isolate problems.

#### Which delivery metrics do you track, and how do you avoid misusing them?

The DORA metrics are a good starting point *(verify the current definitions)*:
- **Deployment frequency.**
- **Lead time for changes:** from commit to production.
- **Change failure rate.**
- **Time to restore service,** now also described as failed deployment recovery time.

Add metrics for flow, such as cycle time, work in progress and time spent waiting for review. Add system metrics, such as SLO compliance and on-call load, and a team health signal, such as a regular survey. See [DORA vs SPACE](../../cheatsheets/engineering-management.md#dora-vs-space).

To avoid misuse:
- Use them to find problems at the team level, not to score individuals.
- Do not compare teams with very different work.
- Pair metrics to reveal gaming: speed with quality, and throughput with incidents.
- Look at trends, and ask why before acting.
- Avoid targets on single metrics. Goodhart's law says that when a measure becomes a target, it stops being a good measure.
- Avoid lines of code and commit counts as measures of productivity.
- Let the team see and interpret its own data.

### Leading people without authority

#### How do you mentor a junior engineer differently from a senior one, and how do you delegate work you could do faster yourself?

**Junior engineers need more structure:**
- Clear, well-scoped tasks with an example to follow.
- Frequent short check-ins, and pairing on unfamiliar work.
- Review that explains why, with a focus on a few lessons at a time.
- Gradually less guidance, and growing scope as they progress.

**Senior engineers need more room:**
- A problem, not a task. Agree the outcome, and leave the approach to them.
- Coaching through questions, and honest feedback on blind spots such as influence and communication.
- Visible opportunities: leading a design or a project, and presenting to leadership.
- Sponsorship: recommend them for stretch work, and credit them openly.

**Delegating:**
- Accept the short-term slowdown. It is an investment, and the team gains capacity.
- Match the work to the person's growth goal.
- State the outcome, constraints, and when you want to be consulted.
- Check at agreed points, and avoid hovering or taking the work back at the first problem.
- Keep ownership visible: let them present it and receive the credit.
- Keep the risk bounded. On a high-stakes task, pair or review early and more often.

#### An engineer on your team is underperforming, but you are not their manager. What do you do?

1. **Check your evidence:** specific examples and their effect on the work, rather than general impressions.
2. **Talk to them first:** privately and with curiosity. Describe what you saw and ask what is happening. Unclear expectations, a skill gap or something outside work could be involved.
3. **Help with what you own:** clearer task definitions, pairing, smaller scope or a better fit of work to skills. Set a specific goal and review it together.
4. **Involve the EM early:** share facts, and not just conclusions. Performance management, HR processes and any personal circumstances belong to the manager. Agree who says what, so that the engineer gets a consistent message.
5. **Protect delivery:** adjust plans so the risk is managed, without quietly doing their work or isolating them.
6. **Keep it confidential:** do not discuss it with other team members.

The tech lead provides observations and support, and the EM owns the formal process. Avoid both extremes: going straight to the manager without giving the person a chance, and covering for the problem for months.

#### One engineer's pull requests keep failing review, while another approves everything within minutes. How do you handle each of them?

**The engineer whose pull requests fail review:**
- Look for patterns across pull requests: misunderstanding the requirement, weak testing, unfamiliarity with the code, or skipping self-review.
- Find out why. Perhaps the feedback is arriving too late.
- Change the process: talk through the approach before coding, make pull requests smaller, pair on one change, and use a checklist for self-review.
- Check whether the reviews themselves are fair and consistent.

**The engineer who approves quickly:**
- Bring examples: defects that a review might have caught, or a large pull request approved in two minutes.
- Find the reason. It could be delivery pressure, a wish to be helpful, lack of confidence in commenting, or unclear expectations.
- Clarify what approval means: "I have read this, understood it and would be willing to support it in production."
- Help them improve: review together, and share examples of good comments.

**At team level:**
- Agree expected review standards together, without singling anyone out.
- Automate style checks, so reviewers focus on what matters.
- Look at the pressures, such as a metric or deadline that rewards speed alone.

Keep both conversations private, and bring in the EM if the pattern continues.

#### Your most experienced engineer resigns two weeks before a major release. What do you do?

**Day one: assess**
- Talk to them. Ask what they are working on, what they alone know, what could go wrong and how they would prefer to hand over. Stay professional and thankful.
- Tell the EM immediately. The EM handles the people side, including any counter-offer and the announcement.
- List their work: release tasks, on-call duties, system access, and relationships.

**Reduce release risk:**
- Rank the items only they can do by importance to the release.
- Assign each one a new owner, and pair them now, while knowledge can still be transferred.
- Reconsider release scope: cut or flag features that depend most on them.
- Check permissions and accounts, so nothing breaks when they leave.
- Be honest with product about the new risk level, and offer options: full scope with more risk, reduced scope, or a later date.

**Capture knowledge:**
- Short recorded walkthroughs and updated runbooks, focused on what will actually be needed.
- Target the release and the next few months, rather than trying to document everything.

**Look after the team:**
- Explain the plan openly, as anxiety increases when people lack information. Do not ask others to work unsustainable hours to cover the gap.

**Afterwards:** deal with the root cause, which is a concentration of knowledge. Rotate ownership, pair on critical systems and track where only one person understands something.

#### How do you push back when product or a senior leader asks for something that you think is wrong?

1. **Understand the goal:** ask what they are trying to achieve and why now. They may know something you do not.
2. **Check your facts:** separate what you know from what you are predicting.
3. **Talk about outcomes and costs:** not "this is a bad idea", but "this path has these risks and costs".
4. **Bring options:** "We cannot do all of it by March. We could do A by March, or A and B by May. A meets the main goal." A reasoned alternative works far better than a plain no.
5. **Quantify where you can:** incidents, delay, cost or customer effect.
6. **Say it privately first,** and do not surprise them in a large meeting.
7. **Respect who owns the decision:** product owns priorities. You own the technical advice and its honesty.
8. **Record it and commit:** if they accept the risk, write down the decision and support it fully. Reduce the risk where you can.
9. **Escalate only for serious issues,** such as safety, security, legal or data integrity, and tell them you are doing so.

### Behavioral

#### Tell me about a time you influenced a decision without having authority.

**What they look for:** you understood what others cared about, built a case with evidence and earned agreement, instead of relying on position or persistence.

**STAR outline:**
- **S:** [The decision, who owned it, and why it mattered]
- **T:** [What you wanted to change, and why you lacked authority over it]
- **A:** [What you learned about others' concerns, the evidence you gathered, the allies you worked with, how you adapted the proposal]
- **R:** [The decision reached, its measured effect, and what the relationship was like afterwards]

**Example shape (invented, replace it with your own story):** A tech lead sees that three teams each run their own retry and queue logic, and that this causes repeated incidents. The other teams have no time for a shared library. The tech lead collects incident data, builds a small version that solves one team's current pain, and migrates it for them. Once the first team reports fewer pages, the second team asks for it too. A staff engineer then adopts it as the standard.

#### Tell me about a project you led through heavy uncertainty.

**What they look for:** you made progress before everything was clear, reduced the largest unknowns first, kept people informed and adjusted the plan openly.

**STAR outline:**
- **S:** [The goal, and what was unknown: requirements, technology, data or dependencies]
- **T:** [Your responsibility and the constraint]
- **A:** [How you found the biggest risks, the spikes or thin slices you ran, the decisions you made, how you communicated changes]
- **R:** [What was delivered, how the plan changed, and what you would do differently]

**Example shape (invented, replace it with your own story):** A team must replace a data pipeline whose logic is not documented, and nobody knows how many downstream reports rely on it. The tech lead first runs a two-week investigation that logs all consumers and compares outputs. It finds 40 consumers rather than the assumed 10. The tech lead revises the plan, moves the highest-value consumers first, and gives product a new range with reasons. The migration ends later than first hoped but earlier than the revised date, without a data incident.

See also [Ambiguity & bias for action](../../topics/10-behavioral/ANSWERS.md#ambiguity--bias-for-action).

#### Tell me about a technical decision you got wrong.

**What they look for:** real ownership, a real mistake, early recognition, a clear reversal or repair, and a changed way of working. Avoid a disguised strength such as "I cared too much about quality."

**STAR outline:**
- **S:** [The decision and the reasoning at the time]
- **T:** [Your role in making it]
- **A:** [How you discovered it was wrong, how you told people, what you did to limit or reverse the damage]
- **R:** [The final outcome, and the specific change in how you now decide]

**Example shape (invented, replace it with your own story):** A tech lead pushes for a new NoSQL database for a service, expecting heavy scale. Six months later, access patterns have changed and reporting queries are painful, while traffic is far below the forecast. The tech lead shows the team the data, proposes moving back to a relational database, and leads the migration. The team then adopts a rule that new data stores require an ADR and a test using realistic queries.

See also [Failure & learning](../../topics/10-behavioral/ANSWERS.md#failure--learning).

#### Tell me about a failing project you took over.

**What they look for:** a calm diagnosis, honest communication, hard decisions about scope, and care for the team. Turnaround stories that blame the previous lead make a poor impression.

**STAR outline:**
- **S:** [The project state: late, over budget, low morale, unclear goal]
- **T:** [What you were asked to do]
- **A:** [How you diagnosed it, what you stopped or cut, how you reset expectations, how you rebuilt the plan and the team's confidence]
- **R:** [What shipped; or, if the right answer was to stop, what that saved and what was learned]

**Example shape (invented, replace it with your own story):** A migration is eight months in with no production traffic. The new tech lead talks to each engineer and finds that the team is building everything before cutting over anything. The plan is restructured so that one small tenant moves within four weeks, and a large optional module is cut with product's agreement. The first cutover shows integration issues that had stayed hidden until then. The remaining tenants then move in phases, finishing within the revised date.

## Prep checklist

- [ ] Be able to explain clearly the difference between a tech lead, an EM and a staff engineer, and how you would agree the split on a new team.
- [ ] Prepare a 20-minute walkthrough of a project you led: the goal, the plan, key decisions, how you worked through others, and what went wrong.
- [ ] Write an RFC and an ADR for a real or realistic change.
- [ ] Forecast a date range from sample throughput with the Monte Carlo script, and explain its limits.
- [ ] Build a tech debt register with a scoring method, and describe its top item in business terms.
- [ ] Practise five system design prompts on a timer, always including migration, rollout, team plan and cost.
- [ ] Write a risk register and a release plan with go or no-go criteria.
- [ ] Define SLOs for a service you know, and calculate the error budget.
- [ ] Keep coding practice going: at least two problems a week, plus practical exercises in your main language.
- [ ] Prepare 8–10 stories covering influence, conflict, mentoring, failure, ambiguity and delivery, using the [story bank checklist](../../topics/00-overview/README.md#story-bank-checklist-prepare-one-story-for-each).
- [ ] Drill [Tech lead & agile delivery](../../skills/tech-lead-and-agile-delivery.md) and [Technical leadership & behavioral](../../skills/technical-leadership-and-behavioral.md), plus each tool named in the job description.
