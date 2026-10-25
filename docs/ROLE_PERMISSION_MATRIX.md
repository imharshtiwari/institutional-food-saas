# Role and Permission Matrix

**Status:** In Progress (proposed baseline, not yet reviewed by institutions or contractors; see KI-002) · **Last updated:** 2026-10-25
**Related:** [PRODUCT_REQUIREMENTS](PRODUCT_REQUIREMENTS.md), [SECURITY_AND_PRIVACY](SECURITY_AND_PRIVACY.md)

## 1. Principles

- Permissions are explicit and scoped. Enforcement is on the server; hiding UI is never the control.
- Every grant has a scope: **Platform**, **Institution**, **University**, **Campus**, **Hostel**, **Contractor** (own organization and contracts), **Kitchen/Counter** (assigned units), or **Self**.
- Contractors see only data tied to contracts they hold. Institutions see contractor data tied to their contracts.
- SaaS roles have no default access to tenant business data. Access is by time-boxed, audited break-glass only (decision D-011 context; policy to be defined in Phase 3).
- Each role sees the minimum personal information needed (see SECURITY_AND_PRIVACY §5).

## 2. Legend

| Code | Meaning |
|---|---|
| — | No access |
| R | Read (within scope) |
| RW | Read and create/edit (within scope) |
| RWA | Read, edit and approve |
| S | Self only (own records) |
| SW | Self read and write (own opt-ins, pauses, reviews) |
| X | Execute at counter (scan/claim) |

## 3. Permission groups (columns)

TEN tenant/org and user administration · ELG eligibility and provisioning · PLN plans and pricing · MNU menus and variants · OPT opt-in, cancel, pause · SCN scan/claim · OVR manual fallback, exceptions, walk-ins · STK inventory and procurement · CTR contracts and rates · INV invoices and reconciliation · FIN financial approval and payment status · RVW reviews and complaints · ANL analytics and reports · AUD audit logs

## 4. Matrix (proposed baseline)

| Role | TEN | ELG | PLN | MNU | OPT | SCN | OVR | STK | CTR | INV | FIN | RVW | ANL | AUD |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Super Admin | RW* | — | — | — | — | — | — | — | — | — | — | — | R* | R |
| Support Agent | R* | — | — | — | — | — | — | — | — | — | — | R* | — | — |
| Billing Admin | R* | — | — | — | — | — | — | — | — | — | — | — | — | — |
| Institution Owner | RWA | R | R | R | R | — | R | R | RWA | R | RWA | R | R | R |
| University Administrator | RW | RW | RW | RWA | RW | — | RWA | R | R | R | — | RW | R | R |
| Campus Administrator | RW | RW | R | RW | RW | — | RW | R | R | R | — | RW | R | — |
| Hostel Warden | — | RW | R | R | RW | — | RW | — | — | — | — | RW | R | — |
| Finance Officer | — | — | R | — | — | — | — | — | R | RWA | RWA | — | R | R |
| Procurement Officer | — | — | — | — | — | — | — | RW | R | R | — | — | R | — |
| Auditor | R | R | R | R | R | — | R | R | R | R | R | R | R | R |
| Contractor Owner | RW | — | — | RW | R | — | R | RW | R | RW | R | RW | R | R |
| Contractor Administrator | RW | — | — | RW | R | — | R | RW | R | RW | — | RW | R | — |
| Contractor Finance User | — | — | — | — | — | — | — | — | R | RW | R | — | R | — |
| Kitchen Manager | — | — | — | RW | R | R | R | RW | — | — | — | RW | R | — |
| Kitchen Staff | — | — | — | R | R | — | — | RW | — | — | — | — | — | — |
| Counter Operator | — | — | — | R | R | X | RW | — | — | — | — | — | — | — |
| Supplier User | — | — | — | — | — | — | — | RW | — | R | — | — | — | — |
| Student | — | S | S | R | SW | S | — | — | — | — | — | SW | — | — |
| Day Scholar | — | S | S | R | SW | S | — | — | — | — | — | SW | — | — |
| Faculty | — | S | S | R | SW | S | — | — | — | — | — | SW | — | — |
| Employee | — | S | S | R | SW | S | — | — | — | — | — | SW | — | — |
| Authorized Guest | — | S | S | R | S | S | — | — | — | — | — | SW | — | — |

\* SaaS roles: platform-level operations only (tenant lifecycle, subscription billing, support triage). Tenant business data is accessible only through audited break-glass. "Self" for SCN means presenting one's own credential; it does not permit operating a scanner.

## 5. Scope notes

- **Scoped by assignment:** Hostel Warden (assigned hostels), Campus Administrator (assigned campuses), Kitchen Manager/Staff (assigned kitchens), Counter Operator (assigned counters and meal services).
- **Supplier User:** limited to purchase orders and deliveries addressed to that supplier.
- **Auditor:** read-only everywhere in scope; personal data minimized; no write permission anywhere.
- **Counter Operator OVR:** manual fallback and walk-ins only within limits set by policy P-6 and P-7. Every use is audited and reviewable.
- **AI assistance (Phase 10):** operates with the invoking user's permissions. It cannot approve anything (BUSINESS_RULES FC-3).

## 6. Implementation notes (Planned)

- Permissions are named capabilities (for example `attendance.claim.create`) assigned to roles per scope, stored in the database, checked by a central server-side dependency.
- Frontend navigation reads the user's permission set only to improve usability.
- Each permission and scope boundary has automated tests, including negative cross-tenant tests (TEST_STRATEGY §3).