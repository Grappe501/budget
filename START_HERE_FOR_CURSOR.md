# START HERE — Budget Local Construction

This repository contains a precompiled 15-pass construction program.

## First local instruction to Cursor

Give Cursor this instruction from the repository root:

> Read `CURSOR_15_PASS_EXECUTION_PACKAGE.md`, `build/EXECUTION_INDEX.json`, and the canonical authority documents it names. If the construction control plane/build state has not yet been created locally, execute only **Pass P00** using its script and manifest. Otherwise execute only the next pass/slice marked READY by generated build state. Do not skip validation, do not redesign product behavior, and stop at every operator gate.

## Execution order

P00 → P01 → P02 → P03 → P04 → P05 → P06 → P07 → P08 → P09 → P10 → P11 → **STOP FOR REAL-DATA AUTHORIZATION** → P12 → P13 → P14.

## Important

- Pull latest `main` before beginning a new parent pass.
- Pass scripts live in `cursor_passes/`.
- Machine manifests live in `build/manifests/`.
- `PRECOMPILED_CONSTRUCTION_SPECIFICATION.md` is the most specific implementation contract.
- Never place real household financial data in Git.
- P12 may begin only after the P11 operator gate is explicitly approved.
- P14 may declare ALPHA_ALIVE only if certification evidence is green.

Cursor is the bounded executor. The committed plan is the architect.
