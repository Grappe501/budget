# Cursor Pass 03 — Evidence Import

**Status:** PREWRITTEN / execute only when build state marks P03 READY

## Mission
Build trustworthy synthetic CSV ingestion end-to-end from local file selection through immutable evidence, normalized candidate creation, import history and processing events. Implement adapter/mapping versioning, file/row fingerprints, exact duplicate and overlap handling, validation, transactional commit and explicit failure states.

## Mandatory read-first authority
Read, in order: PRECOMPILED_CONSTRUCTION_SPECIFICATION.md; LEVEL_10_AUTONOMOUS_CONSTRUCTION_HARDENING.md; PRE_CONSTRUCTION_FORENSIC_AUDIT_AND_LAYERED_BUILD_HARDENING.md; PHASE_3_CURSOR_BUILD_PLAN.md; PHASE_2_TECHNICAL_BLUEPRINT.md; PHASE_1_PRODUCT_SPECIFICATION.md; MASTER_PRODUCT_PLAN.md; build/build_state.json and build/next_slice.json when they exist.

## Universal execution rules
Work only in this repository. Do not use RedDirt code or secrets. Do not deploy to Netlify/cloud. Do not add bank connectivity, money movement, Self-Bank or Wealth Builder. Never commit real financial data, credentials, API keys, dumps, screenshots containing real finances, or real CSVs. Do not weaken an invariant to make validation pass. Domain math is deterministic; Lewis never becomes the calculator. Unknown is not zero. Raw evidence is immutable after committed import. Household scope fails closed. Use commands for mutation and shared query/read-model contracts for presentation. Material mutations write audit + durable outbox atomically. Derived recomputation is idempotent. Stop if a required fix exceeds this pass, changes product doctrine, weakens security, requires destructive real-data action, or conflicts with canonical authority.

## Required A–F execution rhythm
A — contracts/scaffolding. B — core implementation. C — integration/dataflow. D — read-model/experience or operational surface. E — tests/adversarial/failure injection. F — validation/generated docs/evidence/build-state closeout.
Do not start the next parent pass unless F is GREEN and next pass is READY.

## Completion evidence
Generate/update latest validation report, latest slice report, build state, current handoff, next slice, BUILD_PROGRESS and generated inventories. Record starting/ending commit, changed paths, migrations, invariants/tests, commands/results, Golden Household result where available, architecture/security results, blockers and rollback checkpoint. Commit only after green validation.

## Pre-authorized sub-slices
A: register ImportBatch/RawTransaction commands, queries, events, state transitions and import invariants. B: parser adapter, preview/mapping/validation/fingerprints/dedup engine and transactional commit. C: commit raw evidence + normalized transaction shell + audit/outbox; processing task hooks. D: /transactions/import and import history/read models with counts/errors/evidence. E: torture corpus: BOM, CRLF, quotes, extra/blank columns, signed/debit-credit, parentheses, reverse order, duplicates, overlap, malformed rows, refunds, same-dollar distinct rows; failure injection mid-import. F: prove exact reimport idempotency, ambiguous rows not silently discarded, raw immutable; Gate A synthetic evidence.

## Pass-specific guardrail
Synthetic files only. Invalid required financial rows block initial Alpha commit rather than disappearing.

## Validation baseline
Run every package script applicable at this stage. By P01 onward this converges on `npm run typecheck`, `npm run lint`, `npm run test`, architecture/contracts validation, and pass-specific integration/E2E suites. By later passes include Golden Household, calculation vectors, isolation, failure injection, leak/security and AI evals as applicable. Never claim a command ran if it does not yet exist; P00/P01 must create the scripted validation surface specified by canonical contracts.

## Exit
P03 becomes COMPLETE only through generated green evidence. Otherwise remain BUILDING/BLOCKED/OPERATOR_GATE. Do not manually edit state to bypass validation.
