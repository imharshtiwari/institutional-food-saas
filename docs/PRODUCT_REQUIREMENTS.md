# Product Requirements

**Status:** In Progress · **Last updated:** 2026-10-25
**Related:** [BUSINESS_RULES](BUSINESS_RULES.md), [ROLE_PERMISSION_MATRIX](ROLE_PERMISSION_MATRIX.md), [PHASE_TRACKER](PHASE_TRACKER.md)

## 1. Vision

An institutional food management platform covering the full chain:

Discover menus → Opt in to meals → Verify meal collection → Manage queues → Plan food production → Manage ingredients and procurement → Reconcile contractor invoices → Analyze cost and quality.

It serves multiple universities, campuses, hostels, kitchens, dining counters, contractors and diner categories.

## 2. Scale objective

Initial **design objective**: five universities and 50,000 registered users. This is a target to validate through realistic load testing (Phase 11). It is **not** a claim of proven capacity.

## 3. Roles

**SaaS:** Super Admin, Support Agent, Billing Admin.
**Institution:** Institution Owner, University Administrator, Campus Administrator, Hostel Warden, Finance Officer, Procurement Officer, Auditor.
**Food contractor:** Contractor Owner, Contractor Administrator, Contractor Finance User, Kitchen Manager, Kitchen Staff, Counter Operator, Supplier User.
**Diners:** Student, Day Scholar, Faculty, Employee, Authorized Guest.

Permissions are defined in [ROLE_PERMISSION_MATRIX](ROLE_PERMISSION_MATRIX.md).

## 4. Glossary

| Term | Meaning |
|---|---|
| Tenant | An Institution account whose private data is isolated (decision D-007) |
| Meal service | Breakfast, lunch, snacks or dinner |
| Meal opportunity | One diner × one date × one meal service |
| Entitlement | A server-validated right to receive one meal (from a plan window, a quota credit, or a purchase) |
| Opt-in | A diner's demand-planning signal. Not an entitlement and not proof of collection |
| Claim | A successfully committed meal collection |
| Rejected scan | A scan attempt that did not produce a claim, recorded separately |
| Menu variant | Regular or fasting/festival version of a meal |
| Walk-in | An authorized collection without prior opt-in, under explicit rules |

## 5. Functional requirement areas

| ID | Area | Phase | Status |
|---|---|---|---|
| FR-01 | Tenant, organization and user administration | 3–4 | Planned |
| FR-02 | Identity, SSO and eligibility provisioning | 3, 9 | Planned |
| FR-03 | Meal plans (weekly, monthly, quota, hybrid), pause, renewal | 5 | Planned |
| FR-04 | Daily opt-in and cancellation within cutoffs | 5–6 | Planned |
| FR-05 | QR/ID verification, claims, rejected scans, manual fallback, walk-ins | 5 | Planned |
| FR-06 | Offline scanning and audited synchronization | 5 (design), later | Planned |
| FR-07 | Menus, regular/fasting variants, menu versions | 6 | Planned |
| FR-08 | Queue status and kitchen production planning | 6 | Planned |
| FR-09 | Reviews, complaints, escalation | 6 | Planned |
| FR-10 | Day scholar / faculty / employee / guest purchases and credits | 5, 8 | Planned |
| FR-11 | Inventory, procurement, waste | 7 | Planned |
| FR-12 | Contracts, invoice reconciliation, adjustments, disputes | 8 | Planned |
| FR-13 | Notifications and university integrations | 9 | Planned |
| FR-14 | Analytics and human-approved AI assistance | 10 | Planned |

Frontend screens for these areas are built first with mock data (Phases 1–2), then connected (Phase 4 onward).

## 6. Experience priorities

**Student:** today's menu; meal opt-in; weekly/monthly passes; meal history; collected/missed/remaining; holiday pause (effect shown before confirmation); festival menu selection; queue status; feedback and complaints.

**Counter operator:** fast scan; clear accepted/rejected states; duplicate feedback; eligibility and entitlement status; authorized manual fallback; offline and sync status.

**Contractor dashboard:** attendance and meal counts; opt-ins versus collections; walk-ins and missed meals; menu demand; reviews and complaints; operations and finance.

## 7. Non-functional requirements

- Responsive on desktop, tablet and mobile; keyboard accessible (see UI_DESIGN_SYSTEM).
- Server-side authorization and tenant isolation on every protected operation (see SECURITY_AND_PRIVACY).
- Meal verification is fast and transactional; no heavy analytics in the scan path.
- Affordable to build and operate; scale through measured improvements, not speculative infrastructure.
- Mock data is always clearly labeled in the UI.

## 8. Out of scope for Phase 0

Any business feature, the full frontend, the backend, and any dependency installation.