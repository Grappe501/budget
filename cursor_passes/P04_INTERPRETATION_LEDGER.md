# Cursor Pass 04 — Interpretation Ledger

**Status:** PREWRITTEN / execute only when build state marks P04 READY

## Mission
Turn imported evidence into an inspectable economic ledger with normalized transactions, merchants/aliases, categories, economic types, transfer/refund/reimbursement/reversal relationships, allocation seam, deterministic classification precedence and evidence lineage.

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
A: register economic types, classification authority and ledger query contracts. B: merchant fingerprinting, categories, classification engine, relationship/link services and allocation invariants. C: import processing invokes interpretation through outbox/task flow; confirmed authority locks against lower inference. D: Transactions ledger/detail/search/filter/evidence and confidence/authority states. E: transfer neutrality, reimbursement/refund semantics, relationship isolation, allocation sum invariant, no raw mutation, two-household adversarial tests. F: Golden Household ledger known answers and generated lineage/inventory closeout.

## Pass-specific guardrail
Do not use AI as authoritative classifier. AI candidate hook may remain interface-only until P10.

## Validation baseline
Run every package script applicable at this stage. By P01 onward this converges on `npm run typecheck`, `npm run lint`, `npm run test`, architecture/contracts validation, and pass-specific integration/E2E suites. By later passes include Golden Household, calculation vectors, isolation, failure injection, leak/security and AI evals as applicable. Never claim a command ran if it does not yet exist; P00/P01 must create the scripted validation surface specified by canonical contracts.

## Exit
P04 becomes COMPLETE only through generated green evidence. Otherwise remain BUILDING/BLOCKED/OPERATOR_GATE. Do not manually edit state to bypass validation.
