# rung1: governed AI changes for Indian businesses on ERPNext

**Owner's portfolio record.** This repository describes my work on rung1, a product of elan1. It contains documents
only: no source code, no secrets and no customer data. Every number on these pages was measured from the project's
git history, its PostgreSQL catalogue or its generated inventory report on 13 Oct 2026; none is estimated.

> **Status (13 Oct 2026)**
>
> Built from plan through 12 of 18 stages (S0–S9, S12, S14 closed), 143 of 202 units (71%), 57 of 105 steps; now
> completing S10, S11, S13, S15, S16, S17; production deployment on AWS Mumbai (steps 75–79) not started yet.
> This page will say "in production" only when that is true.
>
> Still to be completed: 62 units (16 in progress, 43 not started), the six open stage closes, the two-person review
> of safety-critical units, production deployment (12 units, D1–D12) and the later phases P2–P4. See
> [ROADMAP.md](ROADMAP.md).

## What rung1 is

A business owner or accountant on ERPNext asks for a change in plain language ("add a field for the transporter's
GST number", "make a report of overdue invoices by customer"). rung1 turns that request into a governed change:

1. **Ask** in plain language.
2. **AI plans** the change and asks clarifying questions.
3. **Built in a sandbox**: a copy of the business's own ERPNext site, never the live one.
4. **Gates test it**: up to eight deterministic checks (ledger intact, GST filed periods untouched, and others).
5. **A deterministic kernel decides**: allow, refuse or send to a person. No AI model is in the decision path.
6. **A named person approves**, and that person can never be the one who asked.
7. **The promoter publishes** the identical tested bundle to the live site; it is the only component that can write
   there.
8. **A proof record and exactly one meter event** are written for every outcome, including refusals (charged at zero).
9. **Undo for 30 days.**

The idea: AI can propose and build, but only deterministic code and a named human can let a change reach a company's
books.

## My role

- **Designed the plan**: 18 build stages (S0–S17), then a production deployment stage and three later phases
  (P2–P4): 202 units in 105 ordered steps, with twelve safety invariants and the check that enforces each one.
- **Directed and governed a team of AI agents** (Claude Code agents): writer agents built each unit, a separate
  read-only adversarial reviewer agent attacked it, path-auditor agents checked each stage for bypasses, and a
  13-auditor conformance audit checked the whole build against the plan.
- **Made every architecture decision and approval**: accepted all 12 architecture decision records, gave the rulings
  on open questions, granted step-by-step standing approvals, and decided each stage close.
- **AI agents wrote the code under my rules.** The rules were enforced by hooks and CI checks, not by trust: a
  protected-path hook blocks writes to safety-critical folders until the governing decision record is accepted by me.

How the agents were run: [AGENTIC-AI-METHOD.md](AGENTIC-AI-METHOD.md).

## Headline numbers (measured 13 Oct 2026)

| Measure | Value |
|---|---|
| Build period so far | 26 Sep 2026 (first commit) to 13 Oct 2026 |
| Stages closed into `main` | 12 of 18 build stages |
| Units done | 143 of 202 (71%) |
| Steps fully done | 57 of 105 |
| Lines of code on `main` (source + tests + generated) | 1,36,454 |
| Lines of code including open stages, counted once | about 1.73 lakh (1,36,454 + 36,509 net in progress) |
| Commits | 852 on `main`; 1,323 across all branches (each counted once) |
| Database tables | 45 on `main`; 93 with open stages |
| API routes (`/v1`) | 86 on `main`; 152 with open stages |
| Database migrations | 33 on `main`; 60 with open stages |
| Test files / test functions on `main` | 185 / 1,667 |
| Largest full test run at a stage close | 2,932 passed (S12 close, 6 Oct 2026) |
| Console routes / screen files | 127 / 248 |
| Architecture decision records | 12, all accepted by me |
| Unit specs / independent unit reviews | 159 / 147 (across `main` and open stages) |
| Production deployment | not started (0 of 12 units) |

Full tables, with "done" and "still to do" side by side: [BY-THE-NUMBERS.md](BY-THE-NUMBERS.md).

## Contents

| Page | What it covers |
|---|---|
| [ARCHITECTURE.md](ARCHITECTURE.md) | The flow, the components, the twelve safety invariants and the technology stack |
| [AGENTIC-AI-METHOD.md](AGENTIC-AI-METHOD.md) | How a team of AI agents was planned, run, reviewed and audited |
| [BUILD-LOG.md](BUILD-LOG.md) | Dated timeline and one row per stage: dates, units, tests, audit verdict |
| [BY-THE-NUMBERS.md](BY-THE-NUMBERS.md) | Every measured figure, done and still to do |
| [DECISIONS.md](DECISIONS.md) | The architecture decisions and owner rulings, in plain words |
| [LESSONS.md](LESSONS.md) | Real problems met during the build and how each was solved |
| [ROADMAP.md](ROADMAP.md) | What is not done yet, in order |
| [screenshots/README.md](screenshots/README.md) | Console screens to capture later |

## Confidentiality

rung1 is elan1's product. This repository describes the owner's work on it and contains no code, no secrets, no
pricing and no customer data. The plan documents, prompt library and source code remain private to elan1.
