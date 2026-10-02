# Budget — Master Product Discovery & Build Plan

**Status:** Discovery in progress  
**Repository:** Grappe501/budget  
**Canonical branch:** main  
**Started:** October 2, 2026

## 1. Product Vision

Budget is a privacy-first household financial operating system being designed first around a real household and ultimately architected so it can become a sellable consumer product.

The first implementation will run locally on the development computer. Cloud deployment and commercial packaging come later, after the household workflow is proven.

The system should help a household understand where its money goes, plan where money should go, anticipate obligations, manage bills, identify patterns, make better financial decisions, and progressively automate appropriate financial work while preserving human control.

## 2. Product Development Doctrine

1. **Real household first, reusable product second.** Build against actual household workflows rather than hypothetical personas, while separating household-specific configuration from reusable application logic.
2. **Local-first development.** Initial application runs on the development machine. Netlify deployment is explicitly deferred.
3. **Human-controlled money movement.** Analysis, reminders, preparation, and recommendations may become highly automated. Actual financial transactions require deliberate security and authorization architecture.
4. **AI assists; records remain authoritative.** AI may classify, explain, detect patterns, forecast, and recommend, but raw financial records and deterministic calculations remain the source of truth.
5. **Never commit secrets.** No bank credentials, API keys, account numbers, tokens, or other secrets belong in Git.
6. **Auditability.** Imports, categorization changes, bill changes, AI suggestions, approvals, and future financial actions should be traceable.
7. **Commercial architecture from the beginning.** Household-specific data and rules must be separable from the core product so a future multi-household version does not require a rewrite.

## 3. Initial Capability Universe

These are discovery candidates, not yet approved requirements.

### Household & Financial Model
- Household members and roles
- Income sources and pay schedules
- Accounts and balances
- Recurring obligations
- Variable expenses
- Assets and liabilities
- Goals and sinking funds
- Household-specific financial rules

### Budgeting
- Monthly and pay-period budgeting
- Category budgets
- Fixed versus variable spending
- Zero-based / envelope-compatible planning
- Rollover categories
- Actual vs planned
- Cash-flow calendar
- Forecasting and scenario planning

### Transactions & Data Ingestion
- CSV/file imports from banks and cards
- Import normalization
- Duplicate detection
- Merchant normalization
- Transaction splitting
- Transfers
- Refunds/reimbursements
- Recurring transaction detection
- Future direct financial-data connections, subject to provider/security design

### AI Financial Organization
- Suggested transaction categories
- Merchant recognition
- Household-specific categorization memory
- Spending summaries
- Anomaly detection
- Recurring-charge discovery
- Subscription discovery
- Plain-language financial Q&A
- Forecast explanations
- Savings opportunities
- User-confirmed learning loop

### Bills
- Bill registry
- Due dates
- Minimum/statement/full balances
- Autopay status
- Reminders
- Cash-flow impact
- Payment confirmation/reconciliation
- Future payment initiation only after dedicated security, provider, authorization, and audit design

### Dashboard
- Cash available
- Upcoming income
- Upcoming bills
- Safe-to-spend concept
- Budget health
- Category progress
- Cash-flow forecast
- Net worth
- Debt
- Savings goals
- Alerts and action center
- AI financial briefing

### Security & Privacy
- Local-first storage strategy
- Encryption strategy
- Secret management
- Sensitive-field handling
- Authentication/household permissions
- Backups
- Export/delete controls
- Audit log
- Commercial privacy/security requirements

## 4. AI / OpenAI Architecture Rule

Budget may use the same locally managed OpenAI credential already available to the operator's development environment, but the credential itself must never be copied into this repository or committed to Git.

The eventual build should use environment variables and a documented local setup path. Before implementation, the exact local environment strategy will be selected. Commercial deployment will require per-environment secret management rather than dependence on another project's folder.

AI features must distinguish:
- deterministic financial calculations;
- source-grounded financial facts;
- classifications/inferences;
- recommendations;
- actions requiring human approval.

## 5. Development Phases

### Phase 0 — Product Discovery
Walk through the household's actual financial life one question at a time. Decide what the system must know, show, automate, explain, protect, and eventually commercialize.

### Phase 1 — Product Specification
Convert discovery into:
- feature registry;
- information architecture;
- data model;
- screen inventory;
- workflow maps;
- AI capability map;
- security model;
- automation/approval model;
- MVP and later-release boundaries.

### Phase 2 — Technical Blueprint
Select the local application architecture, database, import pipeline, AI layer, security approach, testing strategy, and migration path toward a commercial application.

### Phase 3 — Cursor Build Plan
Create large, gated implementation passes for Cursor with acceptance tests and operator review points.

### Phase 4 — Household Alpha
Run the application against real household data locally. Measure classification accuracy, budgeting usefulness, forecasting usefulness, bill reliability, usability, and trust.

### Phase 5 — Product Hardening
Remove household assumptions, create onboarding, permissions, security controls, provider integrations, support systems, legal/privacy architecture, and commercial-grade UX.

### Phase 6 — Commercial Product
Package and deploy only after the local household system proves the underlying workflows.

## 6. Discovery Method

Discovery will proceed conversationally, **one question at a time**.

For each answer we will determine:
- the real-world need;
- required data;
- desired user experience;
- automation level;
- AI role;
- security implications;
- reusable commercial requirement;
- whether it belongs in MVP, later, or should not be built.

The master plan will evolve as decisions are made.

## 7. Current Decisions

- Repository: Grappe501/budget.
- Initial development: local computer.
- Initial proving environment: real household finances.
- Long-term intent: potentially sellable consumer application.
- AI: OpenAI-enabled.
- Secrets: never stored in Git.
- Financial ingestion: required.
- AI-assisted transaction categorization: required.
- Budget planning: required.
- Dashboard: required.
- Bill management: required.
- Optional automation is desired, with financial-action security/authorization to be designed before implementation.
- Netlify deployment: deferred during initial build.
- Cursor: expected implementation environment after the master design is mature.

## 8. Open Discovery Queue

The following domains must be resolved through the one-question-at-a-time process:

Household model; financial accounts; income; budget philosophy; transaction ingestion; categorization; bills; debt; savings; goals; subscriptions; cash flow; credit cards; reimbursements; irregular expenses; taxes; assets; net worth; forecasting; alerts; AI assistant; document/receipt handling; permissions; privacy; backups; financial-data connectivity; payment automation; mobile needs; reports; exports; commercial onboarding; pricing/product boundaries.

## 9. Immediate Discovery Question

**What is the single most important thing you want Budget to tell you when you open it each day?**

This answer will define the product's primary dashboard hierarchy and help establish what Budget is fundamentally optimizing for.


## 10. Discovery Decision 001 — Financial Safety Is the North-Star Experience

### Primary question
The first screen must answer: **Are we financially safe right now, and for how long?**

The product should translate the household's real financial position into an understandable runway such as days, weeks, months, or years of financial safety. This must not be a simplistic bank-balance calculation. It should ultimately consider available cash, expected income, observed spending, required bills, debt obligations, recurring charges, timing of cash flows, and appropriate reserves.

### Dashboard hierarchy
The primary dashboard should progress naturally from:
1. current financial safety/runway;
2. forward projection if nothing changes;
3. why the projection looks that way;
4. spending and income breakdown;
5. upcoming obligations and cash-flow pressure points;
6. recommended opportunities to improve the trajectory;
7. drill-downs into the underlying transactions, categories, bills, debts, and recurring charges.

### Baseline / trajectory engine
Budget should establish the household's actual baseline from real records rather than relying only on a manually entered ideal budget. It should compare income with observed spending and obligations and project what happens if behavior remains substantially unchanged.

The user should later be able to create scenarios and immediately see how changes affect financial runway—for example reducing a category, cancelling a recurring charge, paying off a debt, changing a payment date, receiving different income, or changing savings behavior.

### Spending intelligence
Every meaningful spending category should be drillable from summary to transactions. The system should help the household understand actual behavior, compare it with the desired plan, identify change opportunities, and see the projected consequence of those changes.

### Bill intelligence
A bill should become a financial object rather than merely a calendar reminder. Where applicable, its record should support amount, due date, recurrence, payment status, autopay status, terms, remaining payments or balance, interest/fees when relevant, source documentation, and payoff implications.

### Debt elimination planning
The system should support household debt/payoff planning that shows how eliminating one obligation changes future cash flow and how freed cash can be redirected toward other obligations or goals. Strategies should be modeled transparently rather than presented as unexplained instructions.

### Recurring-charge intelligence
Budget should detect likely recurring transactions and subscriptions from imported financial activity, group related merchants, surface duplicate or overlapping services, allow the household to identify what each charge actually is, track whether it is still wanted, and model the savings from cancellation.

### Bill-pay operating principle
The desired end state includes a simple payment experience and eventual autopay management. However, payment initiation and account-connected automation are later secure-integration capabilities.

The system must distinguish at least:
- scheduled/expected payment;
- external autopay already configured;
- recommended payment;
- user-approved payment;
- confirmed/reconciled payment.

A future automation layer should understand cash-flow timing well enough to warn when an otherwise normal automatic payment could create a short-term cash problem before expected income arrives. The long-term objective is a household stable enough that routine obligations can safely remain automated.

### Financial coaching doctrine
The product should progressively make the user more capable at managing household finances. Recommendations should therefore explain the relevant numbers, reasoning, tradeoffs, and projected consequences. The user should be able to inspect the underlying data rather than being asked to trust an opaque AI conclusion.

### Product implication
The emerging product concept is a **Household Financial Command System**: a system of record, forecasting engine, financial coach, decision simulator, obligation manager, and—only with appropriate future security and authorization—financial action layer.


## 11. Discovery Decision 002 — Safety, Resilience, and Emergency Runway

### Safety is directional, not merely a reserve balance
For this household, the primary definition of **financially safe** is a structurally cash-positive trajectory: recurring/expected income is sufficient to cover the household's actual ongoing spending and obligations, and the household is not steadily consuming reserves to maintain its current lifestyle.

Budget should therefore prominently calculate and explain **net cash direction**. A proposed financial move—paying off an obligation, making an unusually large payment, increasing spending, moving money, or adopting another plan—should be modeled before execution. If the move causes the projected household trajectory to become cash-negative, Budget should make that consequence obvious.

### Higher resilience state — working name: Fortified
A separate status above ordinary safety will represent the household's ability to withstand a complete loss of income for one year.

Working definition: **Fortified = sufficient appropriate accessible reserves to sustain approximately 12 months of modeled household expenses with zero new income.**

The final name is intentionally unresolved. The calculation must eventually define which assets count as accessible reserves and which expenses belong in the applicable spending baseline.

### Multiple runway views
A single runway number is insufficient. Budget should eventually show at least:
- **Current-lifestyle runway:** how long accessible reserves sustain observed/current household spending if income becomes zero.
- **Essential/survival runway:** how long reserves sustain a deliberately reduced emergency budget if income becomes zero.
- **Gap to 12-month resilience:** money and/or spending reduction required to reach the one-year target.

### Emergency budget / Survival Mode
Budget should support a preplanned reduced-spending state. The household can identify what can be cut, paused, reduced, renegotiated, or eliminated if income falls or another financial shock occurs.

The system should help distinguish essential obligations from discretionary spending and should model how each reduction extends runway. The objective is to answer not merely 'How long can we survive?' but also 'What changes would make our resources last longer, and by how much?'

### Decision simulation requirement
Before meaningful financial decisions, Budget should be capable of comparing the current baseline with the proposed change and showing effects on:
- monthly cash surplus/deficit;
- near-term cash availability;
- ordinary financial safety;
- current-lifestyle runway;
- emergency runway;
- progress toward the 12-month resilience target;
- downstream debt/bill obligations where applicable.

This establishes a core product rule: **Budget should forecast the consequence before the household commits to the financial move.**


## 12. Discovery Decision 003 — Income Ledger and Six-Month Income Pipeline

Budget must model both **income received** and **income expected/planned over at least the next six months**. Forecast income must not be treated as equally certain.

### Known household income patterns
Initial household use cases include:
- Kelly's salaried employment, paid every other Friday;
- detailed paycheck composition when available, including gross pay, taxes, insurance and other deductions, so the system can reconcile gross compensation with actual household cash received;
- a variable bonus generally available approximately quarterly;
- Steve's campaign-season income, including its applicable start/end window;
- mileage and similar reimbursements;
- prospective SaaS/product income from the planned Elves Tribal software launch;
- irregular software/project payouts associated with work performed when funding/projects are available.

Amounts and detailed structures remain to be entered later.

### Income classification requirement
Incoming cash must be classified by economic meaning. At minimum the model should distinguish:
- earned/ordinary household income;
- variable compensation/bonus;
- temporary or time-bounded income;
- business/product revenue versus personal take-home income;
- reimbursement/expense recovery;
- irregular/project income;
- prospective/planned income not yet received.

A reimbursement must not automatically inflate household earnings because it may simply offset an earlier household-funded expense. Business revenue must not automatically be treated as personal spendable income because costs, taxes, retained business cash, and owner compensation may need to be separated.

### Income confidence model
Future income should carry a confidence/status rather than being silently included as guaranteed cash. Candidate states include:
- received/reconciled;
- committed/high-confidence;
- expected;
- variable/range-based;
- prospective/opportunity;
- excluded from safety baseline.

The exact vocabulary will be refined later. The Financial Safety Engine should default to conservative assumptions and make clear which future income is included in each projection.

### Six-month income pipeline
Budget should provide a forward income view showing expected timing, expected amount or range, source, type, confidence, recurrence/end date, and actual receipt when it occurs.

Forecasts should support at least a conservative case, expected case, and upside/scenario case without presenting uncertain future revenue as fact.

### Paycheck intelligence
When paycheck details are supplied, Budget should be capable of tracking gross compensation through deductions to net deposit. This creates future opportunities to analyze taxes, insurance, benefits, withholding, and potential household-level changes while preserving source records.

### SaaS/business boundary
The planned Elves Tribal SaaS revenue introduces a future requirement to distinguish **business economics from household economics**. Budget should eventually model how business revenue, business expenses, taxes/reserves, and owner distributions/compensation flow into the household without commingling the concepts.

The exact business structure and tax treatment are deliberately unresolved and should not be assumed by the application or AI.


## 13. Discovery Inventory 004A — Accounts, Property, Vehicles, Debt and Insurance (Part 1)

**Status:** incomplete inventory; discovery answer intentionally paused and will continue before advancing to the next question.

### Banking and day-to-day spending
- One joint bank account currently serves as the household's primary banking/spending account.
- Current day-to-day purchasing is primarily through debit rather than new credit-card borrowing.
- Existing credit-card balances/debts are being paid down, but the household is not currently using those cards for ordinary new credit purchases.
- Bank/debit transaction ingestion is therefore a high-priority source for understanding actual household spending behavior and identifying reductions.

### Farm / primary property
The household has a farm consisting of approximately 15 acres and an approximately 1,600-square-foot older ranch house, with a mortgage.

Budget should treat this as more than a recurring mortgage bill. The future property record should be capable of tracking:
- mortgage balance and terms;
- principal versus interest when source data permits;
- scheduled payoff trajectory;
- additional-principal scenarios;
- estimated property value;
- estimated equity (clearly labeled as an estimate where applicable);
- ownership goal/progress;
- insurance and other property-related recurring costs;
- scenario comparisons showing what additional payments do to payoff time, interest, liquidity, and overall household financial safety.

A major household goal is ultimately to own the land/property free of the mortgage. The system should help determine when accelerated payoff is financially sustainable rather than assuming faster payoff is always optimal.

### Vehicles
Known vehicles/obligations currently include:
- Grace's vehicle, with a loan/payment through Carvana that the household is paying;
- Kelly's Nissan Altima, with a loan/payment that is approaching payoff;
- a Buick Enclave owned without a current loan/payment.

Budget should support assets whose user/beneficiary and payer differ—for example, a vehicle used by an adult child but paid by the household.

### Vehicle insurance
The household currently pays insurance covering all three vehicles. Vehicle insurance should be linkable to the applicable vehicles while still appearing correctly as a household obligation.

### Property / premises insurance
Known or planned insurance-related items include:
- homeowners insurance, currently described as an annual expense;
- a planned renters-insurance need for the headquarters location that is not yet an active household expense and therefore should be represented as a planned/future obligation rather than falsely recorded as currently paid.

The exact relationship between property insurance, homeowners coverage, mortgage escrow, and other property coverage remains to be verified from source documents rather than assumed.

### Product requirement — Financial relationship graph
Budget should not model everything as isolated transactions. It should support relationships such as:

**Asset → financing/debt → payment → insurance → responsible household member → cash-flow effect → payoff/equity goal.**

This relationship model will allow the system to answer questions such as:
- What does this asset really cost the household each month/year?
- How much equity do we have?
- When will the debt be paid off?
- What happens if we add $X to principal?
- How much cash flow is released after payoff?
- Where should that released payment be redirected?
- Does accelerating payoff reduce our near-term financial safety?

### Existing credit-card payoff requirement
Although credit cards are not currently being used for ordinary new purchases, existing balances belong in the debt-elimination system. They should not be confused with active spending instruments merely because payments appear in the bank ledger.
