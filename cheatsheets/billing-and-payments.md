# Billing and Payments Cheatsheet

Terms for payment flows, ledgers, usage-based billing, invoicing, compliance and the finance metrics engineers meet in fintech and SaaS systems.

> Related: [Billing, payments and order-to-cash skills](../skills/billing-payments-and-order-to-cash.md) · [Security and compliance skills](../skills/security-and-compliance.md) · [System design questions](../topics/03-system-design/README.md) · [Data and SQL questions](../topics/07-data-and-sql/README.md) · [Business and customer terms](business-and-customer-terms.md) · [Distributed systems](distributed-systems.md)

## Correctness foundations

| Term | What it means |
|---|---|
| **Idempotency** | Repeating an operation has the same effect as doing it once; prevents double charges when requests are retried. |
| **Idempotency key** | A unique client-supplied value identifying one logical request, so the server returns the original result on a retry. |
| **Outbox pattern** | Saving an event in the same database transaction as the business change, then publishing it; prevents lost or phantom events. |
| **Webhook** | An HTTP callback a provider sends on events; may arrive late, twice or out of order, so verify and deduplicate. |
| **Minor units** | Storing money as integers of the smallest unit, such as cents or paise; avoids floating-point rounding errors. |
| **FX rate** (Foreign Exchange rate) | The conversion rate between currencies; store the rate used on each transaction so totals stay auditable. |
| **Audit trail** | A permanent record of who changed what and why; needed to explain any charge years later. |

## Ledgers and accounting

| Term | What it means |
|---|---|
| **Double-entry ledger** | Every transaction posts equal debits and credits across accounts, so errors show up as imbalances. |
| **Debit and credit** | The two sides of a ledger entry; whether each raises or lowers a balance depends on the account type. |
| **Journal entry** | One balanced posting of debits and credits for a transaction; the basic unit of a ledger. |
| **Chart of accounts** | The organised list of all ledger accounts, such as cash, receivables and revenue; structures financial reporting. |
| **Append-only ledger** | Entries are never edited or deleted, only added; balances are derived, which gives a reliable history. |
| **Reversal entry** | A new entry that cancels an earlier one; the correct way to fix a mistake without rewriting history. |
| **Trial balance** | A report showing total debits equal total credits; a basic integrity check on the ledger. |
| **AR** (Accounts Receivable) | Money customers owe the business for invoiced goods or services; an asset on the balance sheet. |
| **AP** (Accounts Payable) | Money the business owes its suppliers; a liability on the balance sheet. |
| **Revenue recognition** | Recording revenue when it is earned, not when cash arrives; an annual prepayment is usually recognised monthly. |
| **Deferred revenue** | Cash received for services not yet delivered; a liability until it is earned. |
| **ASC 606 and IFRS 15** (Accounting Standards Codification; International Financial Reporting Standards) | The US and international revenue recognition standards; finance teams own interpretation *(verify)*. |
| **Write-off** | Removing an uncollectable debt from receivables and recording it as a loss; also known as bad debt. |

## Card payment flow

| Term | What it means |
|---|---|
| **PSP** (Payment Service Provider) | A company such as Stripe or Adyen that lets merchants accept many payment methods through one integration. |
| **Payment gateway** | The technical service that securely passes payment details from merchant to processor; often bundled within a PSP. |
| **Issuer** | The cardholder's bank; approves or declines payments and bears responsibility for the cardholder relationship. |
| **Acquirer** | The merchant's bank or partner that receives card payments on the merchant's behalf. |
| **Card network** | The scheme connecting issuers and acquirers, such as Visa or Mastercard; sets rules and routes transactions. |
| **Authorisation** (authorise) | The issuer approves a charge and holds the funds, but no money has moved yet; holds eventually expire. |
| **Capture** | The merchant claims authorised funds, often when goods ship; can sometimes be partial. |
| **Settlement** | Funds actually move from issuer to acquirer, then to the merchant's balance, usually days after capture. |
| **Payout** | The PSP transfers settled funds, minus fees, to the merchant's bank account. |
| **Void** | Cancelling an authorisation before capture, releasing the hold without a refund. |
| **Refund** | Returning captured money to the customer at the merchant's initiative. |
| **Soft vs hard decline** | Soft declines, such as insufficient funds, may succeed on retry; hard declines, such as a stolen card, should not be retried. |
| **MoR** (Merchant of Record) | The legal seller in a transaction, responsible for tax, refunds and chargebacks; can be outsourced to a platform. |
| **Interchange fee** | A fee paid by the acquirer to the issuer on card transactions; forms part of a merchant's costs. Rates are regulated in places *(verify)*. |
| **MDR** (Merchant Discount Rate) | The total percentage a merchant pays to accept card payments; includes interchange and other fees. |

### Authorise, capture, settle, pay out

| Step | What happens | Money moves? |
|---|---|---|
| Authorise | Issuer approves and holds funds | No, only held |
| Capture | Merchant claims the held funds | Not yet |
| Settle | Issuer funds reach acquirer and merchant balance | Yes |
| Pay out | PSP sends funds to merchant's bank | Yes |

## Disputes, fraud and compliance

| Term | What it means |
|---|---|
| **Chargeback** | A cardholder disputes a charge and the issuer forcibly reverses it; adds fees, and high rates risk penalties. |
| **Representment** | The merchant's evidence-backed challenge to a chargeback, submitted through the acquirer. |
| **3DS** (3-D Secure) | An issuer authentication step for online cards; reduces fraud and can shift liability for fraud to the issuer. |
| **SCA** (Strong Customer Authentication) | A European requirement for multi-factor authentication on many online payments, usually met with 3DS *(verify)*. |
| **PCI DSS** (Payment Card Industry Data Security Standard) | Security rules for anyone handling card data; compliance effort depends on how card data is touched *(verify)*. |
| **PCI scope** (Payment Card Industry scope) | The systems that store, process or transmit card data; keeping it small cuts audit effort and risk. |
| **SAQ** (Self-Assessment Questionnaire) | A PCI form sized to your exposure: SAQ A for fully outsourced card handling, SAQ D for the broadest *(verify)*. |
| **PAN** (Primary Account Number) | The full card number; the core data PCI rules protect, so avoid ever storing it. |
| **CVV** (Card Verification Value) | The card's short security code; PCI rules prohibit storing it after authorisation. |
| **Card tokenisation** | Replacing card numbers with tokens from the provider; takes real card data out of your systems. |
| **Network token** | A card-network-issued token that survives card reissue; can improve approval rates and security. |
| **Card updater** | A service that refreshes expired or replaced card details automatically, reducing failed recurring charges. |

### Refund vs credit note vs chargeback

| | Refund | Credit note | Chargeback |
|---|---|---|---|
| Started by | Merchant | Merchant | Cardholder through issuer |
| What it does | Returns money to the payment method | Reduces an invoice or gives account credit | Forcibly reverses the charge |
| Extra cost | Usually low | None | Fee and reputation risk |

## Other payment rails

| Term | What it means |
|---|---|
| **UPI** (Unified Payments Interface) | India's real-time account-to-account payment system, run by NPCI; dominant for retail payments. Rules change *(verify)*. |
| **NPCI** (National Payments Corporation of India) | The organisation that operates UPI and other Indian retail payment systems. |
| **VPA** (Virtual Payment Address) | A UPI identifier such as name@bank that hides bank details; used to send and collect money. |
| **UPI Autopay** (Unified Payments Interface Autopay) | Recurring UPI payments under a customer-approved mandate; limits and rules apply *(verify)*. |
| **RBI e-mandate** (Reserve Bank of India e-mandate) | Indian rules on recurring card payments, requiring registration, notifications and additional authentication above limits *(verify)*. |
| **ACH** (Automated Clearing House) | The US bank-to-bank network for batch payments and direct debits; cheap but slower than cards. |
| **SEPA Direct Debit** (Single Euro Payments Area) | A European scheme for pulling euro payments from bank accounts under mandate; common for B2B subscriptions. |

## Usage-based billing

| Term | What it means |
|---|---|
| **Metering** | Capturing usage events, such as API calls or tokens; events must be durable and deduplicated, as they become money. |
| **Aggregation** | Rolling metered events into billable quantities per period; must handle duplicate and late events. |
| **Rating** | Applying prices to measured usage to compute charges; separates price logic from usage collection. |
| **Rate card** | The published or negotiated list of prices per unit; contract versions matter when rating historical usage. |
| **Reserve and commit** | Holding estimated credit before work, then charging actual use and releasing the remainder; prevents prepaid overspend under concurrency. |
| **Prepaid credits** | Balance bought in advance and drawn down as used; needs atomic deduction, expiry rules and low-balance alerts. |
| **Committed spend** (drawdown) | A customer commits a total spend for a period and uses it over time, usually for a discount. |
| **Overage** | Usage beyond the included allowance, charged at an agreed rate; a common source of bill disputes. |
| **Tiered pricing** | Each band of usage has its own unit price, with only units within that band priced at it. |
| **Volume pricing** | Total quantity decides one unit price applying to all units; differs from tiered pricing at band edges. |
| **Invoice cut-off** | The time after which usage rolls into the next bill; late events need a clear policy. |

### Tiered vs volume pricing

| | Tiered | Volume |
|---|---|---|
| Rate applies to | Units within each band | All units |
| At a band boundary | Smooth increase in total | Total can fall |

## Subscriptions and invoicing

| Term | What it means |
|---|---|
| **O2C** (Order-to-Cash) | The whole process from quote and order through invoicing, payment, collections and revenue recognition. |
| **Billing cycle** | The recurring period a subscription is charged for, such as monthly or annually, from a billing anchor date. |
| **Proration** | Charging part of a price for part of a period, such as crediting unused time on a mid-cycle upgrade. |
| **Invoicing** | Producing a legal request for payment with line items and tax; key record for finance and audit. |
| **Invoice immutability** | A finalised invoice is never edited; corrections use credit notes or new invoices, preserving legal records. |
| **Credit note** | A document that reduces an issued invoice or leaves a credit balance; the correct way to fix billing errors. |
| **Dunning** | The process of retrying failed payments and reminding customers before suspending access; recovers revenue that would otherwise churn. |
| **Grace period** | Time access continues after payment failure while dunning runs; balances revenue protection against customer goodwill. |
| **Involuntary churn** | Customers lost through failed payments rather than choosing to leave; reduced by dunning and card updaters. |
| **Reconciliation** | Matching internal records against provider reports and bank statements; catches missing, duplicated or wrong transactions. |
| **Settlement report** | A provider file listing transactions, fees and payouts for a period; a key reconciliation input. |
| **Reconciliation break** | A mismatch found during reconciliation; needs an owner, investigation and correction, with an ageing report for the rest. |

## Tax

| Term | What it means |
|---|---|
| **VAT** (Value Added Tax) | A consumption tax charged at each supply stage, with businesses reclaiming tax on inputs; used widely outside the US *(verify)*. |
| **GST** (Goods and Services Tax) | A VAT-style consumption tax used in India and elsewhere; rates and rules vary *(verify)*. |
| **CGST, SGST and IGST** (Central, State and Integrated Goods and Services Tax) | India's GST split: central plus state for intra-state sales, integrated for inter-state sales *(verify)*. |
| **Place of supply** | The rule that decides where a sale is taxed, and so which tax applies; a common cross-border complication *(verify)*. |
| **Sales tax** | A US-style tax usually charged only at final sale, with rates varying by state and locality *(verify)*. |
| **Tax nexus** | A connection to a US state that creates a duty to collect its sales tax; can arise from sales volume *(verify)*. |
| **Reverse charge** | The buyer rather than seller accounts for tax, common in cross-border B2B sales *(verify)*. |
| **Tax-inclusive vs tax-exclusive price** | Whether the displayed price already contains tax; affects rounding, invoices and how totals are calculated. |
| **E-invoicing** (electronic invoicing) | Issuing invoices in a government-specified electronic format, sometimes with real-time clearance; mandates vary by country *(verify)*. |

## Finance metrics

| Term | What it means |
|---|---|
| **MRR** (Monthly Recurring Revenue) | Normalised subscription revenue per month; annual plans count as one-twelfth, and one-off fees are excluded. |
| **ARR** (Annual Recurring Revenue) | Recurring revenue on an annualised basis, roughly MRR times 12; the headline figure for SaaS scale. |
| **DSO** (Days Sales Outstanding) | Average days taken to collect after invoicing; lower means faster cash. Calculated as receivables over credit sales, times days. |
| **ARPU** (Average Revenue Per User) | Revenue divided by active users; useful for tracking pricing and mix changes over time. |
| **Billing vs revenue vs cash** | Billing is what was invoiced, revenue is what was earned, cash is what was collected; these often differ. |
