# Engineering Management Cheatsheet

Delivery, planning, people, hiring, org design and operational terms used by tech leads and engineering managers.

> Related: [Technical leadership and behavioral skill](../skills/technical-leadership-and-behavioral.md) · [Behavioral questions](../topics/10-behavioral/README.md) · [Technical delivery manager role](../roles/15-technical-delivery-manager/README.md) · [Observability and reliability](observability-and-reliability.md) · [Business and customer terms](business-and-customer-terms.md) · [Interview frameworks](interview-frameworks.md) · [Engineering management skill](../skills/engineering-management.md) · [Tech lead & agile delivery skill](../skills/tech-lead-and-agile-delivery.md)

## Scrum roles and ceremonies

| Term | What it means |
|---|---|
| **Scrum** | Framework delivering work in fixed-length sprints, with set accountabilities, events and artefacts; the Scrum Guide was last revised in 2020 *(verify)*. |
| **Product Owner** | Accountable for maximising product value and ordering the backlog; one person, not a committee, so priorities stay clear. |
| **Scrum Master** | Accountable for the team's Scrum effectiveness; coaches, removes impediments and improves the system, usually without line-managing the team. |
| **Developers** | Scrum's term for everyone creating each sprint's increment, not only coders; they own the sprint plan and its quality. |
| **Sprint** | Fixed time-box of one month or less, commonly two weeks, containing all other Scrum events and producing a usable increment. |
| **Ceremonies** | Common name for Scrum's events: Sprint Planning, Daily Scrum, Sprint Review and Sprint Retrospective; the Scrum Guide says events. |
| **Sprint Planning** | Event opening a sprint, where the team sets the Sprint Goal, selects backlog items and plans the work. |
| **Daily Scrum** (stand-up) | A 15-minute event for Developers to inspect progress towards the Sprint Goal and adjust the plan; not a status report for managers. |
| **Sprint Review** | End-of-sprint session with stakeholders to inspect results and adapt the backlog; more than a one-way demo. |
| **Sprint Retrospective** | The team's review of how it worked, ending with concrete improvements; closes the sprint and drives continuous improvement. |
| **Backlog refinement** | Ongoing splitting, detailing and estimating of upcoming items; an activity rather than a formal Scrum event, and it makes planning quicker. |
| **Product backlog** | Single ordered list of work the product may need; the Product Owner is accountable for its order. |
| **DoD** (Definition of Done) | Shared quality standard, such as reviewed, tested and deployed, that work must meet to count as done; prevents hidden unfinished work. |
| **DoR** (Definition of Ready) | Optional team checklist for when an item can enter a sprint; not in the Scrum Guide and can become an unhelpful gate. |

## Kanban, flow and estimation

| Term | What it means |
|---|---|
| **Kanban** | Flow-based method that visualises work on a board, limits work in progress and improves continuously, without prescribed sprints or roles. |
| **WIP limit** (Work in Progress limit) | Cap on items in a workflow stage; reduces context switching, exposes bottlenecks and shortens cycle time. |
| **Lead time** | Time from a request being raised to its delivery; reflects how long the customer actually waits. |
| **Cycle time** | Time from work starting to finishing; shows process speed. Definitions vary, so state the start and end points. |
| **Throughput** | Items finished per period; with cycle time, it supports forecasting without story points. |
| **Little's Law** | In a stable system, average WIP equals throughput times average cycle time; explains why reducing WIP shortens delivery time. |
| **Story points** | Relative estimate of size, complexity and uncertainty, not hours; meaningful only within one team, and not part of the Scrum Guide. |
| **Velocity** | Story points a team completes per sprint; useful for that team's forecasting, misleading as a productivity score or comparison between teams. |
| **Burndown chart** | Remaining work plotted against time for a sprint or release; shows progress at a glance but can hide scope changes. |
| **Burnup chart** | Completed work and total scope as separate lines; makes scope growth visible, which helps when discussing dates with stakeholders. |

### Scrum vs Kanban

| | Scrum | Kanban |
|---|---|---|
| Cadence | Fixed-length sprints | Continuous flow |
| Roles | Product Owner, Scrum Master, Developers | None prescribed |
| Limits work through | Sprint scope | WIP limits per stage |
| Change during work | Sprint Goal holds; scope can be renegotiated | New items pulled when capacity frees up |
| Main measures | Velocity, Sprint Goal met | Cycle time, throughput |
| Often suits | Planned product delivery | Support, operations, interrupt-driven work |

### Lead time vs cycle time vs lead time for changes

| Metric | Starts | Ends |
|---|---|---|
| Lead time | Request raised | Delivered to the customer |
| Cycle time | Work started | Work finished |
| Lead time for changes (DORA) | Code committed | Running in production |

## Jira terms

| Term | What it means |
|---|---|
| **Work item** (formerly issue) | Jira's generic name for any tracked item; Atlassian has been renaming "issues" to "work items" *(verify)*. |
| **Epic** | Large body of work spanning several sprints, broken into stories and tasks; groups related work under one outcome. |
| **Story** | Small piece of user-visible value that fits in a sprint, often written as "As a user, I want X, so that Y". |
| **Task** | Needed work that is not directly user-facing, such as a library upgrade; tracked much like a story. |
| **Sub-task** | Smaller step inside a parent work item, usually assigned to one person; shows progress within the parent. |
| **Bug** | Defect where the product does not behave as intended; tracked separately so quality trends and fix work stay visible. |

### Epic vs story vs task vs sub-task

| Type | Typical size | Example |
|---|---|---|
| Epic | Several sprints | Self-service password reset |
| Story | Fits in a sprint | User resets a password from an emailed link |
| Task | Fits in a sprint; not user-facing | Upgrade the email library |
| Sub-task | Hours to a few days | Write the reset email template |

## Planning and prioritisation

| Term | What it means |
|---|---|
| **Roadmap** | Planned direction and priorities over time, tied to outcomes; communicates intent and trade-offs, and should not read as a fixed promise. |
| **OKRs** (Objectives and Key Results) | Goal framework pairing a qualitative objective with measurable key results, often quarterly; aligns teams on outcomes rather than task lists. |
| **KPIs** (Key Performance Indicators) | Metrics tracked continuously to show the health of a product, team or process, such as uptime or churn; signal when action is needed. |
| **Capacity planning** | Estimating engineering time actually available after leave, on-call, support and meetings; stops roadmaps promising more than the team can deliver. |
| **KTLO** (Keep the Lights On) | Maintenance, upgrades and support needed to keep systems running; reserve capacity for it, or roadmap estimates will be too optimistic. |
| **RFC** (Request for Comments) | Written proposal shared for feedback before a significant change; surfaces risks early, builds buy-in and records the alternatives considered. |
| **ADR** (Architecture Decision Record) | Short, lasting record of one architectural decision with its context and consequences; tells future engineers why the system is this way. |
| **Tech debt** (technical debt) | Future cost of expedient or outdated technical choices; sometimes a sensible trade-off, but should be visible, sized and repaid by business impact. |
| **RICE** (Reach, Impact, Confidence, Effort) | Prioritisation score: (Reach × Impact × Confidence) ÷ Effort; popularised by Intercom, it makes product ideas easier to compare. |
| **MoSCoW** (Must have, Should have, Could have, Won't have) | Sorts scope into four priority buckets; useful for agreeing what ships when the deadline is fixed. |
| **RACI** (Responsible, Accountable, Consulted, Informed) | Chart of who does the work, who owns the outcome, who gives input and who is updated; keep one Accountable person per task. |
| **DACI** (Driver, Approver, Contributors, Informed) | Decision-focused chart naming who drives, who approves, who contributes and who is told; speeds decisions by naming one approver. |
| **Dependency mapping** | Identifying which teams, systems and work rely on each other; exposes sequencing risk and blockers before dates are committed. |
| **One-way vs two-way door** | Amazon shorthand: hard-to-reverse decisions deserve careful process, while reversible ones should be made quickly by those close to the work. |

### OKR vs KPI

| | OKR | KPI |
|---|---|---|
| Purpose | Change something | Monitor health |
| Time frame | Usually a fixed quarter | Ongoing |
| Example | Halve checkout errors this quarter | Checkout error rate |

### RFC vs ADR

| | RFC | ADR |
|---|---|---|
| When | Before the decision | When the decision is made |
| Purpose | Gather input | Record the outcome and why |
| Length | Often several pages | Usually a page or less |
| Afterwards | Closed once decided | Kept; replaced by a newer ADR, not rewritten |

### RICE vs MoSCoW

| | RICE | MoSCoW |
|---|---|---|
| Output | Ranked numeric score | Four priority buckets |
| Best for | Comparing many product ideas | Agreeing scope for a release |
| Watch out for | False precision in the inputs | Everything becoming a "Must" |

### RACI vs DACI

| | RACI | DACI |
|---|---|---|
| Best for | Ownership of tasks and deliverables | One specific decision |
| Single owner | Accountable | Approver |

## People management

| Term | What it means |
|---|---|
| **EM** (Engineering Manager) | Line manager accountable for a team's people, delivery and health, including hiring, coaching, performance and cross-team alignment. |
| **Tech lead** | Senior engineer guiding a team's technical direction and delivery, often hands-on with no direct reports; at many companies a role, not a level. |
| **Staff engineer** | Senior individual contributor level with technical impact across teams, through direction-setting, hard problems and mentoring; scope varies between companies. |
| **IC** (Individual Contributor) | Someone with no direct reports, at any seniority; senior ICs can match or outrank managers in level. |
| **1:1** (One-to-one) | Regular private meeting between manager and report, mainly on the report's agenda: coaching, career and blockers rather than status updates. |
| **SBI** (Situation, Behavior, Impact) | Feedback model: state when and where, the observable behaviour and its effect; keeps feedback specific and factual rather than judging character. |
| **Radical Candor** | Kim Scott's feedback approach of caring personally while challenging directly; honesty without care and care without honesty both fail. |
| **Career ladder** | Documented levels and the expectations at each, often with separate manager and IC tracks; makes growth and promotion decisions clearer and fairer. |
| **Performance review** | Periodic assessment against level expectations, often feeding pay and promotion; with regular feedback, nothing in it should be a surprise. |
| **Calibration** | Managers comparing proposed ratings or promotions across teams for consistency; reduces leniency and individual bias when based on evidence. |
| **PIP** (Performance Improvement Plan) | Formal, time-boxed plan with clear goals for someone underperforming; legal requirements and outcomes vary by country, so involve HR early. |
| **Attrition** | Share of people leaving in a period: leavers divided by average headcount. Regretted attrition, losing people worth keeping, is the sharper signal. |
| **Bus factor** | How many people could leave before a project stalls; a bus factor of one is a key-person risk worth fixing. |
| **Span of control** | Number of direct reports per manager; a common rule of thumb is five to eight. Too many limits coaching; too few adds layers. |
| **Skip-level** | Meeting with a manager's manager, skipping the direct manager; surfaces unfiltered views and gives senior leaders direct signal on team health. |
| **Psychological safety** | Shared belief that people can speak up or admit mistakes without punishment; Google's Project Aristotle found it the top factor in team effectiveness. |

### Tech lead vs EM vs staff engineer

Titles and scope vary between companies; this is the common pattern.

| | Tech lead | EM | Staff engineer |
|---|---|---|---|
| Question answered | How should this be built? | Is the team healthy and delivering? | Which technical problems matter most across teams? |
| Direct reports | Usually none | Yes | Usually none |
| Typical scope | One team | One team, sometimes more | Several teams or wider |
| Career track | Often a role on the IC track | Management | IC |
| Works through | Technical direction, code, reviews | People, process, priorities | Influence, strategy, hard problems |

## Hiring

| Term | What it means |
|---|---|
| **Headcount** | Number of approved, budgeted positions; new roles usually need budget sign-off, so it constrains plans as much as engineering capacity does. |
| **Backfill** | Replacement hire for someone who left; budgets often treat it separately from new headcount, and approval is not guaranteed. |
| **Job levels** | A company's seniority grades, tied to expectations and pay; the same title or number can mean different scope elsewhere, so compare scope. |
| **Scorecard** | Competencies and rating scale agreed before interviewing; each interviewer records evidence against it, making decisions fairer and easier to compare. |
| **Structured interview** | Same job-related questions and rating guide for every applicant; research generally finds it predicts job performance better than unstructured interviews. |
| **Debrief** | Meeting where interviewers compare evidence and reach a decision; writing feedback first reduces groupthink and anchoring on the loudest voice. |
| **Bar raiser** | Amazon term for a trained interviewer outside the hiring team who protects the hiring standard and can veto a hire. |
| **Onboarding** | Getting a new hire productive and connected, often using a 30-60-90 day plan and a buddy; shortens time to first contribution. |

## Org design

| Term | What it means |
|---|---|
| **Conway's Law** | Systems tend to mirror the communication structure of the organisations building them, so org design directly shapes architecture. |
| **Inverse Conway Manoeuvre** | Deliberately shaping teams and communication paths to encourage the wanted architecture, instead of letting current structure dictate it. |
| **Team Topologies** | Model from the Skelton and Pais book: four team types and three interaction modes, designed to limit cognitive load and speed flow. |
| **Stream-aligned team** | Owns a continuous flow of work for a product, service or user group, end to end; the default team type. |
| **Platform team** | Provides internal self-service capabilities so other teams can deliver with less cognitive load; should treat the platform as a product. |
| **Enabling team** | Specialists who help other teams build missing capabilities for a limited time, coaching rather than doing the work for them. |
| **Complicated-subsystem team** | Owns a component needing deep specialist knowledge, such as a maths-heavy engine, so other teams need not carry that load. |
| **Interaction modes** | How teams work together in Team Topologies: collaboration, X-as-a-Service or facilitating; chosen deliberately and changed as needs evolve. |
| **Cognitive load** | Mental effort a team must carry to do its work; too much slows delivery, so team scope should be sized around it. |
| **Two-pizza team** | Amazon idea: a team small enough to be fed by two pizzas, roughly under ten people; small teams need less coordination. |

## Delivery and productivity metrics

| Term | What it means |
|---|---|
| **DORA metrics** (DevOps Research and Assessment) | Originally four software delivery measures of speed and stability; recent reports rename metrics and add rework rate *(verify)*. |
| **Deployment frequency** | How often code is deployed to production; higher frequency usually means smaller, lower-risk changes. |
| **Lead time for changes** | Time from code being committed to running in production; shows how quickly a change can reach users. |
| **Change failure rate** | Share of deployments causing a failure that needs a fix or rollback; a core stability signal. DORA now says change fail rate *(verify)*. |
| **Failed deployment recovery time** | Time to restore service after a failed deployment; replaced DORA's broader time to restore service metric *(verify)*. |
| **SPACE** (Satisfaction and well-being, Performance, Activity, Communication and collaboration, Efficiency and flow) | Framework treating developer productivity as multi-dimensional; use metrics from several dimensions, mixing surveys and system data, rather than one number. |

### DORA vs SPACE

| | DORA | SPACE |
|---|---|---|
| Measures | Software delivery performance | Developer productivity, broadly |
| Data | System data or surveys | Surveys and system data |
| Best for | Tracking delivery speed and stability | Avoiding a single-metric view of productivity |
| Watch out for | Gaming when used as a team league table | Too many metrics, no clear decisions |

## Reliability and incidents

| Term | What it means |
|---|---|
| **SLI** (Service Level Indicator) | The measured signal of service health, such as success rate or latency; should reflect what users experience. |
| **SLO** (Service Level Objective) | Internal target for an SLI over a window, such as 99.9% over 30 days; gives leaders a shared basis for reliability trade-offs. |
| **SLA** (Service Level Agreement) | External contractual promise on service levels, usually with credits if missed; normally set looser than the matching SLO. |
| **Error budget** | Unreliability an SLO allows (99.9% leaves 0.1%); spending it too quickly justifies pausing risky releases for reliability work. |
| **Incident commander** | Person coordinating an incident response: assigns roles, sets priorities and manages updates, usually leaving hands-on debugging to others. |
| **SEV** (Severity level) | Agreed scale, such as SEV1 to SEV4, ranking incident impact; sets who is paged and how often updates go out. |
| **Blameless postmortem** | Post-incident review focused on systems and contributing factors, not individual fault; honest accounts lead to owned actions that reduce recurrence. |
| **MTTR** (Mean Time to Restore) | Average time to restore service after an incident; the R can also mean repair, respond or resolve, so define it first. |

### SLI vs SLO vs SLA

| | SLI | SLO | SLA |
|---|---|---|---|
| What it is | A measurement | An internal target | A contract |
| Example | 99.95% of requests succeeded | 99.9% success over 30 days | 99.5% monthly, or service credits |
| Main audience | Engineers | Team and leadership | Customers and legal |
| If missed | Not applicable | Prioritise reliability work | Credits or penalties |

## Cloud budget and FinOps

| Term | What it means |
|---|---|
| **FinOps** (Finance plus DevOps) | Practice making engineering, finance and business jointly accountable for cloud spend; FinOps Foundation phases are Inform, Optimize and Operate *(verify)*. |
| **Cloud budget** | Planned cloud spend for a team or product over a period; alerts on variance catch overruns before the monthly bill. |
| **Showback** | Reporting each team's cloud costs without charging them; builds cost awareness with little process. |
| **Chargeback** | Charging cloud costs to each team's budget; creates stronger accountability but needs accurate cost allocation. |
| **Cost allocation tags** | Labels such as team or product on cloud resources; without them, spend cannot be attributed to an owner. |
| **Unit economics** | Cloud cost per customer, request or transaction; shows whether spend scales efficiently, which total spend alone cannot show. |
| **Commitment discounts** | Lower rates for committing to usage, such as AWS Savings Plans or Google committed use discounts; unused commitment still costs money *(verify)*. |
