# Budget — Phase 2 Technical Blueprint

**Version:** 1.0  
**Status:** Technical architecture baseline  
**Product contract:** `PHASE_1_PRODUCT_SPECIFICATION.md`  
**Initial target:** Local Steve + Kelly Household Alpha  
**Commercial posture:** Alpha-simple, migration-aware, no disposable architecture

---

## 1. Architecture Decision Summary

Budget Alpha will be a **local-first TypeScript web application** with:

- **Next.js + React + TypeScript** for the application/UI/server boundary;
- **PostgreSQL** as the authoritative application database from day one;
- **Prisma ORM + migrations** for typed persistence and schema evolution;
- **local PostgreSQL via Docker Compose** for repeatable development;
- **Zod** at all untrusted/AI/import boundaries;
- **decimal-safe integer money representation** in the application/domain layer;
- **Papa Parse or equivalent mature CSV parser** behind a Budget-owned import adapter;
- **OpenAI API only through a server-side Lewis gateway** with structured outputs and no browser-exposed key;
- **Vitest** for domain/unit tests;
- **Playwright** for critical end-to-end workflows;
- a **versioned deterministic financial engine** separate from Lewis/AI;
- append-oriented **audit events** for material mutations;
- encrypted-at-rest strategy built around the local machine/volume plus application-level encryption for especially sensitive stored artifacts where warranted;
- local-only operation for Alpha, with no Netlify or production cloud dependency.

The architecture deliberately uses technologies that can survive the move to a commercial multi-household service without requiring the financial domain to be rewritten.

---

## 2. Repository / Application Shape

Recommended initial repository shape:

```text
budget/
  docs/
    architecture/
    runbooks/
  prisma/
    schema.prisma
    migrations/
    seed.ts
  src/
    app/
      (app)/
        page.tsx
        transactions/
        budget/
        calendar/
        bills/
        review/
        settings/
        reconciliation/
      api/
        imports/
        lewis/
        reconciliation/
    components/
      financial/
      lewis/
      navigation/
      review/
    domain/
      money/
      transactions/
      classification/
      recurring/
      budget/
      forecast/
      true-available-cash/
      reconciliation/
      audit/
    server/
      db/
      imports/
      lewis/
      services/
      security/
    schemas/
    lib/
  tests/
    fixtures/
    unit/
    integration/
    e2e/
  local-data/              # gitignored; sensitive runtime/import staging only
  docker-compose.yml
  .env.example
  .gitignore
  package.json
```

### Boundary rule

UI/routes may call application services. Application services may call domain engines and repositories. Domain financial math must not depend on React, route handlers, OpenAI, or Prisma-specific objects.

This separation is essential so calculations can be tested independently and later reused in background jobs/mobile/API surfaces.

---

## 3. Runtime Architecture

### 3.1 Alpha topology

On the development machine:

**Browser → local Next.js app → server/application services → PostgreSQL**

Optional AI path:

**server Lewis gateway → OpenAI API**

No financial data needs to be publicly reachable for Alpha.

### 3.2 Network exposure

Default dev binding should remain loopback/local machine. Do not expose the Alpha server or database to the LAN/Internet merely for convenience.

If phone access is later enabled during local testing, that is a dedicated security/configuration step rather than the default development posture.

### 3.3 Commercial migration

Later commercial deployment can replace local PostgreSQL with managed PostgreSQL and local app runtime with hosted application infrastructure while preserving:
- domain objects;
- migrations;
- calculation engines;
- service contracts;
- household scoping;
- audit model;
- Lewis gateway contracts.

---

## 4. Database Decision — PostgreSQL + Prisma

### Why PostgreSQL now

Budget's core is relational and audit-heavy: transactions, imports, recurrence, rules, bills, calendar events, forecasts, recommendations, reconciliation and later debt/goals/documents.

Using PostgreSQL in Alpha avoids a SQLite-to-Postgres behavior/schema migration later and supports:
- strong constraints;
- transactional imports;
- JSON fields where justified;
- precise numeric/integer types;
- indexing;
- future row-level/multi-household patterns;
- mature backup/export tooling.

### Prisma role

Prisma provides:
- explicit schema/migrations;
- typed data access;
- developer-friendly local workflow;
- controlled schema evolution.

Financial logic must not be embedded in Prisma hooks or opaque database behavior. Constraints protect integrity; domain services own business rules.

---

## 5. Money, Dates and Precision

### 5.1 Money representation

For ordinary USD household amounts, persist and calculate **integer cents** using a 64-bit-safe database/application representation.

Never use JavaScript floating-point arithmetic for authoritative money calculations.

Where later interest/APR calculations require fractional precision, use a decimal library / PostgreSQL numeric for the rate/math boundary and explicitly round monetary outputs according to a versioned rule.

### 5.2 Currency

Alpha defaults to USD but financial records should retain a currency code so commercial architecture is not hard-coded invisibly.

### 5.3 Dates

Store:
- instants/timestamps in UTC where an actual timestamp exists;
- financial calendar dates as date-only values when the concept is a local due/pay date;
- household timezone explicitly.

Do not turn an inferred withdrawal date into a contractual due-date timestamp.

---

## 6. Physical Data Architecture

Every household-owned domain row must carry `household_id` either directly or through an enforced parent relationship. Even with one Alpha household, this prevents commercial multi-tenancy from requiring a conceptual rewrite.

Core Alpha tables/entities should include:

- households
- household_members
- financial_accounts
- import_batches
- raw_transactions
- transactions
- merchants
- merchant_aliases/fingerprints
- categories
- classification_rules
- income_sources
- income_events
- recurring_series
- bills
- budget_baselines
- budget_targets
- protected_allocations
- calendar_events
- forecast_runs
- forecast_entries
- recommendations
- review_items
- reconciliation_sessions
- reconciliation_adjustments
- audit_events
- model_artifacts / AI artifacts where structured reusable results need provenance

### Raw versus interpreted transaction split

`raw_transactions` is append/immutable evidence.

`transactions` is the current normalized interpretation and links back to raw evidence.

Corrections mutate/version interpretation, not raw source.

### Source provenance

Consequential facts should support provenance metadata:
- source type;
- source object/row;
- actor;
- extraction/inference method;
- confidence;
- confirmation state;
- timestamp/version.

---

## 7. CSV Import Architecture

### Pipeline

```text
file selection
→ fingerprint
→ parser preview
→ column mapping
→ schema validation
→ normalized candidate rows
→ duplicate/overlap analysis
→ transactional commit of ImportBatch + RawTransactions
→ deterministic normalization
→ learned-rule application
→ inference queue
→ review queue
→ import summary
```

### Idempotency

Use multiple protections:
- file fingerprint;
- source-row fingerprint;
- account/date/amount/description similarity checks for overlap;
- import batch identity.

Exact duplicate rows may be safely recognized, but ambiguous near-duplicates must be surfaced rather than silently discarded.

### Import adapters

CSV mapping is adapter-based. A generic mapper handles arbitrary exports; later bank-specific templates can be added without changing normalized domain contracts.

### Sensitive source files

Do not commit CSVs. Runtime source files belong in gitignored local storage and should be deletable after successful immutable row ingestion if the household chooses.

Testing uses synthetic fixtures only.

---

## 8. Deterministic Classification and Learning Engine

Classification precedence:

1. explicit user-confirmed override;
2. active household learned rule;
3. deterministic system rule;
4. high-confidence recurring/merchant inference;
5. AI classification candidate;
6. unresolved Review item.

A lower-priority mechanism must not silently overwrite a higher-authority interpretation.

### Merchant fingerprints

Normalize case/punctuation/noise, preserve original descriptor, and support aliases/fingerprints rather than exact-string-only matching.

### Rule versioning

Rules record:
- creator/source;
- match criteria;
- output;
- effective scope;
- created/updated time;
- active state.

Retroactive application is a deliberate service operation with preview, transaction and audit event.

---

## 9. Recurrence / Bill Detection

Alpha recurrence detection should begin deterministic/statistical rather than LLM-first.

Candidate signals:
- normalized merchant/fingerprint;
- repeated amounts or bounded amount range;
- intervals clustered around weekly/biweekly/monthly/quarterly/annual cadence;
- repeated day-of-month/pay-period timing;
- sufficient occurrence count.

Output:
- candidate series;
- cadence;
- typical amount/range;
- next expected window;
- confidence;
- evidence transaction IDs.

User confirmation can promote an inferred series to a confirmed household relationship.

Contractual due date remains separate and unknown until explicitly confirmed/evidenced.

---

## 10. Forecasting Engine

The forecast engine is a pure/versioned domain service.

Inputs:
- as-of balance;
- protected allocations;
- included income events;
- confirmed/expected calendar events;
- recurring expected events;
- necessary-spending assumptions;
- negative carry-forward;
- scenario/confidence policy.

Outputs:
- ordered forecast entries;
- projected balance after each entry;
- lowest projected cash point;
- horizon ending position;
- included/excluded assumptions;
- confidence/degradation reasons.

### Scenarios

Alpha should support at least a conservative/default calculation policy internally, with room for expected/upside scenarios later.

Prospective income is excluded from conservative spendable cash unless explicitly promoted by household confirmation/policy.

### Reproducibility

Persist forecast metadata/version/assumptions or enough inputs to reproduce important dashboard results.

---

## 11. True Available Cash Engine

True Available Cash is not an AI result.

The engine calculates, for the next-income horizon:

```text
authoritative usable cash
- protected reserve/allocation
- included required obligations before horizon
- included necessary/normal spending before horizon
- adopted protected commitments
- negative carry-forward
= True Available Cash
```

Implementation must identify source objects behind every term.

### Explainability object

Return both the result and structured components:

```ts
{
  amountCents,
  horizonEnd,
  nextIncomeEventId,
  components: [...],
  exclusions: [...],
  warnings: [...],
  calculationVersion
}
```

The dashboard renders this object; Lewis may explain it but may not recalculate it independently.

---

## 12. Reconciliation Architecture

Reconciliation is an application service over immutable evidence + current interpretations.

A session records:
- authoritative account balance/as-of;
- modeled balance/as-of;
- opening difference;
- candidate causes reviewed;
- corrections/linkages;
- explicit adjustment if required;
- closing difference/status.

Manual reconciliation adjustments are first-class records, never fake bank transactions.

A material unresolved difference emits a data-trust warning consumed by forecast/TAC/Lewis.

---

## 13. Lewis / OpenAI Architecture

### 13.1 Server-only gateway

All OpenAI calls pass through `src/server/lewis/`.

The browser never receives the API key.

Environment:
- `OPENAI_API_KEY` locally;
- optional model/config variables;
- no secret values committed;
- startup/config check reports only configured/not configured.

Budget may reuse an operator-managed local key, but Budget must not read another project's secret file as a permanent architecture. The operator supplies the environment to Budget.

### 13.2 Structured contracts

Lewis calls use explicit Zod-validated input/output contracts.

Examples:
- transaction classification candidate;
- review prioritization explanation;
- daily briefing;
- recommendation explanation.

Invalid model output is rejected/degraded gracefully rather than written as financial truth.

### 13.3 Data minimization

Send only the financial context necessary for the specific task. Avoid shipping the entire transaction history to every prompt.

Prefer structured aggregates, relevant transaction subsets, masked labels and IDs over unnecessary raw sensitive data.

### 13.4 AI artifact provenance

Reusable AI results record:
- purpose;
- model/config identifier;
- prompt/contract version;
- input-data fingerprint or relevant source references;
- structured output;
- validation status;
- created time;
- superseded state.

Do not log API keys or raw provider responses containing unnecessary sensitive context.

### 13.5 Graceful no-AI mode

The deterministic product must remain usable if OpenAI is unavailable. Lewis surfaces an unavailable/degraded state while import, ledger, calculations, review, calendar and reconciliation continue functioning.

---

## 14. Security and Sensitive Data

### 14.1 Threat posture for Alpha

Alpha contains real household financial information. “Local” does not mean non-sensitive.

Primary Alpha risks:
- accidental Git commit;
- malware/other local users;
- lost/stolen machine;
- unencrypted backups;
- debug logs/screenshots;
- exposed dev server;
- leaked API key;
- source CSV duplication.

### 14.2 Required controls before real data

- `.gitignore` blocks `.env*`, local-data, database dumps, CSV/import files and backup artifacts except explicit safe examples;
- secret scanning in validation/CI where feasible;
- full-disk encryption on the development machine is a prerequisite operational control for real household data;
- PostgreSQL is not exposed publicly;
- strong local OS account protection;
- application logs redact/minimize financial payloads;
- no raw financial fixtures in tests;
- backup procedure produces encrypted storage or is placed on an encrypted volume;
- restore procedure is tested before Alpha is considered durable.

### 14.3 Application-level encryption

Do not invent custom cryptography.

For Alpha, rely on OS/full-disk encrypted storage for the database volume plus application-level authenticated encryption for especially sensitive blob/document payloads when those capabilities arrive.

If application-level encrypted fields are introduced, use a vetted standard primitive/library and a key sourced from the environment/OS secret facility, separate from ciphertext storage.

### 14.4 Authentication

Because Alpha is local and initially operated on the trusted development machine, do not build a commercial identity provider before the product is alive.

Use a **local household access gate/session** sufficient to prevent casual unauthorized browser access, with credentials/secrets stored as salted password hashes / environment-backed setup—not plaintext.

The domain still models Steve/Kelly as distinct Owners so actor attribution is preserved.

Before any LAN/mobile exposure, remote deployment, or commercial use, authentication must be upgraded/reviewed as a dedicated security gate.

---

## 15. Audit Architecture

Audit events are append-oriented and generated by application services for material actions.

Examples:
- import committed;
- interpretation changed;
- classification rule created/disabled;
- retroactive correction applied;
- authoritative balance confirmed;
- reconciliation completed/adjusted;
- reserve policy changed;
- recommendation accepted/rejected;
- consequential configuration changed.

Audit payloads should reference domain IDs and concise before/after structured state, not duplicate every sensitive source row.

Audit records are not a substitute for database backups.

---

## 16. Backup / Recovery

Before real household Alpha use:

1. provide a scripted PostgreSQL backup command;
2. write backup to a gitignored encrypted/local-protected location;
3. record schema/migration version;
4. provide restore command/runbook;
5. perform at least one restore test into a disposable local database;
6. document CSV/source-file retention policy.

Later commercial backup strategy will use managed encrypted backups, point-in-time recovery and household export/delete policies.

---

## 17. Testing Strategy

### Unit tests
Pure domain tests for:
- money math;
- sign convention;
- TAC;
- reserve exclusion;
- spend-forward;
- recurrence intervals;
- forecast ordering;
- confidence inclusion;
- no-double-counting;
- reconciliation differences;
- classification precedence.

### Property/invariant tests
Where practical:
- import twice does not duplicate authoritative rows;
- transfers do not inflate income/spending;
- protected dollars cannot simultaneously be ordinary available dollars;
- same forecast inputs/version produce same result;
- raw transaction cannot be mutated through normal application service;
- forecast event affects balance no more than once.

### Integration tests
Use a disposable test PostgreSQL database for:
- imports;
- transactions;
- rule application;
- retroactive correction transactionality;
- reconciliation;
- audit events.

### E2E
Playwright covers:
1. first-run setup;
2. CSV import;
3. review/correction;
4. current balance/reconciliation;
5. dashboard/TAC drilldown;
6. Lewis briefing degraded and enabled states.

### Fixtures
Only synthetic household data in repository.

---

## 18. Development Validation Commands

Phase 3/Cursor should establish scripts similar to:

```text
npm run dev
npm run typecheck
npm run lint
npm run test
npm run test:integration
npm run test:e2e
npm run db:up
npm run db:down
npm run db:migrate
npm run db:seed
npm run db:backup
npm run db:restore:check
npm run validate
```

`npm run validate` should become the routine build gate combining static checks and appropriate test suites.

Exact command implementation belongs to Phase 3.

---

## 19. Observability and Cost Controls

### Local observability
Track:
- import duration/counts;
- unresolved review counts;
- classification source/confidence;
- forecast calculation version;
- reconciliation status;
- Lewis request purpose/status/latency;
- AI token/cost estimate where available.

### Privacy
Do not create verbose logs containing raw transaction histories merely for debugging.

### AI budget
Maintain per-feature AI usage telemetry so the eventual $3–$5/month commercial aspiration can be tested against actual variable cost.

Routine learned classification should trend toward zero incremental AI calls.

---

## 20. Commercial Migration Contract

Alpha architecture must preserve these future moves:

### Multi-household
Household scoping exists from the beginning. Commercial authorization must enforce it at every repository/service boundary and later database policy layer where appropriate.

### Hosted database
PostgreSQL schema/migrations migrate to managed PostgreSQL rather than a new database model.

### Authentication
Local access gate is replaceable with commercial auth/OAuth/passkeys without changing financial ownership semantics.

### Storage
Local file/blob adapter becomes object storage adapter for documents/receipts.

### Jobs
In-process Alpha analysis can move behind a queue/background worker for imports, document extraction, recurring analysis and notifications.

### Banking
A financial-data provider adapter can populate the same normalized account/transaction contracts used by CSV.

### Actions
Payment/cancellation/provider-action integrations attach behind explicit authorization/audit services rather than directly inside Lewis.

---

## 21. Technical Decisions Explicitly Deferred

Phase 2 does not select:
- production cloud host;
- production managed PostgreSQL vendor;
- bank-data provider;
- payment provider;
- commercial identity provider;
- production object storage;
- push/SMS/email provider;
- document OCR vendor/model;
- Wealth Builder brokerage/investment integration.

Selecting them now would add dependency without helping Alpha Alive.

---

## 22. Phase 2 Architecture Gates

Phase 2 is complete when the blueprint provides explicit answers for:

- app/runtime stack — **decided**;
- database/ORM — **decided**;
- local database operation — **decided**;
- money/date conventions — **decided**;
- import architecture — **decided**;
- deterministic rules/recurrence architecture — **decided**;
- forecast/TAC boundary — **decided**;
- reconciliation architecture — **decided**;
- Lewis/OpenAI boundary — **decided**;
- local security posture — **decided**;
- authentication posture — **decided for Alpha, gated before exposure**;
- audit model — **decided**;
- backup/restore posture — **decided**;
- testing strategy — **decided**;
- commercial migration path — **decided**;
- deferred provider choices — **explicitly bounded**.

The next phase is **Phase 3 — Cursor Build Plan**, where this blueprint is converted into ordered, gated implementation passes with validation commands and operator review checkpoints.

---

## 23. Technical North Star

**The financial engine must remain trustworthy without Lewis; Lewis makes the trustworthy engine understandable, proactive and useful.**

That boundary is the most important technical decision in Budget.
