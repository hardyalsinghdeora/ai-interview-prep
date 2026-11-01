# Business and Customer Terms Cheatsheet

Commercial, sales, delivery and stakeholder terms used in solutions and forward deployed engineering work.

> Related: [Roles overview](../roles/README.md) · [Customer scenarios questions](../topics/09-customer-scenarios/README.md) · [Billing and payments](billing-and-payments.md) · [Interview frameworks](interview-frameworks.md)

## Engagement stages

| Term | What it means |
|---|---|
| **Discovery** | Early conversations to understand the customer's problem, users, data and constraints; shapes everything that follows. |
| **POC** (Proof of Concept) | A small, time-boxed test showing something can work technically, usually on limited or sample data. |
| **POV** (Proof of Value) | A trial focused on proving measurable business benefit, not only technical feasibility; usage varies between companies. |
| **Pilot** | A limited rollout to real users with real data, to test value and adoption before wider deployment. |
| **MVP** (Minimum Viable Product) | The smallest version that solves the core problem and generates real learning. |
| **Success criteria** | Measurable conditions agreed upfront that define whether a POC or pilot worked; they prevent later disputes. |
| **Production deployment** | The system running live for its intended users, with support, monitoring and security in place. |
| **Time to value** | How long a customer takes to get real benefit after buying; a key measure for deployment teams. |

### POC vs pilot vs production

| | POC | Pilot | Production |
|---|---|---|---|
| Question answered | Can it work? | Does it deliver value here? | Can it run at scale? |
| Users and data | Few users, sample data | Real users, real data, limited scope | All intended users |
| Typical length | Weeks | Weeks to months | Ongoing |

## Contracts and procurement

| Term | What it means |
|---|---|
| **MSA** (Master Service Agreement) | The overarching legal contract between two parties, setting general terms so later work can be added quickly. |
| **SOW** (Statement of Work) | Defines specific project scope, deliverables, timeline and fees under an MSA; the reference point for scope disputes. |
| **SLA** (Service Level Agreement) | A contractual promise on service levels such as uptime or response time, usually with credits if missed. |
| **NDA** (Non-Disclosure Agreement) | A contract to keep shared information confidential; usually signed before detailed discussions. |
| **DPA** (Data Processing Agreement) | Contract defining how a vendor handles personal data for a customer; needed under privacy laws such as GDPR *(verify)*. |
| **RFI** (Request for Information) | A buyer's early, broad request to learn what vendors offer, used to build a shortlist. |
| **RFP** (Request for Proposal) | A formal buyer document asking vendors to propose solutions and pricing against stated requirements. |
| **RFQ** (Request for Quotation) | A request for pricing on an already well-defined need; focuses on cost more than approach. |
| **PO** (Purchase Order) | A buyer's formal document authorising a purchase; many companies cannot pay without one. |
| **Order form** | A short document stating what is bought, for how long and at what price, usually under an MSA. |
| **Change order** | A formal, agreed amendment to the SOW for changed scope, time or cost; protects both sides. |
| **Change control** | The process for assessing and approving scope changes; stops scope creep without saying no to everything. |
| **Scope creep** | Gradual, unapproved growth in project scope; a common reason for late and unprofitable projects. |
| **Security review** | The customer's assessment of a vendor's security, often by questionnaire; can block deals for weeks, so start early. |

### MSA vs SOW vs SLA

| Document | Covers | Answers |
|---|---|---|
| MSA | General legal terms | How do the parties work together? |
| SOW | One specific project | What exactly will be delivered? |
| SLA | Service performance | What level of service is promised? |

## Value and revenue

| Term | What it means |
|---|---|
| **ROI** (Return on Investment) | Benefit relative to cost, as (benefit minus cost) divided by cost; the core language of business cases. |
| **TCO** (Total Cost of Ownership) | Full lifetime cost, including licences, infrastructure, staff and maintenance; shows the real cost beyond purchase price. |
| **Payback period** | How long until cumulative benefits cover the cost; executives often find it easier to judge than ROI. |
| **Business case** | A document justifying an investment using costs, benefits, risks and alternatives; needed to secure budget. |
| **ARR** (Annual Recurring Revenue) | Recurring revenue on a yearly basis; the main scale measure for subscription businesses. |
| **MRR** (Monthly Recurring Revenue) | Recurring revenue on a monthly basis; useful for tracking short-term growth. |
| **ACV** (Annual Contract Value) | The yearly value of one contract; helps compare deals of different lengths. |
| **TCV** (Total Contract Value) | The full value of a contract over its whole term, including one-off fees. |
| **NRR** (Net Revenue Retention) | Revenue this year from last year's customers, including expansion, churn and downgrades; above 100% means growth without new logos. |
| **GRR** (Gross Revenue Retention) | Like NRR, but excluding expansion; capped at 100%, showing how well existing revenue is kept. |
| **Churn** | Customers or revenue lost over a period; high churn undermines growth however strong sales are. |
| **Logo** | Sales shorthand for a customer company; "new logo" means a newly won customer. |
| **Upsell** | Selling a higher tier or more of the same product to an existing customer. |
| **Cross-sell** | Selling a different product to an existing customer. |
| **Expansion** | Growth in revenue from existing customers through upsell, cross-sell or more usage. |
| **Renewal** | A customer extending a contract when it ends; the moment value delivered is tested. |
| **CAC** (Customer Acquisition Cost) | Average sales and marketing spend to win one customer; compared against lifetime value. |
| **LTV** (Customer Lifetime Value) | Total gross profit expected from a customer over the relationship; also written CLV. |
| **COGS** (Cost of Goods Sold) | Direct cost of delivering the product, such as hosting and support; high LLM costs can squeeze margins. |

### NRR vs GRR

| | NRR | GRR |
|---|---|---|
| Includes expansion | Yes | No |
| Can exceed 100% | Yes | No |
| Tells you | Overall growth from the customer base | How much revenue is kept |

### ARR vs ACV vs TCV

| Metric | Scope | Example: 3-year deal at 100k a year, plus 30k one-off |
|---|---|---|
| ARR | Recurring only, annualised | 100k |
| ACV | Yearly contract value (definitions vary on one-off fees) | 100k or 110k |
| TCV | Whole term, including one-offs | 330k |

## Sales and go-to-market

| Term | What it means |
|---|---|
| **TAM** (Total Addressable Market) | Total revenue available if a product won its entire market; used to size opportunity. |
| **SAM** (Serviceable Addressable Market) | The part of TAM a product can realistically serve, given its geography and product fit. |
| **SOM** (Serviceable Obtainable Market) | The part of SAM a company can realistically win in the near term. |
| **GTM** (Go-To-Market) | The plan for reaching and selling to customers, including segment, pricing, channels and sales motion. |
| **ICP** (Ideal Customer Profile) | A description of the customers most likely to buy and succeed; focuses sales and product effort. |
| **Pipeline** | The set of open deals at different stages; used to forecast future revenue. |
| **Sales quota** | The revenue target assigned to a seller for a period; drives their priorities and timing pressure. |
| **CRM** (Customer Relationship Management) | The system tracking accounts, contacts and deals; the record of sales activity. |
| **Technical win** | The point where the customer's technical team agrees the product meets their needs; the SE's main goal. |
| **MEDDPICC** (Metrics, Economic buyer, Decision criteria, Decision process, Paper process, Identify pain, Champion, Competition) | A qualification framework for complex B2B deals that shows what is known and where a deal is at risk. |
| **BANT** (Budget, Authority, Need, Timeline) | A simpler sales qualification framework, often used early and for smaller deals. |
| **Economic buyer** | The person who can approve the spend; a technically successful deal fails without them. |
| **Champion** | An influential insider who actively sells your solution internally; losing one puts deals and renewals at risk. |
| **Paper process** | The legal, procurement and security steps needed to sign; a common cause of late surprises. |

## Customer-facing roles

| Term | What it means |
|---|---|
| **TAM** (Technical Account Manager) | A post-sale technical adviser who guides a customer's adoption, escalations and health; not the market-size TAM. |
| **AE** (Account Executive) | The salesperson who owns a deal and revenue target; closes new business and often expansions. |
| **SDR** (Sales Development Representative) | Qualifies inbound and outbound leads and books meetings for AEs; generates early pipeline. |
| **BDR** (Business Development Representative) | Often focuses on outbound prospecting; the split from SDR varies by company. |
| **SE** (Solutions Engineer) | The technical partner to sales; runs discovery, demos and POCs to win the technical decision. |
| **SA** (Solutions Architect) | Designs how a product fits a customer's architecture; can work before and after the sale. |
| **FDE** (Forward Deployed Engineer) | A software engineer embedded with customers, shipping solutions and feeding patterns back to product. |
| **CSM** (Customer Success Manager) | Owns a customer's post-sale outcomes, adoption and renewal health; usually less technical than a TAM. |
| **AM** (Account Manager) | Manages the commercial relationship with existing customers, including renewals and expansions. |
| **PS** (Professional Services) | A vendor's paid delivery team for implementation, migration and custom work. |
| **SI** (System Integrator) | A third-party firm that implements and integrates technology for customers, often on large programmes. |

## Delivery and governance

| Term | What it means |
|---|---|
| **RAID log** (Risks, Assumptions, Issues, Dependencies) | A living record of project risks and blockers with owners and dates; useful only if actively reviewed. |
| **Risk vs issue** | A risk might happen; an issue already has. Risks need mitigation plans, while issues need resolution now. |
| **RAG status** (Red, Amber, Green) | A traffic-light project health rating; red should trigger a plan, not blame. Not retrieval-augmented generation. |
| **Steering committee** | Senior sponsors from both sides who meet periodically to review progress and make major decisions; the top escalation forum. |
| **Working group** | The regular meeting of hands-on people who plan work and clear day-to-day blockers. |
| **Critical path** | The longest chain of dependent tasks, which sets the earliest finish date; a slip here delays the project. |
| **Milestone** | A significant checkpoint marking a stage's completion; good milestones are verifiable, not only dates. |
| **UAT** (User Acceptance Testing) | Testing by real users or customer representatives to confirm the system meets needs before go-live. |
| **Go-live** | The moment a system starts being used for real; requires readiness checks and a rollback plan. |
| **Cutover** | The planned switch from an old system to a new one, usually with a detailed, rehearsed runbook. |
| **Hypercare** | An intensive support period straight after go-live, with faster responses and closer monitoring, before normal support. |
| **Handover** | Transferring responsibility to support or customer operations, with documents and training; prevents ongoing dependence on the builders. |
| **Change management** | Helping people adopt new ways of working through communication, training and support; adoption often fails without it. |
| **Adoption** | How much intended users actually use the solution in their work; deployed but unused delivers no value. |
| **QBR** (Quarterly Business Review) | A regular review with customer leaders of value delivered, health, risks and next steps. |
| **NPS** (Net Promoter Score) | Customer loyalty measure: share of promoters minus share of detractors, from a "would you recommend" question. |
| **CSAT** (Customer Satisfaction Score) | A survey measure of satisfaction with a specific interaction or service. |

## Stakeholders

| Term | What it means |
|---|---|
| **Stakeholder** | Anyone affected by a project or able to influence it; missing one early often causes late blockers. |
| **Stakeholder map** | A view of who has influence and interest, and what each person cares about; guides engagement. |
| **Executive sponsor** | The senior leader who owns the outcome, provides budget and clears obstacles; projects without one tend to stall. |
| **Decision-maker** | The person with authority to say yes; distinguish them from those who only influence or use the product. |
| **Blocker** (detractor) | A person who can slow or stop progress, often with legitimate concerns worth understanding. |
| **End user** | The person who actually uses the system day to day; their workflow decides whether it gets adopted. |
| **SME** (Subject Matter Expert) | Someone with deep domain knowledge; vital for requirements, labelling data and judging output quality. |
| **RACI** (Responsible, Accountable, Consulted, Informed) | A chart clarifying each person's role per task or decision; avoids confusion about ownership. |
| **Escalation path** | The agreed route for raising problems to more senior people; agreeing it early keeps escalations calm. |
| **C-suite** | Top executives such as the CEO (Chief Executive Officer) and CFO (Chief Financial Officer); they care about outcomes, risk and cost. |
| **CIO** (Chief Information Officer) | The executive responsible for internal IT systems; often owns integration and vendor decisions. |
| **CTO** (Chief Technology Officer) | The executive responsible for technology strategy; often the customer's senior technical voice. |
| **CISO** (Chief Information Security Officer) | The executive responsible for security; can approve or stop a deployment on risk grounds. |
| **Field CTO** (Field Chief Technology Officer) | A senior vendor technologist who advises customer executives and represents the field to product. |
