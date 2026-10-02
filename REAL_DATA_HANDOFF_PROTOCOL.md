# Budget — Real-Data Handoff Protocol

**Version:** 1.0
**Purpose:** Precompile the only planned human interaction required between synthetic construction and real household calibration.

## 1. Trigger
This protocol activates only after P11 is GREEN and the Real-Data Readiness report is GREEN.

Cursor must not request or inspect real household financial data before this point.

## 2. Cursor's exact operator checkpoint
At the P11 gate Cursor must present a concise checkpoint stating:
- P00–P11 are GREEN or identify any exception;
- backup/restore proof status;
- local access/security status;
- leak/ignore status;
- real-data authorization is currently FALSE;
- P12 will import real household data locally and will never commit the source CSV or sensitive transaction detail.

Then Cursor must ask the operator for these confirmations, and only these confirmations:
1. Confirm this development machine has full-disk encryption enabled.
2. Confirm the OS account/login is protected.
3. Confirm the Budget backup destination is protected.
4. Confirm Budget is not intentionally exposed to the public internet or unreviewed LAN access.
5. Confirm: **I authorize Budget P12 to use my real household financial CSV locally.**

Do not ask the operator to paste balances, account numbers, credentials, transaction details or API keys into chat.

## 3. CSV request
After all five confirmations are affirmative, Cursor must instruct the operator:

> Place/export the household bank CSV into the local Budget import staging area shown by the application, or use Budget's Import screen/file picker. Do not copy the CSV into a Git-tracked folder. Tell me when the file has been selected/staged.

Preferred staging root is `local-data/imports/`, which must be gitignored. The UI/file-picker path is preferred when implemented.

Cursor must not require the operator to rename the file unless necessary for filesystem compatibility.

## 4. Authorization representation
The authorization is LOCAL operational state, not a Git-tracked assertion containing personal information.

The system records:
- gate identifier;
- authorized=true;
- authorization timestamp;
- local actor/operator identifier;
- readiness evidence version/commit;
- no financial payload.

If build state is Git tracked, it may record only that P12 is OPERATOR_GATE/READY in a non-sensitive way. The actual real-data authorization must be enforced by local operational state so cloning the repository elsewhere does not inherit authorization.

## 5. Pre-import checkpoint
Immediately before opening the real CSV:
- verify source path is ignored/untracked;
- run Git leak guard;
- create/verify a fresh protected DB backup;
- record application/schema version;
- verify target DB is the authorized household DB and not test DB;
- verify destructive reset/seed commands are locked against that DB;
- verify logging redaction mode;
- verify no test runner targets the household DB.

If any check fails, stop before reading/importing the CSV.

## 6. CSV inspection
Budget may inspect the local file through the product import adapter.

Cursor reports only non-sensitive diagnostics unless the operator is looking directly at the local UI:
- filename may be sanitized;
- row count;
- candidate date range;
- detected columns;
- parse error counts;
- duplicate/overlap counts;
- mapping confidence/status.

Do not echo transaction descriptions, balances, account numbers, full source rows or other sensitive values into Git-tracked reports or terminal summaries intended for copying.

## 7. Mapping
Use the built import workflow. Required semantic fields:
- transaction date;
- description;
- amount OR debit+credit;
- optional running balance.

If mapping is ambiguous, ask the operator only the minimum local question needed, preferably through the app UI.

Do not modify parser logic merely to fit one household export unless the change is a generalized adapter improvement backed by synthetic regression fixtures.

## 8. Commit and calibration
Once mapping validates:
1. import through normal P03 evidence pipeline;
2. preserve RawTransaction evidence;
3. run P04 interpretation;
4. run P05 review/learning;
5. run P06 reconstruction;
6. run P07 forecast/TAC;
7. run P08 reconciliation;
8. refresh P09 UX snapshot;
9. invoke P10 Lewis only after deterministic truth is sufficiently trusted.

Do not bypass product workflows with ad-hoc SQL.

## 9. Operator interactions during calibration
Cursor may ask the operator to confirm unknown household facts through the application, including:
- which detected deposits are income;
- expected next payday when not reliably inferred;
- whether a recurring item is a real bill/subscription;
- category/merchant corrections;
- authoritative current bank balance for reconciliation;
- reserve account/balance/policy facts;
- whether an observed date is a contractual due date.

Ask only when the application cannot safely infer the fact.

Never ask for online banking credentials.

## 10. Reconciliation requirement
P12 cannot close GREEN until:
- an authoritative current BalanceObservation is supplied/confirmed locally;
- reconciliation status is known;
- any unresolved difference is explicit;
- Data Trust reflects that difference honestly.

A non-zero unresolved difference may be allowed only if it is explicit, understood as unresolved, and does not make TAC certification misleading. Otherwise P12 remains blocked.

## 11. Manual TAC proof
Before P12 closes:
- choose the active household snapshot;
- display locally the TAC components;
- independently recompute from the same authoritative components using the deterministic calculation-vector/manual-check tool;
- confirm result matches exactly in cents;
- record only PASS/FAIL, component count, calculation version and snapshot ID/fingerprint in Git-safe evidence—not real dollar values.

## 12. Sensitive evidence rule
Git-safe P12 evidence may contain:
- row counts;
- date-span length without sensitive merchant data;
- classification/review counts;
- reconciliation state;
- unresolved item counts;
- test pass/fail;
- snapshot/calculation IDs or safe fingerprints;
- performance timing;
- aggregate accuracy percentages.

It must not contain:
- transaction descriptions;
- real balances;
- real income amounts;
- real bill amounts;
- account numbers;
- raw CSV content;
- screenshots containing real finances.

## 13. CSV retention
After successful import, the operator may keep the source CSV in the ignored local-data area for recovery/reimport or remove it. Budget must not silently delete the operator's only source copy.

## 14. Automatic continuation
After P12 becomes GREEN:
- commit only Git-safe code/contracts/tests/evidence;
- mark P12 COMPLETE and P13 READY;
- continue automatically into P13;
- after P13 GREEN continue automatically into P14;
- P14 may require final operator usability acceptance but must not require a new architecture/build script.

## 15. Final operator acceptance
At P14 Cursor presents the six Definition-of-Alive questions in the local application:
1. Are we financially safe right now?
2. What can we actually spend before the next payday?
3. What money is already spoken for?
4. What is likely to happen next?
5. Why does Budget believe these numbers?
6. What does Lewis recommend doing first, and why?

The operator confirms whether the product answers these intelligibly. This is product acceptance, not permission for Cursor to alter financial doctrine.

## 16. Failure behavior
If real data exposes a generalized defect:
- create a sanitized reproduction using synthetic data;
- fix the generalized defect;
- add regression coverage;
- rerun synthetic suite;
- rerun affected real calibration locally;
- never commit the real reproducer.

If the defect requires doctrine change, stop for operator approval.

## 17. End state
A single initial Cursor launch may therefore proceed:
P00 → ... → P11 → **ask operator confirmations + request CSV** → P12 → P13 → P14 → ALPHA_ALIVE if all gates pass.

No additional construction prompt is required.
