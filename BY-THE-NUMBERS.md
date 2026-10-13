# By the numbers

Every figure here is measured, not estimated. Sources: the project's generated inventory report (13 Oct 2026, 14:06
IST), which reads committed git trees and the PostgreSQL catalogue; the build checklist (updated 13 Oct 2026); and
`git` commands run on 13 Oct 2026. Lakh notation is used for large line counts (1,00,000 = one lakh).

**Reading the columns.** *On `main`* is work merged at a stage close. *In progress* is work on the six open stage
branches (S10, S11, S13, S15, S16, S17), reviewed unit by unit but not yet closed. *Still to do* is work not yet
finished.

## Progress

| Measure | Done | In progress | Still to do | Total |
|---|---:|---:|---:|---:|
| Build stages closed (S0–S17) | 12 | 6 open | 6 closes | 18 |
| Units (all phases) | 143 (71%) | 16 | 43 not started | 202 |
| Steps | 57 fully done | 17 partly done | 31 not started | 105 |
| Production deployment units (D1–D12) | 0 | 0 | 12 | 12 |
| Later phases P2–P4 (units) | 0 | 0 | 31 | 31 |

All 43 units not started are deployment (D1-D12) and the later phases (P2-P4).

## Units by stage

| Stage | What | Done | Still to do | State |
|---|---|---:|---:|---|
| S0 | Harness, rules, contracts | 9 | 0 | Closed |
| S1 | Control plane: tenants, sign-in, audit | 8 | 0 | Closed |
| S2 | elan1 kernel | 13 | 2 (see note above) | Closed |
| S3 | Write API and run lifecycle | 8 | 0 | Closed |
| S4 | Orchestrator (Temporal) | 7 | 0 | Closed |
| S5 | ERPNext foundation | 7 | 1 (see note above) | Closed |
| S6 | ERP tool server | 8 | 0 | Closed |
| S7 | AI agent pipeline | 10 | 0 | Closed |
| S8 | Compiler and sandbox | 7 | 0 | Closed |
| S9 | Gates G1–G8 | 9 | 0 | Closed |
| S10 | Approval, promoter, proof: first real governed write | 10 | 1 in progress | Open |
| S11 | Undo and Records | 4 | 5 in progress | Open |
| S12 | Credits and billing | 8 | 0 | Closed |
| S13 | Assurance | 4 | 3 in progress | Open |
| S14 | Agents, people, API keys | 9 | 0 | Closed |
| S15 | CA console, WhatsApp, links | 5 | 3 in progress | Open |
| S16 | Ops console, security | 8 | 2 in progress | Open |
| S17 | Onboarding, data import | 6 | 2 in progress | Open |
| DEPLOY | Production on AWS Mumbai | 0 | 12 | Not started |
| P2 | Attach mode and Tally | 0 | 13 | Not started |
| P3 | CRM and Voice | 0 | 10 | Not started |
| P4 | Commerce | 0 | 8 | Not started |
| **Total** | | **143** | **59** | |

## Code and system size

| Measure | On `main` | Added by open stages (in progress) | Combined |
|---|---:|---:|---:|
| Lines of code (source + tests + generated) | 1,36,454 | +36,509 net, counted once | 1,72,963 (about 1.73 lakh) |
| of which source lines | 97,869 | | |
| of which test lines | 37,527 | | |
| of which generated lines (TypeScript client) | 1,058 | | |
| Code files | 903 | | |
| Documentation (Markdown lines, not counted as code) | 60,191 in 294 files | | |
| Database tables (three databases) | 45 | +48 | 93 |
| Database columns | 414 | | |
| API routes (`/v1`) | 86 | +66 | 152 |
| Database migrations | 33 | +27 | 60 |
| Console routes | 127 | +0 | 127 |
| Screen files | 248 | | 248 |
| MCP tools (ERP) | 20 | +0 | 20 |
| Gates with a module | 5 | +5 | 10 |
| Temporal workflows / activities | 4 / 29 | | |
| Model aliases through the gateway | 6 | | |
| Frozen error and refusal codes | 142 | | |

By language on `main`: Python 69,091 lines; TSX 44,551; TypeScript 16,077; CSS 3,681; YAML 1,672; other 1,382.

How the open-stage lines were counted: the open stages share merged commits, so adding each stage's own diff would
count shared code twice (that sum is +74,558). Counted once across all open stages, their code adds 41,772 lines and
removes 5,263: net +36,509.

## Quality and safety

| Measure | On `main` | Added by open stages (in progress) |
|---|---:|---:|
| Test files | 185 | +58 (243 combined) |
| Test functions (as written, before parametrisation) | 1,667 | +197 to +344 per stage (overlapping; not summed) |
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
| Commits | 852 | 1,323 distinct across every branch, including 419 on the open stage branches that are not on `main` |
| Unit specs | 118 | 159 |
| Independent unit review cases | 107 | 147 |
| Stage close reports | 12 | 12 |
| Stage path audits | 13 | 13 |
| Architecture decision records | 7, all accepted | 12, all accepted |
| Conformance audit (7 Oct): units audited | 142 | |
| Conformance audit: blockers / majors found | 6 / 49 | |
| Audit fix and ruling units merged into `main` (12–13 Oct) | 9 | |
| Calendar days from first commit to this report | 18, inclusive (26 Sep to 13 Oct 2026) | |

## Not measured

- Test pass counts for the open stages: their full suites run at each stage close, which has not happened yet.
- Uncommitted work in worktrees: only committed trees are counted.
- Production figures of any kind: nothing is deployed.
