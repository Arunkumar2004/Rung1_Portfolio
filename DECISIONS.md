# Decisions

Plain-language summaries of the architecture decision records (ADRs) and the owner rulings that shaped rung1. Each ADR
was drafted by an agent with `Status: Proposed`, attacked by an independent reviewer agent, revised, and then accepted
by me; no agent may set the acceptance line. All 12 records listed here are accepted. These are my own summaries,
not the text of the records.

## How decisions were made

- **A decision record before safety-critical code.** The kernel, promoter, credential broker, rollback, the ERP write
  tools and the harness cannot be written until their record is accepted. A hook enforces this for protected folders.
- **Failure cases first.** Most records open with a table of every way the component can fail, what each failure
  writes and which code it returns, before describing normal behaviour.
- **Revisions, not rewrites.** When an accepted record turned out to be silent or self-contradictory, a short revision
  record was written ("-r1") rather than editing the original.

## Architecture decision records

| Record | Decision in plain words | Why |
|---|---|---|
| ADR-0001 Runtime and environment | Use Temporal for workflows, with every step safe to repeat. Pin ERPNext and Frappe to version 15 across the whole fleet. A sandbox is a clone of the tenant's own site made by backup and restore, never sharing a writable store with live. This record also governs the harness folders. | Durable, replayable workflows suit long approvals; one ERP version avoids per-tenant surprises; testing on a true copy of the customer's site makes gate results meaningful. |
| ADR-0002 API conventions | One versioned JSON API with bearer tokens; tenant and actor taken only from the token; an idempotency key on every state-changing request; a governed "no" is a normal answer with a code, a rule and what would change it, metered at zero; errors use a standard problem format with a registered code; sensitive actions need a fresh step-up check. | Fixing these once stopped every later stage from re-arguing them, and made refusals a first-class product feature rather than an error. |
| ADR-0002-r1 Role refusals are errors | A caller whose role does not allow an action gets an HTTP 403 error, not a governed refusal. | The audit found the same code answered in two shapes; a permission problem is not a business decision and should not produce a proof record. |
| ADR-0003 Promoter and credential broker | A nine-step publishing protocol: lock the site; re-read the live schema and confirm the stored bundle bytes match what was tested; open the promotion at the broker, stop other writes, obtain a single-use token and take a snapshot; apply the calls once each; run migrations; re-run gates G1–G3 on the live site; restore from the snapshot if anything failed; release the lock; and only then hand off to the kernel to write the record. Every failure before the first write ends with no write; failure after it ends in an automatic restore. Hardened workflow access is a precondition of the first live promotion. | The live site is the customer's books. Exactly one writer, a token that works once, and a written answer for every failure keep a bad change from landing half-done. |
| ADR-0004 elan1 kernel (L1) | The kernel is the only component that can say allow, refuse or escalate. Everything above it reaches it through one port. Each decision walks an ordered ladder of checks, writes one record in a hash-linked chain and exactly one meter event, and the whole chain can be re-verified by recomputing every digest. | Putting all decisions in one small, model-free component makes them auditable and testable, and keeps product code from bypassing them. |
| ADR-0004-r1 Decision read and approval answer | The promoter may read back the exact allowed intent by its write id, and approvals are answered inside the kernel through a fixed order of checks: a human (not an agent), not the requester, holding an approver role, on a permitted channel, with a valid step-up. | Without a read-back the promoter would have to trust workflow input; answering approvals in the kernel keeps "approver is not requester" in one place. |
| ADR-0005 ERP write tools and inverse | The agent's only hands on ERPNext are a closed set of tools. Every write tool returns its own inverse; the undo set is assembled only from those returns; tools never touch a live site except under the promoter's token. | An undo that a model wrote could be wrong in exactly the moment it is needed; an undo built from the ERP's own answers is deterministic. |
| ADR-0006 Rollback | Rollback never holds a writer of its own: a fast undo restores the pre-change snapshot through the promoter, and a forward undo is an ordinary governed change built from the stored inverses. A change marked not reversible is routed to a person. No model anywhere on the path. | One live-writing identity is easier to secure and audit than two; treating undo as a governed change gives it the same gates, approval and record. |
| ADR-0007 Meter reconcile | A nightly two-way comparison: every meter event has a record, every record has exactly one event, every refusal is quantity zero. A break blocks invoicing and raises an alert. The reconciler reads with a select-only role. | Billing must be provable: every rupee on an invoice should trace to a decision record, and refusals must cost nothing. |
| ADR-0007-r1 Spend reads the meter | The customer's spend view may read the meter read-only, and a healthy charge that lands inside a short grace window is not counted as a reconciliation break. | The original record contradicted itself and left the spend screen unable to show real figures. |
| ADR-0008 Erasure | Erasing a person deletes the per-person salt, so salted digests in the proof chain can no longer be matched to that person's data; proof and audit rows are never edited. Erasure is a case that needs approval, and the person who opens it cannot approve it. The record states plainly what erasure does not achieve. | Privacy law requires erasure, but the proof chain must stay verifiable; being exact about the limits avoids overclaiming. |
| ADR-0009 Staff access and break-glass | Staff sign in through a separate realm with multi-factor authentication; a staff token is refused on every tenant route; every ops call is logged before it runs; any staff read of a tenant's records needs a support session, and break-glass access has its own recorded path. | Support staff are the most likely route for a cross-tenant leak; separate identity and logged, approved sessions close it. |

## Owner rulings

Rulings I gave in chat when a record or a review left a question open. Each was then written into a record, a spec
or a code register.

| Date | Ruling | Effect |
|---|---|---|
| 3 Oct 2026 | Only the promoter mints live-site tokens; rollback never mints. | One live-writing identity; rollback goes through the promoter. |
| 3 Oct 2026 | Add dedicated platform failure codes for the promoter's preparation steps (write stop, snapshot, fit test, baseline, live read). | No generic "internal error" for a failed promotion step. |
| 3 Oct 2026 | The tenant's owner or operations head approves every support session, read-only sessions included. | Staff cannot read a tenant's records without the tenant's say. |
| 3 Oct 2026 | An undo that would delete a document's version history stays refused. | Fail closed rather than erase history. |
| 3 Oct 2026 | Print-format templates are allowed under the write-tool allow-list. | A common customer request is in scope. |
| 3 Oct 2026 | The billing reconciliation cut-off reading is accepted. | Unblocked meter reconcile. |
| 13 Oct 2026 | The GST filed-period gate (G4) can never be overridden. | A change touching a filed GST period is always refused. |
| 13 Oct 2026 | The spend view reads the meter (became ADR-0007-r1). | Real spend figures on the console. |
| 13 Oct 2026 | A missing role is a 403 error (became ADR-0002-r1). | One shape per code. |
| 13 Oct 2026 | The gate author for the gate units is the project owner, built by AI agents under the owner's direction. | Honest authorship record. |
| 13 Oct 2026 | Stale development databases are backed up and dropped. | Checks read the right databases. |
| 13 Oct 2026 | Register a code for a failed seal step. | Every promotion failure has a registered code. |
| 13 Oct 2026 | Demo values on gate pages stay, with a visible "Demo" label, until S10 makes them real. | No screen presents sample data as real. |
| 13 Oct 2026 | Two-person review of safety-critical units: keep them marked "review pending", keep building, and decide the reviewers before going live. | Recorded as an open item before deployment. |
| 13 Oct 2026 | Accepted ADR-0004-r1; approved the secrets-store and secret-scanner downloads. | Unblocked the credential broker and promoter work in S10. |
