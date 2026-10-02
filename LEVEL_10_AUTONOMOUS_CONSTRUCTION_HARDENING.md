# Budget — Level 10 Autonomous Construction Hardening

**Version:** 1.0
**Status:** Mandatory pre-construction control plane
**Authority:** Supplements and strengthens all prior Phase 0–3 documents
**Goal:** Make Budget as close as practical to a self-building, self-checking, self-documenting financial application while preserving operator control over consequential gates.

---

# 1. Executive Audit Verdict

The first forensic hardening correctly added the missing architectural connective tissue. It is necessary but not sufficient for autonomous construction.

A system can have excellent architecture and still decay during AI-assisted implementation because the builder must repeatedly infer:
- what files it may touch;
- which contracts are authoritative;
- which dependencies a feature has;
- which invariants apply;
- which tests prove completion;
- which derived artifacts must be regenerated;
- whether schema changes are safe;
- whether a pass is actually complete;
- whether documentation is stale;
- whether the next pass is allowed to begin.

This Level 10 pass therefore adds a **Construction Control Plane** above the application architecture.

The target is not blind autonomous coding. The target is:

> **Cursor can inspect canonical machine-readable state, select the next authorized slice, generate/modify only permitted layers, run deterministic gates, produce evidence, update state, and stop automatically at operator gates.**

The application and its build system should both become progressively self-describing.

---

# 2. New Master Architecture: Two Systems Built Together

Budget construction now builds two cooperating systems.

## System A — Budget Product
The household financial application:

Evidence → Interpretation → Planning → Deterministic Intelligence → Financial Snapshot → Lewis → Experience.

## System B — Budget Construction Control Plane
The machinery that governs how System A is safely built:

Canonical Contracts
→ Dependency Graph
→ Slice Manifest
→ Code/Schema Generation
→ Static Architecture Checks
→ Fixture/Test Generation
→ Validation Gates
→ Evidence Bundle
→ Build-State Update
→ Next-Slice Selection
→ Operator Gate where required.

Neither system is optional.

---

# 3. Deep Audit — Additional Gaps Found

This audit identifies additional gaps beyond the 20 from Level 1.

## 3.1 Canonical documents are human-readable but not machine-enforceable
Cursor can read Markdown and still violate it.

**Add:** machine-readable architecture manifest, invariant registry, entity registry, calculation registry, route registry, permission registry and slice manifests.

## 3.2 No single source of truth for dependency legality
Folder conventions alone cannot prevent React importing persistence internals or Lewis bypassing read models.

**Add:** import-boundary lint rules and architecture dependency tests.

## 3.3 No automated next-pass/slice selector
The plan tells humans what comes next; Cursor can still jump ahead.

**Add:** build-state-driven next-slice recommendation generator.

## 3.4 No pass manifest schema
A pass needs explicit inputs, outputs, allowed paths, forbidden paths, prerequisites, commands and completion evidence.

**Add:** versioned Slice Manifest contract.

## 3.5 No evidence bundle for completed work
A green terminal claim is not durable proof.

**Add:** generated validation report containing commit, commands, results, migrations, contract changes, coverage/invariant results and blockers.

## 3.6 No architectural drift detector
Later code can bypass boundaries established earlier.

**Add:** dependency graph validator, forbidden-import checks, direct-Prisma-use checks and UI-financial-math checks.

## 3.7 No schema drift detector
Prisma schema and entity registry can diverge.

**Add:** generated schema inventory compared with canonical entity contract.

## 3.8 No route/screen drift detector
Product spec screens may never be wired or stale routes may remain.

**Add:** route registry + route smoke tests + screen contract matrix.

## 3.9 No invariant registry
Critical rules are scattered across prose/tests.

**Add:** named invariant IDs such as MONEY-001, RAW-001, TAC-001, AI-001, SCOPE-001.

## 3.10 No invariant-to-test traceability
A test suite can be green while a requirement has no test.

**Add:** requirements/invariants coverage matrix.

## 3.11 No financial calculation specification fixtures
Golden Household is broad, but each calculation needs micro-vectors.

**Add:** Calculation Vector Library with input/output known answers.

## 3.12 No mutation contract registry
Commands are named examples, not formal contracts.

**Add:** Command Registry describing inputs, authorization, transactionality, emitted changes, audit behavior and invalidations.

## 3.13 No query/read-model registry
Shared read models need explicit schemas.

**Add:** Query Registry + Zod contracts used by UI and Lewis context builders.

## 3.14 No event registry
Domain Change Events need canonical names/payloads/consumers.

**Add:** Event Registry and event-to-invalidation map.

## 3.15 No stale-state detector
Derived artifacts can remain stale if recomputation fails.

**Add:** generation/input fingerprints and stale-state health checks.

## 3.16 No recomputation idempotency proof
Repeated change events could create duplicate derived records.

**Add:** recompute idempotency invariants/tests.

## 3.17 No causal lineage graph
Explainability needs more than provenance fields.

**Add:** lineage links from dashboard result → calculation → forecast entries → domain facts → source evidence.

## 3.18 No data-quality scoring model
“Data Trust” is conceptual but lacks deterministic dimensions.

**Add:** Data Trust Scorecard dimensions without pretending to be a universal credit-like score: reconciliation status, unresolved materiality, import coverage, inferred-vs-confirmed exposure, forecast freshness.

## 3.19 No freshness/SLA semantics
A dashboard number may be mathematically right but stale.

**Add:** generated-at, source-as-of, stale-after policy and UI freshness indicators.

## 3.20 No background-work abstraction
Alpha can run synchronously, but long import/AI/recompute work will later need jobs.

**Add:** Task Runner interface now, in-process implementation Alpha, queue adapter later.

## 3.21 No retry/dead-letter model
Failed derived recomputation/AI tasks need explicit recovery.

**Add:** task attempt status, retry policy, terminal failure visibility; no silent loops.

## 3.22 No failure-injection testing
Happy-path tests do not prove financial integrity under crashes.

**Add:** fault tests around import commit, retroactive correction, recompute, reconciliation and backup.

## 3.23 No partial-write crash doctrine
What happens if process dies between domain mutation and recompute?

**Add:** transactional outbox or equivalent durable pending-change mechanism.

## 3.24 In-process events alone can lose post-commit recomputation
This is a significant architecture risk.

**Add:** local transactional outbox table. Domain mutation + outbox record commit atomically. Worker/in-process dispatcher processes pending changes idempotently. This survives crashes and maps cleanly to future queues.

## 3.25 No outbox observability
Pending/failed changes could be invisible.

**Add:** system health panel and validation for unprocessed/failed outbox items.

## 3.26 No snapshot consistency contract
Dashboard cards could derive from different model generations.

**Add:** Financial Snapshot generation ID; related cards/Lewis context consume a coherent snapshot version.

## 3.27 No deterministic random/test seed policy
Generated synthetic data can become irreproducible.

**Add:** seeded fixture factory.

## 3.28 No synthetic data generator
Golden Household alone will not expose scale/variation.

**Add:** deterministic household factory capable of 1, 3, 5, 10 years and configurable transaction volume.

## 3.29 No mutation/fuzz/property-testing plan
Financial parsers and calculations benefit from generated edge cases.

**Add:** property-based tests for money, dates, duplicate detection, ordering and invariants.

## 3.30 No locale/file-encoding hardening
CSV files can contain BOM, quoted commas, CRLF, odd encodings, negative parentheses, currency symbols.

**Add:** import corpus covering formatting variants and explicit supported/unsupported encoding behavior.

## 3.31 No timezone/DST edge suite
Payday/calendar behavior can break around DST/month-end.

**Add:** date-vector suite including leap year, DST, month-end, weekends and year boundary.

## 3.32 No bank-balance semantics contract
CSV “balance” columns may be running, available, ledger, reversed ordering or absent.

**Add:** Balance Observation model; never assume a CSV balance column is current authoritative cash without adapter/user confirmation.

## 3.33 No pending-vs-posted model
CSV may not include pending transactions, future connectors will.

**Add:** transaction posting status contract from beginning.

## 3.34 No transaction split model in physical architecture
Discovery listed splitting, but Alpha schema plan does not explicitly protect it.

**Add:** TransactionAllocation/Split architecture so categories/attribution can later divide one bank transaction without rewriting ledger identity.

## 3.35 No transfer-pair model
Transfer neutrality requires pairing/linkage.

**Add:** TransferLink entity/relationship with confidence/confirmation.

## 3.36 No refund/reimbursement linkage model
Economic meaning improves when refund/reimbursement links to original expense.

**Add:** TransactionRelationship generic relation or explicit RecoveryLink.

## 3.37 No recurring-series version/history
If cadence changes, current row alone loses historical interpretation.

**Add:** series observations/revisions or effective-dated recurrence model.

## 3.38 No user override precedence persistence
A future rerun of inference could overwrite a confirmed human choice.

**Add:** field-level authority/source or interpretation lock semantics.

## 3.39 No field-level provenance strategy
Object-level provenance is insufficient when amount is observed, due date user-confirmed and cadence inferred.

**Add:** Fact/FactAssertion pattern or field-level provenance metadata for consequential mixed-source objects.

## 3.40 No uncertainty propagation algorithm
Confidence labels exist, but how do uncertain inputs affect TAC/forecast confidence?

**Add:** deterministic confidence policy; never have LLM invent confidence.

## 3.41 No range arithmetic doctrine
Variable bills/income need low/expected/high or range treatment.

**Add:** MoneyRange contract and forecast scenario policy.

## 3.42 No negative/zero/credit edge doctrine
Refunds, charge reversals, zero-dollar authorizations, fees and interest require explicit transaction semantics.

**Add:** transaction economic-type registry.

## 3.43 No cash-wallet architecture in Alpha foundations
Voice/cash is deferred, but bank cash withdrawals need correct current interpretation.

**Add:** CashWithdrawal/CashAllocation-compatible model seam now, without building full voice UI.

## 3.44 No opening-balance concept
Forecast/reconciliation needs a defined anchor.

**Add:** BalanceObservation/Reconciliation anchor semantics.

## 3.45 No fiscal/period engine
Monthly/pay-period views need consistent period boundaries.

**Add:** Period service for calendar month, pay period and custom horizon.

## 3.46 No policy for deleted bank imports
Removing an erroneous import must safely unwind interpretations/derived state.

**Add:** import supersede/void workflow, not raw-row deletion.

## 3.47 No bulk-operation safety contract
Retroactive rules can alter hundreds of transactions.

**Add:** preview token/hash, confirmation, transactional apply, before/after summary, undo strategy.

## 3.48 No reversible-change strategy
Audit alone does not provide undo.

**Add:** compensating commands for eligible interpretation changes; immutable evidence remains unchanged.

## 3.49 No privacy classification taxonomy
Not all fields have equal sensitivity.

**Add:** data classes: public/system, household financial, highly sensitive credential/identifier, derived analytics, operational logs.

## 3.50 No log schema/redaction enforcement
“Minimize logs” is not enough.

**Add:** structured logger with allowlisted fields; raw financial payload logging prohibited by lint/review.

## 3.51 No secure export contract
Long-term data ownership requires export.

**Add:** deferred UI but architecture requires household-scoped export service seam and canonical portable format planning.

## 3.52 No deletion/legal-retention architecture
Commercial product will need deletion while audit/financial integrity has dependencies.

**Add:** lifecycle policy design deferred for production but entity ownership graph must support household-scoped purge/export.

## 3.53 No dependency/software supply-chain policy
Financial app should not casually accumulate packages.

**Add:** dependency approval doctrine, lockfile, audit, minimal packages, no abandoned parser/crypto libraries.

## 3.54 No generated SBOM/dependency inventory
Commercial hardening benefits from it.

**Add:** build artifact inventory later; basic dependency report now.

## 3.55 No database constraint audit
Application validation alone is insufficient.

**Add:** constraint registry: uniqueness, FK, checks where Prisma/Postgres supports them, migration SQL review.

## 3.56 No database index registry
Indexes can drift.

**Add:** query-to-index matrix and explain checks on high-volume synthetic fixture.

## 3.57 No query budget / N+1 automated check
Performance regressions can hide.

**Add:** instrumentation in integration tests for key read models.

## 3.58 No accessibility acceptance matrix
“Accessible” is too broad.

**Add:** keyboard, focus, labels, contrast, reduced motion, semantic headings, table/list alternatives, screen-reader status announcements.

## 3.59 No responsive breakpoint contract
Mobile-first hierarchy needs explicit behavior.

**Add:** viewport test matrix in Playwright.

## 3.60 No visual regression strategy
UI can degrade unnoticed.

**Add:** stable synthetic-state screenshots in E2E after core UX exists; no sensitive real-data screenshots.

## 3.61 No UX copy registry/doctrine
Financial language can become inconsistent (“available,” “safe,” “balance,” “reserve”).

**Add:** canonical financial vocabulary and prohibited ambiguous labels.

## 3.62 No empty/unknown/error state matrix
Financial trust is damaged by zeros that actually mean unknown.

**Add:** every metric supports value/zero/unknown/unavailable/stale/error states distinctly.

## 3.63 No “zero is data” invariant
Missing value must never default to $0.

**Add:** NULL/unknown semantics throughout.

## 3.64 No rounding/display contract
Cents calculations may be shown as rounded dollars inconsistently.

**Add:** display formatter registry; calculations retain cents.

## 3.65 No AI evaluation harness
Prompt tests alone cannot measure Lewis quality.

**Add:** fixed synthetic cases with expected grounding, prohibited claims, tone and action-boundary checks.

## 3.66 No AI prompt/version registry
Model artifacts mention version but no central registry.

**Add:** prompt registry with IDs, schemas, model class, data minimization policy and eval suite.

## 3.67 No AI fallback hierarchy
If one model/task fails, behavior should be defined.

**Add:** deterministic fallback → retry where safe → alternate configured model only if policy permits → unresolved/manual.

## 3.68 No prompt-injection/data-content doctrine
Bank descriptions/documents can contain arbitrary text.

**Add:** imported financial text is untrusted data, never instruction. Lewis system contract explicitly isolates it.

## 3.69 No AI write-authority enforcement
A prompt could return an action-looking payload.

**Add:** AI outputs are proposals/artifacts only; commands require explicit application-layer authorization.

## 3.70 No cost ceiling/circuit breaker
Telemetry exists but not enforcement.

**Add:** per-feature/session/month local AI budget caps and graceful degradation.

## 3.71 No AI cache invalidation doctrine
Cached advice can become stale after finances change.

**Add:** cache keyed by financial snapshot/input fingerprint and prompt version.

## 3.72 No model-change regression gate
Changing OpenAI model can alter behavior.

**Add:** Lewis eval suite must pass before default model/prompt change.

## 3.73 No “advice age” indicator
Old advice may no longer be valid.

**Add:** recommendation generated-against snapshot ID and superseded/stale state.

## 3.74 No household policy engine
Reserve/approval/confidence rules are scattered.

**Add:** HouseholdPolicy aggregate with versioned settings consumed by engines.

## 3.75 No feature flag/capability registry
Deferred features may leak into UI accidentally.

**Add:** capability registry; Alpha capabilities explicit and later capabilities disabled.

## 3.76 No commercial tenancy enforcement tests
household_id exists, but accidental cross-household query remains possible.

**Add:** two-household adversarial fixture and repository authorization tests from early passes.

## 3.77 No authorization decision point abstraction
Local gate now, commercial auth later.

**Add:** ActorContext + AuthorizationPolicy interface from Pass 1.

## 3.78 No operator/admin distinction
Developer operations and household actions should not be conflated.

**Add:** OperatorContext for maintenance tasks, Household ActorContext for financial actions.

## 3.79 No maintenance-mode/safe-mode
If migrations/recompute fail, app may display stale confidence.

**Add:** system health state can disable consequential recommendations and show maintenance/degraded mode.

## 3.80 No health/readiness model
DB “connected” is insufficient.

**Add:** health dimensions: DB, migrations, outbox, recompute freshness, AI optional, backup freshness, real-data gate, snapshot freshness.

## 3.81 No startup invariant checks
App should refuse unsafe startup states.

**Add:** startup validation for environment, schema version, forbidden real-data state, policy registry and migration compatibility.

## 3.82 No fixture/environment isolation
Tests could accidentally point at real DB.

**Add:** environment guard rejects test commands against non-test database and real-data DB against fixture reset commands.

## 3.83 No destructive command interlocks
db reset/seed can be catastrophic after Pass 12.

**Add:** real-data lock disables destructive scripts unless explicit multi-step operator override.

## 3.84 No database clone/sandbox workflow
Post-real-data debugging should not mutate live household state casually.

**Add:** sanitized/synthetic reproduction preferred; protected local clone workflow when truly needed.

## 3.85 No release/checkpoint tagging doctrine
Need known-good recovery points.

**Add:** pass-completion Git tags/checkpoints and migration compatibility record.

## 3.86 No changelog/decision record automation
Architecture decisions can disappear in commits.

**Add:** ADR registry for consequential deviations plus generated build progress.

## 3.87 No “docs match code” gate
Canonical docs can become fiction.

**Add:** generated inventories and docs freshness checks.

## 3.88 No code ownership map for AI
Cursor needs to know which directories own which concepts.

**Add:** machine-readable ownership/layer map.

## 3.89 No allowed-change budget per slice
AI can rewrite too much.

**Add:** slice manifest allowed_paths/forbidden_paths and expected migrations/contracts.

## 3.90 No rollback plan per pass
If a pass fails after broad changes, recovery is unclear.

**Add:** pre-pass checkpoint, migration rollback/data strategy, code revert instructions.

## 3.91 No autonomous repair policy
When a gate fails, Cursor needs boundaries for repair.

**Add:** may repair within active slice/owned layers; must stop if repair requires product/architecture change or forbidden path.

## 3.92 No escalation taxonomy
Cursor needs to know when to ask Steve/ChatGPT.

**Add:** BLOCKED_PRODUCT_DECISION, BLOCKED_ARCHITECTURE, BLOCKED_SECURITY, BLOCKED_REAL_DATA, BLOCKED_EXTERNAL_SERVICE, BLOCKED_UNKNOWN.

## 3.93 No completion confidence score
Pass status should not be binary from self-report.

**Add:** gate-derived completion status: NOT_STARTED, BUILDING, VALIDATING, BLOCKED, GREEN, OPERATOR_GATE, COMPLETE.

## 3.94 No automated master handoff packet
New AI/Cursor threads need exact current state.

**Add:** generated `CURRENT_BUILD_HANDOFF.md/json` from build state, contracts, latest report and next slice.

## 3.95 No deterministic setup bootstrap
“Fresh clone works” needs automation.

**Add:** bootstrap/preflight script that checks Node/Docker/env/DB and gives actionable results without secrets.

## 3.96 No environment contract schema
.env.example is human documentation only.

**Add:** typed environment parser and generated environment reference.

## 3.97 No package-script contract
Scripts can drift across docs.

**Add:** script registry validated against package.json.

## 3.98 No artifact generation strategy
Registries should generate code/docs where possible.

**Add:** generated enums/types/validation docs from canonical registries where safe, avoiding duplicate hand-maintained definitions.

## 3.99 No build reproducibility record
Need versions of Node/package manager/Postgres.

**Add:** pin runtime/tool versions and record them in validation evidence.

## 3.100 No explicit “self-build stop conditions”
Autonomy without stops is dangerous.

**Add:** Cursor must stop on failed invariant, unauthorized migration, security ambiguity, real-data boundary, unknown destructive effect, or canonical-contract conflict.

---

# 4. Construction Control Plane Files

Pass 0–2 should establish the following canonical machine-readable artifacts.

Recommended structure:

```text
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
  slices/
    P00.json
    P01.json
    ...
  build_state.json
  latest_validation_report.json
  latest_slice_report.json
  next_slice.json
  current_handoff.json

tests/
  golden-household/
  calculation-vectors/
  import-corpus/
  date-vectors/
  adversarial/
  ai-evals/
```

Markdown reports may be generated from these artifacts for humans.

---

# 5. Slice Manifest Contract

Every pass or sub-slice gets a manifest with:

- slice_id;
- title;
- parent_pass;
- objective;
- prerequisites;
- canonical_inputs;
- allowed_paths;
- forbidden_paths;
- required_contracts;
- contracts_allowed_to_change;
- expected_entities;
- expected_commands;
- expected_queries;
- expected_events;
- expected_migrations;
- required_invariants;
- required_tests;
- validation_commands;
- operator_review;
- real_data_allowed;
- external_network_allowed;
- AI_allowed;
- destructive_operations_allowed;
- completion_evidence;
- rollback_checkpoint;
- next_candidate_slices.

Cursor must read the manifest before coding.

---

# 6. Invariant Registry

Examples:

### DATA / EVIDENCE
- RAW-001: RawTransaction source payload is immutable after committed import.
- RAW-002: Source evidence can be superseded/voided only through explicit import lifecycle, never rewritten.
- UNKNOWN-001: Unknown financial values are not coerced to zero.

### MONEY
- MONEY-001: Authoritative USD calculations use integer cents.
- MONEY-002: Floating-point values never enter authoritative money math.
- MONEY-003: Display rounding never changes stored/calculated cents.

### HOUSEHOLD
- SCOPE-001: Every household-owned query is household scoped.
- SCOPE-002: Cross-household access fails closed.

### IMPORT
- IMPORT-001: Exact re-import is idempotent.
- IMPORT-002: Ambiguous overlap is surfaced, not silently discarded.
- IMPORT-003: Partial failure cannot create a falsely successful ImportBatch.

### INTERPRETATION
- AUTH-001: Confirmed human interpretation outranks automated inference.
- RULE-001: Retroactive bulk rule application requires preview matching the applied set.

### FORECAST
- FCST-001: Forecast ordering is deterministic.
- FCST-002: Prospective excluded income does not enter conservative TAC.
- FCST-003: One economic event cannot reduce projected cash twice.

### TAC
- TAC-001: Protected reserve is excluded from ordinary available cash.
- TAC-002: TAC result is reproducible from its calculation artifact.
- TAC-003: Negative carry-forward is explicit.

### RECONCILIATION
- REC-001: Reconciliation adjustment is never represented as a bank RawTransaction.
- REC-002: Material unresolved difference degrades trust/forecast confidence.

### AI
- AI-001: Lewis cannot modify authoritative financial state directly.
- AI-002: Lewis cannot perform authoritative financial arithmetic.
- AI-003: Imported text is untrusted data, not instructions.
- AI-004: Lewis advice is tied to a Financial Snapshot generation.

### SECURITY
- SEC-001: Real financial files/secrets are never committed.
- SEC-002: Destructive DB reset is disabled after real-data authorization.
- SEC-003: Test environment cannot target authorized real-data database.

Every invariant must map to one or more automated tests or an explicit operator gate.

---

# 7. Transactional Outbox — Critical Upgrade

The previous in-process event design is insufficient for crash safety.

Add `DomainOutboxEvent`.

When a command changes authoritative state:

1. begin DB transaction;
2. write domain mutation;
3. write AuditEvent if required;
4. write DomainOutboxEvent describing downstream invalidation;
5. commit all atomically;
6. dispatcher processes outbox;
7. recompute idempotently;
8. mark outbox event processed or failed;
9. failures remain visible/retryable.

This prevents the state where a transaction correction commits but the forecast/TAC/dashboard remains silently stale because the process crashed before recomputation.

Alpha may dispatch immediately in-process, but durability is preserved.

---

# 8. Financial Snapshot Generation Contract

Create a coherent `FinancialSnapshot` generation.

It contains or references:
- snapshot_id;
- household_id;
- generated_at;
- source_as_of;
- authoritative balance anchor;
- reconciliation/data-trust state;
- current income horizon;
- protected allocations;
- spending/baseline summary;
- forecast generation;
- TAC calculation;
- safety/trajectory;
- upcoming events;
- unresolved material issues;
- warnings;
- input fingerprint;
- calculation policy versions.

Dashboard and Lewis use the same snapshot ID.

A stale snapshot is explicitly stale; it is never silently presented as current.

---

# 9. Data Lineage Graph

Every important number should be traceable:

```text
Dashboard TAC $X
→ TACCalculation #123
→ ForecastGeneration #88
→ CalendarEvent IDs
→ RecurringSeries / IncomeEvent / ProtectedAllocation
→ Transaction interpretations
→ RawTransaction evidence / user confirmation
```

Lineage does not require a graph database. Stable references and query services are sufficient.

The product should eventually be able to answer “Why is this number $X?” mechanically before Lewis writes prose.

---

# 10. Data Trust Model

Do not reduce trust to a mysterious single score.

Maintain dimensions:
- reconciliation state;
- import coverage;
- unresolved high-materiality items;
- proportion of important forecast dollars confirmed vs inferred;
- forecast freshness;
- pending failed recomputations;
- snapshot freshness.

UI may summarize as HIGH / LIMITED / NEEDS_ATTENTION or similar, but drilldown shows dimensions.

Lewis cannot claim high certainty when the deterministic trust model says otherwise.

---

# 11. Fact Authority & Field-Level Provenance

Objects such as Bill contain facts from mixed sources.

For consequential facts support:
- value;
- authority/source type;
- source reference;
- confidence where inferred;
- confirmed_by;
- confirmed_at;
- effective_from/to where applicable.

Authority precedence should distinguish:
1. immutable external evidence;
2. explicit household confirmation;
3. deterministic derivation;
4. statistical inference;
5. AI suggestion.

Exact precedence can be fact-type specific. AI suggestion never silently outranks confirmed human/source facts.

---

# 12. Transaction Relationship Model

Protect future correctness now.

Support relationships such as:
- TRANSFER_PAIR;
- REFUND_OF;
- REIMBURSES;
- REVERSAL_OF;
- CASH_ALLOCATION;
- SPLIT_PARENT/ALLOCATION.

One bank transaction remains one bank transaction. Economic interpretation can be distributed through allocations/relationships without duplicating source cash movement.

---

# 13. Balance Observation Model

Do not treat a CSV balance column as universally authoritative.

Create BalanceObservation semantics:
- account;
- amount;
- balance type (ledger/available/running/user-confirmed/imported-unknown);
- as-of date/time;
- source;
- confidence/confirmation.

Reconciliation chooses/records the authoritative anchor.

---

# 14. Household Policy Aggregate

Centralize household rules:
- reserve percentage;
- reserve protection state;
- confidence inclusion policy;
- next-income horizon policy;
- approval policy;
- materiality thresholds;
- notification policy;
- future Self-Bank policy references.

Policy changes are versioned/audited and trigger recalculation.

---

# 15. Task Runner / Durable Work

Create a task abstraction for:
- post-import processing;
- recurrence analysis;
- snapshot rebuild;
- Lewis generation;
- later document extraction.

Alpha implementation may run tasks immediately, but tasks have:
- id;
- type;
- household;
- input fingerprint;
- state;
- attempts;
- error category;
- created/started/completed;
- idempotency key.

This prepares clean migration to a real queue later.

---

# 16. Golden Household 2.0

Upgrade the Golden Household into a formal specification, not merely a fixture.

## Scenario families
- Normal month.
- Tight cash month.
- Bonus month.
- Car payoff month.
- Utility spike.
- Subscription price increase.
- Duplicate import.
- Overlap import.
- Missing transaction/reconciliation.
- Merchant correction.
- Refund.
- Reimbursement.
- Transfer pair.
- Cash withdrawal.
- Spend-forward.
- Reserve-protected scenario.
- Unknown next income.
- Prospective income excluded.
- Negative TAC.
- Same-day payday/bill.
- Month/year boundary.
- Leap year.
- DST boundary.
- Stale snapshot.
- Failed recomputation.

Each scenario has known-answer assertions.

---

# 17. Calculation Vector Library

For every financial engine maintain tiny human-verifiable vectors.

Example TAC vector:
- bank cash = $1,000;
- protected reserve = $100;
- required bills = $300;
- necessary spending = $200;
- carry-forward = $50;
- expected TAC = $350.

Vectors cover negatives, zero, unknown, ranges and ordering.

A calculation engine cannot be declared green merely because Golden Household passes.

---

# 18. Import Torture Corpus

Synthetic CSV corpus should include:
- UTF-8 BOM;
- CRLF/LF;
- quoted commas;
- quoted multiline description if parser supports;
- blank rows;
- duplicate headers;
- extra columns;
- date formats;
- currency symbols;
- parentheses negatives;
- explicit debit/credit;
- signed amounts;
- reversed row order;
- same-day identical-dollar distinct transactions;
- zero amount;
- refund/credit;
- running balance;
- missing balance;
- malformed amount/date;
- whitespace/noise;
- very large but valid household amount;
- overlapping files.

Unsupported formats fail explicitly.

---

# 19. Adversarial Household Isolation Harness

From Pass 2 onward, tests create Household A and Household B with similar IDs/data.

Attempt:
- cross-household transaction query;
- correction;
- rule application;
- reconciliation;
- snapshot read;
- Lewis context;
- audit read.

Every attempt must fail closed or return only authorized household data.

This becomes essential for commercialization even while Alpha has one real household.

---

# 20. Architecture Enforcement

Automated checks should prohibit:
- UI importing Prisma client;
- domain importing React/Next;
- Lewis importing persistence repositories directly where snapshot/query contract should be used;
- authoritative financial math inside components;
- OpenAI calls outside Lewis gateway;
- raw SQL outside approved persistence/migration areas;
- direct mutation of raw evidence;
- direct use of `Date.now()` in domain calculations;
- direct `Math.random()` in deterministic domain/test fixture generation;
- console logging of financial payloads.

Use lint rules, dependency-cruiser/madge-style checks, custom scripts or AST/static scans chosen in construction.

---

# 21. Self-Generated Documentation

Where possible generate human docs from registries:
- entity inventory;
- command/query/event inventory;
- route map;
- invariant coverage;
- calculation versions;
- state machines;
- environment variables;
- package scripts;
- active capabilities;
- build progress.

Human prose explains why; generated docs prove what exists.

---

# 22. Self-Generated Next Slice

A generator reads:
- build_state;
- completed prerequisites;
- latest validation;
- unresolved blockers;
- operator gates.

It emits `build/next_slice.json` and human `docs/NEXT_BUILD_SLICE.md`.

Cursor may only execute a slice marked READY.

If multiple are possible, Phase 3 ordering remains default unless explicitly changed by operator-approved plan update.

---

# 23. Autonomous Repair Boundary

When validation fails, Cursor may automatically repair only if:
- change remains inside active slice allowed paths;
- no canonical product behavior changes;
- no new dependency/provider is introduced without approval;
- no destructive migration;
- no real data;
- no security boundary weakened;
- no invariant is removed/relaxed to make tests pass.

Cursor must stop/escalate when a fix requires violating those conditions.

Never “fix” a failing invariant by weakening the invariant.

---

# 24. Operator Gates

Human approval remains mandatory for:
- real-data authorization;
- destructive migration after real data;
- external network exposure;
- new financial provider integration;
- money movement/action layer;
- changing core calculation policy after real-data calibration;
- weakening security/invariants;
- changing Lewis authority;
- production deployment;
- commercial authentication/privacy model.

Self-building does not mean self-authorizing.

---

# 25. Build Evidence Bundle

Every completed slice generates a machine-readable report:

- slice ID;
- starting commit;
- ending commit;
- changed paths;
- migrations;
- contracts changed;
- invariants touched;
- tests added;
- validation commands/results;
- Golden Household result;
- architecture check result;
- security/leak result;
- real-data flag;
- known limitations;
- blockers;
- rollback checkpoint;
- recommended next slice.

Human report is generated from this.

---

# 26. Trust Readiness Matrix

Track at least:
- Evidence Integrity
- Import Idempotency
- Household Isolation
- Interpretation Authority
- Calculation Correctness
- Forecast Reproducibility
- TAC Correctness
- Reconciliation
- Lineage/Explainability
- Derived-State Freshness
- AI Grounding
- AI Cost Control
- Local Security
- Backup/Restore
- Migration Safety
- Accessibility
- UX Comprehension
- Performance
- Build Reproducibility

Each dimension has evidence-based states, not arbitrary percentages:
NOT_BUILT / PARTIAL / PROVEN_SYNTHETIC / PROVEN_REAL / HARDENED.

Construction percentage can remain for progress visualization but cannot substitute for this matrix.

---

# 27. Self-Build Bootstrap Sequence

Before feature Pass 3 begins, Passes 0–2 must establish enough control plane that later passes become increasingly mechanical.

## Pass 0 hardened output
- repository safety;
- machine build_state;
- slice schema;
- invariant registry skeleton;
- capability registry;
- ownership/layer map;
- real-data lock;
- validation report schema;
- handoff generator skeleton;
- next-slice generator skeleton.

## Pass 1 hardened output
- runtime/toolchain pinned;
- app shell/design primitives;
- typed env;
- typed errors;
- Clock;
- IDs/fingerprints;
- ActorContext/AuthorizationPolicy;
- architecture import boundaries;
- route registry;
- script registry;
- health framework.

## Pass 2 hardened output
- domain schema;
- command/query/event registries;
- outbox;
- audit;
- task runner;
- state machines;
- HouseholdPolicy;
- BalanceObservation;
- transaction relationships/allocations;
- calculation registry;
- provenance/authority model;
- Golden Household generator;
- calculation-vector harness;
- adversarial household fixture;
- architecture validators;
- invariant coverage report.

After Pass 2, Cursor should be able to build later vertical slices by filling known contracts rather than inventing infrastructure.

---

# 28. Revised Pass Dependency Principle

Do not increase the 15-pass count merely to appear thorough.

Instead, allow **sub-slices** within a pass where the control plane determines ordering.

Example Pass 7:
- P07A calculation contracts/vectors;
- P07B forecast engine;
- P07C TAC engine;
- P07D snapshot integration;
- P07E calendar UI;
- P07F Golden Household + fault validation.

The parent pass is complete only when every required sub-slice is green.

This provides autonomous granularity without turning the human roadmap into dozens of disconnected phases.

---

# 29. Definition of Done 2.0

A feature/slice is complete only if all applicable conditions are true:

1. Product requirement traced.
2. Invariant IDs identified.
3. Entity/data contract registered.
4. Command/query contracts registered.
5. Authorization defined.
6. Household scoping proven.
7. State transitions defined.
8. DB constraints/indexes reviewed.
9. Transaction boundary defined.
10. Provenance/authority preserved.
11. Audit behavior defined.
12. Outbox event emitted if downstream state changes.
13. Invalidation dependencies registered.
14. Recompute idempotent.
15. Derived artifacts fingerprinted/versioned.
16. Shared read model updated.
17. Lineage available.
18. Unknown/zero/stale/error states handled.
19. UI uses design primitives.
20. Accessibility checks pass.
21. Lewis consumes shared truth only.
22. AI output cannot directly mutate authoritative state.
23. Unit tests pass.
24. Integration tests pass.
25. Property/vector tests pass where applicable.
26. Golden Household regression passes.
27. Adversarial isolation passes where applicable.
28. Failure-injection test passes where applicable.
29. Architecture drift checks pass.
30. Security/leak checks pass.
31. Documentation/inventories regenerated.
32. Evidence bundle generated.
33. Build state updated.
34. Rollback checkpoint exists.
35. Next slice generated.
36. Operator gate honored.

---

# 30. Failure Injection Program

At appropriate passes deliberately simulate:
- process failure after DB mutation but before recompute;
- outbox handler failure;
- duplicate event delivery;
- AI timeout;
- malformed AI JSON;
- DB connection interruption during import;
- malformed CSV mid-file;
- retroactive correction failure;
- stale browser update conflict;
- backup interruption;
- restore into wrong schema version;
- stale Financial Snapshot;
- missing next income;
- unresolved reconciliation difference.

Expected behavior must preserve financial integrity and expose recoverable state.

---

# 31. Commercial Escape-Hatch Audit

Every Alpha implementation should answer:
- Can local adapter be replaced without domain rewrite?
- Is household scope already explicit?
- Is ActorContext replaceable by commercial auth?
- Can in-process Task Runner become queue worker?
- Can local files become object storage?
- Can CSV source coexist with bank connector source?
- Can local Postgres move to managed Postgres?
- Can Lewis gateway change model/provider?
- Can one household be exported/deleted independently?
- Can action layer be added without granting Lewis direct authority?

If any answer becomes “no,” architecture drift gate fails.

---

# 32. Product Vocabulary Contract

Create canonical terms and prevent misleading synonyms.

Examples:
- **Bank Cash** — observed/confirmed account cash, not spendable.
- **Protected Reserve** — cash excluded from ordinary spending.
- **True Available Cash** — modeled safe-to-spend through defined horizon.
- **Expected** — forecasted based on evidence, not guaranteed.
- **Confirmed** — household/source confirmed.
- **Predicted** — modeled inference.
- **Reconciled** — matched to authoritative balance through defined point.
- **Unknown** — not zero.

Lewis and UI use the same vocabulary.

---

# 33. System Health Surface

Even Alpha should have an operator/developer health surface separate from household UX showing:
- DB/migration state;
- outbox pending/failed;
- task failures;
- snapshot freshness;
- last successful recompute;
- backup status;
- AI configured/optional;
- real-data authorization;
- latest validation commit;
- build version.

Never expose secrets/raw financial payloads there.

---

# 34. Real-Data Lock 2.0

After authorization, protection becomes stricter, not looser.

Before authorization:
- real data rejected.

After authorization:
- destructive reset/seed commands disabled against the real DB;
- synthetic test tooling cannot target it;
- migrations require backup/check;
- real source files remain gitignored;
- validation reports remain non-sensitive;
- screenshot/fixture generation from real data prohibited;
- debugging defaults to IDs/aggregates, not raw descriptions.

---

# 35. “Virtually Builds Itself” Target State

By the end of Pass 2, a new Cursor thread should be able to:

1. read generated handoff;
2. read next_slice;
3. verify prerequisites;
4. inspect allowed/forbidden paths;
5. build the slice;
6. generate/update required registries;
7. run validators;
8. repair permitted failures;
9. run Golden Household;
10. generate evidence report;
11. update build_state only if green;
12. generate the next slice;
13. stop at operator gate automatically.

That is the practical self-building standard for Budget.

---

# 36. New Pre-Construction Gate

Construction is not authorized merely because Phase 3 exists.

Pass 0 mission must now treat these as canonical:
- MASTER_PRODUCT_PLAN.md
- PHASE_1_PRODUCT_SPECIFICATION.md
- PHASE_2_TECHNICAL_BLUEPRINT.md
- PHASE_3_CURSOR_BUILD_PLAN.md
- PRE_CONSTRUCTION_FORENSIC_AUDIT_AND_LAYERED_BUILD_HARDENING.md
- LEVEL_10_AUTONOMOUS_CONSTRUCTION_HARDENING.md

The Level 10 document governs construction-control-plane requirements when earlier documents are less specific.

No real-data rule remains absolute.

---

# 37. Final Conclusion

The previous hardening made Budget architecturally coherent.

This hardening makes the **construction process itself coherent, inspectable, recoverable and increasingly autonomous**.

The new model is:

> **Contracts define the system. Registries make the contracts machine-readable. Slices make work bounded. Validators enforce architecture. Golden data proves behavior. Evidence bundles prove completion. Build state selects what comes next. Operator gates retain human authority.**

### Level 10 north star

**Budget should not merely be built correctly. It should continuously prove that it is still being built correctly.**
