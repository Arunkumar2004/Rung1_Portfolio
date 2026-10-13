# By the numbers

Every figure here is measured, not estimated. Sources: the project's generated inventory report (13 Oct 2026, 15:32
IST), which reads committed git trees and the PostgreSQL catalogue; the build checklist (updated 13 Oct 2026, after
the report); and `git` commands run on 13 Oct 2026. Lakh notation is used for large line counts (1,00,000 = one lakh
= 100,000).

**Reading the columns.** *On `main`* is work merged at a stage close. *In progress* is work on the six open stage
branches (S10, S11, S13, S15, S16, S17), reviewed unit by unit but not yet closed. *Still to do* is work not yet
finished.

## Progress

| Measure | Done | In progress | Still to do | Total |
|---|---:|---:|---:|---:|
| Build stages closed (S0–S17) | 12 | 6 open | 6 closes | 18 |
| Units (all phases) | 148 (73%) | 11 | 43 not started | 202 |
| Steps | 60 fully done | 14 partly done | 31 not started | 105 |
| Production deployment units (D1–D12) | 0 | 0 | 12 | 12 |
| Later phases P2–P4 (units) | 0 | 0 | 31 | 31 |

All 43 units not started are deployment (D1–D12) and the later phases (P2–P4).

## Units by stage

| Stage | What | Done | Still to do | State |
|---|---|---:|---:|---|
| S0 | Harness, rules, contracts | 9 | 0 | Closed |
| S1 | Control plane: tenants, sign-in, audit | 8 | 0 | Closed |
| S2 | elan1 kernel | 15 | 0 | Closed |
| S3 | Write API and run lifecycle | 8 | 0 | Closed |
| S4 | Orchestrator (Temporal) | 7 | 0 | Closed |
| S5 | ERPNext foundation | 8 | 0 | Closed |
| S6 | ERP tool server | 8 | 0 | Closed |
| S7 | AI agent pipeline | 10 | 0 | Closed |
| S8 | Compiler and sandbox | 7 | 0 | Closed |
| S9 | Gates G1–G8 | 9 | 0 | Closed |
| S10 | Approval, promoter, proof: first real governed write | 10 | 1 in progress | Open |
| S11 | Undo and Records | 6 | 3 in progress | Open |
| S12 | Credits and billing | 8 | 0 | Closed |
| S13 | Assurance | 4 | 3 in progress | Open |
| S14 | Agents, people, API keys | 9 | 0 | Closed |
| S15 | CA console, WhatsApp, links | 6 | 2 in progress | Open |
| S16 | Ops console, security | 9 | 1 in progress | Open |
| S17 | Onboarding, data import | 7 | 1 in progress | Open |
| DEPLOY | Production on AWS Mumbai | 0 | 12 | Not started |
| P2 | Attach mode and Tally | 0 | 13 | Not started |
| P3 | CRM and Voice | 0 | 10 | Not started |
| P4 | Commerce | 0 | 8 | Not started |
| **Total** | | **148** | **54** | |

## Code and system size

| Measure | On `main` | Added by open stages (in progress) | Combined |
|---|---:|---:|---:|
| Lines of code (source + tests + generated) | 1,36,758 | +38,275 net, counted once | 1,75,033 (about 1.75 lakh; 175,033) |
| of which source lines | 97,889 | | |
| of which test lines | 37,811 | | |
| of which generated lines (TypeScript client) | 1,058 | | |
| Code files | 904 | | |
| Documentation (Markdown lines, not counted as code) | 60,338 in 296 files | | |
| Database tables (three databases) | 45 | +47 | 92 |
| Database columns | 414 | | |
| `/v1` API routes (write API and kernel) | 89 | +68 | 157 |
| of which write API routes | 86 | +68 | 154 |
| Database migrations | 33 | +27 | 60 |
| Console routes | 127 | +0 | 127 |
| Console screens (catalogued) | 124 | +0 | 124 |
| Screen files | 248 | | 248 |
| MCP tools (ERP) | 20 | +0 | 20 |
| Gates with a module (of G1–G8) | 5 (G1, G2, G3, G4, G8) | +0 | 5 |
| Temporal workflows / activities | 4 / 29 | | |
| Model aliases through the gateway | 6 | | |
| Frozen error and refusal codes | 142 | | |
| Architecture decision records | 7 | +5 | 12 |

By language on `main`: Python 69,384 lines; TSX 44,551; TypeScript 16,088; CSS 3,681; YAML 1,672; other 1,382
(shell, TOML, SQL, JavaScript, HTML).

How the open-stage lines were counted: the open stages share merged commits, so adding each stage's own diff would
count shared code twice. Counted once across all open stages (every commit on a stage branch that is not on `main`),
their code adds 43,612 lines and removes 5,337: net +38,275.

## Quality and safety

| Measure | On `main` | Added by open stages (in progress) |
|---|---:|---:|
| Test files | 186 | +61 (247 combined) |
| Test functions (as written, before parametrisation) | 1,676 | +197 to +518 per stage (overlapping; not summed) |
| Largest full suite at a stage close | 2,932 passed, 69 skipped (live-gated), 1 environmental failure that passed on re-run (S12 close, 6 Oct) | not yet run at a close |
| Live ERPNext tests at the S6 close | 922 passed | |
| Seeded violations (checks proven able to fail) | 45 | |
| Invariant check modules | 7 | |
| Tables with row-level security enabled and forced | 41 of 45 | |
| Row-level security policies | 50 | |
| Insert-only tables (by grant) | 27 | |
| Non-superuser roles that can bypass row-level security | 0 | |
| Check constraints / foreign keys / indexes | 134 / 62 / 89 | |

The test suite was not re-run for the inventory report; test pass counts are quoted from the stage close records.

## Process and records

| Measure | On `main` | Across `main` and open stages |
|---|---:|---:|
| Commits | 856 | 1,293 distinct, including 437 on the open stage branches that are not on `main` |
| Unit specs | 119 | 162 |
| Independent unit review cases | 107 | 149 |
| Stage close reports | 12 | 12 |
| Stage path audits | 13 | 13 |
| Other review records (decision record reviews, audit fixes, rulings) | 19 | |
| Architecture decision records | 7, all accepted | 12, all accepted |
| Conformance audit (7 Oct): units audited | 142 | |
| Conformance audit: blockers / majors found | 6 / 49 | |
| Audit fix and ruling units merged into `main` (12–13 Oct) | 10 | |
| Calendar days from first commit to this report | 18, inclusive (26 Sep to 13 Oct 2026) | |

## Not measured

- Test pass counts for the open stages: their full suites run at each stage close, which has not happened yet.
- Uncommitted work in worktrees: only committed trees are counted.
- Production figures of any kind: nothing is deployed.
