# Architecture

**Status:** In Progress · **Last updated:** 2026-10-25
**Related:** [DATABASE_DESIGN](DATABASE_DESIGN.md), [API_CONTRACTS](API_CONTRACTS.md), [SECURITY_AND_PRIVACY](SECURITY_AND_PRIVACY.md), [PHASE_TRACKER](PHASE_TRACKER.md)

Nothing in this document is implemented yet; all components are **Planned**.

## 1. Principles

1. Modular monolith first. No microservices or Kubernetes unless a documented requirement justifies them.
2. Clear separation of frontend and backend.
3. No unnecessary infrastructure or dependencies. Add components only when measurements or requirements demand them.
4. The server is the authority for authorization, tenant scoping and entitlement.
5. Scale through measured, evidence-based improvements.

## 2. Stack

| Layer | Technology |
|---|---|
| Frontend | React, TypeScript, Vite, Tailwind CSS, React Router, typed API client, Vitest, React Testing Library; a server-state library when real API integration begins (D-015); PWA capabilities when appropriate |
| Backend | Python, FastAPI, SQLAlchemy, Alembic, Pydantic, pytest; REST under `/api/v1` |
| Database | PostgreSQL |

Exact versions are **not recorded yet**. They are pinned after being verified to install together (Phase 1 for frontend, Phase 3 for backend) and then recorded in §11.

## 3. Repository layout

```text
institutional-food-platform/
  docs/        permanent documentation (12 files)
  frontend/    React app (Phase 1)
  backend/     FastAPI app (Phase 3)
  .env.example placeholders only (Planned; added in Task 0.2)
```

## 4. Backend module boundaries (Planned)

Modules live in one deployable. Each owns its tables and exposes a service layer. Other modules call the service layer, not each other's tables.

| Module | Responsibility | Phase |
|---|---|---|
| identity_access | users, roles, permissions, sessions, SSO | 3, 9 |
| tenancy | institutions, universities, campuses, hostels, kitchens, counters, contractors | 3 |
| plans_entitlements | plans, pauses, entitlement ledger, purchases | 5 |
| attendance | opt-ins, claims, rejected scans, offline sync | 5 |
| menus_kitchen | menus, variants, versions, production planning, queues | 6 |
| feedback | reviews, complaints | 6 |
| inventory_procurement | stock, purchase orders, waste | 7 |
| contracts_finance | contracts, rates, invoices, reconciliation, disputes | 8 |
| integrations_notifications | directory import, SSO connectors, notifications | 9 |
| analytics | reporting read models, AI assistance | 10 |
| audit | append-only audit events | 3 |

## 5. Multi-tenancy design

- Tenant = Institution (D-007). Tenant-owned tables carry `tenant_id`.
- Tenant context comes from the authenticated principal and the authorized resource, never from an unverified client-supplied value.
- Layer 1 (mandatory): server-side authorization and scoped queries through a shared data-access pattern.
- Layer 2 (to be evaluated in Phase 3, D-011): PostgreSQL row-level security as defense in depth.
- Contractors are separate organizations linked to institutions by contracts. They access only data tied to those contracts.

## 6. Meal claim flow (Planned, Phase 5)

1. Receive scan (short-lived credential, counter, meal service, idempotency key).
2. Validate credential and resolve the diner. Cached data is never used to decide entitlement.
3. Begin transaction. Resolve the entitlement. Insert the claim; a unique constraint ensures one successful claim per entitlement. Write the ledger entry for quota credits.
4. Commit. Only then return success.
5. On rejection, record the scan attempt in a separate record that survives a rolled-back claim.
6. After commit, background tasks handle notifications, review invitations and report refreshes. None run inside the transaction.

## 7. Offline scanning (Planned design, later implementation)

Bounded credential validity, durable local storage on the device, local duplicate checks, and audited synchronization. Offline claims are **unverified** until reconciled; conflicts (two devices accepting the same entitlement) are detected at sync and resolved by an auditable workflow. The system never claims global duplicate prevention while scanners are offline.

## 8. Caching and background work

- Published menus may be cached. Entitlement and eligibility checks never rely on cache.
- Start with in-process background tasks and a database-backed job table. Introduce a separate queue/worker only if measurements require it.
- Avoid polling. Prefer on-demand fetch; consider server-sent events only if queue status needs it, after measurement.

## 9. Frontend architecture (Planned)

Feature-oriented folders, shared accessible components, role-specific layouts and routes, a typed API client, and a mock-data layer that can be swapped for the real API in Phase 4. Permission-aware navigation is a usability aid only.

## 10. Scaling approach

Measure first (Phase 11 load tests against agreed targets). Likely order of improvements: indexes and query fixes, connection pooling, read-only menu caching, multiple application workers, read replicas, then anything heavier. Each step requires evidence.

## 11. Decision register

Statuses: **Accepted** (from the project brief or approved), **Proposed** (not yet approved; to be settled in the implementation phase named in the entry), **Deferred** (intentionally postponed). Entries marked Accepted other than D-001 and D-002 were approved by the developer on 2026-10-25 (Task 0.2, Option A).

| ID | Decision | Status |
|---|---|---|
| D-001 | Modular monolith; no microservices or Kubernetes | Accepted |
| D-002 | Stack as listed in §2 | Accepted |
| D-003 | Single Git repository with `docs/`, `frontend/`, and `backend/` at the root | Accepted |
| D-004 | npm as the Node package manager | Accepted |
| D-005 | Choose compatible Python and PostgreSQL versions after verifying them during the relevant implementation phases | Proposed |
| D-006 | Native Windows PostgreSQL installation; Docker Desktop remains optional | Accepted |
| D-007 | Tenant is an institution (which may own several universities); contractors are separate organizations linked through contracts | Accepted |
| D-008 | UUID primary keys for exposed entities; institutional identifiers remain separate fields (UUID generation strategy is decided in Phase 3) | Accepted |
| D-009 | UTC timestamps (timestamptz), configured IANA time zones and ISO 4217 currencies (per campus/tenant), and exact monetary representations (exact decimals or integer minor units, never floats) | Accepted |
| D-010 | Decide between cookie sessions with CSRF protection and bearer tokens; document the choice in an ADR | Deferred to Phase 3 |
| D-011 | Evaluate PostgreSQL row-level security as defense in depth in Phase 3 and define the audited break-glass access policy | Proposed |
| D-012 | Cursor pagination for large append-only lists (claims, scans, audit) and bounded page-based pagination for smaller administrative lists | Accepted |
| D-013 | Choose the hosting provider and production environment topology | Deferred to Phase 11 |
| D-014 | Choose the CI provider and pipeline | Deferred to Phase 1 |
| D-015 | Choose the frontend server-state library when real API integration begins | Deferred to Phase 4 |
| D-016 | Establish record-retention periods with institutional and legal input | Deferred until institutional and legal input is available (no implementation phase assigned) |

### Local toolchain observations (Task 0.2)

Reported by the developer on 2026-10-25 from their Windows machine. The AI has not verified these independently. They are observations, not decisions: they do not resolve D-005 and no Python, PostgreSQL or npm version has been selected.

| Tool | Observation |
|---|---|
| Node.js | v22.15.0 |
| Git | 2.49.0.windows.1 |
| Python | 3.11.0 is available through `python`; 3.13.3 is available through `py`. Which interpreter the backend will use is not decided (D-005) |
| npm (bundled CLI) | 10.9.2 when invoked directly through `C:\Program Files\nodejs\node_modules\npm\bin\npm-cli.js` |
| npm (ordinary `npm` command) | Reports 6.14.18. Unresolved; needs a safe follow-up investigation (PHASE_TRACKER KI-004) |
| PostgreSQL | Not yet reported |