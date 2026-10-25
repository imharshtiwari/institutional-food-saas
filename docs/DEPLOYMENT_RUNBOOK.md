# Deployment Runbook

**Status:** Planned (nothing is deployed; production sections are Deferred to Phase 11) · **Last updated:** 2026-10-25
**Related:** [ARCHITECTURE](ARCHITECTURE.md), [SECURITY_AND_PRIVACY](SECURITY_AND_PRIVACY.md), [TEST_STRATEGY](TEST_STRATEGY.md)

## 1. Environments

| Environment | Purpose | Status |
|---|---|---|
| Local (Windows, VS Code) | Development | Planned |
| Staging | Pre-production verification | Deferred (Phase 11) |
| Production | Live service | Deferred (Phase 11) |

Hosting provider is undecided (D-013).

## 2. Local prerequisites (observations recorded; version selection stays under D-005)

Git; Node.js (LTS) with npm; Python; PostgreSQL (native Windows installation, D-006); VS Code. Installed versions observed on the developer's machine are recorded in ARCHITECTURE §11. They are observations, not version selections.

**npm on this machine (KI-004):** the ordinary `npm` command resolves to 6.14.18. Use the bundled npm CLI explicitly: `node "C:\Program Files\nodejs\node_modules\npm\bin\npm-cli.js" <arguments>`. Do not change PATH or uninstall either npm package without a new decision.

## 3. Local setup for Phase 0 (PowerShell)

```powershell
cd path\to\institutional-food-saas
git init
git status
git add .
git diff --staged
git commit -m "chore: project foundation and documentation (Phase 0, Task 0.1)"
```

Review `git diff --staged` for secrets or unwanted files before committing. Frontend and backend setup steps are added in Phases 1 and 3.

The initial commit has been made (developer-reported): branch `master`, commit b791e59, message "chore: establish project foundation", 19 files. The commands above are the reference procedure; the commit message used differs from the example.

## 4. Configuration

Use environment variables. Copy `.env.example` (reported as part of the initial commit) to `.env` locally; `.env` is never committed. Production values come from the hosting secret store.

## 5. Database operations (Planned, Phase 3 onward)

Migrations via Alembic only. Procedures to document when the backend exists: apply migration, roll back, create/restore backup, verify restore.

## 6. Release procedure (Deferred, Phase 11)

Pre-release checklist: tests pass, dependency/security scan, migration rehearsal on staging, backup taken, rollback plan, post-deploy smoke tests including a scan-claim check and a cross-tenant denial check.

## 7. Monitoring and incidents (Deferred, Phase 11)

Health checks, error and latency monitoring, scan-endpoint alerting, backup monitoring, incident contacts and notification duties.

## 8. Backups and recovery (Deferred, Phase 11)

Backup frequency, retention (D-016), encryption, restore drills, recovery objectives to be agreed with institutions.