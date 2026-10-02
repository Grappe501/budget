# Budget — Phase 1 Product Specification

**Version:** 1.0  
**Status:** Phase 1 specification baseline  
**Canonical discovery source:** `MASTER_PRODUCT_PLAN.md` through Discovery Decision 051  
**Build target:** Local Steve + Kelly Household Alpha  
**Scope doctrine:** Specification, not implementation. Phase 2 selects technical architecture.

---

## 1. Purpose

This document converts completed product discovery into an implementation-grade specification for the first usable Budget household alpha.

The Alpha Definition of Alive is:

> Import the real household bank CSV → reconstruct approximately the last year → identify income, bills, recurring spending and meaningful categories → let the household quickly correct what Budget gets wrong → learn from those corrections → open into a clear dashboard that tells the household where it stands today, what it can safely spend through the next payday, what is coming next, and what Lewis recommends doing first.

The Alpha must tell the truth before it tries to become comprehensive.

---

## 2. Alpha Product Contract

After initial import/review, the household must be able to answer:

1. Are we financially safe right now?
2. What can we actually spend before the next payday?
3. What money is already spoken for?
4. What is likely to happen next?
5. Why does Budget believe these numbers?
6. What does Lewis recommend doing first, and why?

If Budget cannot support one of these answers with inspectable evidence or explicit assumptions, it must show uncertainty rather than manufacture precision.

### Non-negotiable invariants

- Raw imported financial evidence is immutable.
- Interpretations may change; source evidence does not.
- Deterministic financial calculations are authoritative over AI prose.
- Unknown facts remain unknown.
- Inferred timing is not represented as a contractual due date.
- AI recommendations never silently become consequential financial decisions.
- No money movement occurs in Alpha.
- Protected money is excluded from ordinary spendable cash according to household rules.
- Every important dashboard number must have a drill-down explaining its composition.
- Corrections must be auditable and, where appropriate, reversible.
- Steve and Kelly are equal Owners with independent authority and full shared visibility.

---

## 3. Alpha Information Architecture

### 3.1 Primary navigation

The first Alpha should expose a compact primary navigation:

- **Home** — Lewis briefing + essential financial cards.
- **Transactions** — normalized ledger, search/filter, evidence and corrections.
- **Budget** — observed baseline, category position, pay-period spending.
- **Calendar** — forward financial calendar and cash-flow horizon.
- **Bills** — recurring obligations and detected recurring charges.
- **Review** — unresolved classifications/recurring patterns/import issues.
- **Settings / Data** — household configuration, imports, reserve rule, reconciliation, audit activity.

Debt, goals, documents, advanced reports and Self-Bank may receive model foundations or placeholders where necessary, but they must not displace Alpha-critical navigation.

### 3.2 Home dashboard hierarchy

The Home screen must be low-density and answer the household's immediate questions in this order:

1. **Lewis Briefing**
   - concise statement of current position;
   - most important upcoming risk/change/opportunity;
   - one grounded recommended next action when useful;
   - quiet/non-alarmist state when nothing material requires attention.

2. **True Available Cash / Safe to Spend**
   - dominant daily number;
   - horizon: through the next known income event;
   - drill-down to exact components.

3. **Safety / Trajectory**
   - cash-positive/cash-negative direction;
   - current confidence;
   - runway where sufficiently supported.

4. **Upcoming Financial Calendar**
   - next few material events;
   - next income;
   - obligations/expected spending before it;
   - link to full calendar.

5. **Protected Money**
   - Cash Reserve allocation relevant to spendable-cash calculation;
   - clearly excluded from ordinary availability.

6. **Spending Position**
   - actual observed spending versus reconstructed/active plan;
   - category drill-down.

7. **Recurring Obligations / Bills**
   - material upcoming items and unresolved recurring discoveries.

8. **Data Trust**
   - reconciliation status;
   - unresolved high-impact review count;
   - forecast confidence when materially degraded.

Desktop may arrange these spatially; mobile must preserve the hierarchy without forcing dense tables.

---

## 4. Core Alpha Workflows

### WF-01 — First CSV import

**Entry:** user selects a bank CSV.

**Required flow:**
1. preserve original import metadata/file identity;
2. preview detected columns;
3. map date, description, debit/credit or signed amount, balance if present, and optional fields;
4. validate parseability before commit;
5. show row count/date range and detected issues;
6. import idempotently;
7. identify duplicates/overlap without silently dropping ambiguous rows;
8. create normalized transaction records linked to raw source rows;
9. run deterministic normalization;
10. run classification/recurrence inference;
11. produce an import summary;
12. route uncertain/high-impact items to Review.

**Failure behavior:** malformed or ambiguous data must fail visibly with recoverable guidance; partial silent imports are prohibited.

### WF-02 — Financial reconstruction

From imported history, Budget reconstructs:
- inflows and likely income sources;
- expenses;
- transfers/reimbursements where identifiable;
- normalized merchants;
- meaningful categories;
- likely recurring charges;
- probable bill cadence/timing;
- observed category baseline;
- seasonal/irregular patterns only when evidence is sufficient.

Every inference stores confidence and evidence references.

### WF-03 — Review and correction

Review is prioritized by financial impact, not row order.

A user can:
- confirm/change category;
- rename/normalize merchant;
- identify income/transfer/reimbursement;
- confirm/reject recurring relationship;
- attribute a transaction to household/member where relevant;
- choose correction scope when a learned rule is appropriate.

Correction scopes must support:
- this transaction only;
- historical matching records;
- this + future;
- all matching history + future.

Before broad retroactive changes, show affected transaction count/date range/aggregate dollars.

### WF-04 — Teach Budget once

Confirmed corrections may create household rules. Rules must:
- use normalized merchant/fingerprint logic;
- preserve provenance;
- have inspectable match criteria;
- support disable/remove;
- avoid overriding contradictory high-confidence evidence silently.

Deterministic learned rules should be preferred over repeated AI calls.

### WF-05 — Establish current cash truth

The user provides/confirms the current authoritative bank balance and relevant reserve allocation.

Budget compares its ledger/model position to that balance.

If mismatched:
- expose difference;
- offer Reconciliation Mode;
- investigate pending/timing/import/duplicate/missing/manual causes;
- never insert an invisible balancing transaction.

Forecast confidence is degraded when a material unresolved difference remains.

### WF-06 — Determine next income horizon

Budget identifies the next sufficiently confident income event, initially including Kelly's biweekly paycheck once confirmed.

The next-income horizon is the default boundary for True Available Cash.

Uncertain prospective income must not be silently included in the conservative spendable-cash calculation.

### WF-07 — Calculate True Available Cash

Conceptual Alpha formula:

**Usable current cash**
minus **protected Cash Reserve**
minus **confirmed/expected required obligations before next included income**
minus **expected necessary/normal spending before next included income**
minus **other adopted protected allocations applicable to the horizon**
equals **True Available Cash through next income**.

The engine must avoid double-counting a transaction/event represented in multiple models.

The drill-down must show every included component, source/status and amount.

Predicted spending must be visibly distinguished from confirmed obligations.

### WF-08 — Forward financial calendar

Calendar events can include:
- confirmed/expected income;
- recurring bills;
- inferred recurring charges;
- planned reserve allocations;
- expected necessary spending aggregates where useful;
- later: One-Time Goals and other roadmap events.

Each event has a type/status/confidence. The calendar must be able to calculate projected cash position after events.

Dashboard shows a compact horizon; full Calendar provides detail.

### WF-09 — Lewis daily briefing

Lewis receives structured calculated context rather than being asked to invent financial math.

Briefing structure:
1. where the household stands;
2. what materially changed or is approaching;
3. why it matters;
4. what Lewis recommends considering first;
5. consequence of that recommendation;
6. evidence/assumptions available via drill-down.

Lewis must follow the Grandfather Approach doctrine while being publicly named **Lewis**: calm, practical, protective, direct, plainspoken, non-shaming.

### WF-10 — Reconciliation

Reconciliation compares authoritative account balance with Budget's modeled balance through a known point.

It supports:
- pending/posted timing;
- missing/overlapping imports;
- duplicates;
- transfers;
- cash;
- refunds/reversals;
- fees/interest;
- explicit manual reconciliation adjustments as last-resort auditable objects.

Completion records reconciled balance/date and unresolved items.

---

## 5. Screen Specification

### S-01 Home
**Purpose:** answer the six Alpha questions rapidly.

**Must show:** Lewis briefing, True Available Cash, safety/trajectory, upcoming events, reserve/protected amount, spending position, trust/reconciliation indicator.

**Must not:** become a giant financial report, show architecture/debug language, or force the user to interpret raw accounting.

### S-02 Import
**Purpose:** safely ingest CSV.

**States:** select → preview/map → validate → import → processing → summary → review.

**Must show:** date range, rows, mapping, warnings, duplicates/overlap findings, import result.

### S-03 Transactions
**Purpose:** authoritative normalized ledger.

**Capabilities:** date/merchant/category/type search and filtering, transaction detail, raw-source link, correction, split-ready architecture, attribution.

### S-04 Review Queue
**Purpose:** resolve only things worth human attention.

**Prioritization:** high-dollar/high-consequence uncertainty first, then recurring-impact items, then lower-impact ambiguity.

**Actions:** confirm, correct, teach rule, defer, mark unresolved.

### S-05 Budget
**Purpose:** show reality-first household baseline and current spending position.

**Must distinguish:** observed baseline versus any active target.

**Alpha emphasis:** pay-period/current-month category behavior and remaining capacity; no need for an elaborate traditional envelope system before the core truth engine works.

### S-06 Calendar
**Purpose:** show when money arrives/leaves and resulting projected cash.

**Views:** implementation may choose responsive month/list/horizon presentation in Phase 2/UX build, but mobile must have a clear chronological list.

**Event detail:** amount/range, type, status, confidence, source/relationship, projected cash effect.

### S-07 Bills / Recurring
**Purpose:** turn recurring activity into understandable obligations.

**Must support:** detected merchant, typical amount/range, cadence, observed timing, confidence, user-confirmed identity, next expected occurrence, history.

Contractual fields remain unknown until confirmed/document-backed.

### S-08 Reconciliation
**Purpose:** explain balance disagreement.

**Must show:** authoritative balance, modeled balance, difference, candidate causes, actions taken, remaining difference, completion status.

### S-09 Settings / Data
**Alpha configuration:** household Owners, current account, reserve percentage/rule, current reserve allocation/balance as modeled, imports, learned rules, audit/activity, local AI configuration status without exposing secrets.

---

## 6. Alpha Domain Model — Logical Objects

Phase 2 chooses database technology and physical schema. These logical objects are required.

### Household
- id
- name
- configuration
- timezone
- currency
- approval policy
- visibility policy

### HouseholdMember
- id
- household_id
- display_name
- role
- status

### FinancialAccount
- id
- household_id
- type
- institution/display name
- masked label only
- authoritative current balance + as-of timestamp when manually confirmed/imported
- reconciliation state

### ImportBatch
- id
- account_id
- source type
- filename/display metadata
- file fingerprint
- import timestamp
- date range
- row counts
- status
- mapping version

### RawTransaction
- immutable source representation
- import_batch_id
- source row identity/hash
- original fields

### Transaction
- normalized transaction identity
- account_id
- raw source linkage
- posted/transaction date(s)
- amount/direction
- normalized description/merchant
- transaction type
- category
- attribution
- recurring linkage
- confidence
- interpretation status

### Merchant
- canonical merchant identity
- aliases/fingerprints
- learned household metadata

### Category
- hierarchy
- household/system origin
- essential/flexible/discretionary semantics where configured

### ClassificationRule
- match criteria/fingerprint
- resulting interpretation
- scope
- provenance
- active state
- confidence

### IncomeSource / IncomeEvent
- source/type
- cadence/window
- expected amount/range
- confidence/status
- actual transaction linkage

### RecurringSeries
- merchant/relationship
- cadence
- typical amount/range
- observed timing
- confidence
- confirmed identity
- next expected occurrence

### Bill
- recurring-series linkage where applicable
- provider/display name
- confirmed vs inferred fields
- amount/range
- due/withdrawal timing distinction
- status
- source provenance

### BudgetBaseline
- period
- observed category metrics
- derivation version
- source date range

### BudgetTarget
- category/period
- target amount
- origin (user/accepted experiment/etc.)
- status

### ProtectedAllocation
Logical foundation for money physically held in an account but excluded for a purpose.
- type (Cash Reserve; later One-Time Goal)
- amount
- account
- status
- provenance

### CalendarEvent
- event type
- date/date range
- amount/range
- confirmed/inferred/predicted/planned status
- confidence
- linked domain object
- cash-flow treatment

### ForecastRun
- as-of timestamp
- horizon
- scenario
- assumptions/version
- projected events/positions
- confidence

### Recommendation
- Lewis recommendation type
- trigger
- evidence
- assumptions
- proposed action
- expected consequence
- status
- requires approval flag

### ReviewItem
- issue type
- priority/materiality
- evidence
- proposed resolution
- status

### ReconciliationSession / ReconciliationAdjustment
- account
- as-of date
- authoritative balance
- modeled balance
- difference
- investigated items
- explicit adjustment if required
- actor/timestamp
- completion status

### AuditEvent
- actor
- timestamp
- action
- object
- before/after or references
- reason/provenance

The physical schema must preserve room for later Debt, FinancialDocument, OneTimeGoal, ReserveLoan/SelfBankLoan, FinancialExperiment and behavior-history objects without forcing Alpha to implement their full workflows.

---

## 7. Financial Calculation Contracts

### 7.1 Sign convention
Phase 2 must choose and document one internal money sign convention. UI formatting may differ, but calculations may not mix conventions.

### 7.2 Transfer neutrality
Transfers between household-owned accounts are not income or spending. Alpha currently has one primary account but the model must not hard-code single-account economics.

### 7.3 Reimbursements
Confirmed reimbursements must be separable from earned income and, where linked, from the expense they recover.

### 7.4 Protected reserve
The household's default policy is to protect 10% of applicable incoming money, with actual physical monthly savings transfer planned later. Alpha must support enough reserve state to exclude protected reserve money from True Available Cash.

The exact automation of reserve sweeps is not Alpha-blocking.

### 7.5 Spend-forward
If the household intentionally spends against the next pay period, Budget must represent the negative carry-forward explicitly. It may not make the current period look healthier by hiding the future burden.

### 7.6 Forecast confidence
At minimum calculations distinguish:
- confirmed/received;
- high-confidence expected;
- inferred/predicted;
- prospective/excluded.

Exact labels may be normalized in Phase 2, but conservative safety calculations must not treat all confidence classes equally.

### 7.7 No double counting
A bill represented by a recurring series and a calendar event must affect forecast cash once. The model must use stable relationships/identities to prevent duplicate deductions.

### 7.8 Calculation explainability
Every derived financial metric stores or can reproduce:
- as-of time;
- source inputs;
- rule/version;
- assumptions;
- exclusions;
- result.

---

## 8. Lewis Alpha Specification

### 8.1 Lewis is an advisor, not the calculator
Financial engines calculate balances, forecasts, runway and True Available Cash. Lewis consumes structured results and evidence.

### 8.2 Allowed Alpha behaviors
Lewis may:
- explain calculations;
- summarize household position;
- prioritize review;
- identify recurring/spending patterns;
- propose classifications;
- surface material changes;
- recommend a next action;
- update routine model housekeeping under Decision 050 when confidence rules permit.

### 8.3 Prohibited Alpha behaviors
Lewis may not:
- fabricate missing financial facts;
- silently alter raw records;
- initiate payments/transfers;
- access external accounts;
- commit the household to obligations;
- represent predictions as contractual facts;
- make consequential financial changes without human confirmation.

### 8.4 Advice contract
For consequential recommendations:
**Here is what I see → why it matters → what I would consider → what that changes → you decide.**

Strong affordability advice is permitted:
“You can physically pay for it, but I don't think you can afford it yet,” when supported by the model.

### 8.5 Cost discipline
AI calls should be reserved for ambiguity, language understanding, pattern explanation and recommendation generation where they add value. Deterministic parsing/rules/cached merchant memory/batching should handle routine work.

Store reusable AI-derived structured results so unchanged data is not repeatedly reprocessed.

---

## 9. Household Alpha Governance

### Owners
Steve and Kelly are equal Owners.

### Authority
Either Owner can independently approve household decisions in Alpha.

### Transparency
Both Owners have complete shared visibility into household financial data and material Lewis activity.

### Audit
Material changes record actor, time, object, reason and relevant before/after state.

### Local-first security
Alpha is local-only. Secrets are environment variables and never committed. Raw financial files/data must be treated as sensitive local data. Phase 2 must specify storage encryption/backups/authentication appropriate to the local environment before real data is imported.

---

## 10. Alpha Acceptance Gates

### Gate A — Import trust
- one real bank CSV can be mapped and imported;
- reimport/overlap does not create silent duplicates;
- raw rows are preserved;
- import summary reconciles row counts.

### Gate B — Reconstruction
- transactions normalize into usable ledger;
- meaningful income/expense/transfer classifications exist;
- recurring candidates are detected;
- uncertainty is visible.

### Gate C — Correction learning
- household can correct mistakes quickly;
- broad correction scope is previewed;
- future matching behavior reflects accepted rules;
- corrections are auditable.

### Gate D — Cash truth
- authoritative balance can be entered/confirmed;
- discrepancies are surfaced;
- reconciliation can explain/resolve or explicitly leave a difference unresolved.

### Gate E — Forecast
- next known income is represented;
- obligations/expected necessary spending through that horizon are represented without double counting;
- projected cash positions are reproducible.

### Gate F — True Available Cash
- protected reserve and committed/expected needs are deducted according to explicit rules;
- result has a complete drill-down;
- negative carry-forward is visible.

### Gate G — Dashboard
A household Owner can answer the six Alpha questions without navigating through raw data first.

### Gate H — Lewis
- briefing is grounded in calculated/evidenced context;
- recommendation has rationale/consequence;
- unsupported facts are not invented;
- consequential action remains human-controlled.

### Gate I — Local data safety
- secrets absent from repository;
- sensitive data storage/backups/access strategy implemented according to Phase 2 blueprint;
- audit trail functions for material changes.

### Gate J — Definition of Alive
Steve can import the real household history, correct it, and reasonably trust the resulting dashboard enough to use it for an actual near-term household spending decision.

---

## 11. Explicitly Deferred from Alpha Alive

Approved product capabilities that remain roadmap items rather than Alpha blockers:

- full financial-document extraction/intelligence;
- receipt line-item intelligence beyond what is needed for initial CSV alpha;
- direct bank connectivity;
- payment initiation/autopay management;
- authorized subscription cancellation;
- mature voice-first capture;
- complete One-Time Goal planner;
- full Cash Reserve loan / Self-Bank workflows;
- long-term Financial Experiments;
- mature longitudinal behavior intelligence;
- advanced seasonal models requiring live history;
- deep debt payoff workspace;
- P&L/advanced financial statement views;
- exact elected commercial permission/privacy variants;
- Wealth Builder/investment management;
- multi-household SaaS packaging;
- production cloud deployment.

Architecture must not make these impossible, but Alpha work must not expand merely to implement them.

---

## 12. Roadmap Contracts Preserved from Discovery

The following concepts are intentionally preserved for later phases and must not be lost:

- Lewis progressive “Things I Still Need to Learn” queue;
- financial document evidence hierarchy;
- One-Time Goals and protected sinking-fund envelopes;
- household Financial Calendar as a growing planning surface;
- Reserve Lock;
- Self-Bank doctrine: Fed funds target-range upper bound at origination + 0/+2/+4/+6 point purpose spread, fixed for loan life;
- payoff-event rule: 10% of released payment capacity redirected to Cash Reserve, remaining capacity dynamically considered;
- permanent Household Financial Life Record;
- measured Financial Experiments;
- predictive seasonal/irregular forecasting;
- whole-household opportunity hunting;
- subscription/negotiation intelligence;
- future commercial configurable approvals/privacy;
- eventual authorized action layer only after dedicated security design.

---

## 13. Phase 1 Completion Criteria

Phase 1 is complete when this specification is accepted as the build contract and any remaining questions are **technical implementation choices**, not unresolved product behavior needed for Alpha Alive.

Phase 2 must next decide:
- local application stack;
- local database/storage;
- sensitive-data encryption and backup strategy;
- authentication/session approach for the household alpha;
- CSV parsing/import architecture;
- deterministic rules engine;
- forecasting/calculation engine boundaries;
- OpenAI integration boundary and structured contracts;
- audit/event architecture;
- testing strategy;
- migration path toward commercial multi-household deployment;
- development/validation commands and repository structure.

No implementation should begin merely because a technical choice appears obvious; Phase 2 should make those choices explicit first.
