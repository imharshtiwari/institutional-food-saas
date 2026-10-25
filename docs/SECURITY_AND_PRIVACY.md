# Security and Privacy

**Status:** In Progress (requirements and planned controls; none implemented) · **Last updated:** 2026-10-25
**Related:** [ROLE_PERMISSION_MATRIX](ROLE_PERMISSION_MATRIX.md), [BUSINESS_RULES](BUSINESS_RULES.md), [DEPLOYMENT_RUNBOOK](DEPLOYMENT_RUNBOOK.md)

## 1. Secrets and configuration

- Never commit secrets or production credentials. `.env` files must be git-ignored; a `.env.example` (Planned, Task 0.2) will contain placeholders only.
- Production secrets live in the hosting environment's secret store (decided in Phase 11).
- Review `git diff` before every commit.

## 2. Authentication and sessions

- Mechanism decided in Phase 3 (D-010). If cookie-based: HttpOnly, Secure, SameSite settings plus CSRF protection. If bearer tokens: short lifetimes and a safe storage strategy.
- University SSO (OIDC/SAML) proves identity only (BUSINESS_RULES ID-1).
- Rate limiting and lockout on authentication endpoints.

## 3. Authorization and tenant isolation

- Server-side permission and scope checks on every protected operation.
- Prevent insecure direct object references: every object lookup is scoped by tenant and authorized scope.
- Cross-tenant access is tested explicitly (TEST_STRATEGY).
- Row-level security evaluated as defense in depth (D-011).

## 4. Input validation and output handling

Validate at every trust boundary (API input, imports, file uploads, SSO claims, offline sync payloads). Use parameterized queries via SQLAlchemy. Escape output; avoid unsafe HTML rendering in the frontend.

## 5. Personal data minimization by role

Each role sees only the personal data it needs. Examples: counter operators see name/photo and entitlement status for the scanned diner only; contractors see aggregated and meal-level data without unnecessary personal details; auditors see minimized data. Exact field lists per role are defined in Phase 3 and tested.

## 6. QR and offline security

QR codes carry no sensitive personal information or reusable credentials; credentials are short-lived. Offline devices store only what is required, protected on the device, with bounded validity and audited sync. See BUSINESS_RULES QR-1 to QR-7.

## 7. Logging and audit

- Do not log passwords, secrets, tokens or unnecessary personal data.
- Record sensitive administrative actions (role changes, exceptions, retroactive pauses, manual overrides, approvals, break-glass access) as append-only audit events with actor, scope, time and reason.

## 8. Dependency and infrastructure security

Dependency and vulnerability checks, security headers, TLS configuration, backup encryption and access review are completed in Phase 11 at the latest. Dependency additions earlier follow AI_DEVELOPMENT_RULES §6.

## 9. Privacy and compliance (needs review)

- Data-protection obligations depend on jurisdiction (for example, India's data-protection law for deployments there). **Legal review is required**; this document does not provide legal advice (KI-003).
- Some diners may be minors. Consent and guardian handling must be reviewed before pilot.
- Retention periods are undefined (P-12, D-016). The schema will support retention and anonymization.
- Diners should be able to see what is recorded about them, subject to policy.

## 10. Threat areas to test (Planned)

Cross-tenant data access, privilege escalation, QR replay/sharing, concurrent-scan races, offline conflict abuse, manual-override misuse, invoice tampering, injection, and denial of service on scan endpoints.

## 11. Incident response

A basic incident-response procedure (contacts, containment, notification duties) is drafted in Phase 11 and recorded in DEPLOYMENT_RUNBOOK.