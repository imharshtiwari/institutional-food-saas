# Database Design

**Status:** Planned (conventions and entity inventory only; no schema exists) · **Last updated:** 2026-10-25
**Related:** [ARCHITECTURE](ARCHITECTURE.md), [BUSINESS_RULES](BUSINESS_RULES.md), [SECURITY_AND_PRIVACY](SECURITY_AND_PRIVACY.md)

Database: PostgreSQL. Access via SQLAlchemy. Schema changes only through Alembic migrations.

## 1. Conventions (Proposed; see decisions D-008, D-009)

- Primary keys: UUID for exposed entities.
- Every tenant-owned table has a non-null `tenant_id`, indexed, and included in composite keys and foreign keys where it prevents cross-tenant references.
- Timestamps: `timestamptz` in UTC. Meal dates are stored as dates interpreted in the campus timezone.
- Money: exact decimals or integer minor units plus a currency code. Never floats.
- Audit columns: `created_at`, `updated_at`; actor ids where relevant.
- Deletion: business records are not hard-deleted. Use status/soft-delete and retention jobs per D-016.
- Constraints belong in the database (unique, foreign key, check), not only in application code.

## 2. Entity groups (all Planned)

| Group | Entities | Phase |
|---|---|---|
| Tenancy | institution, university, campus, hostel, kitchen, counter, contractor_org, contractor_contract_link | 3 |
| Identity | user, external_identity (institutional id, SSO subject), role, permission, role_assignment (with scope), session | 3 |
| Diners | diner_profile (category, eligibility source), eligibility_record, guest_authorization | 3, 5 |
| Plans | meal_plan, plan_enrollment, pause, entitlement, credit_ledger_entry, purchase | 5 |
| Attendance | opt_in, claim, scan_attempt (rejected and accepted attempts), offline_batch, sync_conflict | 5 |
| Menus | meal_service, menu, menu_version, menu_item, menu_variant, variant_selection | 6 |
| Feedback | review, complaint, complaint_event | 6 |
| Inventory | ingredient, stock_movement, purchase_order, delivery, waste_record | 7 |
| Finance | contract, contract_rate, invoice, invoice_line, reconciliation_run, adjustment, dispute, approval | 8 |
| Platform | audit_event, notification, job, import_run | 3, 9 |

## 3. Tenant protection

- Application-layer scoping is mandatory.
- Row-level security evaluated in Phase 3 (D-011). If adopted, policies are tested like any other behavior.

## 4. Entitlement model (conceptual)

- **Window plans** (weekly/monthly): an entitlement per meal opportunity (diner × date × service) generated or derived from the enrollment and pause records.
- **Quota plans:** a credit balance derived from an append-only `credit_ledger_entry` (grants, consumption, expiry, adjustment). Balance is computed from or reconciled against the ledger.
- **Hybrid:** window rules plus a quota cap.
- **Purchases** (day scholar, faculty, employee, guest) create an entitlement before consumption.
- Unused meal opportunities and remaining quota credits are separate concepts and separate data (BUSINESS_RULES MP-3).

## 5. Claim integrity (Planned constraints)

- At most one successful claim per entitlement, enforced by a unique constraint on the claim's entitlement reference (partial where status is successful, if rejected rows live in the same table).
- Preferred shape: `claim` holds successful collections only; `scan_attempt` holds every attempt with outcome and reason. A rejected attempt never writes a claim row.
- Claim records the served menu variant and menu version.
- Idempotency keys are unique per client operation so retries do not duplicate.
- Offline-origin claims carry a verification state (`unverified` until reconciliation) and the device batch reference.

## 6. Indexing principles

Index foreign keys, tenant-scoped lookup paths, and time-ranged queries on claims, scan attempts, opt-ins and audit events. Add further indexes only with a measured query. Keep analytics queries off the transactional path (read models later).

## 7. Migrations

- Alembic only; never hand-edit a migrated database.
- Each migration is reviewed, reversible where practical, and tested on a copy of realistic data volumes before production.

## 8. Retention

Retention periods are policy parameter P-12 and decision D-016. The schema will support retention and anonymization jobs. No periods are defined here.