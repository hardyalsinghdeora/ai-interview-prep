# Interview Frameworks Cheatsheet

Answer frameworks, interview loop and round terms, seniority levels, and offer and negotiation vocabulary.

> Related: [Overview and frameworks](../topics/00-overview/README.md) · [Recruiter and motivation questions](../topics/01-recruiter-and-motivation/README.md) · [Decomposition questions](../topics/04-decomposition/README.md) · [Customer scenarios questions](../topics/09-customer-scenarios/README.md) · [Behavioral questions](../topics/10-behavioral/README.md) · [Roles overview](../roles/README.md) · [Technical leadership and behavioral skills](../skills/technical-leadership-and-behavioral.md) · [System design](system-design.md) · [Business and customer terms](business-and-customer-terms.md)

## Answer frameworks

| Term | What it means |
|---|---|
| **STAR** (Situation, Task, Action, Result) | A structure for behavioral stories: context, your responsibility, what you did, and the measured outcome. |
| **STAR+L** (Situation, Task, Action, Result, Learning) | STAR plus what you learnt or would change; shows self-awareness and growth, and handles follow-ups. |
| **Story bank** | A set of 8 to 10 rehearsed stories that together cover most behavioral prompts; each story fits several questions. |
| **"I", not "we"** | Describe your own actions in stories; interviewers score your contribution, not your team's. |
| **MECE** (Mutually Exclusive, Collectively Exhaustive) | Splitting a problem into parts that do not overlap and leave nothing out; keeps analysis clear. |
| **Decomposition** | Breaking a vague problem into solvable parts: clarify, scope, break down, data, solution, risks, measure. |
| **Clarifying questions** | Questions that establish users, goals and constraints before solving; interviewers score them as much as the solution. |
| **Assumption stating** | Saying your assumptions aloud so the interviewer can correct them; keeps you moving through ambiguity. |
| **Thin slice** (v0) | The simplest end-to-end version that delivers value; shows you can ship before you scale. |
| **Back-of-envelope estimate** | Quick rough calculation of scale, cost or capacity; shows your design decisions rest on numbers. |
| **Thinking out loud** | Narrating your reasoning as you work; gives interviewers signal and lets them help you. |

### Method summaries

| Method | Steps |
|---|---|
| Decomposition | Clarify → scope → break down (MECE) → data → solution sketch → risks and trade-offs → measure |
| System design | Requirements → API → data model → high-level design → deep dives → trade-offs → monitoring |
| AI system design | Use case and metric → knowledge sources → model → prompt, RAG or agent → evals → guardrails → latency and cost → monitoring |
| Customer role-play | Listen → acknowledge → clarify the need → align on outcome → options with trade-offs → next steps → follow up in writing |

## System design terms

| Term | What it means |
|---|---|
| **Functional requirements** | What the system must do, such as upload a document; fixes the scope of the design. |
| **NFR** (Non-Functional Requirements) | How well it must work: latency, availability, scale, security and cost; these drive the architecture. |
| **High-level design** | The main components and data flows, sketched before detail; aligns you with the interviewer early. |
| **Deep dive** | Detailed exploration of the hardest part, such as scaling or consistency; where senior depth shows. |
| **Trade-off** | What you gain and give up with each choice; naming these matters more than picking the "right" answer. |

## Customer role-play terms

| Term | What it means |
|---|---|
| **Paraphrasing** | Repeating the customer's point in your own words; shows listening and catches misunderstandings early. |
| **Acknowledge without over-promising** | Recognising the customer's frustration while committing only to what you can actually deliver. |
| **Stated need vs real need** | What the customer asks for versus the outcome they want; asking why uncovers the difference. |
| **Options with a recommendation** | Giving choices with trade-offs and saying which you would pick; respects the customer and shows judgement. |
| **Written follow-up** | Confirming decisions, owners and dates in writing after the conversation; prevents later disputes. |

## Loop and round terms

| Term | What it means |
|---|---|
| **Interview loop** (on-site) | The main set of consecutive interviews, often 4 to 6, whether held virtually or in person. |
| **Recruiter screen** | The first call, covering motivation, background, logistics and compensation expectations. |
| **Hiring manager screen** | An early conversation with the future manager, on relevant experience and role fit. |
| **Technical screen** | A short early coding or technical interview, deciding whether you proceed to the full loop. |
| **Take-home** | A build exercise done on your own time, often followed by a live session defending your choices. |
| **DS&A round** (Data Structures and Algorithms) | Coding interview on algorithmic problems, assessed on correctness, complexity and communication. |
| **Practical coding round** | Building or fixing something realistic, such as calling an API or parsing data; closer to daily work. |
| **System design round** | Designing a large system aloud, assessed on requirements, trade-offs and depth. |
| **Decomposition round** (case round) | Breaking down a vague business problem into a plan; common for forward deployed roles. |
| **Learning round** | Learning a new domain or API quickly, then applying or explaining it; tests learning speed. |
| **Behavioral round** | Questions on past behaviour, using stories to assess ownership, collaboration and judgement. |
| **Customer role-play round** | The interviewer plays a difficult customer; assesses empathy, communication and handling of pressure. |
| **Demo or presentation round** | Presenting a product or your own solution to a simulated audience; common for solutions roles. |
| **Written exercise** | An email, document or ticket reply written under time limits; tests clarity and tone. |
| **Debugging round** | Finding and fixing a fault in code or a system; assesses method rather than speed alone. |
| **Bar raiser** | An interviewer outside the hiring team who protects the hiring standard, often with strong influence on the decision. |
| **Debrief** | The interviewers' meeting to compare evidence and decide; each should have written feedback first. |
| **Scorecard** | The structured form interviewers use to rate defined signals; makes decisions more consistent. |
| **Hiring committee** | A group that reviews interview packets and makes or checks decisions at some companies. |
| **Team match** | Pairing with a specific team after passing the main interviews, at companies that hire centrally. |
| **Reference check** | Conversations with former colleagues near the end of the process, to confirm the picture from interviews. |

## Levels

| Term | What it means |
|---|---|
| **IC** (Individual Contributor) | A person with impact through their own work and influence, rather than by managing people. |
| **Levelling** | Deciding what seniority to hire at; it sets pay band and expectations, and differs between companies. |
| **Senior engineer** | Reliably owns large projects within a team, with little supervision. |
| **Staff engineer** | Leads across teams, sets technical direction and solves ambiguous problems; judged on broad impact, not output alone. |
| **Principal engineer** | Shapes direction across an organisation or market over multiple years; often the executive-level technical voice. |
| **Influence without authority** | Getting others to act through evidence and trust rather than reporting lines; expected at Staff level and above. |
| **Leverage** | Multiplying impact through others, such as with reusable designs, standards and mentoring. |

### Senior vs Staff vs Principal

| | Senior | Staff | Principal |
|---|---|---|---|
| Scope | Team or project | Several teams or a major customer | Organisation, portfolio or market |
| Time horizon | Weeks to a quarter | Quarters to a year | Multiple years |
| Main output | Delivered systems | Direction, designs, alignment | Strategy, standards, major decisions |
| Interview emphasis | Depth and execution | Ambiguity, trade-offs, influence | Vision, business judgement, executive presence |

Titles and scope vary by company, so check the job description.

## Offer and negotiation

| Term | What it means |
|---|---|
| **TC** (Total Compensation) | The full yearly value of an offer, including base, bonus and equity; compare offers on this. |
| **Base salary** | Fixed annual pay; the most certain component of an offer. |
| **Bonus** | Variable pay tied to company or personal performance; ask how often the target was actually paid. |
| **OTE** (On-Target Earnings) | Base plus commission at full quota; common in sales-linked roles such as solutions engineering. |
| **CTC** (Cost to Company) | The employer's total cost, common in Indian offers; may include items that never reach take-home pay. |
| **Sign-on bonus** | A one-off payment on joining, often repayable if you leave early. |
| **Equity** | Ownership in the company, usually through RSUs or options; its value can change substantially. |
| **RSU** (Restricted Stock Unit) | A promise of company shares after vesting; usually taxed when they vest *(verify)*. |
| **Stock option** | The right to buy shares at a fixed price; valuable only if the share value rises above it. |
| **Strike price** | The fixed price at which a stock option can be exercised. |
| **Vesting** | How equity is earned over time, commonly over four years. |
| **Vesting cliff** | A period, often one year, before any equity vests; leaving earlier forfeits it. |
| **Refresher grant** | Additional equity awarded after joining, usually for performance; affects pay in later years. |
| **Pay band** | The salary range a company sets for a level; levelling often matters more than negotiating within it. |
| **Verbal vs written offer** | A verbal offer signals intent; only the written offer contains binding terms to rely on. |
| **Competing offer** | An offer from another company; often the strongest negotiation evidence, if real. |
| **Counter-offer** | A proposal in response to an offer; also, a current employer's attempt to retain you. |
| **Exploding offer** | An offer with a short deadline, designed to limit comparison; it is reasonable to ask for more time. |
| **Notice period** | The time you must serve before leaving your current employer; affects start date and can be longer in some countries. |
| **Non-compete** | A clause restricting work for competitors after leaving; enforceability varies by location *(verify)*. |
