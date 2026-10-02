# Budget — Pre-Construction Forensic Audit & Layered Build Hardening

**Version:** 1.0  
**Status:** Required pre-construction architecture/build-plan overlay  
**Applies to:** `PHASE_3_CURSOR_BUILD_PLAN.md` Passes 0–14  
**Purpose:** Ensure each construction pass extends one permanent system rather than creating feature islands that must be rewired later.

---

## 1. Audit Verdict

The Phase 3 plan has the correct major feature order and a strong real-data safety gate.

The principal pre-construction weakness is **cross-layer wiring**.

A feature-by-feature sequence can still fail if each pass independently creates:
- its own query logic;
- its own financial aggregates;
- its own confidence rules;
- its own recalculation behavior;
- direct UI-to-database coupling;
- ad hoc audit writes;
- duplicate representations of calendar/recurring events;
- AI context assembled differently from dashboard context.

Budget therefore needs a permanent **system spine** established early and extended in every pass.

The hardened doctrine is:

> **Every financial fact enters once, is interpreted through explicit layers, produces versioned derived state, and reaches the UI and Lewis through shared read models. Corrections travel back through controlled commands and invalidate/recompute all affected downstream state.**

---

## 2. Missing or Under-Specified Areas Found by Audit

### 2.1 No explicit command/query architecture
The plan describes services, but not a hard rule separating mutations from reads. Without it, screens may mutate Prisma directly or duplicate financial logic.

**Hardening:** introduce application commands for mutations and query/read-model services for presentation.

### 2.2 No explicit dependency/recalculation graph
A category correction can affect baseline spending, necessary-spend assumptions, forecast, TAC, dashboard and Lewis. The current plan says “recalculate affected aggregates” but does not define how downstream dependencies are discovered.

**Hardening:** build a versioned invalidation/recompute coordinator.

### 2.3 No canonical derived-state policy
It is unclear which values are computed on demand versus persisted snapshots. This can create stale dashboard numbers.

**Hardening:** define authoritative facts vs derived projections/read models; persisted derived artifacts carry input/version fingerprints.

### 2.4 No domain-event/change propagation contract
Audit events record history but should not be abused as the internal recalculation bus.

**Hardening:** distinguish Domain Change Events from immutable Audit Events.

### 2.5 No explicit read-model/view-model layer
Dashboard, calendar and Lewis could each query raw tables differently.

**Hardening:** shared financial snapshot/read-model contract feeds both UI and Lewis.

### 2.6 No lifecycle/state-machine registry
ImportBatch, ReviewItem, RecurringSeries, Recommendation and ReconciliationSession have states but transitions are not centrally defined.

**Hardening:** explicit allowed transitions + transition tests.

### 2.7 No migration compatibility doctrine
Prisma migrations are selected, but not the rule for preserving imported financial history as schema evolves.

**Hardening:** forward-only migrations, no destructive reset after real-data gate, backup before consequential migration, data migrations explicit/tested.

### 2.8 No schema/data contract versioning for import adapters
Bank export formats change.

**Hardening:** mapping profiles and normalization contracts are versioned.

### 2.9 No formal calculation policy registry
Forecast/TAC versions are mentioned but not centrally governed.

**Hardening:** calculation policy IDs/versions become first-class and appear in explainability artifacts.

### 2.10 No exact same-day cash-flow ordering doctrine
Pass 7 tests same-day ordering but does not establish the policy mechanism.

**Hardening:** deterministic event priority/tie-break contract; never rely on database row order.

### 2.11 No performance/data-volume budgets
A one-year checking ledger is modest, but poor N+1/query design can still make review/dashboard sluggish.

**Hardening:** indexes/query budgets and realistic synthetic volume fixture.

### 2.12 No accessibility/design-system foundation early enough
UX appears mainly in Pass 9. Rebuilding components late would create churn.

**Hardening:** Pass 1 creates tokens/primitives/layout/navigation/form/table/status components; later passes compose them.

### 2.13 No explicit error taxonomy/recovery model
Import, database, calculation and AI failures need different user behavior.

**Hardening:** typed application errors and recoverable user states from the start.

### 2.14 No clock abstraction
Financial software tests become brittle if domain code calls current time directly.

**Hardening:** injected Clock/household date service; deterministic tests.

### 2.15 No stable ID/fingerprint doctrine
IDs and fingerprints are mentioned but not standardized.

**Hardening:** central ID/fingerprint utilities and canonical serialization rules.

### 2.16 No database transaction boundary doctrine
Multi-step operations such as import, retroactive correction and reconciliation need atomicity.

**Hardening:** application service owns transaction boundary; UI never orchestrates multi-write consistency.

### 2.17 No explicit optimistic/concurrency posture
Even local Steve/Kelly use can create two tabs/actions.

**Hardening:** updated-at/version checks for consequential mutable interpretations and idempotency keys for repeatable commands where needed.

### 2.18 No delete/archive/retention semantics
Financial evidence should not be casually hard-deleted.

**Hardening:** immutable evidence; archive/void/supersede semantics for interpretations; explicit local source-file retention.

### 2.19 No complete synthetic “golden household”
Individual fixtures exist, but not one end-to-end known-answer dataset.

**Hardening:** create a synthetic 12-month Golden Household with hand-calculated expected outputs and edge cases.

### 2.20 No formal build-state ledger
Pass reports are required, but nothing canonical tracks pass/gate state.

**Hardening:** add a machine-readable build-state file and human-readable progress ledger.

---

## 3. Permanent Layer Architecture

Every pass must respect these layers.

### Layer 0 — Safety & Build Governance
Repository guardrails, environment policy, real-data gate, build-state ledger, validation.

### Layer 1 — Evidence
Immutable external facts:
- ImportBatch;
- RawTransaction;
- later source documents/provider payloads.

Evidence is never rewritten to make interpretation convenient.

### Layer 2 — Interpretation
Current household meaning:
- normalized Transaction;
- Merchant;
- Category;
- ClassificationRule;
- attribution;
- recurring relationships;
- Bill/Income interpretations;
- confirmation/confidence/provenance.

### Layer 3 — Planning / Financial Model
Household policy and expected future state:
- BudgetBaseline/Target;
- ProtectedAllocation;
- IncomeEvent;
- CalendarEvent;
- necessary-spend assumptions;
- carry-forward;
- later goals/debt plans.

### Layer 4 — Deterministic Intelligence
Pure/versioned engines:
- recurrence analysis;
- forecast;
- TAC;
- safety/runway;
- reconciliation math;
- opportunity metrics.

No OpenAI dependency.

### Layer 5 — Financial Snapshot / Read Models
Shared presentation contract assembled from Layers 1–4:
- current cash truth;
- TAC;
- safety/trajectory;
- upcoming events;
- spending position;
- recurring obligations;
- Data Trust;
- evidence links;
- warnings/confidence.

**Dashboard and Lewis consume the same snapshot family.**

### Layer 6 — Advisory Intelligence
Lewis:
- classification candidates where useful;
- explanation;
- prioritization;
- recommendation;
- briefing.

Lewis cannot bypass Layers 1–5 to invent authoritative state.

### Layer 7 — Experience
React/UI:
- commands for mutations;
- read models for display;
- progressive disclosure;
- evidence/drill-down;
- accessibility/responsiveness.

### Layer 8 — Future Action
Payments, cancellations, banking actions, etc. **Not implemented in Alpha.**
Future action layer must sit behind authorization/audit and consume the same domain model.

---

## 4. Canonical Dataflow

```text
External financial evidence
        ↓
[Evidence Layer]
ImportBatch + immutable RawTransaction
        ↓
[Interpretation Pipeline]
normalize → merchant fingerprint → deterministic rules → inference → review
        ↓
[Household Meaning]
Transaction + Income + Recurring + Bill + classifications
        ↓
[Planning Layer]
reserve + budget baseline/targets + calendar + assumptions
        ↓
[Deterministic Engines]
forecast + TAC + safety + reconciliation/trust
        ↓
[Financial Snapshot / Read Models]
        ↙                         ↘
[UI / Drilldowns]             [Lewis Context]
        ↑                         ↓
        └──── user commands ← recommendations
                    ↓
          controlled mutation
                    ↓
         invalidation/recompute
                    └──────────────→ refreshed snapshot
```

No later pass may create a shortcut that bypasses this flow for authoritative state.

---

## 5. Command / Query Contract

### Commands
All mutations enter through named application commands/services, e.g.:
- ImportTransactions
- CorrectTransaction
- CreateClassificationRule
- ApplyRuleRetroactively
- ConfirmRecurringSeries
- ConfirmIncomeSource
- SetAuthoritativeBalance
- SetProtectedAllocation
- Start/CompleteReconciliation
- Accept/RejectRecommendation

A command:
1. validates authorization/household scope;
2. validates input;
3. opens DB transaction where needed;
4. changes domain state;
5. emits Domain Change Event(s);
6. writes Audit Event(s);
7. commits;
8. invokes/schedules downstream recomputation;
9. returns a typed result.

### Queries
UI and Lewis do not assemble financial truth through arbitrary Prisma calls.

Use query services/read models such as:
- getHomeFinancialSnapshot
- getTransactionLedger
- getReviewQueue
- getCalendarProjection
- getTACExplanation
- getReconciliationView
- getLewisBriefingContext

Direct Prisma use is confined to repository/server persistence code.

---

## 6. Domain Change Events vs Audit Events

### Domain Change Event
Operational signal describing what changed and what downstream models may now be stale.

Examples:
- TransactionsImported
- TransactionInterpretationChanged
- ClassificationRuleChanged
- RecurringSeriesConfirmed
- IncomeEventChanged
- ProtectedAllocationChanged
- AuthoritativeBalanceChanged
- ReconciliationStatusChanged

Initially these may be handled synchronously/in-process; architecture must allow later queue processing.

### Audit Event
Immutable human/accountability history of material action.

Do not make audit logs the only mechanism by which the application knows what to recalculate.

---

## 7. Recalculation / Invalidation Graph

Create a central dependency registry.

Examples:

**TransactionsImported**
→ merchant/classification
→ recurring analysis
→ budget baseline
→ calendar candidates
→ forecast
→ TAC
→ financial snapshot
→ Lewis context stale

**TransactionInterpretationChanged**
→ category aggregates
→ budget baseline
→ recurring where relevant
→ necessary-spend assumptions
→ forecast
→ TAC
→ snapshot
→ Lewis stale

**RecurringSeriesConfirmed**
→ bill/income relationship
→ calendar
→ forecast
→ TAC
→ snapshot
→ Lewis stale

**ProtectedAllocationChanged**
→ forecast
→ TAC
→ snapshot
→ Lewis stale

**AuthoritativeBalanceChanged / ReconciliationStatusChanged**
→ Data Trust
→ forecast confidence
→ TAC confidence
→ snapshot
→ Lewis stale

### Alpha implementation
This does not require Kafka or a distributed event system. A typed in-process recompute coordinator is enough.

The important requirement is one explicit dependency graph rather than scattered “remember to refresh X” code.

---

## 8. Derived-State Policy

### Authoritative
Persist:
- source evidence;
- user confirmations;
- household rules/policies;
- current interpretations;
- reconciliation decisions;
- protected allocations;
- accepted planning facts.

### Derived
Compute/recompute:
- aggregates;
- recurring candidates;
- forecast entries;
- TAC;
- safety metrics;
- dashboard snapshot;
- Lewis context.

Derived state may be persisted for performance/audit only if it includes:
- calculation/model version;
- input fingerprint;
- generated timestamp;
- household;
- stale/superseded semantics.

Never let a cached derived value silently outrank newer authoritative facts.

---

## 9. Golden Household Test Harness

Before real data, create a synthetic 12-month household designed to exercise the entire system.

Include:
- biweekly salary;
- quarterly bonus;
- reimbursement;
- ordinary debit spending;
- utilities with seasonal variation;
- subscriptions;
- mortgage-like recurring payment;
- vehicle-like payment ending during the year;
- refund;
- cash withdrawal;
- transfer;
- merchant descriptor variants;
- one duplicate CSV export;
- overlapping export;
- one missing-row reconciliation scenario;
- category correction requiring retroactive rule;
- recurring amount increase;
- next-payday horizon;
- protected reserve;
- spend-forward example;
- unresolved/predicted item.

Maintain a **Golden Expected Results** file containing non-sensitive hand-calculated expectations:
- row counts;
- duplicate handling;
- income totals;
- transfer neutrality;
- selected category totals;
- recurring series;
- forecast checkpoints;
- TAC result;
- reconciliation difference;
- dashboard key numbers.

This becomes the permanent regression fixture for every major pass.

---

## 10. State Machines

Before implementing each lifecycle entity, define transitions.

Examples:

### ImportBatch
PREVIEWED → VALIDATED → COMMITTED → PROCESSED
with FAILED/ROLLED_BACK where appropriate.

### ReviewItem
OPEN → DEFERRED | RESOLVED | DISMISSED
with reopen support where needed.

### RecurringSeries
CANDIDATE → CONFIRMED | REJECTED
confirmed series may later become INACTIVE.

### Recommendation
PROPOSED → ACCEPTED | REJECTED | EXPIRED/SUPERSEDED.

### ReconciliationSession
OPEN → RESOLVED | CLOSED_WITH_DIFFERENCE | ABANDONED.

Illegal transitions fail explicitly and are tested.

---

## 11. Calculation Policy Registry

Create a central registry for:
- Money/sign convention;
- event ordering;
- forecast inclusion;
- confidence treatment;
- TAC formula version;
- reserve treatment;
- rounding rules;
- safety/runway formula version.

Every calculation artifact references the applicable policy version.

### Same-day ordering
Define explicit priority, for example through a policy table, rather than relying on insertion/database order. The final business ordering should be documented and tested before real data.

---

## 12. UI Contract from Pass 1

Do not wait until Pass 9 to create design structure.

Pass 1 should establish reusable primitives:
- app shell;
- responsive navigation;
- page header;
- financial amount;
- status/confidence badge;
- card;
- table/list;
- empty/error/loading state;
- form fields;
- confirmation dialog;
- drill-down drawer/modal/page pattern;
- evidence/source indicator;
- alert/advisory panel;
- accessible focus/keyboard behavior;
- typography/spacing tokens.

Feature passes use these primitives. Pass 9 integrates/refines rather than redesigning the application.

---

## 13. Error & Recovery Contract

Define typed categories:
- ValidationError
- ImportFormatError
- DuplicateConflict
- AuthorizationError
- NotFound
- DomainInvariantError
- ReconciliationRequired
- CalculationUnavailable
- ExternalAIUnavailable
- PersistenceError
- ConfigurationError

User-facing surfaces translate them into calm, actionable states.

Never expose stack traces, database payloads, secrets or raw AI provider errors to the user.

---

## 14. Clock, IDs and Fingerprints

### Clock
Domain services receive a Clock/household-time abstraction. Tests use a fixed clock.

### IDs
Choose one stable server-generated ID strategy and use it consistently.

### Fingerprints
Central canonical hashing utilities define:
- file fingerprint;
- source-row fingerprint;
- AI input fingerprint;
- calculation input fingerprint.

Fingerprint serialization is versioned to avoid accidental changes when object property ordering changes.

---

## 15. Transactionality and Concurrency

Atomic operations include:
- import commit;
- retroactive correction;
- classification-rule application;
- reconciliation completion;
- consequential configuration changes.

Use database transactions at application-service boundaries.

For mutable interpretations, use version/updated-at conflict detection where stale browser state could overwrite a newer change.

Repeatable commands/imports use idempotency where appropriate.

---

## 16. Migration Doctrine

### Before real-data gate
Schema may evolve rapidly, but migrations must still be reproducible from empty DB.

### After real-data gate
- no casual database reset;
- forward migrations only;
- protected backup before consequential migration;
- destructive changes require explicit data migration and verification;
- migration tested against a copy/synthetic equivalent first;
- schema version recorded in backup/diagnostics.

This rule must be present before Pass 12.

---

## 17. Performance Budgets

Use the Golden Household plus a larger synthetic fixture.

Alpha targets:
- common dashboard/read-model query feels immediate locally;
- transaction ledger supports at least several years/tens of thousands of rows without architectural change;
- no N+1 query patterns in transaction/review/recurring lists;
- indexes exist for household/date/account/merchant/category/status relationships used frequently;
- AI is never on the critical path for deterministic page rendering.

Exact millisecond thresholds may be measured during construction rather than invented now; regressions should be recorded.

---

## 18. Layered Construction Overlay for Passes 0–14

The 15-pass count remains valid. The passes are hardened as follows.

### Pass 0 — Governance layer
Add:
- build-state ledger;
- real-data gate;
- leak checks;
- architecture boundary docs;
- canonical dependency map.

### Pass 1 — Runtime + UX primitive layer
Add:
- app/runtime/database;
- design tokens/primitives;
- typed errors;
- Clock;
- ID/fingerprint utilities;
- service/repository/query directory contracts.

### Pass 2 — Domain + persistence layer
Add:
- schema;
- repositories;
- commands/queries;
- audit;
- Domain Change Events;
- state-machine helpers;
- calculation policy registry skeleton;
- migration doctrine.

### Pass 3 — Evidence ingestion vertical slice
Build end-to-end:
**CSV → evidence → interpretation shell → import read model → UI**
and trigger change events/recompute hooks even before all downstream engines exist.

### Pass 4 — Interpretation vertical slice
**Transaction → merchant/category/type → read model → correction command → invalidation**

### Pass 5 — Learning vertical slice
**review → correction → rule → retroactive transaction → audit/change event → recompute**

### Pass 6 — Reconstruction vertical slice
**transactions → recurring/income/bills/baseline → calendar candidates → snapshot inputs**

### Pass 7 — Deterministic intelligence vertical slice
**planning inputs → forecast → TAC → safety → explainability → snapshot**
using Golden Household known answers.

### Pass 8 — Trust vertical slice
**authoritative balance → reconciliation → Data Trust → confidence propagation → snapshot**

### Pass 9 — Experience composition layer
Dashboard consumes shared snapshot/read models only. No new financial formulas in React.

### Pass 10 — Advisory layer
Lewis consumes the same shared snapshot/evidence contracts and may issue commands only through approved human workflows.

### Pass 11 — Operational safety layer
Access, backup/restore, leak scanning, migration/restore proof, real-data readiness.

### Pass 12 — Real-data calibration
Household facts/rules enter through product commands, not code patches.

### Pass 13 — Hardening
Performance, accessibility, mobile, trust clarity, false-positive reduction.

### Pass 14 — Acceptance
Full Golden Household regression + real household Gates A–J.

---

## 19. Build-State Ledger

Create:
- `data/build_state.json` or equivalent machine-readable state;
- `docs/BUILD_PROGRESS.md` human-readable mirror.

Track:
- active pass;
- completed passes;
- validation status;
- operator gates;
- real-data authorization false/true;
- construction progress;
- trust readiness dimensions;
- known blockers;
- last validated commit.

Cursor updates this only after gates pass.

---

## 20. Hardened Definition of “Wired Correctly”

A feature is not complete merely because its screen works.

It is wired correctly only when:

1. authoritative source is identified;
2. domain object exists;
3. household scope is enforced;
4. mutation uses a command/service;
5. DB transaction boundary is correct;
6. provenance is preserved;
7. material action is audited;
8. Domain Change Event/invalidation occurs;
9. affected derived state recomputes or becomes explicitly stale;
10. shared read model updates;
11. UI renders from the read model;
12. Lewis sees the same truth if relevant;
13. tests cover invariants and failure/retry behavior;
14. Golden Household regression remains green.

This checklist applies to every construction pass.

---

## 21. Pre-Construction Hard Gate

Before Pass 0 implementation begins, the build plan is considered hardened only when Cursor is instructed that:

- `PHASE_3_CURSOR_BUILD_PLAN.md` defines pass scope;
- this document defines cross-pass wiring requirements;
- neither may be ignored;
- a pass cannot create shortcuts around the layered architecture;
- the Golden Household is the permanent synthetic regression spine;
- build-state/trust readiness must be updated after every pass;
- no real household data before the explicit gate.

---

## 22. Final Audit Conclusion

No additional major product-discovery cycle is needed.

The missing work was architectural **connective tissue**, not another feature list.

The hardened construction model is:

> **Evidence → Interpretation → Planning → Deterministic Intelligence → Shared Financial Snapshot → Lewis → Experience**

with controlled commands flowing back down and a central invalidation/recompute graph keeping everything synchronized.

This is the architecture that should be wired from the first construction pass onward.

### Hardened build north star

**One financial truth. One dependency spine. Many views. No hidden rewiring later.**
