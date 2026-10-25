# Business Rules

**Status:** In Progress · **Last updated:** 2026-10-25
**Related:** [PRODUCT_REQUIREMENTS](PRODUCT_REQUIREMENTS.md), [DATABASE_DESIGN](DATABASE_DESIGN.md), [SECURITY_AND_PRIVACY](SECURITY_AND_PRIVACY.md)

All rules below are **Planned** until implemented and tested. Rules reference configurable **policy parameters** (§12); this document deliberately does not invent their values.

## 1. Multi-tenancy (MT)

- MT-1 Every institution's private data is isolated.
- MT-2 Authorization and tenant scoping are enforced on the server.
- MT-3 Defense-in-depth database protections are planned where appropriate (decision D-011).
- MT-4 A tenant identifier supplied by the browser is never trusted without an authorization check.

## 2. Identity (ID)

- ID-1 SSO (OIDC or SAML, per institution) proves identity only. It does not prove enrollment, hostel assignment or meal eligibility.
- ID-2 Provisioning and eligibility come from an authoritative directory, approved import, or administrator workflow.
- ID-3 Users are keyed by stable institutional identifiers, not email alone.
- ID-4 Transaction records follow documented retention policies (parameter P-12).

## 3. Meal plans (MP)

- MP-1 Plan types: weekly, monthly, quota-based, hybrid.
- MP-2 Configurable: activation, start/end dates, expiry, renewal, pause, cutoff, carry-forward, entitlement rules.
- MP-3 **Unused meal opportunities** (time-window plans) are distinct from **remaining quota credits** (balances). They are tracked separately and never conflated in counts or UI.
- MP-4 A holiday pause shows its effect (days/meals/credits affected, dates) before confirmation.
- MP-5 Retroactive pauses and exceptions require an explicit authorized policy and are audited.

## 4. Daily opt-in (OI)

- OI-1 Services: breakfast, lunch, snacks, dinner.
- OI-2 Diners may opt in or cancel within configured cutoffs.
- OI-3 Opt-in is a demand-planning signal. It is not proof of attendance and never counts as collection.
- OI-4 Variant choice respects eligibility, capacity, cutoff and entitlement.

## 5. Attendance and claims (AT)

- AT-1 Only a successfully **committed** claim counts as collected. Booking or opt-in never does.
- AT-2 Duplicate prevention uses database constraints plus transactional logic.
- AT-3 Concurrent scans cannot create two successful claims for the same entitlement.
- AT-4 Success is never returned before the transaction commits.
- AT-5 Rejected scans are recorded separately from attendance.
- AT-6 Authorized walk-ins, exceptions and additional purchases follow explicit rules and are audited.
- AT-7 Analytics, notifications and reports run outside the claim transaction.

## 6. QR and offline (QR)

- QR-1 QR codes contain no sensitive personal information and no reusable credentials.
- QR-2 Credentials are short-lived or otherwise protected (validity: parameter P-8).
- QR-3 When online, eligibility is validated on the server. Cached data is never authoritative for entitlement.
- QR-4 Offline scanning uses bounded credential validity, durable local storage, local duplicate checks and audited synchronization.
- QR-5 Independent offline scanners cannot guarantee global duplicate prevention without coordination. The system says so.
- QR-6 Physical institutional IDs and controlled manual fallback are supported.
- QR-7 An offline claim is labeled **unverified** until reconciliation completes. It is never shown as globally confirmed before then.

## 7. Festival and fasting menus (FM)

- FM-1 Regular and fasting variants are supported.
- FM-2 Variant selection happens within configured cutoffs.
- FM-3 Variants of the same meal entitlement yield at most one claim.
- FM-4 The variant actually served is recorded.
- FM-5 No silent substitution when a selected variant runs out. The shortfall is surfaced and handled by an explicit action.

## 8. Reviews (RV)

- RV-1 Review invitations trigger after a successful claim, not after booking.
- RV-2 Reviews link to the meal and the menu version actually served.
- RV-3 No further prompts once a review is submitted.
- RV-4 Complaints support assignment, escalation and resolution.
- RV-5 A rating alone is not proof of a food-safety violation.

## 9. Day scholars, faculty, employees, guests (DS)

- DS-1 Separate eligibility and pricing rules per category.
- DS-2 Supported: authorized single-meal purchases, prepaid credits, guest meals, applicable plans.
- DS-3 A purchase creates a valid entitlement **before** it can be consumed.
- DS-4 The same duplicate-claim protections apply to all categories.

## 10. Financial controls (FC)

- FC-1 Contractor invoices are reconciled against authorized claims and contract rates.
- FC-2 Adjustments, disputes, approvals and audit trails are recorded.
- FC-3 AI-generated suggestions never bypass human approval.
- FC-4 No financial transaction is invented. No payment is marked successful without authoritative confirmation.

## 11. Conceptual claim rule

A claim consumes exactly one entitlement. For window plans the entitlement is the diner's meal opportunity (diner × date × service). For quota plans it is one credit. At most one successful claim can reference an entitlement; the database enforces this, not only application code. Details: DATABASE_DESIGN §5.

## 12. Policy parameters requiring institutional decision

None of these has a value yet. Defaults must be proposed by the institution, not invented by the developer.

| ID | Parameter |
|---|---|
| P-1 | Opt-in and cancellation cutoff per meal service |
| P-2 | Variant selection cutoff |
| P-3 | Pause: minimum notice, maximum length, retroactivity limits |
| P-4 | Carry-forward rules per plan type |
| P-5 | Renewal and expiry grace |
| P-6 | Walk-in eligibility and pricing per diner category |
| P-7 | Manual fallback limits per counter operator |
| P-8 | QR credential validity (online and offline) |
| P-9 | Offline validity window and maximum offline claims per device |
| P-10 | Complaint escalation timelines |
| P-11 | Invoice dispute windows |
| P-12 | Data retention periods per record type |