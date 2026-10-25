# Test Strategy

**Status:** Planned (no tests exist yet) · **Last updated:** 2026-10-25
**Related:** [AI_DEVELOPMENT_RULES](AI_DEVELOPMENT_RULES.md), [BUSINESS_RULES](BUSINESS_RULES.md), [SECURITY_AND_PRIVACY](SECURITY_AND_PRIVACY.md)

## 1. Principles

- Tests are written alongside each feature, not afterwards.
- A task is **Tested** only when required checks have run and results are recorded.
- Concurrency, constraint and tenant-isolation tests run against real PostgreSQL, not a substitute database.
- Reports state where tests ran (developer machine, CI, AI sandbox).

## 2. Tools

| Layer | Tools |
|---|---|
| Backend | pytest (unit, API, database integration) |
| Frontend | Vitest and React Testing Library; accessibility checks on key components |
| End-to-end | Tool chosen later; critical flows only (D-014 context) |
| Load | Tool and targets decided before Phase 11 |

## 3. Required coverage areas

| Area | Examples | Earliest phase |
|---|---|---|
| Authentication and authorization | login/session, permission checks per role | 3 |
| Tenant isolation | user of tenant A cannot read/write tenant B by id, filter or crafted request | 3 |
| Meal eligibility | no plan, expired, paused, wrong service, wrong variant | 5 |
| Pass activation and expiry | boundaries at start/end, renewal, carry-forward | 5 |
| Pause and cutoff | preview matches result; cutoff edges; retroactive policy | 5 |
| Duplicate and concurrent scans | N parallel scans → exactly one claim; retried idempotency key | 5 |
| Walk-in eligibility | allowed/denied per category and policy | 5 |
| Offline synchronization | duplicate across devices, expired credential, conflict handling, unverified labeling | 5+ |
| Financial reconciliation | claims × rates, adjustments, disputes, approvals; AI cannot approve | 8 |
| Permission restrictions | each matrix entry, including negative cases | 3+ |
| Input validation | malformed, oversized, wrong-type, unknown fields | 3+ |
| Regression | each fixed defect gets a test | ongoing |

## 4. Frontend checks (Phases 1–2)

Component tests for design-system components, route/permission-aware navigation tests, loading/empty/error/success state tests, keyboard-navigation tests, and visible "demo data" label tests.

## 5. Load testing (Phase 11)

Targets (concurrent diners, scans per second at peak meal times, p95 latency, error rate) are agreed with institutions before testing. Capacity is only claimed for the tested configuration and data volume. The 50,000-user figure remains an objective until then.

## 6. Quality gates

Before a Git checkpoint: relevant tests pass, linting/type checks pass (once configured), `git diff` reviewed, docs and tracker updated.

## 7. Task 0.1 checks

Structural documentation checks only (file presence, status vocabulary, links, role/phase consistency, secret-like values). Results appear in the hand-off report.