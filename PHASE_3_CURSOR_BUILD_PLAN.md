# Budget — Phase 3 Cursor Build Plan

**Version:** 1.0  
**Status:** Implementation plan baseline  
**Inputs:** `PHASE_1_PRODUCT_SPECIFICATION.md`, `PHASE_2_TECHNICAL_BLUEPRINT.md`  
**Target:** Local Steve + Kelly Household Alpha — Definition of Alive  
**Execution model:** Large sequential Cursor passes with hard validation gates

---

## 1. Mission

Build the smallest trustworthy Budget that can ingest the household's real bank history, reconstruct financial reality, learn from corrections, calculate near-term safe-to-spend cash, reconcile to the real bank balance, and produce a grounded Lewis briefing.

This plan is intentionally ordered so visual polish cannot outrun financial correctness and real household data cannot enter the system before the safety/trust foundation is proven.

---

## 2. Cursor Operating Contract

Before every pass, Cursor must read:

1. `MASTER_PRODUCT_PLAN.md`
2. `PHASE_1_PRODUCT_SPECIFICATION.md`
3. `PHASE_2_TECHNICAL_BLUEPRINT.md`
4. this file
5. relevant code/docs from prior passes

### Cursor rules

- Do not invent new product behavior when the canonical documents answer the question.
- Do not broaden Alpha scope with deferred roadmap features.
- Do not place real household financial data, API keys, secrets, screenshots, dumps or CSVs in Git.
- Use synthetic fixtures until the Real-Data Gate is explicitly approved.
- Do not let Lewis perform authoritative financial arithmetic.
- Do not mutate raw imported evidence after commit.
- Every pass ends with validation and a concise implementation report.
- Fix red gates before beginning the next pass.
- Prefer coherent major passes over scattered micro-patches.
- No Netlify/production deployment during Alpha construction.
- No direct bank/payment integrations.
- No Wealth Builder implementation.
- No Self-Bank implementation beyond future-safe schema boundaries explicitly required by the current pass.

### Required pass report

At the end of each pass, Cursor reports:
- files created/changed;
- migrations created;
- commands run;
- tests/results;
- acceptance criteria status;
- known limitations;
- whether any deferred-scope feature was touched;
- next pass readiness;
- explicit statement that no real household data/secrets were committed.

---

## 3. Build Program Overview

### Pass 0 — Repository Preflight & Guardrails
### Pass 1 — Application Foundation & Local Database
### Pass 2 — Financial Domain Spine & Audit Model
### Pass 3 — Synthetic CSV Import Engine
### Pass 4 — Transaction Ledger, Merchant Normalization & Classification
### Pass 5 — Review Queue & Teach-Budget Learning
### Pass 6 — Recurring Income, Bills & Household Reconstruction
### Pass 7 — Forecast, Calendar & True Available Cash Engine
### Pass 8 — Reconciliation & Data-Trust Layer
### Pass 9 — Alpha Dashboard & Core UX Integration
### Pass 10 — Lewis Advisory Layer
### Pass 11 — Security, Backup/Restore & Real-Data Readiness
### HARD GATE — Operator Approval for Real Household Data
### Pass 12 — First Real Household Import & Calibration
### Pass 13 — Alpha UX/Trust Hardening
### Pass 14 — Definition-of-Alive Acceptance & Household Alpha Closeout

---

# PASS 0 — Repository Preflight & Guardrails

## Goal
Make the repository safe to build before application code or financial data exists.

## Build
- audit current repository contents;
- establish README/start-here developer orientation;
- add/strengthen `.gitignore`;
- explicitly ignore:
  - `.env`, `.env.local`, environment variants except safe example;
  - `local-data/`;
  - CSV/XLS/XLSX/QFX/OFX financial import files;
  - PostgreSQL dumps/backups;
  - logs containing runtime data;
  - screenshots/temp exports where appropriate;
- add `.env.example` with variable names only;
- add synthetic-fixture doctrine;
- create docs/runbooks structure;
- add architecture/product docs references;
- add secret/data leak check script;
- define no-real-data marker/gate document;
- create initial package metadata only as needed for validation tooling.

## Acceptance
- repository contains no known secrets/real financial files;
- ignored paths demonstrably stay untracked;
- secret/data scan passes;
- canonical docs linked from README;
- explicit REAL_DATA_ALLOWED=false-style operator gate exists as policy/config, default false.

## Validation
- Git status clean after generated/local test artifacts;
- leak/secret validation script;
- documentation links resolve.

## Operator review
Confirm repository safety before scaffolding.

---

# PASS 1 — Application Foundation & Local Database

## Goal
Create a running local application and repeatable PostgreSQL development environment.

## Build
- scaffold Next.js + React + TypeScript;
- establish App Router structure from Phase 2;
- add Prisma;
- add PostgreSQL Docker Compose with loopback-only port binding;
- configure DATABASE_URL through environment;
- create base scripts:
  - dev
  - typecheck
  - lint
  - test
  - db:up/down
  - db:migrate
  - db:seed
  - validate
- install/configure Vitest and Playwright foundations;
- install Zod;
- create local app shell/navigation placeholders;
- add local configuration health page/status;
- create DB connection health check;
- establish formatting/lint policy;
- create synthetic seed household with Steve/Kelly-like generic aliases, not real financial data.

## Acceptance
- fresh clone/setup instructions work;
- PostgreSQL starts locally;
- migration command works;
- app connects to DB;
- app binds locally;
- typecheck/lint/tests green;
- no OpenAI required to run core shell.

## Operator review
Open the local app and confirm shell/navigation/dev ergonomics.

---

# PASS 2 — Financial Domain Spine & Audit Model

## Goal
Implement the physical schema and pure money/domain primitives before import UI.

## Build
Prisma schema/migrations for Alpha entities:
- Household
- HouseholdMember
- FinancialAccount
- ImportBatch
- RawTransaction
- Transaction
- Merchant / alias-fingerprint structure
- Category
- ClassificationRule
- IncomeSource / IncomeEvent
- RecurringSeries
- Bill
- BudgetBaseline / BudgetTarget
- ProtectedAllocation
- CalendarEvent
- ForecastRun / ForecastEntry
- Recommendation
- ReviewItem
- ReconciliationSession / Adjustment
- AuditEvent
- ModelArtifact

Implement:
- household scoping;
- IDs/timestamps;
- immutable-raw service boundary;
- integer-cent Money helpers;
- sign convention;
- date/timezone conventions;
- confidence/status enums;
- provenance structures;
- audit service;
- repository/service boundaries;
- synthetic seed categories/accounts.

## Tests
- money arithmetic;
- sign convention;
- transfer neutrality primitives;
- household isolation queries;
- raw transaction mutation prohibited by service;
- audit append behavior;
- schema constraints.

## Acceptance
- clean migration from empty DB;
- seed works;
- all financial amounts use approved representation;
- domain tests do not import React/OpenAI;
- every household-owned record is scope-safe.

---

# PASS 3 — Synthetic CSV Import Engine

## Goal
Prove trustworthy ingestion with synthetic bank exports.

## Build
- CSV parser adapter;
- upload/select UI;
- preview;
- column detection/mapping;
- date/amount/description/balance mapping;
- support signed-amount and separate debit/credit forms;
- validation;
- file fingerprint;
- row fingerprint;
- duplicate/overlap analysis;
- transactional import commit;
- immutable RawTransaction creation;
- normalized Transaction creation;
- import summary;
- rollback/failure behavior;
- import history;
- synthetic fixture set covering:
  - ordinary checking;
  - duplicate export;
  - overlapping date ranges;
  - malformed rows;
  - separate debit/credit;
  - signed amount;
  - refund;
  - transfer-like descriptions;
  - repeated merchants.

## Critical invariant
Re-importing the same file or overlapping export cannot silently duplicate authoritative financial activity.

## Tests
- parser unit tests;
- fingerprint determinism;
- exact duplicate detection;
- overlap warnings;
- transactional failure;
- row-count accounting;
- idempotency integration test.

## Acceptance
Phase 1 Gate A passes with synthetic data.

---

# PASS 4 — Transaction Ledger, Merchant Normalization & Classification

## Goal
Turn raw imports into a usable, inspectable financial ledger.

## Build
- Transactions screen;
- filters/search;
- transaction detail/evidence view;
- merchant normalization/fingerprints;
- original descriptor preserved;
- category hierarchy;
- deterministic classification rules;
- income/expense/transfer/reimbursement types;
- classification precedence implementation;
- confidence/source badges;
- unresolved classification creation;
- transaction attribution foundations;
- calculation-safe treatment of transfers/reimbursements.

## Tests
- merchant normalization;
- precedence;
- transfer neutrality;
- reimbursement treatment;
- evidence linkage;
- category aggregates;
- no raw mutation.

## Acceptance
A synthetic year can be browsed and meaningfully summarized without Lewis.

---

# PASS 5 — Review Queue & Teach-Budget Learning

## Goal
Make correction fast and make repeated correction unnecessary.

## Build
- prioritized Review screen;
- materiality scoring;
- confirm/change category;
- merchant correction;
- transaction-type correction;
- recurring-candidate decision hooks;
- correction scope:
  - this only;
  - history;
  - this + future;
  - all history + future;
- retroactive preview:
  - count;
  - date range;
  - aggregate dollars;
- ClassificationRule creation;
- deterministic future rule application;
- rule inspection/disable;
- audit events;
- recalculate affected aggregates.

## Tests
- scope behavior;
- preview equals applied set;
- retroactive operation atomicity;
- disabled rule no longer applies;
- user override outranks lower inference;
- audit/provenance.

## Acceptance
Phase 1 Gate C passes with synthetic data.

---

# PASS 6 — Recurring Income, Bills & Household Reconstruction

## Goal
Reconstruct the household's observed financial rhythm from transaction history.

## Build
- recurrence detection engine;
- cadence clustering;
- amount range;
- timing window;
- evidence list;
- recurring-series confirmation/rejection;
- recurring Bills screen;
- separate inferred withdrawal timing from contractual due date;
- income-source detection/confirmation;
- biweekly income support;
- observed budget baseline by meaningful categories;
- current-month/pay-period spending position;
- recurring-charge annualized run rate;
- high-impact unresolved items routed to Review;
- first reconstruction summary.

## Tests
- monthly/biweekly/quarterly/annual synthetic patterns;
- variable utility range;
- false-positive resistance;
- next expected window;
- income confidence;
- contractual-date separation.

## Acceptance
Phase 1 Gate B passes with synthetic data.

---

# PASS 7 — Forecast, Calendar & True Available Cash Engine

## Goal
Build the deterministic heart of Budget.

## Build
- pure forecast engine;
- versioned calculation policy;
- CalendarEvent generation/linkage;
- chronological cash projection;
- protected Cash Reserve allocation;
- 10% reserve-policy representation sufficient for Alpha;
- next-income horizon selection;
- necessary/normal spending assumptions;
- confidence inclusion policy;
- negative carry-forward/spend-forward;
- True Available Cash engine;
- structured explainability result;
- no-double-counting protections;
- Calendar UI;
- TAC drilldown UI;
- safety/cash-direction summary;
- confidence degradation warnings.

## Tests — exhaustive
- integer money invariants;
- protected reserve exclusion;
- confirmed vs predicted event treatment;
- next-income boundary;
- negative TAC;
- spend-forward;
- no double counting;
- same inputs/version = same outputs;
- prospective income excluded;
- transfer neutrality;
- ordering on same-day events defined/tested;
- edge dates/paydays;
- missing next income graceful state.

## Acceptance
Phase 1 Gates E and F pass with synthetic data.

## Operator review
This is a major trust checkpoint. Review calculations with hand-worked synthetic examples before continuing.

---

# PASS 8 — Reconciliation & Data-Trust Layer

## Goal
Make disagreement with the bank visible and solvable.

## Build
- authoritative current balance entry/confirmation;
- Reconciliation screen;
- modeled-vs-authoritative comparison;
- candidate causes:
  - import gaps;
  - duplicates/overlap;
  - pending/timing;
  - transfers;
  - refunds/reversals;
  - cash;
  - fees/interest;
  - manual interpretations;
- resolution actions;
- explicit ReconciliationAdjustment;
- reconciled-through date/balance;
- unresolved difference;
- Data Trust status consumed by forecast/TAC;
- audit history.

## Tests
- exact reconciliation;
- unresolved difference;
- adjustment is not Transaction/RawTransaction;
- forecast confidence degradation;
- correction closes difference;
- audit.

## Acceptance
Phase 1 Gate D passes with synthetic data.

---

# PASS 9 — Alpha Dashboard & Core UX Integration

## Goal
Make the financial engine understandable at a glance.

## Build
Home dashboard hierarchy:
1. Lewis briefing placeholder/degraded deterministic summary;
2. dominant True Available Cash;
3. safety/trajectory;
4. next financial events;
5. protected reserve;
6. spending position;
7. bills/recurring;
8. Data Trust.

Implement:
- responsive navigation;
- desktop/mobile layouts;
- progressive disclosure;
- drilldowns/drawers/pages;
- evidence links;
- empty/loading/error states;
- provisional/confirmed/predicted visual language;
- no debug/architecture copy in user-facing UI;
- accessible semantics/keyboard/focus;
- financial number formatting.

## UX rule
Layer 1 stays simple. Complexity moves into drill-down.

## Acceptance
Phase 1 Gate G passes with synthetic data except live Lewis content.

## Operator review
Hostile UX review: can Steve understand the household position without being taught the application?

---

# PASS 10 — Lewis Advisory Layer

## Goal
Add the named advisor without weakening deterministic truth.

## Build
- server-only OpenAI gateway;
- Zod structured contracts;
- model/config environment variables;
- configured/unconfigured health;
- daily briefing contract;
- recommendation contract;
- optional ambiguous transaction classification candidate;
- data minimization;
- input fingerprints/caching/model artifacts;
- token/cost telemetry;
- graceful no-AI mode;
- prompt doctrine:
  - Lewis voice;
  - no invented facts;
  - distinguish known/inferred/predicted;
  - explain why;
  - strong affordability advice allowed when supported;
  - consequential decisions remain human;
- Lewis activity/recommendation audit where material.

## Security
Never expose API key client-side or in logs.

## Tests
- contract validation;
- malformed model output fallback;
- no-AI fallback;
- grounded fixture tests;
- prompt/context excludes unnecessary raw history;
- deterministic dashboard numbers unchanged whether Lewis is enabled or disabled.

## Acceptance
Phase 1 Gate H passes with synthetic data.

---

# PASS 11 — Security, Backup/Restore & Real-Data Readiness

## Goal
Prove the local Alpha is safe enough to receive actual household financial history.

## Build
- local household access gate/session;
- Steve/Kelly Owner actor selection/identity;
- salted credential hashing/environment-backed setup as appropriate;
- loopback-only runtime verification;
- DB exposure verification;
- log redaction review;
- secret scanning;
- financial-file Git leak scanning;
- backup script;
- encrypted/protected backup-location requirement;
- restore-check script into disposable DB;
- schema-version metadata;
- local-data cleanup/retention controls;
- real-data readiness command/report;
- operational runbook.

## Mandatory operator prerequisites
Before Real-Data Gate:
- development machine full-disk encryption confirmed by operator;
- strong OS login confirmed;
- backup destination protection confirmed;
- no public/LAN exposure unless separately reviewed.

## Tests
- access gate;
- backup succeeds;
- restore reproduces synthetic state;
- secrets absent from build/client;
- ignored real-data extensions stay untracked;
- logs inspected;
- `npm run validate` green.

## Acceptance
Phase 1 Gate I passes.

---

# HARD GATE — REAL HOUSEHOLD DATA AUTHORIZATION

Cursor must STOP.

No real Steve/Kelly CSV is imported until the operator explicitly approves crossing this gate after reviewing the Pass 11 readiness report.

Required green evidence:
- Passes 0–11 complete;
- all validation green;
- import idempotency proven;
- TAC hand-worked cases proven;
- reconciliation proven;
- raw immutability proven;
- backup + restore tested;
- Git/secret/data leak checks green;
- local access controls active;
- operator confirms disk/OS/backup protections.

Only then set the local operational gate to allow real-data ingestion. The repository itself must never contain the real financial file/data.

---

# PASS 12 — First Real Household Import & Calibration

## Goal
Cross from software proof to household truth carefully.

## Operator-assisted workflow
1. create fresh protected backup/checkpoint;
2. import the first real CSV locally;
3. record only non-sensitive diagnostics in Git/docs;
4. inspect row/date totals;
5. resolve mapping;
6. verify duplicate/overlap behavior;
7. inspect high-dollar inflows/outflows;
8. review transaction-type classifications;
9. confirm recurring income;
10. confirm major bills/recurring charges;
11. work Review queue;
12. establish authoritative current balance;
13. reconcile;
14. configure/confirm reserve state;
15. verify next income;
16. run forecast/TAC;
17. compare critical calculations manually;
18. enable Lewis briefing only after deterministic outputs are trusted.

## Rule
Do not patch code to “make Steve's numbers look right.” Fix generalized logic or record household-specific facts/rules through the product.

## Output
A local calibration report may record:
- row counts;
- accuracy percentages;
- unresolved item counts;
- reconciliation difference/status;
- categories needing rule improvements;
- forecast/TAC validation status;
but must not commit sensitive transaction detail.

## Acceptance
Real data successfully exercises Gates A–F.

---

# PASS 13 — Alpha UX / Trust Hardening

## Goal
Use real household experience to remove friction without changing core doctrine.

## Build only evidence-driven fixes:
- confusing labels;
- correction speed;
- merchant-rule ergonomics;
- review prioritization;
- recurring false positives;
- calendar readability;
- TAC explanation;
- reconciliation clarity;
- Lewis usefulness/noise;
- mobile responsiveness;
- accessibility;
- performance on one-year transaction history.

## Hostile user audit
Review every Alpha screen for:
- “What am I looking at?”
- “What should I do?”
- “Can I trust this number?”
- “Where did it come from?”
- “Can I fix it?”
- “What happens if I change it?”

Remove:
- developer copy;
- architecture cues;
- redundant cards;
- excessive warnings;
- unexplained confidence labels;
- dead-end screens.

## Acceptance
The household can operate the daily loop without developer guidance.

---

# PASS 14 — Definition-of-Alive Acceptance & Household Alpha Closeout

## Goal
Prove Budget is alive.

## Formal acceptance test
Using the real local household model, Steve must be able to answer:

1. Are we financially safe right now?
2. What can we actually spend before the next payday?
3. What money is already spoken for?
4. What is likely to happen next?
5. Why does Budget believe these numbers?
6. What does Lewis recommend doing first, and why?

## Gate review
Re-run Phase 1 Gates A–J.

### Required closeout evidence
- import trust;
- reconstruction quality;
- correction learning;
- reconciled/current cash truth;
- forecast reproducibility;
- TAC explainability;
- dashboard usability;
- Lewis grounding;
- local data safety;
- actual household usefulness.

## Deliverables
- `docs/ALPHA_ACCEPTANCE_REPORT.md` with no sensitive financial detail;
- `docs/ALPHA_OPERATOR_RUNBOOK.md`;
- `docs/KNOWN_LIMITATIONS.md`;
- `docs/POST_ALPHA_BACKLOG.md`;
- final validation output;
- next-phase recommendation.

When Gate J passes, status becomes:

**BUDGET HOUSEHOLD ALPHA — ALIVE**

---

## 4. Validation Ladder

Each pass uses the strongest applicable subset; by Pass 11 the standard gate is:

```text
npm run typecheck
npm run lint
npm run test
npm run test:integration
npm run test:e2e
npm run validate
```

Database passes additionally run migration/seed checks.

Import passes run idempotency fixtures.

Financial-engine passes run invariant/hand-worked cases.

Security pass runs backup/restore and leak checks.

A red command blocks progression.

---

## 5. Build Progress Model

Track two different progress measures:

### Construction progress
- P0 Guardrails — 4%
- P1 Foundation — 8%
- P2 Domain/Data — 10%
- P3 Import — 10%
- P4 Ledger/Classify — 8%
- P5 Learning — 7%
- P6 Reconstruction — 9%
- P7 Forecast/TAC — 12%
- P8 Reconciliation — 7%
- P9 Dashboard — 7%
- P10 Lewis — 6%
- P11 Security/Readiness — 5%
- P12 Real calibration — 3%
- P13 Hardening — 2%
- P14 Acceptance — 2%

Total: 100%.

### Trust readiness
Track independently:
- data integrity;
- calculation correctness;
- reconciliation;
- explainability;
- AI grounding;
- local security;
- backup/recovery;
- UX comprehension.

Do not call the product “90% ready” merely because many UI files exist. Alpha readiness is gated by trust.

---

## 6. Phase 3 Exit Criteria

Phase 3 is complete when:
- passes are ordered;
- each pass has scope and acceptance criteria;
- real-data boundary is explicit;
- validation ladder is explicit;
- operator review points are explicit;
- deferred scope is protected;
- Cursor can begin Pass 0 without needing to invent architecture or product behavior.

After this plan is accepted, the project enters **Phase 4 — Household Alpha Construction**, beginning with Cursor Pass 0.

---

## 7. First Cursor Mission

The first Cursor execution should be **Pass 0 only**.

Cursor should not immediately scaffold the entire product in the same mission. Pass 0 exists to make the repository safe for financial software before build velocity increases.

After Pass 0 is green and reviewed, proceed to Pass 1.

---

## 8. Build North Star

**Truth before intelligence. Intelligence before automation. Safety before real data.**

Budget is alive only when the household can trust it enough to make a real near-term financial decision from it.
