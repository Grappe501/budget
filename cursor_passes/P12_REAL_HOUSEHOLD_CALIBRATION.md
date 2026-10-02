# Cursor Pass 12 — Real Household Calibration

**Status:** PREWRITTEN / execute only when build state marks P12 READY

## Mission
Only after explicit operator authorization under `REAL_DATA_HANDOFF_PROTOCOL.md`, request/select the Steve/Kelly household CSV locally, import and calibrate household data through product workflows. Never commit sensitive details. Verify mapping, counts, high-materiality classifications, recurring income/bills, reconciliation, reserve, next income, forecast and TAC against manual checks.

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
A: verify local operator authorization created by `REAL_DATA_HANDOFF_PROTOCOL.md`; verify ignored/untracked CSV staging, fresh protected backup, household DB identity, destructive-command lock, redacted logging and test-DB isolation. B: request/accept the CSV through Budget's Import UI or ignored `local-data/imports/`; run normal import workflow without code/SQL shortcuts; report only Git-safe diagnostics. C: resolve review/rules/recurrence/income/bills through product commands; request only minimum unknown facts; obtain/confirm authoritative current BalanceObservation; reconcile; rebuild snapshot. D: inspect household UX and enable Lewis only after deterministic outputs are trusted. E: independently prove TAC exactly in cents without committing real component values; run all synthetic regressions unchanged; sanitize any real-data-discovered bug into a synthetic regression. F: write only protocol-approved aggregate/non-sensitive calibration evidence, mark only truly PROVEN_REAL trust dimensions, commit Git-safe work, mark P13 READY and continue automatically.

## Pass-specific guardrail
Follow `REAL_DATA_HANDOFF_PROTOCOL.md` in full. Never patch code to fit Steve's numbers. General bug fixes must preserve synthetic tests. No real transaction descriptions/amounts/source files in Git reports.

## Validation baseline
Run every package script applicable at this stage. By P01 onward this converges on `npm run typecheck`, `npm run lint`, `npm run test`, architecture/contracts validation, and pass-specific integration/E2E suites. By later passes include Golden Household, calculation vectors, isolation, failure injection, leak/security and AI evals as applicable. Never claim a command ran if it does not yet exist; P00/P01 must create the scripted validation surface specified by canonical contracts.

## Exit
P12 becomes COMPLETE only through generated green evidence. Otherwise remain BUILDING/BLOCKED/OPERATOR_GATE. Do not manually edit state to bypass validation.
