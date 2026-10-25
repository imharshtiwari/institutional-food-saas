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

## 2. Local prerequisites (versions confirmed in Task 0.2 and Phases 1/3)

Git; Node.js (LTS) with npm; Python; PostgreSQL (native Windows installer, D-006); VS Code. Record the verified versions in ARCHITECTURE §11 once confirmed on the developer's machine.

## 3. Local setup for Phase 0 (PowerShell)

```powershell
cd path\to\institutional-food-platform
git init
git status
git add .
git diff --staged
git commit -m "chore: project foundation and documentation (Phase 0, Task 0.1)"
```

Review `git diff --staged` for secrets or unwanted files before committing. Frontend and backend setup steps are added in Phases 1 and 3.

## 4. Configuration

Use environment variables. Once `.env.example` exists (Task 0.2), copy it to `.env` locally; `.env` is never committed. Production values come from the hosting secret store.

## 5. Database operations (Planned, Phase 3 onward)

Migrations via Alembic only. Procedures to document when the backend exists: apply migration, roll back, create/restore backup, verify restore.

## 6. Release procedure (Deferred, Phase 11)

Pre-release checklist: tests pass, dependency/security scan, migration rehearsal on staging, backup taken, rollback plan, post-deploy smoke tests including a scan-claim check and a cross-tenant denial check.

## 7. Monitoring and incidents (Deferred, Phase 11)

Health checks, error and latency monitoring, scan-endpoint alerting, backup monitoring, incident contacts and notification duties.

## 8. Backups and recovery (Deferred, Phase 11)

Backup frequency, retention (D-016), encryption, restore drills, recovery objectives to be agreed with institutions.