<<<<<<< HEAD
# Institutional Food Management Platform

Multi-tenant SaaS for university dining, hostels, campus food contractors, faculty, employees and day scholars.

**Current phase:** Phase 0 – Project foundation and documentation (In Progress)
**Application code:** none yet. No runtime dependencies are installed in this phase.

## Repository layout

| Path | Purpose | Status |
|---|---|---|
| `docs/` | Permanent project documentation (source of truth) | In Progress |
| `frontend/` | React + TypeScript + Vite + Tailwind app | Planned (Phase 1) |
| `backend/` | FastAPI + SQLAlchemy + Alembic service | Planned (Phase 3) |

## Documentation index

1. [PRODUCT_REQUIREMENTS](docs/PRODUCT_REQUIREMENTS.md)
2. [ARCHITECTURE](docs/ARCHITECTURE.md) (includes the decision register)
3. [BUSINESS_RULES](docs/BUSINESS_RULES.md)
4. [ROLE_PERMISSION_MATRIX](docs/ROLE_PERMISSION_MATRIX.md)
5. [API_CONTRACTS](docs/API_CONTRACTS.md)
6. [DATABASE_DESIGN](docs/DATABASE_DESIGN.md)
7. [UI_DESIGN_SYSTEM](docs/UI_DESIGN_SYSTEM.md)
8. [TEST_STRATEGY](docs/TEST_STRATEGY.md)
9. [PHASE_TRACKER](docs/PHASE_TRACKER.md) (includes known issues and deferred work)
10. [AI_DEVELOPMENT_RULES](docs/AI_DEVELOPMENT_RULES.md)
11. [SECURITY_AND_PRIVACY](docs/SECURITY_AND_PRIVACY.md)
12. [DEPLOYMENT_RUNBOOK](docs/DEPLOYMENT_RUNBOOK.md)

## Ground rules

- Work happens phase by phase. See `docs/PHASE_TRACKER.md`.
- Never commit `.env` files, credentials, build output or dependency folders.
- If two documents disagree, stop and resolve the conflict before writing code.