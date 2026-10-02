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


## 23. Discovery Decision 013 — Windfall Allocation and Wealth-Builder Transition

### Unexpected money should receive a recommended job
Unexpected/irregular income should not default to unallocated spendable cash. Once an inflow is identified and classified, Budget should proactively recommend how the deployable amount should be allocated according to the household's current financial stage and policies.

Examples include bonuses, irregular project income and other unplanned household inflows. Reimbursements, restricted-purpose funds and business gross revenue must still be classified correctly before this policy is applied; they are not automatically household windfalls.

### Current household allocation doctrine
The current intended progression is:
**protect reserves according to household policy → optimize debt reduction while the household is below its resilience target → transition eligible surplus/windfalls toward Wealth Builder after the one-year reserve milestone is achieved.**

The exact waterfall, including how quickly reserves are built versus debt is attacked, remains subject to scenario design and later household configuration. This section records the strategic intent rather than prematurely hard-coding a universal financial rule.

### One-year reserve milestone as phase transition
The previously identified approximately 12-month zero-new-income resilience target becomes a major state transition in the product.

Before the milestone, Budget's principal optimization objective is financial stability/resilience plus debt control/elimination. Once the milestone is satisfied under the household's defined reserve calculation, eligible excess capital can begin flowing into the future **Wealth Builder** layer according to household-selected strategy.

### Windfall recommendation engine
For an eligible windfall, Budget should be capable of showing:
- gross inflow and classification;
- amounts excluded because they are reimbursement, restricted, business-retained, tax-reserved or otherwise unavailable;
- reserve allocation required by current policy;
- remaining deployable amount;
- recommended debt/goal allocation;
- expected interest savings and/or cash-flow release;
- resulting payoff changes;
- resulting reserve/runway position;
- current progress toward the Wealth Builder transition;
- user-controlled alternatives/override.

### No silent execution
Recommendation is not authorization. Budget can produce a recommended allocation immediately, but it must not independently make debt payments, transfers or investments without the applicable user authorization and financial-action controls.

### Wealth Builder boundary
Wealth Builder is the planned post-foundation financial layer. Its detailed investment strategy, asset allocation, products and execution rules are intentionally deferred until the debt/cash-control system is designed and operating. Budget 1.0 should expose progress toward eligibility without prematurely becoming an investment platform.

### Product narrative implication
The application now has a clear household journey:
**Understand → Control → Protect → Eliminate Debt → Become Resilient → Build Wealth.**

This journey should eventually be visible in the product so users understand not only today's numbers but the financial stage they are working toward next.


## 24. Discovery Decision 014 — Budget 1.0 Hard Scope Boundary and Post-Consumer-Debt Optionality

### Hard stop before Wealth Builder
The current product cycle is **Budget 1.0: household budgeting, cash-flow control, reserves and debt management**. Detailed Wealth Builder/investment functionality is explicitly out of scope for this build.

Budget 1.0 may preserve data-model/architecture extension points and show that wealth building is a future stage, but it should not spend product-development effort designing investment portfolios, brokerage execution, asset allocation or other Wealth Builder mechanics yet.

### Why the boundary exists
The immediate objective is to get Budget into real household use quickly enough to learn from actual behavior, then refine it until the experience is exceptionally simple, trustworthy and valuable. Household alpha usage is part of product development, not an afterthought.

### Post-consumer-debt flexibility
Budget must not permanently encode one universal rule for what happens after consumer debt is eliminated. Future conditions may differ substantially depending on how quickly the household progresses, interest rates, mortgage terms, income, reserve position and other circumstances.

At that future decision point, Budget should be capable of comparing options such as:
- accelerating the farm mortgage toward pure zero debt;
- beginning Wealth Builder allocations while continuing scheduled mortgage payments;
- splitting surplus between mortgage acceleration and Wealth Builder;
- other household-approved scenarios.

The system should compare consequences rather than selecting a permanent philosophy years in advance.

### Long-term household aspiration
**Pure zero debt** remains an important household goal, including eventual ownership of the farm free of mortgage debt. That aspiration should be trackable without forcing every surplus dollar toward the mortgage when another strategy is intentionally selected.

### Product-value ambition
The commercial ambition is to create a consumer experience with very high perceived utility and polish while maintaining a mass-market price target in the low-single-digit dollars per month range where economics permit. The founder's working target is approximately $3–$5/month, subject to later validation against real infrastructure, financial-data, AI, payment-processing and support costs.

Pricing claims and comparisons to higher-priced products should not be treated as validated until market research and production cost measurements are completed.

### Immediate build priority
Before expanding scope, Budget 1.0 should become excellent at the core loop:
**ingest reality → understand the household → calculate true available cash → protect reserves → anticipate obligations → guide daily spending → recover from deviations → optimize debt → explain every recommendation → learn from household corrections.**

The household should then use this system in real life, generating the evidence needed for subsequent UX hardening and eventual commercial product decisions.


## 25. Discovery Decision 015 — CSV-First Household Alpha

### First real-data ingestion rail
The initial Steve/Kelly household alpha will begin with **CSV transaction export(s) from the joint bank account**. Direct bank connectivity is not required to begin real-world use.

This is an intentional speed-to-learning decision: the household can obtain historical transaction data immediately, allowing Budget's categorization, baseline reconstruction, recurring-charge discovery and cash-flow logic to be tested against real finances before financial-account aggregation is added.

### Initial history target
The importer should support approximately 6–12 months of historical transactions, with the ability to add older/newer exports later. The system should not assume a particular bank CSV schema.

### Source-preservation rule
Imported financial evidence must be preserved. Budget should retain enough import provenance to distinguish the original source values from normalized/enriched application data.

AI categorization, merchant normalization and user corrections must not destructively rewrite the raw imported record.

### CSV import requirements
The alpha importer should be designed to support:
- file upload/drop;
- preview before committing an import;
- flexible column mapping for date, description, amount/debit/credit and other available fields;
- import/source identity and timestamps;
- validation and clear malformed-row reporting;
- duplicate detection/idempotent re-import behavior;
- support for overlapping date-range exports without double-counting;
- preservation of raw descriptions/amounts/dates;
- normalized transaction layer separate from source evidence;
- transaction classification and confidence;
- user correction and learned household rules;
- import rollback/recovery where practical;
- reconciliation summaries so the user can verify what was accepted, skipped or flagged.

### Import safety
The system should never silently discard or invent financial rows to make an import balance. Ambiguities, unsupported formats and possible duplicates should be surfaced for review.

### Household-alpha workflow
The first meaningful product loop should become executable early:
**export bank CSV → import → preserve raw ledger → normalize → classify → identify recurring charges/income/transfers/debt payments → review uncertainties → establish observed baseline → generate first household budget and cash-flow model.**

### Direct bank connectivity deferred
Live bank aggregation/sync remains an important later capability for the commercial product, but it should not block Budget 1.0 household-alpha development. The architecture should keep imported-source abstractions clean enough that a future bank connector can feed the same normalized ledger without replacing the CSV pathway.

CSV/manual import should remain useful even after connectivity exists for historical backfill, unsupported institutions, troubleshooting and user data portability.


## 26. Discovery Decision 016 — The First-Run Financial Reveal

### Desired first-run moment
The household alpha should create immediate value from a single historical bank CSV. After approximately one year of transactions is ingested, Budget should attempt to reconstruct and present a recognizable financial operating model of the household rather than merely showing a transaction table.

The desired experience is essentially:
**upload one bank history → Budget reconstructs how the household financially operates → user reviews/corrects → a working budget is already populated.**

### First-run analysis
From transaction history, Budget should attempt to identify/propose:
- recurring income and approximate pay rhythm;
- recurring payments and subscriptions;
- likely bill/payment timing based on observed transaction dates;
- merchant/payee identity where reasonably inferable;
- spending categories/subcategories;
- likely debt payments/transfers versus ordinary spending;
- utilities and other household obligations;
- variable recurring expenses and approximate ranges;
- monthly/annual spending patterns;
- unusual or unresolved transactions requiring human review.

### Automatically populated financial calendar
Budget should turn detected recurring activity into a proposed forward financial calendar showing, where supported by evidence:
- who/what is paid;
- typical amount or amount range;
- observed payment/withdrawal timing;
- recurrence frequency;
- proposed category;
- confidence/status.

Observed withdrawal timing must not be mislabeled as a contractual due date. A true due date becomes authoritative only when supported by a statement, bill terms or user confirmation.

### Automatically populated observed budget
After analysis and review, Budget should generate a first working budget from historical reality. The user should not face a blank category form after uploading data.

The initial budget should expose both recurring fixed/semifixed obligations and variable spending baselines so the household can see where money has actually been going before setting targets.

### Confidence and review layer
Every inferred financial fact should carry an appropriate provenance/confidence state. The first-run experience should separate:
- high-confidence observed patterns;
- probable patterns needing quick confirmation;
- unresolved/ambiguous items requiring user knowledge.

The review experience should focus attention on financially meaningful uncertainty rather than asking the user to approve hundreds of obvious rows one at a time.

### Household financial breakdown
The reveal should summarize the historical period in understandable household terms, including income observed, major spending categories, recurring obligations, debt-payment patterns, subscriptions/recurring charges, average cash-flow direction and other evidence needed to form the initial household model.

### Product success criterion
A successful first-run experience should make the user feel that Budget **understood their financial life from the evidence they already had**, substantially reducing setup work. The first reveal must remain auditable: every conclusion should be drillable back to the transactions/patterns that caused Budget to propose it.


## 27. Discovery Decision 017 — Guided Financial Interview

### Post-import onboarding mode
After the first-run financial reveal, Budget should lead the user through a **guided financial interview** rather than dropping them into a dashboard and expecting them to discover/correct model errors manually.

### Interview purpose
The system performs the first analytical pass; the human supplies household knowledge only where it materially improves accuracy. The interview should progressively convert inferred patterns into a trusted household financial model.

### Question prioritization
Questions should be prioritized by financial consequence and uncertainty, considering factors such as:
- high-dollar recurring transactions;
- uncertain income/debt/transfer classification;
- obligations that materially affect true available cash;
- recurring transactions with unclear merchant identity;
- timing/due-date uncertainty;
- suspected duplicates/transfers/reimbursements;
- lower-dollar subscriptions and minor classification questions later.

The interview should not force users through hundreds of obvious transactions before delivering value.

### Conversational confirmation examples
Budget may ask focused questions such as:
- 'I see a payment around this time most months. What is it?'
- 'I think this is your electric utility. Is that correct?'
- 'Is this a debt payment, transfer, purchase, or something else?'
- 'I observed this payment near the 14th; is that the actual due date or simply when you normally pay it?'
- 'Does this expense belong to the household, a person, or a business/reimbursable activity?'

### Progressive enrichment
Confirmed answers should update the appropriate structured objects and household rules so the user is not repeatedly asked the same question on future imports.

Where a bill/debt needs deeper information unavailable from the bank ledger, the interview can mark it for later enrichment rather than blocking onboarding.

### Progress and resumability
The interview must show meaningful progress and be safely resumable. Users should be able to pause and use the application before every low-priority uncertainty is resolved.

### Dashboard transition
The dashboard can exist immediately after import, but it should clearly distinguish provisional versus confirmed information. As the guided interview progresses, confidence in the household model increases and recommendations can become correspondingly more specific.

### UX objective
The interview should feel like **Budget learning the household**, not like the user performing data entry for Budget.


## 28. Discovery Decision 018 — Proactive Budget Advisor

### Core product role
Budget's AI companion is explicitly the **Budget Advisor**. It should proactively monitor the household financial model and surface useful guidance rather than requiring the user to know what question to ask.

### Proactive advisory triggers
Candidate triggers include:
- spending pace likely to exhaust true available cash before the next income event;
- projected negative carry-forward into the next pay period;
- an upcoming bill or automatic withdrawal creating a cash squeeze;
- materially unusual transaction amounts;
- recurring charge increases or newly detected subscriptions;
- category spending materially outside its normal/target range;
- income arriving late, early or at an unexpected amount;
- an opportunity to make a debt payment without compromising safety;
- a newly released payment after an obligation is eliminated;
- reserve-policy shortfalls;
- meaningful changes in runway/safety state;
- financial milestones worth recognizing;
- unresolved high-impact transactions that need household confirmation.

### Relevance threshold
Proactivity must not become notification noise. The advisor should use materiality, urgency, confidence and user preferences to decide what deserves interruption. Routine low-value activity should generally update the model silently.

### Advisor interaction pattern
A useful proactive message should normally answer:
1. **What changed?**
2. **Why does it matter?**
3. **What happens if nothing changes?**
4. **What can we do about it?**
5. **What would each meaningful option do to the household plan?**

The user should be able to drill directly from the advisory into the evidence and scenario controls.

### Forecast-driven advice
The advisor should not be limited to retrospective alerts. Where the underlying data supports it, it should warn about likely future pressure before the household reaches the problem—for example, a current spending pace that is projected to create a deficit before payday.

### Tone and behavioral principle
The Budget Advisor should be calm, specific, educational and action-oriented. It should not shame users for spending or use anxiety as an engagement tactic. Its purpose is to reduce financial cognitive load and improve decisions.

### Calculation and AI boundary
Deterministic financial calculations, ledger state and forecast math remain authoritative. AI can interpret, prioritize and explain those results. It should not invent balances, obligations, due dates or mathematical outcomes.

### Notification architecture implication
Budget 1.0 should model advisor events/messages even if the earliest local alpha surfaces them only inside the application. External delivery channels (push, email, SMS, etc.) can be added later without changing the underlying advisory-event model.


## 29. Discovery Decision 019 — The Grandfather Approach

### Advisor voice doctrine
The Budget Advisor should communicate like a **financially brilliant grandfather talking to people he loves**: experienced, calm, practical, protective, plainspoken and willing to tell the truth.

This is a product behavior doctrine, not a requirement to impersonate an elderly person or use stereotyped language.

### Communication principles
The Advisor should:
- explain money in ordinary language;
- be direct when a pattern is materially harming the household;
- connect today's behavior to concrete future consequences;
- teach the reasoning rather than simply issue instructions;
- respect that the household controls its own priorities;
- acknowledge tradeoffs and real life;
- encourage progress without manufacturing praise;
- focus on what can be done next;
- use actual household numbers whenever available.

The Advisor should not:
- shame, humiliate or moralize about spending;
- use fear to drive engagement;
- speak in unnecessary financial jargon;
- treat every optimization as mandatory;
- pretend certainty where the underlying data is uncertain;
- make the user feel that enjoying money is inherently irresponsible.

### Teach through consequences
When Budget identifies a costly pattern, it should translate that pattern into understandable annual and goal-level consequences.

Example style:
'You spent about $10,800 eating out last year. I'm not telling you to stop. But if you trimmed $400 a month, that's $4,800 a year we could put to work getting you free of this debt. Want to see what that would change?'

The exact numbers must always come from the household model; examples are illustrative only.

### Advice pattern
For significant recommendations, the Grandfather Approach should generally communicate:
**Here is what I see → here is why it matters → here is what I would consider → here is what that would change → you decide.**

### Educational objective
A successful Budget Advisor should gradually make the user more financially capable. Over time, users should understand cash flow, interest, debt tradeoffs, reserves, recurring costs and opportunity cost well enough that they increasingly understand the reasoning before the Advisor explains it.

### UX implication
Advisor copy is part of the product's financial education system, not decorative personality text. Voice consistency should eventually be tested across onboarding, overspend recovery, debt recommendations, alerts, milestones and scenario comparisons.


## 30. Discovery Decision 020 — Affordability Judgment and Strong Advice

### Grandfather may give a clear recommendation
The Budget Advisor should not retreat into neutral data presentation when the household asks for a financial judgment. When the model provides sufficient evidence, Grandfather may give a clear recommendation such as:
**'You have enough cash to pay for it, but I don't think you can afford it yet.'**

The recommendation must then explain why and what would need to change for the answer to improve.

### Cash availability is not affordability
Budget must explicitly distinguish:
- **Can I pay for it?** — whether sufficient cash/credit capacity exists to complete the transaction; and
- **Can I afford it?** — whether the purchase fits the household plan without unacceptable damage to obligations, safety, reserves, debt progress or near-term cash flow.

### Affordability engine
For a proposed discretionary purchase, Budget should be able to evaluate factors such as:
- true available cash;
- required obligations before upcoming income events;
- protected reserve policy;
- spend-forward/carry-forward created;
- effect on safety/runway;
- debt payoff delay and added interest where calculable;
- impact on current household goals;
- whether the purchase would require breaking a protected financial rule.

### Strong-advice pattern
When advising against a purchase, Grandfather should communicate:
**what is possible → what is affordable → why → measurable consequence → what milestone/condition would make it affordable → user decides.**

Example style:
'Yes, the money is physically in the account. But I wouldn't call this affordable right now. It would put the next two pay periods under pressure and push this debt payoff back. If we wait until [modeled condition], you can do it without borrowing from your future cash flow.'

All factual amounts/timelines must come from deterministic household calculations rather than invented AI estimates.

### Advice is not unilateral control
A strong 'no' from Grandfather is advisory unless the household has separately configured an enforceable spending control. The household retains authority over its money. The system should record scenario/decision consequences but should not silently block transactions or move funds merely because the Advisor recommends against them.

### Educational objective
Grandfather should teach the household to evaluate purchases in terms of opportunity cost and trajectory, gradually replacing the common question 'Do we have the money?' with the more useful question 'Can we afford this without undermining what we're building?'


## 31. Discovery Decision 021 — Dynamic Purchase Pathways and Creative Scenario Planning

### 'Not yet' should become a pathway to yes
When Grandfather determines that a desired purchase is not currently affordable, the interaction should not end with rejection. Budget should offer to turn the purchase into a goal and construct one or more realistic pathways toward making it affordable.

### Purchase pathway engine
For a proposed purchase, Budget should be able to model combinations of:
- saving a fixed amount per paycheck/pay period;
- target purchase timing;
- cash purchase versus down payment plus financing;
- expected debt payoff dates that release monthly cash flow;
- redirecting released payments after an obligation ends;
- expected/variable income events where appropriately confidence-weighted;
- windfalls/bonuses under household allocation rules;
- spending reductions the household elects to make;
- different down-payment amounts;
- financing payment/term/interest assumptions supplied by the user or verified source;
- effects on reserve policy, true available cash, debt trajectory and safety.

### Timeline-aware example
A valid strategy may deliberately span financial phases. For example, Budget might determine that saving a defined amount each paycheck until a vehicle loan is paid off creates a larger down payment, after which some of the released vehicle-payment capacity could support a carefully modeled new obligation. This should be compared against waiting longer and paying cash or other reasonable alternatives.

The engine must calculate the actual consequences rather than assuming that taking a new note after another is paid off is automatically desirable.

### Creative but grounded planning
Grandfather should look across the household financial timeline for combinations that help achieve a goal, but creativity must remain grounded in known or explicitly modeled numbers. It should not fabricate future income, financing terms, asset values or savings capacity.

### Dynamic goals
Purchase plans should automatically recalculate when material inputs change—for example:
- an obligation is paid off earlier/later than expected;
- income changes;
- actual savings differ from plan;
- the purchase price changes;
- financing terms change;
- an unexpected expense occurs;
- household safety/reserve position changes.

The system should explain what changed and how the target date/path moved.

### Scenario comparison
Rather than presenting only one answer, meaningful purchase decisions should support side-by-side scenarios such as:
**buy sooner with financing / build larger down payment / wait and pay cash / postpone while accelerating debt.**

Comparisons should focus on cash-flow burden, total financing cost where known, time to purchase, effect on debt-free progress, reserves and financial safety.

### Product principle
Grandfather's job is not merely to constrain spending. It is to help households accomplish things they value **without accidentally sacrificing the financial future they are trying to build.**


## 32. Discovery Decision 022 — Optional Guilt-Free Spending and Automatic Debt Redirection

### Safe enjoyment should be an option
Budget should recognize that a sustainable household financial plan can include discretionary enjoyment. When the household's current position supports it, Grandfather may proactively identify an amount that can be spent on a date, outing or other discretionary enjoyment without materially undermining required obligations, protected reserves or the active debt plan.

### Offered, not automatically consumed
Guilt-free/fun money should be presented as an **option**, not assumed spending. The household can accept, reduce or decline the suggested discretionary amount.

### Declined fun money gets a job
If the household declines an offered discretionary allocation, that capacity should not simply disappear back into an undefined spending pool. Budget should recommend redirecting it toward the current debt-paydown target or other applicable priority under the household's active allocation rules.

### Safe-to-enjoy calculation
A suggested discretionary amount should be grounded in the household model, considering at minimum:
- true available cash through the relevant income horizon;
- upcoming required obligations;
- protected reserve policy;
- existing spend-forward/carry-forward;
- active debt-payment commitments;
- safety/runway trajectory;
- other already-approved goals.

### Grandfather behavior
Grandfather should not treat all discretionary spending as failure. When the household can genuinely afford something, the Advisor should be capable of saying so plainly and without guilt. Conversely, it should not manufacture 'fun money' merely to improve engagement when the household's cash position does not support it.

### Decision flow
A candidate interaction is:
**Grandfather identifies safe discretionary capacity → household accepts/reduces/declines → accepted amount becomes an explicit discretionary allowance → declined amount is proposed for debt/priority allocation → forecast updates immediately.**

### Product principle
Budget is optimizing for a financially sustainable life, not maximum deprivation. The system should help users distinguish intentional, affordable enjoyment from spending that quietly undermines their goals.


## 33. Discovery Decision 023 — Cash Wallet and Voice-First Quick Capture

### Cash withdrawal model
When Budget detects a cash withdrawal, it should be able to represent that money as an **unallocated Cash Wallet** rather than forcing the entire withdrawal into a final spending category immediately.

The bank transaction remains the authoritative record that cash left the bank. Subsequent cash-spending entries explain how that withdrawn cash was used and reduce the unallocated Cash Wallet balance; they must not create duplicate bank outflows.

### After-the-fact reconciliation
Users should be able to explain cash spending later. Budget should support partial allocation over time—for example, allocating part of a withdrawal today while leaving the remainder as cash still held/unexplained.

### Voice-first quick capture
The eventual mobile experience should make voice a primary low-friction input method. A user should be able to open Budget and say a short natural-language statement such as:
'I used $100 of my cash for [purpose].'

Budget should parse the statement into a proposed structured entry, identify the likely cash source/wallet where possible, propose the category and attribution, and request clarification only when materially ambiguous.

### Confirmation and auditability
Voice interpretation is not authoritative merely because AI parsed it. Before committing consequential or ambiguous changes, the interface should show the interpreted amount/category/source in a quick confirmation flow. The resulting structured record should preserve enough provenance to show that it originated from user-provided voice/text input.

### Broader conversational capture
The same interaction model should eventually support quick household updates such as:
- explaining a transaction;
- correcting a category;
- identifying a merchant;
- noting who a purchase was for;
- adding context to a receipt;
- recording a cash purchase;
- updating a known bill fact;
- asking Grandfather a financial question.

### Simplicity doctrine
The household should not need to navigate accounting forms for routine corrections. The product should translate ordinary human language into proposed structured financial data while keeping the user in control of uncertain interpretations.

### Mobile architecture implication
Although the first development environment is local/desktop-oriented, Budget 1.0 architecture and interaction design should anticipate a phone-friendly companion experience. Voice capture must be treated as an input channel into the same underlying financial domain model, not as a separate ledger.


## 34. Discovery Decision 024 — Teach Budget Once: Household Financial Memory

### Core learning principle
Budget should **learn from confirmed household corrections** so users do not repeatedly categorize or explain the same financial activity.

When a user identifies a merchant, payee, recurring transaction or descriptor and indicates that the interpretation should apply in the future, Budget should create/update a household-specific classification rule.

### Merchant fingerprinting
Bank transaction descriptions often contain unstable text such as store numbers, terminal/reference IDs, dates or other changing suffixes. Matching should therefore support a normalized **merchant fingerprint** rather than relying only on exact raw-string equality.

The system should preserve the original descriptor while deriving stable matching features for future classification.

### Learned-rule examples
Household rules may eventually capture facts such as:
- normalized merchant/payee identity;
- default category/subcategory;
- whether the transaction is household, individual, business-related or reimbursable;
- likely recurring obligation identity;
- default attribution where appropriate;
- known transfer/payment relationships;
- user-confirmed descriptor aliases.

### Confidence and exception handling
A learned rule should apply automatically when the new transaction sufficiently matches the confirmed pattern. If important characteristics differ materially—such as an unusual amount, conflicting descriptor, different account context or ambiguous match—Budget may flag the transaction rather than blindly applying the rule.

### User control
Users should be able to inspect, correct and remove learned household rules. A correction to one transaction should not necessarily rewrite all history unless the user explicitly chooses a bulk/history correction.

### Cost implication
Household financial memory is also part of the low-AI-cost strategy. Once a transaction pattern has been reliably learned, routine future matches should generally use deterministic/local matching rather than purchasing another AI inference for the same problem.

### Long-term experience target
As the household uses Budget, the amount of manual categorization should fall substantially. The desired trajectory is:
**first import requires meaningful review → Budget learns corrections → future imports need fewer confirmations → routine household activity becomes largely self-organizing while anomalies still receive attention.**


## 35. Discovery Decision 025 — Retroactive Learning and Whole-History Corrections

### Corrections can repair the financial model backward and forward
When a user discovers that a learned merchant/category rule is wrong, Budget should support correcting the matching historical transactions **and** updating the future household rule in one deliberate action.

### Correction scope
The correction workflow should make scope explicit. Useful choices include:
- this transaction only;
- matching historical transactions only;
- this transaction and future rule;
- **all matching history plus future rule**.

For Steve's household, the desired default capability is the final option: when the classification truly was wrong, Grandfather should make it easy to fix everything.

### Preview before bulk correction
Before applying a historical bulk change, Budget should show what it believes matches—for example the number of transactions, date range and aggregate dollars affected—and explain the proposed before/after classification. Ambiguous matches should be separable from high-confidence matches.

### Derived data must recalculate
A historical correction should propagate through dependent household intelligence, including where applicable:
- category totals and historical averages;
- observed/target budgets;
- spending trends;
- recurring-pattern analysis;
- forecasts;
- true-available calculations when relevant to the affected period/model;
- Advisor recommendations and comparisons that depended on the prior classification.

### Immutable source, editable interpretation
The raw imported bank record must remain preserved. Budget changes the household's **interpretation** of that source record rather than silently rewriting the original financial evidence.

### Audit trail and reversibility
Bulk corrections should record who/what initiated the change, the prior interpretation, the new interpretation, affected records and applicable learned-rule change. The architecture should support undo/reversal of an erroneous bulk correction.

### Grandfather interaction
A conversational correction can sound like:
'I found 14 matching transactions totaling $1,860. They're currently classified as Groceries. I can move all 14 to Animal Feed and remember Animal Feed for this merchant from now on. Want me to fix the past and future?'

All counts and dollar amounts in production must be generated from actual matching records, not invented by AI.

### Product principle
Users should not be trapped by early mistakes made while Budget is learning the household. Better information should improve both the historical model and future automation while preserving a trustworthy record of what changed.


## 36. Discovery Decision 026 — Recurring Bill Change Detection and Cost-Creep Watchdog

### Grandfather watches recurring costs over time
Budget should learn the normal amount/range and cadence of recurring household bills and proactively identify material changes rather than treating every recurring payment as an isolated transaction.

### Change detection
For a recurring obligation or subscription, Budget should be able to detect and explain patterns such as:
- a sudden price increase or decrease;
- gradual price creep over multiple billing cycles;
- an expired-looking historical discount pattern;
- duplicate or newly overlapping recurring charges;
- a skipped/late/unexpected charge;
- changes in normal billing cadence;
- unusual usage-sensitive bills relative to their own historical range.

The system should distinguish observed facts from possible explanations. For example, it may say a promotion **may** have expired, but should not claim that as fact unless supported by source data.

### Annualized impact
When useful, Grandfather should translate a recurring increase into understandable forward impact—for example, a $27 monthly increase represents approximately $324 over 12 months if the new amount persists.

Annualization is a projection and should be labeled accordingly.

### Bill price history
Recurring bill objects should retain a time series/history sufficient to show how the household's cost for a service has changed over time. Users should be able to drill from the current bill into historical amounts and source transactions.

### Household cost-creep view
Budget should eventually aggregate recurring-cost changes so Grandfather can identify which providers/categories are responsible for meaningful increases in fixed household spending over a selected period.

Example interaction pattern:
'Your recurring household services are costing about $X more per year at their current run rate than they were Y months ago. These are the largest changes.'

All production figures must be calculated from household records.

### Advisor prioritization
Grandfather should prioritize alerts by materiality rather than notifying on every minor fluctuation. The importance calculation can consider absolute dollar change, percentage change, persistence, annualized effect, household cash position and whether the bill is normally stable or variable.

### Action pathway
A detected increase should be able to lead into a decision workflow: inspect history/source evidence, confirm whether the change is expected, update the known bill amount, mark as variable/seasonal, or create a household action such as reviewing/canceling/renegotiating the service.

### Product principle
Small recurring increases are easy for households to miss because each individual transaction may look harmless. Budget should make cumulative cost creep visible before it quietly becomes a significant permanent reduction in household cash flow.


## 37. Discovery Decision 027 — Household Negotiation Assistant and Savings Resolution Loop

### Detection should lead to resolution
When Budget identifies a meaningful increase or unnecessary recurring cost, Grandfather should be able to help the household act on it rather than stopping at an alert.

### Negotiation packet
For an eligible bill/service, Budget should be able to assemble a concise evidence-backed packet containing available facts such as:
- current recurring amount;
- historical amounts and when a change first appeared;
- amount/percentage of increase;
- estimated 12-month impact if the current run rate continues;
- payment history visible in household records;
- known service/plan facts the household has confirmed;
- prior notes or negotiations;
- the household's desired outcome.

### Conversation preparation
Grandfather can turn the packet into a simple call/chat preparation guide: what changed, what to ask the provider to explain, what outcome to request, questions to ask about lower-cost plans/discounts, and which facts from the household record are useful to mention.

It must not invent competitor pricing, provider policies, contractual rights, cancellation terms or available discounts. External/current claims require a verified source when that capability is later added.

### User-controlled action
Budget may prepare, coach and track a negotiation, but provider calls, cancellations, plan changes, contractual commitments and other consequential external actions remain user-controlled unless a later explicitly authorized action layer is designed with appropriate confirmation, security and audit controls.

### Resolution tracking
A cost-creep event should support a lifecycle such as:
**detected → reviewed → action planned → contacted → outcome recorded → new recurring amount confirmed → household forecast updated.**

### Measurable value
Budget should track verified financial outcomes attributable to completed household actions where the evidence supports doing so. Examples include reduced recurring run rate, avoided future recurring cost, canceled subscription cost and negotiated savings.

A potential user-facing metric is **Budget Helped You Save**, but calculations must distinguish realized/verified savings from projected annualized savings and must avoid claiming causation when it cannot be supported.

### Product principle
Grandfather should help close the loop from **notice → understand → prepare → act → verify → update the plan**. The objective is not merely better financial reporting; it is helping the household improve its actual financial position.


## 38. Discovery Decision 028 — Subscription Intelligence and Future Authorized Cancellation

### Recurring-spend inventory
Budget should maintain a detailed inventory of subscriptions, memberships and other recurring discretionary/semidiscretionary charges discovered from household transaction history.

The goal is to show the household **where recurring money is actually going**, not merely provide a generic subscription category.

### Subscription intelligence
For each detected recurring charge, Budget should attempt to maintain available/confirmed facts such as:
- normalized merchant/provider;
- amount and cadence;
- first/most-recent observed charge;
- price history;
- estimated annual run rate;
- household classification/category;
- household member/beneficiary when known;
- status such as keep/review/cancel/unknown;
- confidence that the pattern is truly recurring;
- source transactions/evidence;
- notes and prior review decisions.

### Periodic subscription review
Grandfather should periodically surface meaningful recurring-spend reviews rather than allowing subscriptions to disappear into background noise. Reviews can summarize monthly and annualized totals and prioritize unrecognized, unused/questionable, recently increased, duplicative or never-reviewed items.

### Conversational decisions
The household should be able to answer naturally with outcomes such as:
- keep it;
- cancel it;
- I don't recognize this;
- we still use it;
- remind/review later;
- this belongs to a specific household member/category.

Budget should learn and record those decisions so repeatedly approved services are not needlessly questioned unless something materially changes.

### Cancellation workflow — Budget 1.0
In the initial product, Budget should support the resolution lifecycle:
**detect → identify → quantify → household decision → provide cancellation/action guidance → track pending cancellation → verify charge stops → update forecast and savings.**

Budget should not claim a subscription is canceled merely because the user intended to cancel it. Verification should rely on user confirmation and/or subsequent financial evidence.

### Future authorized action layer
A later product phase may support Budget reaching out to a provider or navigating an authorized cancellation workflow on the household's behalf. This is explicitly a future capability, not a Budget 1.0 requirement.

Any future cancellation execution must include explicit user authorization, clear disclosure of the action being taken, provider/account authentication appropriate to the integration, confirmation before consequential commitments where appropriate, audit history, outcome verification and safe handling of credentials/tokens. Budget should never request that users place raw passwords or sensitive account credentials into ordinary Advisor conversation.

### Savings accounting
When a recurring charge actually stops, Budget can update future cash-flow projections and record verified or projected savings with clear labeling. Freed recurring cash should then flow through the household's active allocation rules rather than silently becoming unplanned spending.

### Product principle
Recurring expenses should be treated as continuing household decisions. Grandfather's role is to make those decisions visible, intentional and easy to revisit—and eventually, where safely authorized, easier to execute.


## 39. Discovery Decision 029 — Whole-Household Opportunity Hunting

### Grandfather should search for improvement opportunities across all spending
Budget should proactively analyze the household's full spending history for patterns that may offer meaningful opportunities to improve cash flow, debt progress, safety or goal achievement—even when no transaction is technically erroneous and no bill has increased.

### Candidate opportunity patterns
The opportunity engine may examine patterns such as:
- repeated small purchases that become large in aggregate;
- convenience-store/convenience premiums;
- frequent dining/takeout patterns;
- category creep over time;
- duplicate/overlapping services;
- recurring discretionary spending;
- unusually expensive merchant/category patterns relative to the household's own history;
- avoidable fees where supported by the records;
- spending clusters that materially affect an active goal;
- categories where a modest reduction would create meaningful annual capacity.

### Household-relative, not moralistic
Budget should not label spending as 'waste' merely because it is discretionary. The Advisor should first establish the observed pattern and its financial consequence, then let the household decide whether the spending is worth the tradeoff.

### Opportunity framing
A useful Grandfather interaction is:
**what I noticed → what it costs over time → why it matters to your current goals → a realistic change option → what that change would accomplish → you decide.**

For example, instead of 'stop buying snacks,' Grandfather might explain that a particular pattern totals $X over a year and show what reducing it by 20%, 40% or another realistic amount would do to debt payoff or available cash.

All production figures must come from actual household records and deterministic scenario calculations.

### Small-change scenarios
Grandfather should favor practical scenarios over all-or-nothing deprivation. Where appropriate, it can model partial reductions and show the corresponding financial result, allowing the household to choose a level that is sustainable.

### Prioritization
Opportunities should be ranked internally for relevance using factors such as dollars recoverable, ease of change, persistence, effect on active goals and household financial condition. The user experience should avoid overwhelming the household with trivial suggestions.

### Learning from rejection
If the household says a spending pattern is intentional and worth keeping, Budget should remember that decision and reduce repetitive prompting unless the pattern materially changes or the household's financial situation makes it newly significant.

### Product principle
The Advisor is not a spending judge. Its job is to reveal **opportunity cost that is otherwise hard to see** and help the household decide which tradeoffs are worth making.


## 40. Discovery Decision 030 — Celebrate the Win and Redeploy Freed Cash

### Financial milestones should feel meaningful
When the household completes an important financial milestone—especially paying off an obligation—Budget should deliberately recognize and celebrate the accomplishment. The experience should reinforce progress without becoming childish, gamified for its own sake, or financially misleading.

A payoff event should immediately transition from celebration into a clear explanation of **what the household can now do with the money that has been freed.**

### Freed-cash event
When a recurring debt payment ends, Budget should create a structured **freed-cash event** containing the former payment amount/cadence, payoff date, resulting monthly/pay-period capacity and the household decisions made about redeployment.

### 10% payoff-to-reserve rule
Steve's household wants a specific standing rule:
**Whenever an obligation is paid off, 10% of the payment amount that has been freed should be redirected to the protected cash reserve, advancing the household toward the one-year resilience goal.**

This is in addition to the broader reserve-first doctrine already established for applicable incoming money. The implementation must define cadence conversions carefully—for example, a monthly former payment should create a corresponding recurring reserve allocation rather than incorrectly treating the entire payment as a one-time deposit.

### Remaining freed capacity
After the additional 10% reserve allocation, Grandfather should evaluate the remaining freed capacity against the household's current needs and opportunities rather than applying a permanently fixed destination.

Candidate uses include:
- filling near-term cash-flow gaps;
- accelerating high-interest or otherwise costly debt;
- paying off a smaller obligation to release additional monthly cash flow;
- strengthening an underfunded required category;
- advancing an approved purchase/household goal;
- offering some safe discretionary/date/fun capacity when the household can afford it;
- combinations of the above.

### AI role: scenario generation and explanation
AI should help identify and communicate sensible candidate strategies, while deterministic financial calculations determine the actual dollars, timelines, interest effects and cash-flow consequences.

Grandfather should be able to say, in substance:
'You did it. This payment no longer belongs to that debt. Ten percent of the freed payment now strengthens your reserve. Here are the strongest ways we could use the rest, and here is what each one changes.'

### Compare options, don't hide tradeoffs
The system should model alternatives such as high-interest-first, cash-flow-release-first, gap-filling, discretionary allowance, or blended approaches when relevant. It should explain measurable consequences and let the household choose.

### Compounding payoff momentum
When freed cash is redirected to another debt and that second obligation is subsequently eliminated, Budget should recognize the larger newly freed amount and repeat the process. This creates a visible household payoff cascade while continuously strengthening reserves through the 10% payoff-to-reserve rule.

### Celebration with substance
Milestones may include debt payoff, reserve thresholds, improved runway, successful cost reductions and other verified household achievements. Celebrations should immediately connect the achievement to increased freedom/capacity and the next available choices.

### Product principle
Budget should make progress emotionally visible **and financially productive**. A win should feel good, teach the household what changed, and prevent newly freed cash from quietly disappearing into lifestyle drift unless the household intentionally chooses to use some of it that way.


## 41. Discovery Decision 031 — Progressive Disclosure from Simple Household View to Financial-Statement Depth

### Governing UX principle
Budget should be **simple at first glance and exceptionally deep on demand**. The default experience must show only the information needed to understand the household's immediate financial position and next useful action. Detailed financial data should remain available through deliberate drill-down rather than competing for attention on the primary screen.

### Layered information architecture
A candidate depth model is:

**Layer 1 — Household glance**
- Are we financially safe right now?
- true available cash;
- what must be covered before next income;
- current trajectory;
- one/few material Advisor items;
- major progress/milestones.

**Layer 2 — Explanation / quick detail**
Accessible through tap, button, dropdown, drawer, modal/popover or equivalent context-appropriate interaction. Explains what a number means, what changed and the major components behind it.

**Layer 3 — Dedicated analysis page**
Full view for a domain such as debt journey, spending category, bill, income, reserve, subscriptions, goals or cash-flow timeline.

**Layer 4 — Evidence and model detail**
Underlying transactions, classifications, assumptions, calculations, source records, historical comparisons and scenario mechanics.

**Layer 5 — Advanced financial view**
For financially sophisticated users, Budget should eventually support household financial reporting approaching ledger/financial-statement depth, including appropriate income/expense reporting and a P&L-style view where the underlying household data supports it.

### Debt-free journey as drill-down
The complete road-to-debt-free timeline should be available as a dedicated drill-down rather than crowding the main dashboard.

It should be able to compare scenarios such as:
- current trajectory if behavior/allocations remain materially unchanged;
- recommended payoff pathway;
- alternative pathways created by additional monthly capacity;
- payoff cascade as each obligation ends;
- reserve growth alongside debt reduction;
- estimated consumer-debt-free date under each modeled scenario.

All timelines and savings estimates must be derived from known balances, rates/terms where available, payments, cash-flow rules and explicit assumptions. Missing data should be visible rather than silently invented.

### Living timeline
The debt-free pathway should recalculate when material household facts change, including payments, balances, interest terms, income, spending, reserve rules, newly discovered obligations or user-selected allocations. Grandfather should explain significant changes to the projected timeline.

### 'Every number has a why'
Important summary values should be inspectable. The user should be able to move naturally from a simple number to its explanation, then to its components, then to source-level evidence without needing to understand accounting terminology.

### Dual-audience design
Budget must work for someone intimidated by financial software **and** remain useful to a financially sophisticated user. This should be achieved through progressive disclosure, not separate simplistic and professional products.

The beginner is never forced into advanced detail. The banker/accountant-level user is never prevented from reaching it.

### Financial reporting architecture implication
The domain/data model should preserve sufficient structure and provenance to support deeper reporting later. The product should not fake accounting precision from incomplete categorization. P&L-style and other advanced views should clearly define their basis, included/excluded flows and treatment of transfers, reimbursements, debt principal, interest and other non-simple expense movements.

### Mobile simplicity
Because routine household use is expected to happen heavily on a phone, Layer 1 and common Layer 2 interactions should remain highly legible, touch-friendly and low-density. Deep analytical pages may expose substantially more information while retaining clear navigation back to the simple household view.

### Product principle
**Complexity belongs underneath the interface, not on top of the user.** Budget should let someone understand their finances in seconds and, when desired, investigate the same financial model deeply enough to satisfy a sophisticated financial user.


## 42. Discovery Decision 032 — The Budget Advisor Is Named Lewis

### Customer-facing identity
The Budget Advisor's customer-facing name is **Lewis** (L-E-W-I-S).

The name is an intentional personal Easter egg: Steve's banker and high-school classmate is Jeff Lewis. That origin does not need to be explained in normal product UX or marketing; the product simply presents the advisor as Lewis.

### Grandfather is doctrine, not branding
Prior references in this discovery document to **Grandfather** describe the Advisor's behavioral/personality doctrine, not the product character's public name.

Going forward:
- **Lewis** = the named AI financial advisor users interact with.
- **Grandfather Approach** = internal product/design shorthand for Lewis's desired advisory character: financially wise, calm, protective, practical, plainspoken, direct when necessary, educational, non-shaming and respectful of household agency.

Existing requirements written as 'Grandfather should...' should therefore be interpreted as **'Lewis should behave according to the Grandfather Approach...'** They do not imply that the interface should call the agent Grandfather.

### Product language direction
Potential natural interaction labels include:
- Ask Lewis
- Lewis noticed something
- Lewis recommends
- Lewis found an opportunity
- Lewis has a plan
- What Lewis sees
- Talk to Lewis

These are directionally useful examples, not final UI copy.

### Character boundary
Lewis should feel like a trusted financial advisor, not a cartoon character or simulated family member. The warmth comes from the quality and manner of the advice rather than gimmicks, age stereotypes or excessive anthropomorphism.

### Product principle
**Lewis is the interface identity; the Grandfather Approach is the behavioral standard underneath it.**


## 43. Discovery Decision 033 — Home Screen: Lewis Briefing + Essential Financial Cards

### Home experience is a blend
The primary Budget home screen should combine a short proactive **Lewis briefing** with a small set of essential financial cards. It should not force the user to choose between a conversational advisor and a dashboard; Lewis interprets the household, while the cards provide immediate visual grounding.

### Lewis briefing first
At the top of the experience, Lewis should provide a concise, context-aware briefing focused only on what matters now. Depending on household conditions, that may include:
- whether the household is safe through the next income event;
- true available cash;
- an important change since the last review;
- an upcoming risk/obligation;
- a meaningful opportunity;
- a milestone/win;
- one recommended next action.

The briefing should remain short enough to understand at a glance. It is an entry point into conversation and drill-down, not a daily financial essay.

### Essential cards immediately underneath
The first screen should then expose only a handful of high-value cards, potentially including:
- Financial Safety / runway;
- True Available Cash;
- Next Income / obligations before it;
- Reserve progress;
- Debt/payoff progress;
- current spending/budget position;
- Lewis recommendation/opportunity where appropriate.

Final card composition remains a UX-design decision to validate during prototyping; the home screen should not become a grid of every metric Budget knows.

### Drill-down everywhere
Cards and meaningful briefing statements should lead naturally into the progressive-disclosure system established in Decision 031: quick explanation first, dedicated page next, evidence/calculation depth beneath that.

### Quiet when nothing needs attention
Lewis should not manufacture commentary merely to appear intelligent. On an uneventful day, a reassuring concise status is better than unnecessary alerts or advice.

### Mobile-first objective
A household member should be able to open Budget on a phone and understand the current financial position in seconds, then talk to Lewis or drill into any relevant area without navigating a dense accounting dashboard.

### Product principle
**Lewis tells you what matters; the dashboard shows you where you stand; drill-down shows you why.**
