# Roadmap: what is not done yet

**Everything on this page is NOT DONE YET.** As of 13 Oct 2026, rung1 is built from plan through 12 of 18 stages,
with 143 of 202 units (71%) and 57 of 105 steps done. Nothing is deployed and nothing runs in production. Items are
listed in the order the plan runs them.

## 1. Finish the open stages (16 units in progress)

| Stage | Done | Remaining units (all in progress, not yet merged) | Close at step |
|---|---:|---|---:|
| S10 Approval, promoter, proof | 10 of 11 | S10-U11 golden trace filled in | 52 |
| S11 Undo and Records | 4 of 9 | S11-U2 fast undo (restore the pre-promotion snapshot); S11-U4 not-reversible changes routed to a person; S11-U5 undo endpoints; S11-U6 CSV exports; S11-U9 console wiring for Records | 59 |
| S13 Assurance | 4 of 7 | S13-U5 erasure as an approved, logged job; S13-U6 audit, coverage, audit-pack and privacy endpoints; S13-U7 console wiring | 56 |
| S15 CA console, WhatsApp, links | 5 of 8 | S15-U5 CA audit pack across clients; S15-U6 WhatsApp channel adapter; S15-U8 console wiring | 66 |
| S16 Ops console, security | 8 of 10 | S16-U8 release checklist, protocol watch, design partners; S16-U10 ops console wiring | 72 |
| S17 Onboarding, data import | 6 of 8 | S17-U5 opening balances; S17-U8 onboarding wiring | 75 |


## 2. S10: the first real governed write (not done yet)

S10 is the stage where the pieces built so far are joined into one real path: a request is planned, built in the
sandbox, gated, decided by the kernel, approved by a named person, published by the promoter to a live development
site, recorded and metered. Until S10 closes, rung1 has not made a real governed write end to end. Most of the 7 Oct
audit's remaining fix items land here (joining the kernel's decision and seal calls, removing test fakes from the
workflow, routing every refusal through the kernel).

## 3. Close the six open stages (not done yet)

Each close needs: every unit reviewed and merged; a path audit by a read-only agent; the full test suite with the
database and live ERPNext tests; the invariant checks at zero failures; a close report; and a merge into `main`.
Order: S10 first, then S13, S11, S15, S16 and S17 (steps 52 to 75).

## 4. Two-person review of safety-critical units (not done yet)

Units in the no-vibe zone (the kernel, ERP write tools, inverse assembly, promoter, credential broker, meter reconcile,
staff access, parts of the harness) were merged under my standing approvals with "two-person review pending". On 13
Oct 2026 I decided to keep building and to name the two reviewers before going live. This must be done before
production.

## 5. Production deployment on AWS Mumbai (steps 75 to 79, not started)

| Unit | What | Step |
|---|---|---:|
| D1 | Container images | 75 |
| D3 | AWS infrastructure | 75 |
| D2 | CI/CD pipeline | 76 |
| D4 | Production databases | 76 |
| D6 | Production sign-in (Keycloak) | 76 |
| D8 | ERPNext hosting pools | 76 |
| D10 | Monitoring and alerts | 76 |
| D5 | Temporal on the cluster | 77 |
| D7 | Secrets and keys | 77 |
| D9 | Sandbox pool | 77 |
| D11 | Backups and disaster recovery | 77 |
| D12 | Security review and go-live | 78 |

Step 79 closes deployment. Known prerequisites already recorded by the path audits include hardened access to Temporal
(mutual TLS and a per-namespace authoriser) before any real promotion.

Only when D12 is complete and the service is live will these pages say "in production".

## 6. Later phases (not started)

| Phase | What | Units | Steps |
|---|---|---:|---|
| P2 | Attach mode (connecting rung1 to an ERP the customer already runs, with drift detection) and Tally migration with a trial-balance equality gate and CA sign-off | 13 | 80–91 |
| P3 | CRM (Twenty, Zoho CRM) and Voice | 10 | 91–98 |
| P4 | Commerce (Shopify, WooCommerce, ONDC seller) | 8 | 98–105 |

## Summary of what remains

| | Units |
|---|---:|
| In progress in open stages | 16 |
| Production deployment | 12 |
| Later phases P2–P4 | 31 |
| **Total not marked done** | **62** |
