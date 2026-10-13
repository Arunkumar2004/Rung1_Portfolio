# Lessons

Real problems met while running a team of AI agents on a safety-critical build, and what was changed so each one would
not happen again. Each lesson was recorded in the lead session's persistent memory or the build handover notes on the
day it happened, and became a standing rule.

## 1. A wrong merge rule combined code it should have chosen between

**What happened (1 Oct 2026).** To speed up merges, a helper script resolved conflicts by keeping both sides. That is
safe for lists of routes or dependencies, but it was also applied to code in two stage branches. The result was
duplicated logic and two competing database migration heads.

**What changed.** A dedicated repair workflow fixed both branches. Rule: "keep both" is allowed only for router and
dependency lists, never for code or tests; parallel units that each add a migration are re-chained into one linear
history at the merge.

## 2. Usage limits and the halt-on-limit rule

**What happened.** On 6 Oct 2026 a workflow with 96 agent calls hit the five-hour usage limit and 34 agents returned
empty results; the script then crashed on the missing results. On 7 Oct a later run reached the weekly limit, and the
script kept launching agents after the limit, so 322 agent calls failed. Work paused until the limit reset.

**What changed.** Every workflow script now has a semaphore cap (4 agents by default, 8 only on request) and a global
halt: after two empty results in a row it stops calling agents and returns what has finished. Reviewed work is merged
before any large run, so a limit loses as little as possible. A running job is never stopped just to change its agent
count, because that cut off agents mid-task too.

## 3. Disk space

**What happened.** Each unit ran in its own git worktree with its own Python environment, and the build disk ran
short of space.

**What changed.** A preflight script checks free space before any run (at least 5 GB). Finished worktrees' Python
environments are moved to a separate cache drive, never deleted; package caches and test temporary files were moved
off the system drive entirely.

## 4. Shared databases became per-stage databases

**What happened (1 Oct 2026).** All worktrees shared one set of development databases. A unit that added a kernel
migration moved the shared database ahead of every other branch, and other worktrees' tests failed because they could
not find that revision.

**What changed.** Every stage now has its own control, kernel and proof databases, created and migrated by one script;
a new unit worktree copies its stage's settings. Fresh databases are seeded before tests run, and new environment keys
from a unit are copied into the stage's merge worktree before its suite runs.

## 5. Services stopped when the shell that started them ended

**What happened.** Long-running services started as background shell tasks (the ERPNext bench under WSL and the
Temporal dev server) were killed at the task's time limit or when the launching session ended, and test runs then
failed for reasons unrelated to the code.

**What changed.** Services are started as detached, hidden processes, and the preflight checks that each is healthy
(the ERPNext site answers, Temporal reports serving) before any agent starts.

## 6. A teammate's push was merged, not overwritten

**What happened (13 Oct 2026).** A teammate pushed a tracker-sync workflow directly to the GitHub `main` branch while
local `main` had moved on.

**What changed.** Before every push: fetch, inspect any commits on the remote that are not local, merge them in, push,
and verify local and remote match. Force-pushing is never used. The same rule had already been applied on 3 Oct, when
GitHub's starter commit was joined rather than overwritten.

## 7. A green run is not a passing run

**What happened (29 Sep 2026).** Unit worktrees did not receive the git-ignored environment file, so 31 database tests
(migrations, row-level security, sign-in) were silently skipped in every writer and reviewer run. Once the file was
copied in, a real failure appeared. The same day, a reviewed bench-setup script failed four times on its first real run,
because writer and reviewer had only syntax-checked it.

**What changed.** Every worktree gets the environment file; a large skip count counts as a failed check; anything that
installs, provisions or touches an external system must be run for real before it is called done.

## 8. "Live" must be proved, not assumed

**What happened (29 Sep 2026).** A review case and a fix were lost on a detached checkout, and a screen was reported as
reading the tenant name from the API when it still read mock data.

**What changed.** After every batch, each unit branch is checked for its review file and fix commits before merging.
To prove a screen is live, the value is changed in the database and the screen is seen to change.

## 9. Agents treat the latest chat message as their task

**What happened (28 Sep 2026).** Several workflow agents read a casual status question from me as their own
instruction and only reported status; one step produced nothing and a merge silently never happened.

**What changed.** Deterministic work (worktrees, merges, audits) is done directly by the lead session with git and
scripts. Every agent prompt opens by quoting my actual build request. Every claimed merge is checked in git afterwards.

## 10. Resuming a workflow after a break is unreliable

**What happened.** After a session break, a resumed workflow replayed cached results out of order: merge agents
reported work as not merged, and writers' branches did not exist.

**What changed.** After any break a fresh workflow is started from the git state, with new prompt text; cached results
are never trusted.

## 11. Smaller environment traps

- Reviewer agent types could not save their review files from inside unit worktrees, because the hook resolves paths
  against the main checkout; inside worktrees a general agent writes the one review file, and path audits return text
  that the lead session commits.
- Downloads and installs that I approved were refused by sub-agents when the approval was only relayed in a prompt;
  the lead session now performs approved downloads itself.
- Windows line endings in workflow scripts were read as control characters; scripts are written with Unix line
  endings.
- On this machine, `bash` in PowerShell resolves to WSL, which broke a set of tests; full suites run from Git Bash.

## 12. Every test passed, and the system was still not joined up

**What happened (7 Oct 2026).** The 13-auditor conformance audit found that every suite on `main` passed, yet several
steps of the governed pipeline were still stubs or test fakes, a default gate runner wrote fake passes, and one gate
that must never be overridden could be.

**What changed.** The audit became a ranked fix list; nine fix and ruling units were merged on 12 and 13 Oct 2026, and
the rest are assigned to the open stages. The lesson for the method: stage-by-stage tests need a whole-path check
alongside them, which is what the golden trace in S10 is for.
