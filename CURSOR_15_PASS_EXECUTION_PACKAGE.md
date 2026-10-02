# Budget — Cursor 15-Pass Execution Package

**Status:** PRECOMPILED / construction-ready after scripts and manifests below are present
**Human roadmap:** Pass 00 through Pass 14
**Machine granularity:** six pre-authorized sub-slices A–F per parent pass
**Execution rule:** sequential; GREEN unlocks next; Pass 11 stops for explicit real-data authorization.

## Operator workflow
Pull/clone this repository locally, open its root in Cursor, and instruct Cursor: **Read CURSOR_15_PASS_EXECUTION_PACKAGE.md and execute only the next READY pass from build state.** On a fresh clone before build state exists, begin with Pass 00. Cursor must read the individual script for that pass and all canonical authority documents. It may repair within pass boundaries but may not redesign Budget.

## Pass index
- **P00 — Build Factory Bootstrap** → `cursor_passes/P00_BUILD_FACTORY_BOOTSTRAP.md` → manifest `build/manifests/P00.json`
- **P01 — Runtime & Experience Foundation** → `cursor_passes/P01_RUNTIME_EXPERIENCE_FOUNDATION.md` → manifest `build/manifests/P01.json`
- **P02 — Domain & Persistence Factory** → `cursor_passes/P02_DOMAIN_PERSISTENCE_FACTORY.md` → manifest `build/manifests/P02.json`
- **P03 — Evidence Import** → `cursor_passes/P03_EVIDENCE_IMPORT.md` → manifest `build/manifests/P03.json`
- **P04 — Interpretation Ledger** → `cursor_passes/P04_INTERPRETATION_LEDGER.md` → manifest `build/manifests/P04.json`
- **P05 — Review & Learning** → `cursor_passes/P05_REVIEW_LEARNING.md` → manifest `build/manifests/P05.json`
- **P06 — Reconstruction** → `cursor_passes/P06_RECONSTRUCTION.md` → manifest `build/manifests/P06.json`
- **P07 — Deterministic Finance Core** → `cursor_passes/P07_DETERMINISTIC_FINANCE_CORE.md` → manifest `build/manifests/P07.json`
- **P08 — Reconciliation & Trust** → `cursor_passes/P08_RECONCILIATION_TRUST.md` → manifest `build/manifests/P08.json`
- **P09 — Household Command UX** → `cursor_passes/P09_HOUSEHOLD_COMMAND_UX.md` → manifest `build/manifests/P09.json`
- **P10 — Lewis Advisory** → `cursor_passes/P10_LEWIS_ADVISORY.md` → manifest `build/manifests/P10.json`
- **P11 — Real-Data Safety Gate** → `cursor_passes/P11_REAL_DATA_SAFETY_GATE.md` → manifest `build/manifests/P11.json`
- **P12 — Real Household Calibration** → `cursor_passes/P12_REAL_HOUSEHOLD_CALIBRATION.md` → manifest `build/manifests/P12.json`
- **P13 — Evidence-Driven Hardening** → `cursor_passes/P13_EVIDENCE_DRIVEN_HARDENING.md` → manifest `build/manifests/P13.json`
- **P14 — Alpha Certification** → `cursor_passes/P14_ALPHA_CERTIFICATION.md` → manifest `build/manifests/P14.json`

## Global gates
- P00–P10: synthetic data only.
- P11: prove real-data readiness and STOP.
- P12: requires explicit operator authorization outside Git-sensitive data.
- P13: evidence-driven hardening.
- P14: certification; ALPHA_ALIVE only on green evidence.
- No pass may skip failed validation or weaken an invariant.
- No automatic public deployment or money movement exists anywhere in this package.

## Canonical authority
PRECOMPILED_CONSTRUCTION_SPECIFICATION.md has highest implementation specificity, followed by Level 10 hardening, Level 1 hardening, Phase 3, Phase 2, Phase 1, then Master Plan.

## Completion doctrine
A screen working is not completion. A slice is complete only when contracts, persistence, authorization, provenance, audit/outbox, invalidation/recompute, read model, UI/Lewis consumption where relevant, tests, architecture checks, generated docs, evidence bundle and build state are coherent.

## Construction freeze
These scripts pre-authorize implementation scope. New product behavior or architecture changes require an ADR and operator approval where consequential. Cursor does not invent a sixteenth pass.
