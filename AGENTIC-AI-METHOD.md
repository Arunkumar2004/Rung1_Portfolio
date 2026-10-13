# The agentic AI method

How rung1 was built: one owner setting the plan, the rules and the decisions, and a team of AI agents (Claude Code)
doing the writing, reviewing and auditing under those rules. Counts are measured from the repository on 13 Oct 2026.

## 1. The core rule: if a rule matters, a machine enforces it

On 27 Sep 2026, before any backend code existed, I adopted a written build rulebook and made it binding on every
agent. Its central idea is that a rule written only in prose will eventually be broken by an agent, so every rule that
matters must either stop a tool call or fail a check. In practice that meant:

- **Verify, don't assert.** Every unit reports each "done when" line as passed, failed or not measurable, with the
  command and its output. "Looks fine" is not an answer. A skipped test counts as not done.
- **Failure shape first.** Refusal and error cases, with their codes, are written before the happy path.
- **Fixture before code.** A failing test or a seeded violation exists before the implementation, and is shown failing
  once before it passes.
- **Small units.** One unit per agent session, a diff of about 200 lines; anything larger is split before it starts.
- **Zones.** *Free* (screens, scripts, documents), *guided* (tools, checks, configuration, metering: plan and fixtures
  first) and *no-vibe* (the promoter, the credential broker, rollback, the kernel, and the harness itself: drafted only
  against a decision record I have accepted, and marked for review by two people before launch).

## 2. The harness: hooks and checks that agents cannot talk their way past

| Mechanism | What it does |
|---|---|
| Session-start hook | Prints the invariants into every agent session. |
| Pre-tool-use hook | Blocks any write to a protected path (the kernel, promoter, credential broker, rollback, the harness and the CI checks) unless the governing decision record says `Status: Accepted`. Only I set that line. |
| Post-tool-use hook | Flags unsourced values written into backend source, such as a hard-coded id or a typed-in figure. |
| Subagent-stop hook | The adversarial reviewer cannot finish until its review case for the unit exists. |
| `protected-paths.json` | The machine list of protected folders and their governing decision records, with one exception: new seeded violations may be appended by any unit. |
| Invariant checks (`uv run check`) | Seven check modules: import contracts (I1, I3), naming and claim lint (I12), grants (I5, I6), row-level security on every migration (I6), workflow determinism, and a protected-path diff check; then the seeded-violation suite. |
| Seeded violations | 45 deliberate violations on `main` (for example a gate module that imports a model client, or a deleted meter event). Each must turn the check red for the right reason, which proves the check can fail. |
| Pre-push gate | A git hook runs the full check with CI settings, so a check that cannot be measured stops the push. |
| Hook tests | 93 test functions over the hooks themselves. |

## 3. Roles in the agent team

| Role | Who | Output |
|---|---|---|
| Plan, rules, decisions, approvals | Me (the owner) | The plan, 12 accepted decision records, rulings, standing approvals by step range, stage close decisions |
| Orchestrator | A lead Claude session | Dependency graph of units, worktrees, merges, preflight, verification in git |
| Writer | One agent per unit, on its own branch and worktree | Spec, failing fixture, code, tests, evidence |
| Adversarial reviewer | A separate read-only agent in a fresh session | One review case per unit: findings ranked blocker, major, minor; accept or reject |
| Path auditor | A read-only agent per stage | A list of every new route, workflow step, command or job, judged safe, bypass or unclear |
| Conformance auditors | 13 read-only agents, each owning one area | The 7 Oct 2026 audit |

The writer never reviews its own work. A rejected unit goes back to its writer, and the fix is confirmed before the
merge.

## 4. Measured output of the method

| Artefact | Count | Where |
|---|---|---|
| Unit specs | 119 on `main`; 162 across `main` and open stages | `specs/` |
| Independent unit review cases | 107 on `main`; 149 across `main` and open stages | `docs/reviews/<unit>.md` |
| Stage close reports | 12 | `docs/reviews/stage-*.md` |
| Stage path audits | 13 on `main` | `docs/reviews/stage-*-paths*.md` |
| Other review records (decision record reviews, audit fixes, rulings) | 19 on `main` | `docs/reviews/` |
| Architecture decision records | 12, all accepted by me | `docs/adr/` |
| Frozen error and refusal codes | 142 | the code register |

## 5. Running agents in parallel

- **Dependency graphs.** The 202 units are ordered into 105 steps; each step lists the units that can run together and
  the stage gates (start, close) it contains. Workflows were scripted from that graph so a unit starts only when the
  units it depends on are merged.
- **Up to 8 agents at once.** Concurrency is capped by a semaphore: 4 agents by default, 8 when I asked for speed.
- **Halt on limit.** Every workflow script carries a global halt: after two empty agent results in a row it stops
  calling agents and returns what finished. This rule came from two real failures (see [LESSONS.md](LESSONS.md)).
- **Fresh runs, not resumes.** After a break, a new workflow is started from the git state instead of resuming a cached
  run, and every claimed result is checked in git (branch exists, review file present, fix commits present, merged).
- **Strict ownership.** One writer per file; unit branches cut from stage branches; units reach a stage branch only
  through a merge window, and a stage reaches `main` only at its close.
- **Quoted instructions.** Each agent prompt opens by quoting my actual build request, because agents otherwise tend to
  treat a casual chat message as their new task.

## 6. Environment discipline

- **Preflight before every run**: free disk space, required services healthy (ERPNext bench, Temporal), git state of
  every unit the run will touch, and every environment key present in each worktree.
- **Per-stage databases**: each stage worktree has its own control, kernel and proof databases, so a migration on one
  stage cannot break another stage's tests. The inventory measured 18 such databases for the six open stages plus three
  for `main`.
- **Live tests, not only mocks**: stage closes ran live suites against a real ERPNext bench (922 live tests at the S6
  close), a live Temporal dev server (40 orchestrator tests at the S4 close) and the live model gateway (S7 close).
- **Persistent memory**: the lead session keeps a set of memory files (19 indexed entries) recording rules learned
  from failures, my rulings and approvals, so a new session does not repeat an old mistake.
- **Browser sweep**: wiring changes are checked in a real browser at 375, 768 and 1440 pixels.

## 7. Auditing the auditors: the 7 Oct 2026 conformance audit

After twelve stages had closed, I commissioned a full conformance audit rather than trusting the stage closes.

- **Method:** 13 read-only auditors, each owning one area (groups of stages, the API against the endpoint appendix,
  the invariants, the decision records, frontend wiring, layout and migrations, and the open stages). Each ran the
  tests it named and read the code against the plan.
- **Scope:** 142 units audited (108 in closed stages, 34 in open stages).
- **Result:** 81 of 108 closed-stage units matched the plan; 6 blockers and 49 majors were found and de-duplicated
  into a 50-item fix list.
- **Most important finding:** every test on `main` passed, yet the governed pipeline was not joined end to end; some
  steps were still stubs or test fakes. Passing tests were not the same as a working whole.
- **What happened next:** I gave rulings on the nine items only the owner could decide, and fix units were built,
  reviewed and merged into `main` on 12 and 13 Oct 2026 (S0-F1, S2-F1, S4-F1, S7-F1, S9-F3, S12-F1, S12-F2, and the
  ruling units RUL-G4, RUL-CODES and RUL-DEMO), plus two revised decision records. The remaining items are assigned to the
  open stages, chiefly S10.

## 8. What is still owed by the method

The no-vibe units merged under my standing approvals still carry "two-person review pending". I chose (13 Oct 2026)
to keep building and to decide on the two reviewers before going live; this is listed in [ROADMAP.md](ROADMAP.md).
