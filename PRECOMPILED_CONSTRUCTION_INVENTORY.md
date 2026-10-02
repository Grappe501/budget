# Budget Precompiled Construction Inventory

## Canonical planning
- MASTER_PRODUCT_PLAN.md
- PHASE_1_PRODUCT_SPECIFICATION.md
- PHASE_2_TECHNICAL_BLUEPRINT.md
- PHASE_3_CURSOR_BUILD_PLAN.md
- PRE_CONSTRUCTION_FORENSIC_AUDIT_AND_LAYERED_BUILD_HARDENING.md
- LEVEL_10_AUTONOMOUS_CONSTRUCTION_HARDENING.md
- PRECOMPILED_CONSTRUCTION_SPECIFICATION.md

## Execution control
- CURSOR_15_PASS_EXECUTION_PACKAGE.md
- START_HERE_FOR_CURSOR.md
- build/EXECUTION_INDEX.json
- build/manifests/P00.json through P14.json

## Prewritten Cursor scripts
- P00: see manifest `build/manifests/P00.json` for exact script path
- P01: see manifest `build/manifests/P01.json` for exact script path
- P02: see manifest `build/manifests/P02.json` for exact script path
- P03: see manifest `build/manifests/P03.json` for exact script path
- P04: see manifest `build/manifests/P04.json` for exact script path
- P05: see manifest `build/manifests/P05.json` for exact script path
- P06: see manifest `build/manifests/P06.json` for exact script path
- P07: see manifest `build/manifests/P07.json` for exact script path
- P08: see manifest `build/manifests/P08.json` for exact script path
- P09: see manifest `build/manifests/P09.json` for exact script path
- P10: see manifest `build/manifests/P10.json` for exact script path
- P11: see manifest `build/manifests/P11.json` for exact script path
- P12: see manifest `build/manifests/P12.json` for exact script path
- P13: see manifest `build/manifests/P13.json` for exact script path
- P14: see manifest `build/manifests/P14.json` for exact script path

## Freeze rule
The product/architecture plan is frozen for construction. Changes require explicit change control/ADR when they alter canonical behavior, security, financial invariants, pass scope or operator gates.

## Expected local outcome
After a fresh pull, the operator starts with START_HERE_FOR_CURSOR.md. P00 creates the live construction control plane and generated build state. Thereafter Cursor follows generated READY state, but only within the prewritten P00–P14 program.
