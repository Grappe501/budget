# CURSOR — START BUDGET CONSTRUCTION

**Purpose:** This is the single local launch instruction for the precompiled Budget construction program.

## Operator instruction

After pulling/cloning the latest `Grappe501/budget` repository and opening the repository root in Cursor, tell Cursor only:

> **Read `CURSOR_START_CONSTRUCTION.md` and execute it.**

Everything else is defined below.

---

# Cursor execution instruction

You are the bounded construction executor for Budget.

## 1. Read before doing anything

Read these files in this order:

1. `CURSOR_START_CONSTRUCTION.md`
2. `START_HERE_FOR_CURSOR.md`
3. `CURSOR_15_PASS_EXECUTION_PACKAGE.md`
4. `build/EXECUTION_INDEX.json`
5. `PRECOMPILED_CONSTRUCTION_SPECIFICATION.md`
6. `LEVEL_10_AUTONOMOUS_CONSTRUCTION_HARDENING.md`
7. `PRE_CONSTRUCTION_FORENSIC_AUDIT_AND_LAYERED_BUILD_HARDENING.md`
8. `PHASE_3_CURSOR_BUILD_PLAN.md`
9. `PHASE_2_TECHNICAL_BLUEPRINT.md`
10. `PHASE_1_PRODUCT_SPECIFICATION.md`
11. `REAL_DATA_HANDOFF_PROTOCOL.md`
12. `MASTER_PRODUCT_PLAN.md`

Then inspect `build/build_state.json`, `build/next_slice.json`, and generated handoff/state artifacts if they exist.

## 2. Determine the authorized starting point

- If the live construction control plane/build state does not yet exist, begin with **P00 — Build Factory Bootstrap**.
- Otherwise, execute only the next pass/sub-slice marked READY by the generated build state and precompiled manifests.
- The parent-pass order is fixed: P00 → P01 → P02 → P03 → P04 → P05 → P06 → P07 → P08 → P09 → P10 → P11 → operator gate → P12 → P13 → P14.
- Do not invent P15 or any additional parent pass.

## 3. Execute continuously within authorized gates

For each READY parent pass:

1. read its manifest under `build/manifests/`;
2. read the exact Cursor pass script referenced by that manifest;
3. verify prerequisites;
4. execute its authorized A–F sub-slices in order;
5. run all required validation;
6. repair failures autonomously only inside the pass's permitted scope;
7. generate required evidence, reports, inventories, build state, handoff and next-slice artifacts;
8. commit the GREEN pass with a clear checkpoint commit;
9. if the next parent pass is READY and no operator gate applies, continue to the next pass.

You do **not** need the operator to type “next” between ordinary GREEN passes.

## 4. Mandatory stop conditions

Stop immediately and report clearly if:

- a financial/security invariant cannot be satisfied;
- canonical documents conflict in a way the authority order cannot resolve;
- repair requires changing product doctrine or weakening an invariant;
- repair requires work outside the authorized pass;
- a destructive effect is not explicitly authorized;
- a security boundary is ambiguous;
- real household data appears before it is authorized;
- an external service/provider decision requires operator approval;
- a pass cannot produce GREEN evidence.

Never bypass a failed gate by editing build state manually.

## 5. Absolute real-data gate

P00 through P11 must use synthetic/non-sensitive data only.

After P11 is GREEN, activate `REAL_DATA_HANDOFF_PROTOCOL.md`.

**PAUSE CONSTRUCTION AND INTERACT WITH THE OPERATOR.** Present the P11 readiness checkpoint, request the five required security/authorization confirmations, and then request the CSV using the exact local-only workflow in that protocol.

Cursor cannot authorize the real-data gate itself. The operator must explicitly authorize it.

Once the protocol's confirmations are satisfied and the CSV is selected/staged locally, P12 becomes authorized. Continue P12 → P13 → P14 automatically when each pass is GREEN. No new construction prompt is required.

## 6. Universal prohibitions

Do not:

- use or commit real household financial data before P12 authorization;
- commit real CSV files, account numbers, credentials, API keys, database dumps, or sensitive screenshots;
- read/copy RedDirt code or secrets;
- deploy Budget to Netlify or any public/cloud environment;
- add bank connectivity;
- add money movement/bill pay/autopay control;
- add subscription cancellation actions;
- add Self-Bank;
- add Wealth Builder;
- let Lewis perform authoritative financial calculations;
- let AI directly mutate authoritative financial state;
- mutate committed raw financial evidence;
- treat unknown as zero;
- infer observed timing as a contractual due date;
- weaken tests/invariants simply to make a pass green;
- opportunistically upgrade dependencies after the initial lock without authorized change control.

## 7. Architectural doctrine

Preserve:

**Evidence → Interpretation → Planning → Deterministic Intelligence → Shared Financial Snapshot → Lewis → Experience**

Mutations flow through application commands.

Material authoritative mutations commit their required domain change/outbox event and audit record atomically.

Derived recomputation is idempotent and retryable.

Dashboard and Lewis consume the same coherent Financial Snapshot family.

Household scoping fails closed.

Financial math remains deterministic and independently testable without Lewis.

## 8. Pass completion

A parent pass is complete only when its required A–F work is complete and generated evidence is GREEN.

At each pass closeout record:

- pass and slice status;
- starting and ending commit;
- changed paths;
- migrations;
- contracts/registries changed;
- invariants established/proven;
- tests added;
- validation commands/results;
- Golden Household results where applicable;
- architecture checks;
- security/leak checks;
- known limitations/blockers;
- rollback checkpoint;
- next READY pass or operator gate;
- confirmation of real-data status.

## 9. Git discipline

- Work on the current authorized construction branch according to repository state.
- Do not rewrite canonical planning history merely to simplify implementation.
- Commit each GREEN parent pass as a checkpoint.
- Do not commit failed/intermediate sensitive artifacts.
- Before beginning the next parent pass, ensure the previous pass's generated evidence and build state are committed.

## 10. Initial mission

On the first local run, execute **P00 — Build Factory Bootstrap**.

If P00 becomes GREEN, continue automatically into P01 and subsequent GREEN-authorized passes under this file's rules.

The first mandatory human interaction is P11's real-data handoff checkpoint unless an earlier stop condition occurs. Follow REAL_DATA_HANDOFF_PROTOCOL.md, request the CSV locally, then resume P12–P14 automatically after authorization.

## Final instruction

Begin now.

Build Budget from the committed precompiled construction program.

Do not redesign the program while executing it.

**Cursor executes. Contracts govern. Validators prove. Operator gates authorize.**
