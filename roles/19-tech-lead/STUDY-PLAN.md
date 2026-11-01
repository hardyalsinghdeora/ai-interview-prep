# Study Plan — Tech Lead

For senior engineers preparing for a Tech Lead loop at a product company, SaaS vendor or services firm; assumes about 10–12 hours a week for six weeks.

Read the role overview first: [README.md — The role](./README.md#the-role) and the [typical interview loop](./README.md#typical-interview-loop). Tech lead loops combine coding and system design with rounds on delivery, technical decisions and leading others. This plan assumes you are already a capable engineer. It keeps your technical skills warm, and spends most of its time on the judgement and leadership evidence that decides these loops.

## Before you start

Spend 30 minutes rating yourself honestly. Skip or skim areas you rate 4 or 5.

| Skill area | Rate yourself 1–5 | What a 4 looks like |
|---|---|---|
| Role clarity | | You can explain how a tech lead, EM and staff engineer differ, and how you would agree the split. |
| Planning and estimation | | You can break a vague goal into milestones, estimate in ranges and forecast from throughput. |
| Delivery and risk | | You can run a cross-team plan with a fixed date, a risk list and go or no-go criteria. |
| Technical decisions | | You can run an RFC to a decision, resolve disagreement between seniors and write an ADR. |
| System design | | In 45 minutes you drive the design yourself and cover failure, migration, team plan and cost. |
| Tech debt and quality | | You can argue for debt work in business terms, and improve review and test health with data. |
| Reliability | | You can set SLOs, explain error budgets, command an incident and run a blameless review. |
| Mentoring and delegation | | You can give examples of engineers who grew because of how you worked with them. |
| Influence and pushback | | You can describe saying no with options, and winning support without authority. |
| Coding | | You still solve practical problems and most medium DS&A problems within interview time. |

## 6-week plan

Assumes ~10–12 hours a week. Do two coding problems a week throughout, and more if the company uses a DS&A-heavy loop. Use real examples from your own work wherever possible.

### Week 1 — The role, your story and first coding practice

**Goals:** a clear description of the role, and a project story that shows you leading.

- [ ] Read [The role and its boundaries — model answers](./README.md#the-role-and-its-boundaries-1).
- [ ] Read [Tech lead vs EM vs staff engineer](../../cheatsheets/engineering-management.md#tech-lead-vs-em-vs-staff-engineer) and the [Hiring-manager round topics](../../topics/10-behavioral/ANSWERS.md#hiring-manager-round-topics).
- [ ] Drill the fundamentals of [Tech lead & agile delivery](../../skills/tech-lead-and-agile-delivery.md).
- [ ] Refresh coding: [02 — D. Questions asked during/after coding](../../topics/02-coding/ANSWERS.md#d-questions-asked-duringafter-coding) and the [SDE guide's coding round](../18-software-development-engineer/README.md#coding-round-1).

**Deliverable:** a one-page project deep-dive note for a project you led: goal, plan, three decisions with alternatives, how you worked through others, the hardest moment and the result.

**Mock:** a 30-minute hiring manager conversation: "Tell me what you led, and how you know it went well." The partner keeps asking "what did you do personally?"

**End of week check:** you can describe your leadership in specifics rather than "we", and explain how you would agree the split with an EM.

### Week 2 — Planning, estimation and delivery

**Goals:** turn uncertainty into plans, ranges and clear status.

- [ ] Read [Planning, estimation and delivery — model answers](./README.md#planning-estimation-and-delivery-1).
- [ ] Practise breaking problems down: [04 — E. Abstract / product decomposition](../../topics/04-decomposition/ANSWERS.md#e-abstract--product-decomposition).
- [ ] Skim [Kanban, flow and estimation](../../cheatsheets/engineering-management.md#kanban-flow-and-estimation) and [Planning and prioritisation](../../cheatsheets/engineering-management.md#planning-and-prioritisation).
- [ ] Drill the working depth of [Tech lead & agile delivery](../../skills/tech-lead-and-agile-delivery.md).

**Deliverables:**
1. A [throughput forecast](./README.md#how-can-past-throughput-be-used-to-forecast-a-delivery-range) from real or sample data.
2. A delivery plan for the [cross-team launch](./README.md#plan-a-feature-that-spans-web-mobile-and-two-backend-teams-with-a-fixed-external-launch-date): milestones, owners, a risk register and scope tiers.

**Mock:** a 30-minute role-play: a partner plays a product manager demanding [a firm date for the migration](./README.md#product-asks-for-a-firm-date-on-a-six-month-migration-with-many-unknowns-how-do-you-respond), and adds pressure midway.

**End of week check:** you offer ranges with assumptions and options, and you never just say "it depends".

### Week 3 — Technical decisions and system design

**Goals:** make decisions stick, and lead a design round.

- [ ] Read [Technical decisions — model answers](./README.md#technical-decisions-1) and [System design at tech lead level — model answers](./README.md#system-design-at-tech-lead-level-1).
- [ ] Work prompts from [03 — C. Classic system design](../../topics/03-system-design/ANSWERS.md#c-classic-system-design-still-asked), and answer the [follow-up questions](../../topics/03-system-design/ANSWERS.md#d-follow-up-questions-to-expect).
- [ ] Drill [System design](../../skills/system-design.md) and [Large-scale distributed systems](../../skills/large-scale-distributed-systems.md), and skim the [Distributed systems cheatsheet](../../cheatsheets/distributed-systems.md).
- [ ] Skim [Data migration & legacy modernisation](../../skills/data-migration-and-legacy-modernisation.md).

**Deliverables:**
1. An RFC for a real or realistic change, plus an ADR recording the decision.
2. A written migration plan for the [billing module](./README.md#move-a-billing-module-out-of-a-monolith-into-its-own-service-with-no-downtime-and-no-billing-errors).

**Mock:** a 45-minute system design, in which the partner gives few hints and then asks how you would migrate to the design and split the work among teams.

**End of week check:** you can state what you are giving up in each major choice, and name the first milestone.

### Week 4 — Quality, tech debt and team practices

**Goals:** improve engineering health with evidence, without becoming the process police.

- [ ] Read [Quality and tech debt — model answers](./README.md#quality-and-tech-debt-1).
- [ ] Drill [Git & code review](../../skills/git-and-code-review.md), [Testing & quality](../../skills/testing-and-quality.md) and [AI-assisted engineering](../../skills/ai-assisted-engineering.md).
- [ ] Study [13 — Software engineering practices](../../topics/13-cs-fundamentals/ANSWERS.md#software-engineering-practices).
- [ ] Skim the [Testing cheatsheet](../../cheatsheets/testing.md).

**Deliverables:**
1. A tech debt register with a scoring method, and the top three items written in business terms.
2. A one-page team quality standard: pull request template, review expectations, CI gates and how you would roll it out to legacy code.

**Mock:** a 30-minute scenario round covering [slow reviews](./README.md#pull-requests-wait-three-days-for-review-on-your-team-how-do-you-fix-that) and [flaky tests](./README.md#your-test-suite-is-slow-and-flaky-and-the-team-has-stopped-trusting-it-what-do-you-do). The partner asks what data you would look at first.

**End of week check:** you can make the case for debt work so that a product manager would agree, using delivery or incident numbers.

### Week 5 — Reliability and leading people

**Goals:** handle incidents and support load, and show how you develop others.

- [ ] Read [Reliability and operations — model answers](./README.md#reliability-and-operations-1) and [Leading people without authority — model answers](./README.md#leading-people-without-authority-1).
- [ ] Drill [Observability & reliability](../../skills/observability-and-reliability.md), and skim [SLI vs SLO vs SLA](../../cheatsheets/engineering-management.md#sli-vs-slo-vs-sla) and [DORA vs SPACE](../../cheatsheets/engineering-management.md#dora-vs-space).
- [ ] Practise [12 — Production troubleshooting scenarios](../../topics/12-debugging-troubleshooting/ANSWERS.md#b-production-troubleshooting-scenarios) out loud.
- [ ] Read the Senior / Principal section of [Technical leadership & behavioral](../../skills/technical-leadership-and-behavioral.md) and answer five questions.

**Deliverables:**
1. SLOs and an error budget policy for a service you know.
2. A short written incident review for a real or invented outage, with a timeline, contributing factors and three actions.

**Mock:** a 45-minute people and incident round: "an engineer is [underperforming and you are not their manager](./README.md#an-engineer-on-your-team-is-underperforming-but-you-are-not-their-manager-what-do-you-do)", then "your [most experienced engineer resigns](./README.md#your-most-experienced-engineer-resigns-two-weeks-before-a-major-release-what-do-you-do)".

**End of week check:** you can say what belongs to you and what to the EM in each people scenario, and name examples of engineers you have helped grow.

### Week 6 — Behavioral and full loop

**Goals:** polished stories, and a full rehearsal.

- [ ] Read [Behavioral — model answers](./README.md#behavioral-1) and [How do you push back?](./README.md#how-do-you-push-back-when-product-or-a-senior-leader-asks-for-something-that-you-think-is-wrong)
- [ ] Study [10 — Ownership & impact](../../topics/10-behavioral/ANSWERS.md#ownership--impact), [Ambiguity & bias for action](../../topics/10-behavioral/ANSWERS.md#ambiguity--bias-for-action), [Conflict & collaboration](../../topics/10-behavioral/ANSWERS.md#conflict--collaboration) and [Failure & learning](../../topics/10-behavioral/ANSWERS.md#failure--learning).
- [ ] Prepare questions: [14 — About the team and product](../../topics/14-questions-to-ask/ANSWERS.md#about-the-team--product) and [to the hiring manager](../../topics/14-questions-to-ask/ANSWERS.md#to-the-hiring-manager).
- [ ] Tick off the role's [Prep checklist](./README.md#prep-checklist).

**Deliverable:** a story matrix: rows are influence, conflict, mentoring, failure, ambiguity, delivery under pressure, and technical judgement; each row has one or two real stories with your actions and a measured result.

**Mock:** a full mock loop: coding (45), system design (45), project deep dive (45), leadership and delivery (45), and a cross-functional conversation with someone playing a product manager (30).

**End of week check:** every story has a clear action of your own and a result, and survives three follow-up "why" questions.

## 2-week crash plan

| Day | Focus | Tasks | Links |
|---|---|---|---|
| 1 | Role | Read the role; explain the boundaries out loud | [README — role](./README.md#the-role), [Role boundaries answers](./README.md#the-role-and-its-boundaries-1) |
| 2 | Project story | Write and rehearse a deep dive on a project you led | [Hiring-manager round topics](../../topics/10-behavioral/ANSWERS.md#hiring-manager-round-topics) |
| 3 | Planning | Vague goal to plan; firm date on a migration | [Planning answers](./README.md#planning-estimation-and-delivery-1) |
| 4 | Delivery | Throughput forecast; cross-team launch plan | [Forecast answer](./README.md#how-can-past-throughput-be-used-to-forecast-a-delivery-range), [Launch plan answer](./README.md#plan-a-feature-that-spans-web-mobile-and-two-backend-teams-with-a-fixed-external-launch-date) |
| 5 | Decisions | RFC and ADR; resolving disagreement; rewrite or refactor | [Technical decisions answers](./README.md#technical-decisions-1) |
| 6 | System design | One prompt, plus migration and a team plan | [System design answers](./README.md#system-design-at-tech-lead-level-1), [03 — Section C](../../topics/03-system-design/ANSWERS.md#c-classic-system-design-still-asked) |
| 7 | Mock day | System design mock + delivery role-play | [Mock schedule](#mock-interview-schedule) |
| 8 | Billing migration | Write out the phases from memory | [Billing migration answer](./README.md#move-a-billing-module-out-of-a-monolith-into-its-own-service-with-no-downtime-and-no-billing-errors) |
| 9 | Quality | Tech debt in business terms; reviews; tests | [Quality answers](./README.md#quality-and-tech-debt-1) |
| 10 | Reliability | SLOs; incident command; support load | [Reliability answers](./README.md#reliability-and-operations-1) |
| 11 | People | Mentoring; underperformance; resignation | [People answers](./README.md#leading-people-without-authority-1) |
| 12 | Coding | Two practical problems and one DS&A problem | [02 — Coding](../../topics/02-coding/ANSWERS.md), [DS&A topics](../../topics/02-coding/ANSWERS-dsa-topics.md) |
| 13 | Behavioral | Story matrix; full mock loop | [Behavioral answers](./README.md#behavioral-1), [story bank checklist](../../topics/00-overview/README.md#story-bank-checklist-prepare-one-story-for-each) |
| 14 | Rest and review | Light revision; reread your project note | [README checklist](./README.md#prep-checklist) |

## Daily routine

A realistic 90-minute weekday:

| Minutes | Activity |
|---|---|
| 0–30 | Coding twice a week; system design outlines on the other days. |
| 30–60 | The week's topic: one README model answer, then a sibling question without notes. |
| 60–75 | Five questions from a skill list. |
| 75–90 | One leadership story out loud; tighten your own actions and the result. |

Weekends: write the week's deliverable, and do the mock.

## Mock interview schedule

| Week | Mock | With whom |
|---|---|---|
| 1 | Hiring manager conversation | A current engineering manager or senior peer |
| 2 | Date and scope pressure role-play | A product manager, or a peer playing one |
| 3 | System design with migration | A staff or senior engineer; interviewing.io and Exponent offer system design mocks |
| 4 | Scenario round on quality | A peer tech lead |
| 5 | People and incident round | An engineering manager |
| 6 | Full loop | Two or three partners, including one manager and one product person |

Do at least one design or scenario mock every week, and record one leadership story a week.

## Resources

- *The Staff Engineer's Path* by Tanya Reilly.
- *Staff Engineer* by Will Larson, and his writing at https://staffeng.com/.
- *Accelerate* by Nicole Forsgren, Jez Humble and Gene Kim, and the DORA reports (https://dora.dev/).
- *Designing Data-Intensive Applications* by Martin Kleppmann.
- *Site Reliability Engineering* and *The Site Reliability Workbook* (Google, free at https://sre.google/books/).
- *When Will It Be Done?* by Daniel Vacanti, for throughput forecasting.
- *Monolith to Microservices* by Sam Newman.
- *Working Effectively with Legacy Code* by Michael Feathers.
- The Scrum Guide (https://scrumguides.org/).
