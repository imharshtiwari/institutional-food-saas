# Phase Tracker

**Status:** In Progress · **Last updated:** 2026-10-25
**Related:** [AI_DEVELOPMENT_RULES](AI_DEVELOPMENT_RULES.md) (status labels), [ARCHITECTURE](ARCHITECTURE.md) (decision register)

## 1. Phases

| Phase | Scope | Status |
|---|---|---|
| 0 | Project foundation and documentation | In Progress |
| 1 | Frontend design system and application shell | Planned |
| 2 | Complete frontend using realistic, labeled mock data | Planned |
| 3 | Backend foundation and database | Planned |
| 4 | Connect frontend to backend | Planned |
| 5 | Meal plans, entitlements and attendance | Planned |
| 6 | Menus, kitchen operations and student experience | Planned |
| 7 | Inventory, procurement and waste | Planned |
| 8 | Contracts and finance | Planned |
| 9 | University integrations and notifications | Planned |
| 10 | Analytics and AI assistance | Planned |
| 11 | Production readiness, load testing and pilot | Planned |

## 2. Phase 0 tasks

| Task | Description | Status |
|---|---|---|
| 0.1 | Documentation foundation: README and 12 documents | Implemented (README and 12 documents created and committed in b791e59, developer-reported; the developer's review of all 12 documents is not yet recorded, so not marked Tested) |
| 0.2 | Developer review and local foundation: architecture decision review; verify local toolchain on Windows (installed tools recorded as observations only; version selection stays under D-005); review the documents; add `.gitignore` and `.env.example` (placeholders only) before any code or dependency work; confirm staging-area safety; first Git checkpoint | In Progress. Completed: decision review (2026-10-25); local toolchain verification (developer-reported; KI-004 resolved with a workaround); first Git checkpoint b791e59 (developer-reported; the 19 files are reported to include the configuration files, individual files to be confirmed in the staging-area check). Outstanding: recorded developer review of all 12 documents; recorded staging-area safety confirmation |

**Phase 0 exit criteria** (all required):

1. The developer has reviewed all 12 documents and they are mutually consistent.
2. The local toolchain is verified on the developer's machine. Observations are recorded in ARCHITECTURE §11 and version selection remains under D-005. The ordinary `npm` command discrepancy (KI-004) is investigated and resolved or explicitly accepted by the developer.
3. The Git staging area has been inspected (`git status`, `git diff --staged`) and confirmed free of secrets, credentials, `.env` files, build artifacts and dependency folders.
4. The first Git commit has been created by the developer.

D-005 and D-011 may remain Proposed when Phase 0 closes. They are intentionally scheduled for later technical evaluation (D-005 during the relevant implementation phases, D-011 in Phase 3) and do not block Phase 0.

### Exit criteria status (as of 2026-10-25)

| # | Criterion | Status | Evidence and notes |
|---|---|---|---|
| 1 | Developer has reviewed all 12 documents; mutually consistent | Outstanding | No developer review has been recorded. An AI read-through of the README and all 12 documents (in the AI workspace copy, which may differ from the committed files) found minor stale references, corrected in this update, and one open wording conflict (KI-005). |
| 2 | Local toolchain verified; KI-004 resolved or accepted | Complete (developer-reported) | Node.js v22.15.0; bundled npm CLI 10.9.2; Python 3.11.0 via `python` and 3.13.3 via `py`; Git 2.49.0.windows.1; PostgreSQL 18.1 running as `postgresql-x64-18` on localhost:5432; Docker CLI 29.4.0 with the engine unreachable (optional). Details in ARCHITECTURE §11. KI-004 accepted by the developer as a workaround. |
| 3 | Staging area inspected and confirmed free of secrets, credentials, `.env` files, build artifacts and dependency folders | Outstanding (not recorded) | The developer reports 19 files in the initial commit and a clean working tree afterwards. A pre-commit inspection of the staging area was not reported. It can be confirmed after the fact by listing the committed files (`git ls-files`, `git show --name-status b791e59`) and checking for `.env` files, keys, `node_modules`, build output and other unwanted files. |
| 4 | First Git commit created by the developer | Complete (developer-reported) | Branch `master`, commit b791e59, message "chore: establish project foundation", 19 files. |

Phase 0 remains In Progress: criteria 1 and 3 are outstanding. D-005 and D-011 remaining Proposed does not block closure.

### Git checkpoints

| Commit | Branch | Message | Contents | Recorded by |
|---|---|---|---|---|
| b791e59 | master | chore: establish project foundation | 19 files, including all 12 documents and configuration files; working tree clean afterwards | Developer report (not independently verified by the AI) |

### Architecture decision review (2026-10-25)

The developer approved Option A: the applicable Proposed decisions in ARCHITECTURE §11 were accepted while the specified deferred decisions were preserved.

- **Accepted:** D-001, D-002 (from the project brief); D-003, D-004, D-006, D-007, D-008, D-009, D-012 (approved by the developer).
- **Remains Proposed:** D-005 (Python and PostgreSQL versions chosen after verification in the relevant implementation phases); D-011 (row-level security evaluation and audited break-glass policy, Phase 3).
- **Remains Deferred:** D-010 (Phase 3, ADR); D-013 (Phase 11); D-014 (Phase 1); D-015 (Phase 4); D-016 (deferred until institutional and legal input is available; no implementation phase assigned).

Phase 0 remains In Progress. No application code exists, and nothing is Implemented or Tested.

## 3. Checks recorded for Task 0.1

Only structural documentation checks were possible (file presence, status vocabulary, relative links, role/phase consistency, absence of secret-like values). They were run by the AI in its own sandbox against a copy of these documents, not on the developer's machine. The developer should repeat the file-presence check locally. No application tests exist yet.

Additional check on 2026-10-25 (AI workspace copy, not the committed files): the README and all 12 documents were read through, and structural checks were run (relative links, status labels, decision ID references, decision register statuses, known stale phrases, secret-like strings). Result: 0 failures. This is not a recorded developer review, and no application code or tests exist.

## 4. Known issues

| ID | Issue | Status |
|---|---|---|
| KI-001 | Policy values (cutoff times, retention periods, QR validity, walk-in pricing) are intentionally undefined and need institutional decisions. See BUSINESS_RULES §12. | Open |
| KI-002 | Role permission matrix is a proposed baseline, not yet reviewed by any institution or contractor. | Open |
| KI-003 | Data-protection obligations (including consent for diners who are minors) need legal review. See SECURITY_AND_PRIVACY §9. | Open |
| KI-004 | **Cause identified; developer accepted a safe workaround on 2026-10-25.** Developer-reported findings: Node.js is v22.15.0. Two npm packages are installed: the system npm 10.9.2 at `C:\Program Files\nodejs\node_modules\npm` and a user-level npm 6.14.18 at `C:\Users\harsh\AppData\Roaming\npm\node_modules\npm`. The configured npm prefix is `C:\Users\harsh\AppData\Roaming\npm`, and the system `npm.ps1` launcher checks the configured prefix and uses the npm CLI found there when available, so the ordinary `npm` command runs 6.14.18. **Decision (Option A):** keep both npm versions installed and use the bundled npm CLI 10.9.2 explicitly for this project when needed. Command to invoke it: `node "C:\Program Files\nodejs\node_modules\npm\bin\npm-cli.js" --version`. No npm files were deleted or changed and PATH was not modified. **Limitations:** the global installation is not repaired, the ordinary `npm` command still reports 6.14.18, and project npm commands must use the explicit invocation above until a future decision changes this. This is not a dependency version pin. | Resolved with workaround (global npm installation not repaired) |
| KI-005 | Wording conflict: AI_DEVELOPMENT_RULES §2 defines Deferred as "target phase named", but D-016 is Deferred until institutional and legal input is available, with no phase. Not changed in this update. Either name a phase for D-016 later or amend the Deferred definition to allow a named triggering condition. | Open |

## 5. Deferred work

| Item | Target |
|---|---|
| Authentication mechanism decision (D-010) | Phase 3 |
| Server-state library selection (D-015) | Phase 4 |
| Hosting and CI provider (D-013, D-014) | Phase 1 (CI), Phase 11 (hosting) |
| Record-retention periods (D-016) | Deferred until institutional and legal input is available; no implementation phase assigned |
| PWA and offline scanner implementation | Phase 5 onward |
| Load-test targets and tooling | Phase 11 (targets drafted earlier) |

## 6. Change log

| Date | Change |
|---|---|
| 2026-10-25 | Task 0.1: README and 12 baseline documents drafted. |
| 2026-10-25 | Developer approved the applicable architecture decisions (Option A): D-003, D-004, D-006, D-007, D-008, D-009 and D-012 moved to Accepted; D-005 and D-011 remain Proposed; D-010 and D-013 to D-016 remain Deferred. Task 0.2 decision review completed; Task 0.2 remains In Progress. |
| 2026-10-25 | Clarified Phase 0 exit criteria (D-005 and D-011 may remain Proposed; staging-area safety check and first commit required); kept D-016 Deferred without a phase; recorded developer-reported toolchain observations and opened KI-004 (ordinary `npm` reports 6.14.18). |
| 2026-10-25 | KI-004 updated: cause identified (system npm 10.9.2 and user-level npm 6.14.18, with the configured prefix pointing at the user-level install). Developer selected Option A: keep both installed and use the bundled npm CLI 10.9.2 explicitly via `node "C:\Program Files\nodejs\node_modules\npm\bin\npm-cli.js"`. KI-004 marked Resolved with workaround; global installation not repaired. No npm files changed; PATH not modified. Phase 0 and Task 0.2 remain In Progress. || 2026-10-25 | Phase 0 foundation evidence recorded. Initial Git checkpoint (developer-reported): branch master, commit b791e59, message "chore: establish project foundation", 19 files, working tree clean afterwards. Local toolchain recorded (developer-reported): Node.js v22.15.0; bundled npm CLI 10.9.2 with the accepted workaround for the ordinary npm 6.14.18; Python 3.11.0 via `python` and 3.13.3 via `py`; Git 2.49.0.windows.1; PostgreSQL 18.1 service running on localhost:5432; Docker CLI 29.4.0 with the engine unreachable (optional). Task 0.1 marked Implemented. Task 0.2 remains In Progress. Phase 0 remains In Progress: exit criteria 2 and 4 complete, criteria 1 and 3 outstanding. Stale references corrected in ARCHITECTURE, DATABASE_DESIGN, DEPLOYMENT_RUNBOOK, SECURITY_AND_PRIVACY and TEST_STRATEGY; KI-005 opened. D-005 and D-011 remain Proposed; D-010, D-013, D-014, D-015 and D-016 remain Deferred. |
