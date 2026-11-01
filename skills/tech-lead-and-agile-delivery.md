# Tech lead and agile delivery

Tech lead interviews test whether you can turn vague goals into shipped software while keeping the codebase and the team in good shape. Interviewers care less about whether you can recite Scrum terms and more about how you plan under uncertainty, handle slipping work, manage risk and make technical decisions stick. Tech Lead, Senior Engineer and Technical Delivery Manager loops ask most, and Engineering Manager loops often test the same ground, because delivery is a shared responsibility.

> Related: [Engineering management](engineering-management.md) · [Technical leadership and behavioral](technical-leadership-and-behavioral.md) · [Git and code review](git-and-code-review.md) · [Decomposition](../topics/04-decomposition/README.md) · [Technical delivery manager](../roles/15-technical-delivery-manager/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] Walk through Scrum's events, accountabilities and artefacts. What problem is each one meant to solve? *(verify)*
- [ ] Scrum vs. Kanban: when would you choose each, and what does a work-in-progress limit actually do for a team?
- [ ] What should come out of a good sprint planning session, and what should a team refuse to commit to?
- [ ] What do story points measure, what do they not measure, and when would you estimate in days or skip estimates altogether?
- [ ] Definition of Ready vs. Definition of Done: what belongs in each, and how can each one be misused?
- [ ] What is velocity good for, and why should it not be used to compare teams or individuals?
- [ ] RFC vs. ADR: what is each one for, who reads it, and when do you write one?
- [ ] What counts as tech debt? Separate deliberate debt, accidental debt and work that is merely old or disliked.
- [ ] Tech lead vs. engineering manager vs. staff engineer: who owns people, delivery and technical direction, and where do the roles overlap?
- [ ] (SDE) What does a tech lead do that a strong senior engineer does not, and how would you start doing that work before having the title?

## B. Working depth
- [ ] Lead time vs. cycle time vs. throughput: how would you use them, with a cumulative flow diagram, to find a bottleneck?
- [ ] How do you estimate work with high uncertainty? When do you use a spike, and how do you express the result as a range?
- [ ] Your team regularly carries 30% of its sprint into the next one. What could that be telling you, and how do you find out which cause applies?
- [ ] How would you design a Jira workflow for a team? Which statuses earn their place, and which ones only hide waiting time? *(verify)*
- [ ] How do you structure epics, stories and sub-tasks in Jira so the board reflects real progress without becoming admin work? *(verify)*
- [ ] How do you track tech debt and prioritise it against features, and how do you express its cost to product?
- [ ] Which code quality standards do you automate with formatters, linters and CI gates, and which ones do you leave to human review?
- [ ] Pull requests wait three days for review on your team. How do you fix that without lowering review quality?
- [ ] How do you mentor a junior engineer differently from a senior one, and how do you delegate work you could do faster yourself?
- [ ] What makes a risk register useful rather than decorative? Cover likelihood, impact, owner, trigger and mitigation.
- [ ] How do you plan a release that depends on three teams, including cut-off dates, feature flags and rollback?

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] How do you build a 12-month technical roadmap from business goals, and how do you sequence items when each one has a different kind of payoff?
- [ ] How do you run an RFC process so it reaches decisions, avoids bikeshedding and does not stall waiting for approval?
- [ ] How do you introduce a new code standard to a large legacy codebase without blocking every pull request?
- [ ] Why do software estimates keep going wrong, and what can actually improve them? Compare better estimation with throughput-based forecasting.
- [ ] When would you rewrite a component instead of refactoring it incrementally, and how do you stop a rewrite from growing without limit?
- [ ] Release trains vs. continuous deployment: what do you need before moving from one to the other, and what changes in a regulated environment?
- [ ] When is Scrum the wrong fit, for example on a platform team or a team with heavy interrupts, and what would you run instead?
- [ ] When would you adopt a scaling framework such as SAFe or LeSS, and what does it cost in overhead and autonomy? *(verify)*
- [ ] (Lead) How do you stay hands-on without becoming the bottleneck, and which work do you stop taking on as the team grows?
- [ ] (Lead) When should a technical problem move from a tech lead to a staff engineer, and how do you keep ownership clear after that?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Product asks for a firm date on a six-month migration with many unknowns. How do you estimate it, communicate it and update it as you learn?
- [ ] Your most experienced engineer resigns two weeks before a major release. Walk through your risk assessment and your options.
- [ ] On release day, one of five merged features shows a serious defect. How do you run the go or no-go decision, and how do you remove the feature?
- [ ] You, the EM and the staff engineer disagree on whether to rewrite a core service. How do you reach a decision, and how is it recorded?
- [ ] A team spends half its time on production support and keeps missing sprint goals. Redesign how it plans and works.
- [ ] One engineer's pull requests keep failing review, while another approves everything within minutes. How do you handle each of them?
- [ ] Design a delivery plan for a feature that spans web, mobile and two backend teams, with a fixed external launch date.

## E. Hands-on checks (things to be able to DO)
- [ ] Write an RFC for a real change, with context, options, trade-offs and rollout, plus an ADR that records the final decision.
- [ ] Configure a Jira workflow and write JQL filters to find tickets stuck in review and work that was added after a sprint started. *(verify)*
- [ ] Forecast a delivery range from past throughput using a simple Monte Carlo simulation in a spreadsheet or script.
- [ ] Build a tech debt register with a scoring method, then choose the top three items and justify the order.
- [ ] Write a risk register and a release plan for a fictional launch, including go or no-go criteria and a rollback plan.
- [ ] Write a team code quality standard with a pull request template, review expectations and the CI gates that enforce it.

---

**Good answers show:**
- Process treated as a tool: you can explain what each practice is for and when you would change or drop it.
- Honest estimates expressed as ranges with assumptions, and early communication when those assumptions break.
- Tech debt described in business terms, such as slower delivery, incidents or risk, not only as code that feels unpleasant.
- Decisions that are written down, with the options considered and a clear owner.
- Risks spotted early, each with an owner, a trigger and a plan that does not depend on one person.
- Clear role boundaries with the EM, the staff engineer and product, and a growing team that needs you less for routine decisions.

**Prove it:** Take a small open-source project or a fictional team backlog and run one planning cycle on paper: estimate the work, forecast a date range from sample throughput, write one RFC and one ADR, build a short risk register and produce a release plan. Compare the forecast with a real or simulated outcome, then write down what you would change in the next cycle.
