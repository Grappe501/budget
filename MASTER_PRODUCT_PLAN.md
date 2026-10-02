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


## 14. Discovery Inventory 004B — Debt, Restricted Funds, Utilities and Recurring Spending

**Status:** Question 4 inventory remains open for additional remembered items.

### Additional revolving / consumer debt identified
Current remembered obligations now include:
- Lowe's credit account;
- Best Buy credit account;
- Home Depot credit account;
- Capital One general-purpose Visa/credit account, described as one of the larger balances;
- Discover credit account, described as a significant payment/balance obligation;
- U.S. Bank/local card, currently remembered as approximately a $100 monthly payment;
- Synchrony Car Care account;
- another Synchrony-financed account, possibly originally associated with furniture; exact product/creditor relationship to be verified.

All issuer names, balances, APRs, minimums, due dates and account status must later be verified from statements/source records. No guessed financial terms should become authoritative data.

### Medical / provider obligation
- Money is owed to Jeff Powell DDS (dentist). Exact balance, terms and payment arrangement remain to be entered.

Budget must support direct provider balances in addition to conventional loans/cards.

### Legacy farm/feed debt
- Approximately $40,000 is remembered as an outstanding/delinquent feed-related debt associated with the earlier closure of the farm operation.
- It is currently described as sitting unresolved and is a long-term obligation the household wants to address.
- Amount, creditor, legal/status details and enforceable terms must be verified before the system treats them as authoritative.

This establishes a requirement for a **Legacy / Resolution Debt workspace** separate from ordinary monthly revolving debt. It should eventually support verified balance/status, source documents, contacts, payment history, proposed arrangements, settlement/payment-plan scenarios, cash-flow consequences, and notes without the AI making unsupported legal conclusions.

### Collection / judgment-type obligations
- Approximately two or three collection/judgment-type obligations are remembered as having automatic withdrawals.
- Exact creditors, balances, legal status, withdrawal amounts, frequency and remaining obligations remain to be inventoried and verified.

These require a status-aware debt model rather than being treated merely as subscriptions. Automatic withdrawals should be visible in the cash-flow calendar and linked to the underlying obligation.

### Pre-tax / restricted-purpose medical funds
Kelly has a payroll-funded flexible spending-type account used for eligible medical/dental expenses. Exact plan type and rules will be verified later.

Budget should model restricted-purpose funds separately from ordinary cash. The system should be able to understand payroll contributions/deductions, available balance when known, eligible household medical/dental spending, reimbursements/payments, and the effect of using restricted funds instead of checking-account cash. Tax/eligibility rules should be sourced rather than guessed.

### Utilities and essential operating costs identified
Known household operating expenses include:
- electricity;
- water;
- animal feed;
- cellular phone service;
- internet service.

Current household infrastructure also includes:
- septic rather than a recurring sewer utility bill;
- a butane/propane-type fuel system rather than a conventional natural-gas utility bill. Fuel purchases/refills should therefore be modeled as potentially irregular/seasonal household energy expenses rather than assuming a monthly gas bill.

### Recurring digital / entertainment services remembered
Examples currently remembered include:
- Netflix;
- Hulu;
- YouTube TV;
- ChatGPT account/subscription charges;
- other recurring services expected to be discovered through transaction ingestion.

### Recurring-charge discovery requirement
Bank transaction ingestion should automatically identify likely recurring charges rather than requiring perfect manual recall. The system should build a candidate recurring-charge inbox where the household can confirm:
- what the merchant/service actually is;
- whether the charge is expected;
- frequency and typical amount;
- whether the household still uses/wants it;
- which person/purpose it supports;
- whether it is essential, discretionary, business-related, reimbursable or duplicative;
- estimated annual cost;
- effect on financial runway if cancelled or reduced.

Unknown merchant descriptors should remain unresolved until evidence or user confirmation identifies them.

### Expanded debt taxonomy
The product now needs to distinguish at minimum:
- mortgage/property financing;
- vehicle financing;
- active revolving consumer debt being paid down;
- provider/medical debt;
- legacy/delinquent obligations;
- collection/judgment/payment-plan obligations;
- household obligations paid on behalf of another family member;
- planned obligations not yet active.

Debt payoff planning must account for both mathematical optimization and real cash-flow constraints. No recommendation should assume that an extra payment is beneficial if it creates an unsafe near-term cash position.


## 15. Discovery Decision 005 — Reality-First Budget Reconstruction and Wealth Pathway

### Budgeting philosophy
Budget will be **reality-first rather than blank-sheet-first**. The preferred onboarding experience is to ingest approximately 6–12 months of actual household financial transactions and reconstruct how the household truly operates before asking the user to design an ideal budget.

### Proposed onboarding flow
1. Import historical bank/financial transaction records.
2. Preserve the raw source record.
3. Normalize merchant/transaction descriptions without overwriting the original evidence.
4. Detect transfers, reimbursements, income, debt payments and likely recurring transactions so they are not incorrectly treated as ordinary consumption.
5. AI proposes spending categories/subcategories and likely recurring obligations.
6. Assign confidence to classifications.
7. Present a human review queue, prioritizing uncertain, high-dollar, unusual and financially consequential transactions rather than forcing unnecessary review of every obvious item.
8. User confirms/corrects classifications and merchant identities.
9. Store household-specific rules from confirmed corrections so future imports improve.
10. Calculate the observed household baseline from approved data.
11. Generate a proposed forward budget from actual behavior.
12. Recommend specific changes and show their projected effects before the household accepts them.

### AI categorization doctrine
AI categorization is advisory until confirmed or governed by an established household rule. The system should preserve:
- raw imported description;
- normalized merchant;
- proposed category;
- confidence;
- reason/evidence where useful;
- user correction;
- resulting household categorization rule;
- audit history.

This allows Budget to learn without making opaque changes to authoritative financial records.

### Bill/debt enrichment after discovery
A recurring payment discovered in transaction history can begin as a lightweight financial object and later be enriched through a deep-dive workflow with information such as creditor/provider, principal or current balance, APR/interest rate, minimum payment, due date, remaining term/payments, payoff amount/date, autopay status, source statement/document, and other applicable terms.

Not every recurring charge needs debt fields; the object model must distinguish subscriptions, utilities, insurance, debt service and other obligations.

### From observed budget to target budget
Budget should maintain both:
- **Observed baseline:** what the household has actually been doing; and
- **Target plan:** what the household chooses to do next.

The product should never rewrite history to make past spending resemble the target budget. Progress comes from comparing future actual behavior with the chosen plan.

### Wealth Pathway
The long-term objective extends beyond avoiding financial distress. Budget should help the household move through progressively stronger financial states—from cash-flow control, through resilience and debt reduction, toward asset/wealth accumulation.

The AI should not impose a universal definition of the 'best way to wealth.' It should optimize scenarios against household-selected goals, constraints, risk preferences and priorities, using transparent calculations and assumptions.

Candidate pathway decisions may include:
- spending reductions;
- recurring-charge elimination;
- emergency/resilience reserves;
- debt payoff sequencing;
- accelerated principal payments;
- cash-flow released by completed obligations;
- business-income scenarios;
- future savings/investment allocation once appropriate.

For each meaningful recommendation, Budget should show **why**, the numbers/assumptions used, expected benefit, important tradeoffs, effect on cash safety/runway, and alternatives where materially different strategies exist.

### Product UX implication
A major commercial differentiator should be **low-friction financial onboarding**: instead of asking a new user to remember and manually construct their entire financial life, Budget reconstructs a proposed model from their real transaction history and asks them to correct only what needs human knowledge.


## 16. Discovery Decision 006 — Reserve-First Money Flow and Daily Financial Companion

### Household reserve rule
The household wants a default policy of allocating **10% of applicable incoming money to cash reserves before calculating ordinary spendable money**.

The 10% figure is the initial household policy, not a hard-coded product assumption. A commercial version must make reserve policies configurable and define which inflows they apply to.

### Protected reserve behavior
Reserve money should be psychologically and operationally separated from ordinary available cash. The primary dashboard's spendable/available figure should exclude protected reserves by default.

Accessing protected reserve funds should require a deliberate user decision rather than allowing routine spending to silently consume them. A future implementation can use an explicit reserve-release/override flow with reason and audit history.

The application itself must not imply that UI separation creates legal or banking segregation unless funds are actually held in a separate financial account.

### Waterfall concept
Candidate household money flow:
**Income received → classify inflow → reserve allocation → required near-term obligations → available/discretionary cash → debt/goal allocation.**

The exact waterfall must remain flexible enough to handle reimbursements, restricted-purpose funds, business revenue and other inflows that should not automatically receive identical treatment.

### Released-payment rule
When an obligation is eliminated, Budget should immediately recognize the monthly cash flow that has been released. The household wants the reserve policy to continue applying appropriately while the remaining released capacity is intentionally redirected rather than disappearing into lifestyle spending.

The system should ask where released cash should go and model alternatives such as another debt, additional reserves, mortgage principal or another household goal.

### Dynamic debt allocation engine
The household does not want to commit blindly to a single debt philosophy. Budget should continuously compare reasonable payoff strategies, including:
- smaller-balance/cash-flow-release strategies;
- high-interest/interest-minimization strategies;
- hybrid strategies;
- household-defined priority obligations.

Comparisons should show concrete consequences such as interest avoided, payoff dates, monthly cash flow released, effect on reserves, effect on financial safety/runway, and the time until subsequent obligations can be attacked.

The AI may recommend a current strategy, but the underlying calculations and assumptions must be inspectable and the household retains the decision.

### Daily Financial Companion
Budget is intended to be useful at daily resolution, not merely during a monthly budgeting session. The desktop experience should provide an immediate answer to questions such as:
- What can we safely spend today?
- What bills or cash-flow events are approaching?
- Are we moving financially forward or backward?
- Did today's spending materially change the plan?
- What is the highest-value financial action available now?

### Live category allowance
The target budget should support near-real-time category balances. If a category has $100 available for the relevant period and $3 of categorized spending is recorded, the interface should be capable of showing the remaining $97, subject to pending/posted transaction and timing rules.

Category availability should ultimately support appropriate time horizons (day, remaining pay period, month, etc.) rather than misleadingly dividing every category into identical daily allowances.

### Peace-of-mind objective
A core product outcome is **reduced financial cognitive load**. The household should not have to mentally remember which obligation is next, whether spending money is actually available, or whether a financial decision jeopardizes an upcoming bill. Budget should surface upcoming pressure points and explain the current position while keeping the underlying data available for inspection.

### Daily optimization concept
Budget should recalculate recommendations when meaningful inputs change, while avoiding noisy or arbitrary advice. New transactions, income, paid-off obligations, changed balances, upcoming bills, and user scenario changes can update the recommended next financial move.

This creates a core experience analogous to a financial command board: **current position → today's available capacity → upcoming risks → trajectory → recommended next move → scenario controls.**


## 17. Discovery Decision 007 — Physical Reserve Account and Debt-First Product Sequence

### Reserve destination
The household intends to establish a dedicated savings account for cash reserves. Protected reserves should therefore ultimately represent **real funds held separately from ordinary spending cash**, not merely a virtual category inside checking.

### Monthly reserve sweep
Budget should calculate the household's designated reserve contribution and support a monthly reserve sweep into the dedicated savings account.

The initial household policy remains 10% of applicable inflows, subject to later definition of which inflow types qualify and how irregular income is handled. The system should show:
- reserve contribution accrued/owed for the period;
- amount actually transferred;
- reserve account balance when available;
- any shortfall between policy and actual transfer;
- progress toward resilience/runway targets.

### Calculation versus money movement
Reserve calculation and reserve transfer execution are separate capabilities.

Early versions may calculate and instruct the user what to transfer. Direct initiation of bank transfers is a later gated financial-action capability requiring an appropriate financial integration, explicit authorization, strong authentication/security, transaction confirmation, reconciliation, failure/retry handling, revocation controls and an audit trail.

No AI recommendation should independently move household money without the authorization model established for that action.

### Product sequencing
The household's desired financial sequence is now explicitly:
**1. Gain visibility and cash-flow control → 2. establish/protect reserves → 3. control and eliminate debt → 4. build wealth through investing.**

This is a sequencing priority rather than a claim that every dollar must follow an inflexible rule. The scenario engine should still identify material tradeoffs when liquidity, very high-cost debt or other circumstances make an alternative allocation worth considering.

### Investment layer is Phase Two
Investment planning/wealth deployment will become a major second-stage component after the household's debt-management foundation is functioning. The current product-discovery/build effort should architect clean extension points for investment assets and future wealth strategy without allowing investment functionality to distract from the immediate debt/cash-flow mission.

The future investment layer should be treated as a distinct planning and, if ever enabled, action domain with its own suitability, risk, data and authorization requirements.


## 18. Discovery Decision 008 — True Available Cash and Paycheck-to-Paycheck Envelope

### Primary everyday spending number
The household's primary spending figure should be **True Available Cash through the next income event**, not the raw checking-account balance and not merely a monthly category budget.

Conceptually:
**usable cash on hand − protected reserve amount − required obligations before next income − expected normal/necessary spending before next income = true available cash.**

The exact production formula must account for transaction state, timing, known upcoming inflows/outflows and user-confirmed assumptions rather than treating every displayed bank balance as settled cash.

### Pay-period planning horizon
Everyday cash management should be organized around the next meaningful household payday/income event. The interface should show:
- actual account cash;
- protected reserve excluded from spending calculations;
- obligations that must clear before the next payday;
- expected ordinary/necessary spending through that date;
- true discretionary/available cash remaining;
- projected ending cash immediately before the next income arrives.

Users must be able to drill into the calculation and see exactly what is consuming the difference between bank balance and true available cash.

### Forward borrowing / pushing an expense
The household may intentionally choose to defer an obligation or effectively consume capacity from the next pay period. Budget should support modeling that choice rather than pretending it did not happen.

If a decision pushes $X of pressure into the next pay period, the next period should visibly inherit that burden (for example, beginning with a negative carry-forward or equivalent obligation), and the current decision view should show the future consequence before confirmation.

### Decision-against-a-baseline principle
A core UX requirement is that users can always see **what they are making a decision against**. Scenario changes must preserve the original/current plan as a comparison baseline so the household can understand:
- what changed;
- how much cash becomes available now;
- what obligation moved or changed;
- what future period absorbs the consequence;
- whether the change affects safety, reserves or debt progress.

### Cash calendar implication
Budget needs a forward cash calendar/timeline rather than only monthly totals. Income events, bills, automatic withdrawals, expected necessary spending and user-created scenario changes should be placed in time so the engine can identify temporary cash squeezes that a monthly surplus/deficit calculation would miss.

### Dashboard implication
The daily command view should make the hierarchy obvious:
**Bank cash → protected money → committed/needed before payday → TRUE AVAILABLE → projected payday-end position.**

The raw bank balance remains visible for reconciliation, but it should never be presented as synonymous with money that is safe to spend.


## 19. Discovery Decision 009 — Progressive Detail, Receipt Intelligence and Low-Cost AI Economics

### Two-level spending experience
Budget must work well at two levels simultaneously:
1. **Simple transaction level:** a bank transaction can remain a single household/category expense with minimal user effort.
2. **Optional item level:** users who want deeper insight can provide a receipt and allow Budget to reconcile and split the purchase into detailed categories.

Detailed bookkeeping must be optional. A household should receive substantial value without photographing every receipt.

### Receipt capture workflow
A future receipt workflow should allow a user to upload/capture a receipt image (including common image formats such as JPEG) and then:
- extract merchant, date, totals, tax and line items where reliably available;
- propose a match to an imported bank/card transaction;
- propose item-level categories;
- flag uncertainty or mismatches;
- let the user quickly confirm/correct the reconciliation;
- preserve the original receipt/source evidence;
- roll item-level categories back up into simple household views.

Receipt extraction must not silently replace authoritative bank transaction amounts. Differences such as tips, pending authorizations, discounts, tax or imperfect extraction need explicit reconciliation behavior.

### Smallest useful category principle
The data model should permit granular subcategories and item-level classification while the interface defaults to simplicity. Users can drill from household spending → category → subcategory → merchant/transaction → receipt → individual item where data exists.

Granularity should remain useful rather than generating meaningless taxonomy. Household-specific categorization rules should improve over time from confirmed classifications.

### Progressive disclosure UX
The commercial experience should avoid turning personal finance into accounting work. The default path should require very little intervention; advanced detail appears when the user asks for it or when a financially important uncertainty requires confirmation.

### AI cost doctrine
A core commercial requirement is **extremely low variable AI cost per household**. The product should not send every deterministic calculation or previously solved classification to an expensive model.

Architecture should favor, where appropriate:
- deterministic arithmetic/rules for financial calculations;
- local/application-side normalization and caching;
- reusable household merchant/category rules;
- confidence thresholds that avoid repeated AI calls;
- batching when appropriate;
- low-cost model tiers for routine classification/extraction;
- escalation to stronger models only when the expected value justifies it;
- storing structured results so identical work is not repeatedly purchased;
- usage/cost telemetry by AI feature.

AI must be used where it materially improves comprehension, classification, explanation or planning—not as a substitute for ordinary software logic.

### Unit-economics target
The founder wants a mass-market price point potentially in the range of only a few dollars per household/user per month, with an aspirational gross-margin target around **75% after variable AI/processing costs**. These are discovery targets, not validated economics.

Before pricing is finalized, Budget must measure actual costs including AI inference, receipt/document extraction, financial-data connectivity, hosting/storage, payment processing, support and other variable services. Gross margin should be calculated from observed production usage rather than assumed.

### Mission and growth objective
The intended consumer value proposition is broader than expense tracking: help ordinary households understand where their money goes, reduce financial anxiety, get control of debt, build reserves, learn financial decision-making and ultimately progress toward wealth accumulation.

The product should be designed for very low onboarding friction and strong word-of-mouth potential. Product quality and measurable household value—not artificially high AI usage—should drive retention and growth.


## 20. Discovery Decision 010 — Multi-Person Household Workspace and Expense Attribution

### Household-first identity model
Budget should be designed as a **multi-person household financial workspace** rather than a single-user budget with additional logins added later.

Each household member who participates in household spending should be able to have an individual authenticated profile/login and an appropriate view of the shared household financial position.

### Separate financial roles
The data model must distinguish at least:
- **transaction actor:** who made/submitted the purchase or transaction;
- **payer/funding source:** which household account or funding source paid;
- **beneficiary/attribution:** who the expense was for;
- **household expense:** expenses intentionally attributed to the household rather than divided among people.

These concepts must not be collapsed into one 'person' field.

### Household expenses should stay household expenses
Ordinary shared necessities such as groceries and normal household products should default to household-level spending. Budget should not manufacture arbitrary per-person allocations merely because multiple people consume them.

The purpose of attribution is useful understanding, not surveillance or false precision.

### Optional individual/shared attribution
For expenses where attribution is meaningful—such as dining out—the user should be able to choose:
- household/shared;
- me only;
- another individual;
- any selected combination of household members.

A receipt/transaction can therefore be associated with the people who actually participated without requiring a more granular split unless the user wants one.

### Example: family-supported asset
A vehicle can belong to or primarily benefit one family member while its payment comes from the shared household budget. Budget must preserve that distinction so the household can understand both responsibility and beneficiary without distorting cash flow.

### Member experience
A household member's future login/view should support appropriate functions such as:
- see current safe-to-spend/household position according to permissions;
- see relevant category allowances;
- submit/upload receipts;
- identify themselves as transaction actor;
- attribute eligible purchases to themselves, other household members or the household;
- review/correct transactions they are responsible for;
- understand how current spending affects the shared plan.

### Permission requirement
Multi-person access requires a household role/permission model. Not every member should automatically receive identical access to sensitive balances, debts, documents, account connections, settings or financial actions. Exact roles and permissions remain a later discovery/design decision.

### Commercial architecture implication
Household membership, invitations, permissions, attribution and shared financial state are foundational domain concepts and should be present in the initial architecture even if the first household alpha begins with only one administrative user.


## 21. Discovery Decision 011 — Household Roles, Visibility and Optional Open-Family Finance

### Current household clarification
Grace is not currently intended to receive a Budget login because she is living outside the household. Her vehicle remains an example of an expense/asset that may be financially supported by the household without making the beneficiary a household platform user.

### Owner role
Budget needs a household **Owner** role. A household can have more than one owner (for example, two adults/partners). Owners/admin-authorized users should control household membership, visibility and permissions.

### Permission-based visibility
Budget should not hard-code financial visibility solely from age or family relationship. Household owners should be able to decide how much another household member can see.

A future permission model should support at least:
- full household financial visibility for owners/authorized members;
- limited/member-specific financial visibility;
- the ability for owners to intentionally grant broader household visibility to another member.

Exact role names and permission granularity will be designed later.

### Open-family option
Some households may deliberately choose an **open-family financial model** in which children or other household members can see the broader family budget. Budget should support this as an explicit owner-controlled option rather than assuming household finances must always be hidden from non-owner members.

This creates an educational opportunity: where a household chooses transparency, younger members can learn how income, bills, spending, reserves and financial tradeoffs work in a real household context.

### Privacy principle
Openness must be opt-in. Adding a household member should not automatically expose sensitive account balances, debt details, income, financial documents or financial-action controls. Owners must affirmatively grant broader access.

### Architectural implication
Authorization must be enforced at the data/action layer, not merely by hiding interface elements. Household membership, roles and permissions therefore belong in the foundational security/domain architecture.


## 22. Discovery Decision 012 — Active Overspend Recovery and Reserve Lock

### Active recovery behavior
Budget should respond actively when actual spending breaks the current plan. An overage should trigger a recalculation and recovery workflow rather than merely displaying a red negative category.

The recovery experience should explain:
- amount of the overage;
- current-period cash impact;
- which future obligations/capacity are affected;
- viable ways to repair the plan;
- consequence of each proposed repair.

### Recovery sources
Budget may propose reallocating remaining discretionary capacity or other household-approved flexible spending. It should also support **spend-forward**: intentionally consuming spendable capacity from the next income/pay period.

If the household spends forward by $X, the next pay period must visibly inherit that $X burden before the decision is accepted. Budget should show the resulting next-period true available cash and any safety implications.

### Protected reserve is NOT a default recovery source
Protected cash reserves must be excluded from ordinary overspend-repair options by default. The product should behave as though that money is unavailable for routine spending.

Using reserve funds as an overspend source should require a household owner/admin to explicitly enable reserve-access functionality. The default setting is **OFF**.

Even when reserve access is enabled, a reserve withdrawal should be a deliberate action with clear impact on runway/resilience and an audit record; it should not become a one-click suggestion for routine overspending.

### Behavioral design principle
Budget should create constructive friction around breaking long-term protections while making ordinary plan repair easy to understand. The goal is not punishment or shame; it is to make the financial tradeoff visible at the moment of decision.

### Scenario example
If a category is $50 over budget, Budget can show repair paths such as reducing another flexible category, reducing remaining discretionary cash, or carrying $50 into the next pay period. If spend-forward is selected, the next pay period's available amount is reduced by $50 immediately in the forecast. Protected reserve is absent from these choices unless an owner has previously enabled reserve access.

### Permission implication
Reserve-access settings are financially consequential household controls and belong to the owner/admin permission layer. Non-owner household members should not be able to enable protected-reserve spending unless specifically granted that authority.
