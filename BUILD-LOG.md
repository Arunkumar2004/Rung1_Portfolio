# Build log

Built from `git log` on 13 Oct 2026: stage close commits on `main`, the stage close reports, the open stage branches and
the 7 Oct 2026 conformance audit. Dates are in India Standard Time.

## Timeline

| Date (2026) | Commits on `main` that day | What happened |
|---|---:|---|
| 26 Sep | 5 | First commit. The console front end was built first on mock data: design system, component library, console shell, routing and every tenant area. |
| 27 Sep | 61 | Build rulebook adopted (verify first, hooks over prose, zones). Stage S0 (harness, rules, contracts) started; S5 (ERPNext foundation) started in parallel. First standing approval, for steps up to 10. |
| 28 Sep | 38 | **S0 closed.** Hooks, invariant checks, contracts, code register and two decision records in place. |
| 29 Sep | 111 | **S1 closed**: control plane, sign-in and an insert-only audit log, live against the API. |
| 30 Sep | 90 | **S2 closed** (the elan1 kernel) and **S5 closed** (ERPNext bench, provisioner, schema pack). |
| 1 Oct | 184 | **S3 closed** (write API and run lifecycle, real runs in the console) and **S4 closed** (orchestrator on Temporal). The busiest day. |
| 3 Oct | 134 | **S6 closed**: the ERP tool server, with 922 live ERPNext tests. Default branch renamed to `main`; repository moved to the company GitHub. Owner rulings on the promoter, codes, support access and undo. |
| 5 Oct | 124 | **S7 closed** (AI agent pipeline; New request, Questions and Plan ran on the real model), **S8 closed** (compiler and sandbox) and **S9 closed** (gates G1–G8). |
| 6 Oct | 35 | **S14 closed** (agents, people, API keys) and **S12 closed** (credits and billing). Speed measures approved; a large workflow hit the session usage limit. |
| 7 Oct | 9 | 13-auditor conformance audit of S0–S14: 142 units, 6 blockers, 49 majors, a 50-item fix list. The weekly usage limit was reached the same day. |
| 8–11 Oct | 1 | Paused for the weekly limit. One commit by a teammate (a tracker sync), merged later without force. |
| 12 Oct | 34 | Resumed. Audit fix units S2-F1, S4-F1, S7-F1 and S9-F3 merged into `main`. |
| 13 Oct | 26 | Fix units S0-F1 and S12-F1 and ruling units RUL-G4, RUL-CODES and RUL-DEMO merged. Two decision record revisions accepted and merged. Open stages S10, S11, S13, S15, S16 and S17 continued on their branches. Inventory report generated. |

## Closed stages

"Tests at close" is quoted from the close commit message or the close report; the number grew as stages merged,
because each close runs the whole suite on `main`. "Audit (7 Oct)" is the conformance audit's scorecard for that stage:
units that matched the plan out of units audited, then blockers and majors found.

| Stage | What | First unit commit | Closed | Units done | Tests at close | Audit (7 Oct) |
|---|---|---|---|---|---|---|
| S0 | Harness, rules, contracts | 27 Sep | 28 Sep | 9 of 9 | 285 tests; invariant check green | 7 of 9 match; 0 blockers, 3 majors |
| S1 | Control plane: tenants, sign-in, audit | 28 Sep | 29 Sep | 8 of 8 | 204 tests with the database | 7 of 8; 0, 3 |
| S2 | elan1 kernel | 29 Sep | 30 Sep | 13 of 15 | 719 tests with the database | 14 of 15; 0, 5 |
| S5 | ERPNext foundation | 27 Sep | 30 Sep | 7 of 8 | 429 tests including live ERPNext | 7 of 8; 0, 1 |
| S3 | Write API and run lifecycle | 30 Sep | 1 Oct | 8 of 8 | 1,354 tests with the database | 6 of 8; 0, 3 |
| S4 | Orchestrator (Temporal) | 1 Oct | 1 Oct | 7 of 7 | 1,433 tests; 40 on a live Temporal server | 6 of 7; 0, 3 |
| S6 | ERP tool server | 30 Sep | 3 Oct | 8 of 8 | 1,783 tests; 922 live ERPNext tests | 7 of 8; 0, 1 |
| S7 | AI agent pipeline | 1 Oct | 5 Oct | 10 of 10 | 2,137 tests; live model gateway | 4 of 11; 0, 5 |
| S8 | Compiler and sandbox | 1 Oct | 5 Oct | 7 of 7 | 2,354 tests; 263 live | 4 of 7; 0, 2 |
| S9 | Gates G1–G8 | 3 Oct | 5 Oct | 9 of 9 | 2,582 tests | 7 of 10; 2, 3 |
| S14 | Agents, people, API keys | 1 Oct | 6 Oct | 9 of 9 | 2,747 passed, 69 skipped (live-gated), 0 failed; 180 live | 6 of 9; 0, 4 |
| S12 | Credits and billing | 1 Oct | 6 Oct | 8 of 8 | 2,932 passed, 69 skipped (live-gated); 1 environmental failure that passed on re-run; 272 live | 6 of 8; 1, 4 |

Notes:

- Every stage close also has a path audit by a read-only agent (13 path audit files on `main`). Every audit found no
  bypass except S5's, which found three bypasses in provisioner logic that no network path could reach; a follow-up
  unit (S5-U8) fixed them and was merged before the S5 close.
- "Units done" follows the build checklist. S2-U8v, S2-U14 and S5-U8 (merged on 29-30 Sep 2026, before the S2 and S5
  closes) were missing from it until 13 Oct 2026; they are now counted, giving 143 of 202.
- The audit counts some units differently from the checklist (for example S7 counts S7-U9b as its own unit and S9
  counts the F2 fix), so its denominators can differ by one.

## Open stages (in progress, not on `main`)

Measured from each stage branch against `main` on 13 Oct 2026. None of these stages has a close report or a path audit
yet.

| Stage | What | First unit commit | Last commit | Units done | Commits over `main` | Test functions added | Unit reviews added |
|---|---|---|---|---|---:|---:|---:|
| S10 | Approval, promoter, proof: the first real governed write | 1 Oct | 13 Oct | 10 of 11 | 121 | +197 | 10 |
| S11 | Undo and Records | 5 Oct | 13 Oct | 4 of 9 | 130 | +250 | 11 |
| S13 | Assurance | 30 Sep | 13 Oct | 4 of 7 | 140 | +227 | 13 |
| S15 | CA console, WhatsApp, links | 6 Oct | 13 Oct | 5 of 8 | 171 | +317 | 15 |
| S16 | Ops console, security | 1 Oct | 13 Oct | 8 of 10 | 199 | +344 | 18 |
| S17 | Onboarding, data import | 6 Oct | 13 Oct | 6 of 8 | 170 | +316 | 17 |

Stage branches share merged commits (S13, S15 and S17 contain S10 code), so the per-stage commit and test columns
overlap and must not be added up. Counted once, the open stage branches hold 419 commits that are not on `main`.

## Not started

| Stage | What | Units |
|---|---|---|
| DEPLOY | Production on AWS Mumbai | 0 of 12 |
| P2 | Attach mode and Tally migration | 0 of 13 |
| P3 | CRM and Voice | 0 of 10 |
| P4 | Commerce | 0 of 8 |
