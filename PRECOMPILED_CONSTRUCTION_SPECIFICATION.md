# Budget — Precompiled Construction Specification

**Version:** 1.0
**Status:** Final pre-script construction specification
**Purpose:** Resolve implementation questions in advance so all 15 Cursor pass scripts can be authored before local construction begins.

---

## 1. Construction Objective

After this specification and the 15 pass scripts are committed, the desired operator workflow is:

1. clone/pull `Grappe501/budget` locally;
2. open the repository in Cursor;
3. instruct Cursor to read the canonical execution index;
4. Cursor executes Pass 0;
5. validates and records evidence;
6. only a GREEN pass unlocks the next pass;
7. Cursor proceeds in order when directed;
8. Cursor stops at explicit operator gates;
9. no implementation-time product or architecture design is delegated to Cursor.

**Cursor is an executor and bounded repair agent, not the product architect.**

---

# 2. Authority Order

When documents conflict, use this precedence:

1. `PRECOMPILED_CONSTRUCTION_SPECIFICATION.md`
2. `LEVEL_10_AUTONOMOUS_CONSTRUCTION_HARDENING.md`
3. `PRE_CONSTRUCTION_FORENSIC_AUDIT_AND_LAYERED_BUILD_HARDENING.md`
4. `PHASE_3_CURSOR_BUILD_PLAN.md`
5. `PHASE_2_TECHNICAL_BLUEPRINT.md`
6. `PHASE_1_PRODUCT_SPECIFICATION.md`
7. `MASTER_PRODUCT_PLAN.md`

A later document may tighten an earlier rule but may not silently weaken financial integrity, security or human-control boundaries.

---

# 3. Fixed Technology Decisions

## Runtime
- Node.js: pin an active LTS major at Pass 0 execution and record exact version in `.nvmrc` and package engines.
- Package manager: npm; lock with `package-lock.json`; use `npm ci` after lock exists.
- TypeScript: strict mode.
- Web framework: Next.js App Router + React.
- Database: PostgreSQL.
- ORM/migrations: Prisma.
- Runtime validation: Zod.
- Unit/integration tests: Vitest.
- Browser/E2E: Playwright.
- Local DB runtime: Docker Compose.
- Styling: CSS variables + CSS Modules/global CSS; do not introduce a large UI framework unless a later operator-approved ADR changes this.
- Icons: one lightweight icon package may be selected/pinned in Pass 1; no icon sprawl.
- AI: server-only OpenAI adapter behind internal interface; application core cannot import OpenAI SDK.
- Currency Alpha: USD only, represented as integer cents.
- Time: household timezone stored as IANA timezone; default household configuration America/Chicago for the initial household, not hard-coded in domain logic.

## Version selection rule
Do not hard-code dependency versions in planning prose that may stale before execution. During Pass 0/1:
- choose mutually compatible stable releases;
- no prerelease/beta/RC;
- pin exact resolved versions in lockfile;
- record versions in generated environment/build report;
- subsequent passes use lockfile and do not opportunistically upgrade dependencies.
Dependency upgrades require a dedicated change with regression gates.

---

# 4. Fixed Repository Layout

Construction should converge on:

```text
/
  MASTER_PRODUCT_PLAN.md
  PHASE_1_PRODUCT_SPECIFICATION.md
  PHASE_2_TECHNICAL_BLUEPRINT.md
  PHASE_3_CURSOR_BUILD_PLAN.md
  PRE_CONSTRUCTION_FORENSIC_AUDIT_AND_LAYERED_BUILD_HARDENING.md
  LEVEL_10_AUTONOMOUS_CONSTRUCTION_HARDENING.md
  PRECOMPILED_CONSTRUCTION_SPECIFICATION.md

  app/
    (household)/
      page.tsx
      transactions/
      budget/
      calendar/
      bills/
      review/
      reconciliation/
      settings/
    operator/
      health/
    api/
      ai/
      internal/

  src/
    domain/
      money/
      time/
      household/
      accounts/
      transactions/
      merchants/
      categories/
      income/
      recurring/
      bills/
      budget/
      calendar/
      forecast/
      tac/
      reconciliation/
      recommendations/
      audit/
      policies/
    application/
      commands/
      queries/
      events/
      tasks/
      recompute/
      auth/
    infrastructure/
      db/
      repositories/
      imports/
      ai/
      logging/
      backup/
      config/
    read-models/
    ui/
      components/
      primitives/
      formatting/
      vocabulary/

  prisma/
    schema.prisma
    migrations/
    seed.ts

  contracts/
    architecture.layers.json
    entities.registry.json
    commands.registry.json
    queries.registry.json
    events.registry.json
    invariants.registry.json
    calculations.registry.json
    states.registry.json
    permissions.registry.json
    capabilities.registry.json
    routes.registry.json
    vocabulary.registry.json
    errors.registry.json
    environment.registry.json
    scripts.registry.json
    data_classification.registry.json

  build/
    schemas/
    slices/
    build_state.json
    latest_validation_report.json
    latest_slice_report.json
    next_slice.json
    current_handoff.json

  docs/
    generated/
    adr/
    runbooks/
    BUILD_PROGRESS.md
    NEXT_BUILD_SLICE.md
    CURRENT_BUILD_HANDOFF.md

  scripts/
    bootstrap/
    validate/
    generate/
    db/
    backup/
    fixtures/

  tests/
    unit/
    integration/
    e2e/
    golden-household/
    calculation-vectors/
    import-corpus/
    date-vectors/
    adversarial/
    failure-injection/
    ai-evals/

  local-data/       # gitignored
    imports/
    backups/
    exports/
    temp/

  docker-compose.yml
  package.json
  package-lock.json
  tsconfig.json
  .env.example
  .gitignore
```

Equivalent Next.js route-group syntax is allowed, but architectural ownership may not be blurred.

---

# 5. Layer Dependency Rules

Allowed direction:

```text
domain ← application ← infrastructure
   ↑          ↑
read-models ← queries
   ↑
ui/app

AI adapter lives in infrastructure and consumes application/read-model contracts.
```

Rules:
- domain imports no Next.js, React, Prisma, OpenAI, filesystem or network code;
- application imports domain, not UI;
- infrastructure implements interfaces owned by domain/application;
- UI uses application commands and query/read-model contracts;
- UI cannot import Prisma;
- AI cannot import Prisma repositories to assemble financial truth;
- authoritative calculations live in domain engines;
- no financial formula in React components;
- scripts may orchestrate but cannot become hidden business-logic owners.

---

# 6. Core Physical Data Model Decisions

Exact Prisma syntax is implemented in Pass 2, but the conceptual fields are fixed here.

## Household
- id
- name
- timezone
- currency = USD
- policyVersion
- createdAt/updatedAt
- rowVersion

## HouseholdMember
- id
- householdId
- displayName
- role OWNER initially
- active
- createdAt/updatedAt
- rowVersion

## FinancialAccount
- id
- householdId
- name
- type CHECKING/SAVINGS/CASH/OTHER initially
- institutionName nullable
- maskedIdentifier nullable; never full account number required
- active
- createdAt/updatedAt
- rowVersion

## ImportBatch
- id
- householdId
- accountId
- sourceType CSV
- adapterVersion
- fileFingerprint
- originalFilename sanitized
- state
- row counts: source/valid/invalid/duplicate/imported
- dateRangeStart/end nullable
- committedAt nullable
- supersededAt nullable
- createdAt/updatedAt

## RawTransaction
- id
- householdId
- accountId
- importBatchId
- sourceRowNumber
- rowFingerprint
- sourceDateText
- sourceDescriptionText
- sourceAmountText
- sourceDebitText nullable
- sourceCreditText nullable
- sourceBalanceText nullable
- canonicalSourcePayload JSON
- parsedPostedDate nullable
- parsedAmountCents nullable
- createdAt
- immutable after committed batch

## Transaction
- id
- householdId
- accountId
- primaryRawTransactionId
- postedDate
- amountCents using fixed sign convention
- postingStatus POSTED initially; PENDING reserved
- economicType
- normalizedDescription
- merchantId nullable
- categoryId nullable
- interpretationAuthority
- reviewStatus
- rowVersion
- createdAt/updatedAt
- voidedAt nullable

### Sign convention
- inflow increases account cash: positive cents;
- outflow decreases account cash: negative cents.
No debit/credit ambiguity survives normalization.

## TransactionAllocation
- id
- householdId
- transactionId
- categoryId nullable
- amountCents
- beneficiaryMemberId nullable
- purpose text nullable
- authority/provenance
- allocations for a transaction must sum exactly to transaction amount when allocation mode is active.

## TransactionRelationship
- id
- householdId
- fromTransactionId
- toTransactionId nullable where cash target is conceptual
- relationshipType TRANSFER_PAIR/REFUND_OF/REIMBURSES/REVERSAL_OF/CASH_ALLOCATION
- amountCents nullable
- confidence
- confirmed
- provenance

## Merchant
- id
- householdId
- canonicalName
- active
- createdAt/updatedAt

## MerchantAlias
- id
- householdId
- merchantId
- normalizedFingerprint
- sourcePattern
- authority
- active
- unique within household where applicable

## Category
- id
- householdId nullable for system category
- name
- parentId nullable
- type
- active
- sortOrder

## ClassificationRule
- id
- householdId
- ruleType
- matcher JSON typed/validated at application boundary
- action JSON typed/validated
- priority
- active
- authority USER_CONFIRMED
- createdBy
- createdAt/updatedAt
- rowVersion

## ReviewItem
- id
- householdId
- entityType/entityId
- reviewType
- materialityCents nullable
- priorityScore deterministic
- state
- reasonCodes
- createdAt/resolvedAt/deferredUntil
- rowVersion

## IncomeSource
- id
- householdId
- name
- type
- confidencePolicy
- cadence nullable
- active
- confirmed
- provenance

## IncomeEvent
- id
- householdId
- incomeSourceId
- date or date window
- amountCents or MoneyRange
- status RECEIVED/CONFIRMED_EXPECTED/EXPECTED/PREDICTED/PROSPECTIVE
- includedInConservativeForecast boolean derived by policy, not manually authoritative
- provenance

## RecurringSeries
- id
- householdId
- merchantId nullable
- seriesType EXPENSE/INCOME
- cadence
- amount model/range
- timing model/window
- state CANDIDATE/CONFIRMED/REJECTED/INACTIVE
- confidence
- effective dates
- version
- provenance

## Bill
- id
- householdId
- name
- merchantId nullable
- recurringSeriesId nullable
- amount fact/range
- observedTiming nullable
- contractualDueDate nullable and NEVER inferred as contractual
- autopayStatus UNKNOWN/NO/EXTERNAL
- active
- provenance by consequential field

## BudgetBaseline
- id
- householdId
- periodDefinition
- generatedAt
- inputFingerprint
- modelVersion
- category aggregates
- stale/superseded

## BudgetTarget
- id
- householdId
- periodDefinition
- categoryId
- targetCents
- effective dates
- authority

## ProtectedAllocation
- id
- householdId
- accountId nullable
- type CASH_RESERVE/ONE_TIME_GOAL_FUTURE
- amountCents
- protected
- active
- authority
Alpha implements CASH_RESERVE behavior only.

## BalanceObservation
- id
- householdId
- accountId
- amountCents
- balanceType USER_CONFIRMED/IMPORTED_RUNNING/IMPORTED_UNKNOWN/LEDGER/AVAILABLE
- observedAt
- sourceType/sourceRef
- authority
- confirmed

## CalendarEvent
- id
- householdId
- eventType
- sourceEntityType/id
- date or window
- amountCents or MoneyRange
- status
- confidence
- cashEffect
- generationId
- stale/superseded

## ForecastRun
- id
- householdId
- policyVersion
- inputFingerprint
- generatedAt
- horizonStart/end
- openingBalanceObservationId
- state
- stale/superseded

## ForecastEntry
- id
- forecastRunId
- sequence
- date
- priority
- sourceEntityType/id
- amountCents
- confidenceClass
- projectedBalanceCents

## TACCalculation
Add explicitly; do not hide TAC only inside ForecastRun.
- id
- householdId
- forecastRunId
- policyVersion
- inputFingerprint
- horizonEnd
- usableCashCents
- protectedCents
- requiredObligationsCents
- necessarySpendingCents
- carryForwardCents
- resultCents
- generatedAt
- stale/superseded

## FinancialSnapshot
- id
- householdId
- generation
- generatedAt
- sourceAsOf
- inputFingerprint
- forecastRunId
- tacCalculationId
- reconciliationState
- trustState
- payload/read-model references
- stale/superseded

## Recommendation
- id
- householdId
- type
- snapshotId
- state PROPOSED/ACCEPTED/REJECTED/EXPIRED/SUPERSEDED
- structuredPayload
- rationale/evidence refs
- generatedBy DETERMINISTIC/LEWIS
- createdAt/decidedAt

## ReconciliationSession
- id
- householdId
- accountId
- state
- throughDate
- authoritativeBalanceObservationId
- modeledBalanceCents
- differenceCents
- startedAt/completedAt

## ReconciliationAdjustment
- id
- householdId
- sessionId
- amountCents
- reasonCode
- note
- createdBy
- createdAt
- explicit adjustment, never RawTransaction

## AuditEvent
- id
- householdId
- actorId/context
- actionType
- entityType/id
- safe metadata JSON
- createdAt
- append-only

## DomainOutboxEvent
- id
- householdId
- eventType
- payload
- idempotencyKey
- createdAt
- state PENDING/PROCESSING/PROCESSED/FAILED
- attempts
- lastErrorCode nullable
- processedAt nullable

## BackgroundTask
- id
- householdId nullable
- taskType
- inputFingerprint
- idempotencyKey
- state
- attempts
- errorCode
- timestamps

## ModelArtifact
- id
- householdId
- artifactType
- promptId/version
- model identifier
- inputFingerprint
- snapshotId nullable
- structuredResult
- token/cost metadata
- state
- createdAt
No raw secret values.

---

# 7. Authority Model

Canonical authority classes:

1. SOURCE_EVIDENCE
2. USER_CONFIRMED
3. DETERMINISTIC_DERIVATION
4. STATISTICAL_INFERENCE
5. AI_SUGGESTION
6. UNKNOWN

Precedence is fact-specific, but:
- AI_SUGGESTION never overwrites SOURCE_EVIDENCE or USER_CONFIRMED;
- automated reruns never silently overwrite a locked USER_CONFIRMED interpretation;
- SOURCE_EVIDENCE does not automatically mean semantic interpretation is correct;
- UNKNOWN is preserved rather than replaced with zero/default fact.

---

# 8. Money and Range Contracts

## Money
Authoritative money = signed integer cents.

## MoneyRange
- lowCents
- expectedCents
- highCents
Invariant: low <= expected <= high.

## Null
Unknown amount = null/unknown, never zero.

## Arithmetic
- no JavaScript floating point for authoritative dollars;
- percentages/rates may use exact decimal/rational representation selected in domain utility;
- conversions to cents use explicit rounding policy;
- display formatting is separate from arithmetic.

---

# 9. Period and Time Contract

Use household timezone for financial day boundaries.

Domain uses injected Clock.

Period service supports:
- calendar month;
- date range;
- next-income horizon;
- pay period once schedule confirmed.

Date-only financial events use date-only semantics; timestamps are not casually converted across UTC boundaries.

DST does not change a date-only bill/payday.

---

# 10. Import Contract

## Import phases
SELECTED → PREVIEWED → MAPPED → VALIDATED → COMMITTED → PROCESSED.
Failures preserve diagnostic state.

## File handling
- original CSV may be read from `local-data/imports`;
- default policy after successful import: database preserves canonical row evidence; source file may remain local according to operator retention setting;
- never copy source file into Git/project fixtures.

## Deduplication
Use layered evidence:
1. file fingerprint catches exact file repeat;
2. row fingerprint catches exact normalized source row repeat;
3. overlap analysis identifies likely repeated export ranges;
4. ambiguous same-date/same-amount transactions are not discarded solely because values match.

## Row fingerprint
Versioned canonical fields include account context + normalized source date/amount/description + stable source fields; source row number alone is not identity.

## Invalid rows
No silent discard. Validation summary identifies invalid rows. Commit policy must be explicit:
- default Alpha: block commit when required financial fields cannot be parsed, unless user explicitly excludes identified invalid rows through a future reviewed workflow.
For initial Alpha, prefer all-valid commit.

---

# 11. Classification Contract

Precedence:
1. explicit transaction-specific user override;
2. active user-confirmed deterministic rule;
3. system deterministic rule;
4. statistical inference;
5. AI suggestion;
6. unresolved.

AI does not become a persistent rule without confirmation.

Merchant normalization and category classification are distinct.

---

# 12. Bulk Correction Contract

For any history-wide change:
1. calculate candidate set;
2. create preview with fingerprint;
3. show count/date range/aggregate dollars;
4. user confirms;
5. application revalidates preview fingerprint;
6. apply transactionally;
7. write audit + outbox;
8. recompute downstream;
9. preserve compensating/undo metadata where safe.

If candidate set changed after preview, reject and regenerate preview.

---

# 13. Recurrence Contract

Candidate recurrence is inference, not fact.

Detection considers:
- normalized merchant/relationship;
- intervals;
- amount stability/range;
- occurrence count;
- date windows.

A confirmed recurring series may still change amount/cadence later.

Observed withdrawal timing never becomes contractual due date.

False positives must be rejectable and remembered.

---

# 14. Forecast Contract

## Conservative Alpha forecast
Includes:
- confirmed/received cash facts relevant to opening state;
- confirmed/high-confidence expected income;
- confirmed recurring obligations;
- expected necessary spending assumptions adopted by policy;
- protected allocations;
- predicted items only according to explicit inclusion policy and visibly marked.

Excludes:
- prospective/opportunity income from conservative TAC.

## Event ordering
For same financial date, use explicit policy priority. Initial Alpha policy:
1. opening balance anchor exists before day events;
2. confirmed income events;
3. required/confirmed obligations;
4. expected necessary spending aggregates;
5. lower-confidence predicted events.
Within same priority, stable deterministic tie-break by event ID/source key.
This ordering is a modeling convention and must be visible in calculation policy; scenario alternatives may later be supported.

## No double counting
If a Bill/RecurringSeries produces a CalendarEvent, the same underlying expected transaction cannot separately be included again as an independent spending assumption.

---

# 15. True Available Cash Contract

Alpha formula:

```text
True Available Cash =
  Usable Current Cash
  - Protected Cash Reserve
  - Required/Included Obligations through Next Included Income
  - Expected Necessary/Normal Spending through Next Included Income
  - Adopted Negative Carry-Forward
```

Rules:
- horizon is the next sufficiently confident included income event;
- if next income is unknown, TAC is unavailable or uses an explicitly selected alternate horizon; never invent payday;
- prospective income excluded;
- protected reserve excluded;
- values already reflected in usable current cash are not subtracted twice;
- TAC may be negative;
- result carries calculation version/input fingerprint;
- explanation exposes every component.

---

# 16. Safety Contract

Alpha safety is primarily directional:
- projected household cash direction positive/negative over applicable model horizon;
- near-term cash sufficiency;
- data trust.

Do not claim one-year Fortified unless required reserve/expense model is sufficiently configured.

Unknown inputs reduce confidence rather than becoming optimistic assumptions.

---

# 17. Reconciliation Contract

Modeled balance is derived from a defined opening/anchor balance plus included posted transaction effects.

Reconciliation compares modeled state with a user-confirmed authoritative BalanceObservation through a defined date.

Difference:
`authoritative - modeled`.

Resolution may identify missing import, duplicate, interpretation issue where cash semantics matter, or explicit adjustment.

Adjustment is transparent and last-resort; it does not fabricate a bank transaction.

---

# 18. Data Trust Contract

Dimensions:
- reconciliation;
- import completeness;
- unresolved material items;
- forecast input authority;
- snapshot freshness;
- recompute/outbox health.

Summary states:
- TRUSTED_FOR_CURRENT_DECISION
- LIMITED
- NEEDS_ATTENTION
- STALE
- UNAVAILABLE

No arbitrary numeric “trust score” in Alpha.

---

# 19. Financial Snapshot Contract

One coherent snapshot feeds Home and Lewis.

Snapshot rebuild triggers include material changes to:
- transactions/interpretations;
- rules;
- recurring series;
- income;
- bill facts;
- reserve/policy;
- authoritative balance/reconciliation;
- forecast policy.

Snapshot generation is monotonic per household.

A request may use last green snapshot only if clearly fresh under policy; failed recompute marks state degraded/stale.

---

# 20. Outbox/Recompute Contract

Mutation + audit + outbox event commit in one DB transaction.

Dispatcher:
- claims pending event safely;
- invokes registered invalidation/recompute plan;
- operations idempotent;
- marks processed;
- retry bounded;
- terminal failure visible in health and degrades snapshot trust.

Duplicate delivery cannot duplicate economic events or calculations.

---

# 21. Authorization Contract

Interfaces:
- ActorContext
- AuthorizationPolicy

Initial actors:
- Steve Owner
- Kelly Owner
- Operator maintenance context

Household Owners:
- full shared visibility;
- independent Alpha decision authority.

Operator:
- can run maintenance/build/backup tasks;
- does not masquerade as household actor for audited household decisions.

Every command declares required capability.

---

# 22. Capability Contract

Alpha enabled:
- LOCAL_APP
- CSV_IMPORT
- TRANSACTION_LEDGER
- CLASSIFICATION
- REVIEW_LEARNING
- RECURRING_DETECTION
- INCOME_MODEL
- BILLS_RECURRING
- BUDGET_BASELINE
- FINANCIAL_CALENDAR
- FORECAST
- TRUE_AVAILABLE_CASH
- RECONCILIATION
- LEWIS_ADVISORY
- LOCAL_BACKUP

Disabled:
- BANK_CONNECT
- MONEY_MOVEMENT
- BILL_PAY
- AUTOPAY_CONTROL
- SUBSCRIPTION_CANCELLATION_ACTION
- SELF_BANK
- WEALTH_BUILDER
- PRODUCTION_CLOUD
- COMMERCIAL_MULTI_TENANT_UI
- FULL_DOCUMENT_INTELLIGENCE
- MATURE_VOICE

Disabled capabilities cannot surface actionable UI.

---

# 23. Route Contract

Alpha household routes:
- `/` Home
- `/transactions`
- `/budget`
- `/calendar`
- `/bills`
- `/review`
- `/reconciliation`
- `/settings`

Operator:
- `/operator/health`

Import is accessible from Transactions and may use `/transactions/import`.

No dead nav item for deferred features.

---

# 24. UX State Contract

Every financial metric/component explicitly supports:
- KNOWN_VALUE
- KNOWN_ZERO
- UNKNOWN
- LOADING
- STALE
- ERROR
- NOT_APPLICABLE

Never render unknown as $0.

Every consequential number offers “Why?”/drilldown or equivalent path.

Mobile default hierarchy prioritizes TAC and next financial pressure over dense analytics.

---

# 25. Vocabulary Contract

Use consistently:
- Bank Cash
- Protected Reserve
- True Available Cash
- Next Income
- Expected
- Predicted
- Confirmed
- Reconciled
- Unknown
- Needs Review

Avoid ambiguous standalone “Available Balance” when meaning TAC or bank balance.

Lewis uses the same terms.

---

# 26. Lewis Contract

Lewis receives:
- FinancialSnapshot;
- selected evidence summaries;
- unresolved items;
- recommendation context;
- household policy relevant to advice.

Lewis does not receive the whole raw transaction corpus by default.

Lewis output structured:
- headline;
- whatISee;
- whyItMatters;
- whatIWouldConsider;
- whatThatChanges;
- userDecisionPrompt if needed;
- evidenceRefs;
- assumptions;
- uncertainty;
- recommendationType;
- materiality;
- snapshotId.

Lewis cannot call financial mutation commands automatically.

Advice stale when snapshot superseded materially.

---

# 27. AI Cost Contract

Track:
- feature;
- prompt version;
- model;
- input/output tokens where available;
- estimated cost;
- cache hit;
- snapshot/input fingerprint.

Default:
- deterministic first;
- cached structured artifact second;
- AI only where useful.

Add configurable local monthly/feature budget. Exceeding budget degrades to deterministic/manual behavior rather than breaking Budget.

---

# 28. Security Contract

Before real data:
- loopback app/DB;
- secrets only env;
- real-data file extensions ignored;
- structured redacted logs;
- access gate;
- disk encryption/operator confirmation;
- backup restore proof.

Never store:
- bank login password;
- full online-banking credentials;
- OpenAI key in DB/Git;
- unnecessary full account numbers.

Local app is not assumed secure merely because it is local.

---

# 29. Backup Contract

Backup includes:
- database;
- schema/migration metadata;
- application version;
- backup timestamp;
- checksum.

Backup location must be protected.

Restore always tested against disposable DB before real-data authorization.

After real data, destructive migration requires recent verified backup.

---

# 30. Logging Contract

Structured allowlist:
- event name;
- safe IDs;
- status;
- duration;
- counts;
- error code;
- build/version.

Do not log by default:
- raw transaction descriptions;
- source CSV rows;
- account identifiers;
- financial document contents;
- AI secret;
- full AI prompts containing financial details.

Debug financial payload logging prohibited.

---

# 31. Health Contract

Operator health reports:
- app/build version;
- DB reachable;
- schema/migration current;
- outbox pending/failed counts;
- task failures;
- last snapshot generation/freshness;
- latest reconciliation state;
- AI configured yes/no without key;
- backup last verified;
- real-data gate state;
- validation commit.

Health failure can place application in DEGRADED/MAINTENANCE mode.

---

# 32. Testing Contract

Required classes:
- unit;
- integration with isolated test DB;
- E2E;
- Golden Household;
- calculation vectors;
- import torture corpus;
- date vectors;
- adversarial household isolation;
- failure injection;
- AI evals;
- architecture/static checks;
- leak/security checks.

Tests never use real household financial data.

---

# 33. Test Environment Safety

Test DATABASE_URL must be recognizably test-only.

Test/reset/seed scripts refuse to run if:
- real-data authorization marker belongs to target DB;
- DB name/metadata does not identify test environment;
- operator override is absent for any exceptional maintenance workflow.

After real-data gate, `db:reset` is blocked against household DB.

---

# 34. Build-State Contract

Machine state includes:
- planVersion;
- activePass;
- activeSlice;
- pass states;
- slice states;
- lastGreenCommit;
- realDataAuthorized;
- operatorGates;
- trustReadiness dimensions;
- blockers;
- latestValidationReport;
- latestHandoff;
- nextSlice.

State advances only through validation generator, not casual manual editing.

---

# 35. Pass Script Contract

All 15 scripts will use the same skeleton:

1. Identity / pass number.
2. Mission.
3. Read-first canonical files.
4. Prerequisites.
5. Allowed paths.
6. Forbidden paths.
7. Required architecture.
8. Exact build tasks.
9. Required contracts/registries.
10. Required tests.
11. Required generated artifacts.
12. Validation commands.
13. Repair authority.
14. Stop/escalation conditions.
15. Completion evidence.
16. Build-state transition.
17. Next-pass unlock rule.
18. Git checkpoint instructions.
19. Operator review if applicable.
20. Explicit no-real-data statement until Pass 12.

Cursor must not skip sections.

---

# 36. Prewritten Pass Scope Lock

## Pass 0 — Build Factory Bootstrap
Creates governance/control plane skeleton, repository safety, manifests/schemas, build state, invariant/capability registries, generators, leak guards, next-slice/handoff machinery.

## Pass 1 — Runtime & Experience Foundation
Creates Next/TS runtime, Postgres/Docker, Prisma bootstrap, design primitives, env/errors/Clock/IDs/auth abstractions, architecture lint, route shell, health shell.

## Pass 2 — Domain & Persistence Factory
Creates full Alpha schema spine, repositories/interfaces, command/query/event registries, audit, outbox, task runner, policy/authority/provenance, relationships/allocations, Golden harness foundations.

## Pass 3 — Evidence Import
CSV vertical slice + torture corpus + idempotency + immutable evidence.

## Pass 4 — Interpretation Ledger
Transactions/merchant/category/type + ledger + relationship semantics + classification.

## Pass 5 — Review & Learning
Review queue, correction scopes, rule learning, bulk preview/apply, compensating changes.

## Pass 6 — Reconstruction
Income, recurring, bills, baseline, recurring annualization, calendar candidates.

## Pass 7 — Deterministic Finance Core
Period engine, forecast, event ordering, reserve, TAC, safety, snapshot generation, lineage.

## Pass 8 — Reconciliation & Trust
Balance observations, reconciliation sessions/adjustments, Data Trust, confidence/freshness propagation.

## Pass 9 — Household Command UX
Compose Home and all core screens from read models; responsive/accessibility/visual regression.

## Pass 10 — Lewis
AI gateway, structured contracts, prompt registry, evals, caching/cost controls, stale advice.

## Pass 11 — Real-Data Safety Gate
Access, backup/restore, redaction, destructive interlocks, migration safety, readiness report.

## Pass 12 — Real Household Calibration
Operator-gated local import, correction, reconciliation, TAC/manual proof, no sensitive Git artifacts.

## Pass 13 — Evidence-Driven Hardening
Performance, accessibility, UX, false positives, error recovery, real-use friction.

## Pass 14 — Alpha Certification
Full Gates A–J, Golden regression, trust matrix, runbooks, known limitations, Alpha ALIVE certification.

No pass may pull a later pass's feature forward except foundational interfaces explicitly specified here.

---

# 37. Precomputed Sub-Slice Pattern

Every pass may be divided into A–F style slices:
- A contracts/scaffolding;
- B core implementation;
- C integration;
- D UI/read model where applicable;
- E tests/adversarial/failure;
- F validation/docs/closeout.

The exact 15 scripts will enumerate their sub-slices in advance. Dynamic next-slice generation selects only among pre-authorized sub-slices; it does not invent new scope.

---

# 38. Rollback Contract

Before each parent pass:
- record starting commit;
- DB migration state;
- build state.

If pre-real-data pass fails irreparably, code can revert to checkpoint and disposable DB rebuilt.

After real data:
- never rollback DB by destructive reset;
- forward-fix migration preferred;
- restore only through explicit operator recovery procedure.

---

# 39. Dependency Introduction Contract

Cursor may not add arbitrary dependencies.

A new dependency must:
- solve a named requirement;
- be actively maintained/stable;
- avoid duplicating existing capability;
- have acceptable license for eventual commercial product;
- be pinned;
- be recorded in dependency inventory;
- pass audit/build.

Crypto/security-sensitive dependency changes require explicit review.

---

# 40. ADR Contract

Create ADR only when implementation must deviate from this precompiled specification.

ADR includes:
- problem;
- existing canonical rule;
- reason rule cannot work;
- options;
- chosen change;
- security/financial impact;
- migration impact;
- tests;
- operator approval if consequential.

Cursor cannot self-approve a consequential ADR.

---

# 41. Questions Cursor Is NOT Allowed to Decide

Cursor may not independently decide:
- what TAC means;
- whether reserve can be spent;
- whether uncertain income counts;
- whether raw evidence can change;
- whether AI can act;
- whether a due date is inferred as contractual;
- household owner visibility;
- money sign convention;
- authority precedence;
- real-data timing;
- whether tests may use real data;
- whether destructive reset is acceptable after real data;
- whether to introduce direct banking/payments;
- whether to implement Wealth Builder/Self-Bank;
- whether to deploy publicly;
- whether to weaken an invariant;
- whether to skip a failing gate.

These are already decided.

---

# 42. Questions Cursor MAY Decide Locally

Within contracts Cursor may choose:
- private helper names;
- internal function decomposition;
- CSS implementation details consistent with tokens;
- query implementation details consistent with performance/invariants;
- test helper organization;
- safe refactors within allowed paths;
- exact stable dependency patch/minor chosen during initial lock, subject to rules.

It must not turn implementation choices into product behavior changes.

---

# 43. Completion Standard for Precompiled Planning

We are ready to write all 15 Cursor scripts when:
- technology is fixed;
- repository tree is fixed;
- layer rules fixed;
- physical domain spine fixed;
- authority/money/time/import/classification/forecast/TAC/reconciliation/trust/snapshot/outbox/auth/capability/route/UX/Lewis/security/backup/logging/health/testing contracts fixed;
- pass scopes fixed;
- pass script template fixed;
- stop/escalation rules fixed.

This document satisfies those conditions.

---

# 44. Remaining Unknowns Policy

Some household facts remain unknown by design: actual balances, exact bill terms, exact income amounts, exact recurring charges.

Those are **data**, not build-plan questions.

The application must represent unknowns honestly and learn them during Pass 12+.

External service facts that can change over time are adapter/configuration concerns, not reasons to redesign the domain.

---

# 45. Next Planning Artifact

The next and final pre-construction planning artifact is:

**`CURSOR_15_PASS_EXECUTION_PACKAGE.md` plus 15 individual pass scripts/manifests.**

Those scripts should be written now, before local construction.

Once they exist, the planning phase is frozen except for operator-approved change control.

---

# 46. Final Rule

> **Do not ask Cursor to design Budget while building Budget. Design decisions live here. Cursor executes contracts, proves gates, reports evidence, and stops when human authority is required.**
