# AI Development Rules

**Status:** In Progress · **Last updated:** 2026-10-25
**Related:** [PHASE_TRACKER](PHASE_TRACKER.md), [ARCHITECTURE](ARCHITECTURE.md), [TEST_STRATEGY](TEST_STRATEGY.md)

## 1. Source of truth

The 12 documents in `docs/` describe one system. If a request, a document, or existing code conflicts with them, stop and report the conflict. Do not silently pick a side.

## 2. Status labels (used in every document)

| Label | Meaning |
|---|---|
| Planned | Defined, not started |
| In Progress | Work started, not complete |
| Implemented | Written/applied; required checks not all passed or not yet run |
| Tested | Implemented and required checks passed, with results recorded |
| Blocked | Cannot proceed; the blocking dependency or decision is named |
| Deferred | Intentionally postponed; target phase named |

Decision-register entries (ARCHITECTURE.md) additionally use **Accepted** (given in the project brief or approved), **Proposed** (awaiting approval) and **Deferred**.

## 3. Phase discipline

- Phases 0–11 are defined in [PHASE_TRACKER](PHASE_TRACKER.md). Work only on the current task of the current phase.
- Do not skip phases and do not build the whole application in one operation.
- Do not build business features, the full frontend, or the backend before their phase.

## 4. Task protocol

1. Read the relevant docs. In a local environment, inspect existing files before changing them.
2. State: summary of the agreed architecture relevant to the task, files to create/change, order of work, assumptions needing approval.
3. Implement only the task. Preserve existing functionality; no destructive rewrites; do not overwrite unrelated changes.
4. Add tests alongside each feature.
5. Run the required checks.
6. Update the docs and PHASE_TRACKER so they stay consistent.
7. Report: files created/changed; Windows PowerShell commands; checks performed with **actual** results; known limitations; the exact next task.

## 5. Honesty rules

- Never claim a command or test was run unless it actually was. State where it was run (for example, an AI sandbox, not the developer's Windows machine).
- Never mark a task Tested without recorded results.
- Never present mock data as real or imply a live backend where none exists.
- Never claim capacity (users, scans/second) without a documented load test.

## 6. Dependency rules

- Install only what the current task needs.
- Use the stack in ARCHITECTURE.md. Any alternative framework or database needs a written reason and explicit approval.
- Pin versions only after verifying that they install and work together. Record them in the decision register. Do not write version numbers from memory into docs as facts.
- Run dependency/security checks in Phase 11 at the latest.

## 7. Code and security rules

- No secrets in Git. Any `.env.example` contains placeholders only.
- Server-side authorization on every protected operation; hiding UI is never the control.
- Never trust a browser-supplied tenant identifier without an authorization check.
- Validate input at every trust boundary. Do not log secrets, tokens, or unnecessary personal data.

## 8. Working modes

- **Local coding environment:** inspect, change, run checks, show `git diff` before commit.
- **Browser/no filesystem:** produce complete file contents and explicit PowerShell/VS Code steps for the developer to apply.

## 9. Definition of Done (per task)

- Files implemented and reviewed against the relevant docs.
- Required checks run; results recorded.
- Docs and PHASE_TRACKER updated; known issues listed.
- Git checkpoint made by the developer after `git diff` review.

## 10. Session starter template

```text
Project: Institutional Food Management SaaS. Read docs/AI_DEVELOPMENT_RULES.md and docs/PHASE_TRACKER.md.
Current phase/task: <fill in>.
Before coding: summarize relevant architecture, list files, explain order, list assumptions.
Implement only this task. Report files, PowerShell commands, actual check results, limitations, next task.
```