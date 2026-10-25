# API Contracts

**Status:** Planned (conventions only; no endpoint is implemented) · **Last updated:** 2026-10-25
**Related:** [ARCHITECTURE](ARCHITECTURE.md), [ROLE_PERMISSION_MATRIX](ROLE_PERMISSION_MATRIX.md), [SECURITY_AND_PRIVACY](SECURITY_AND_PRIVACY.md)

Base path: `/api/v1`. JSON over HTTPS. FastAPI generates the OpenAPI document; once the backend exists, that document is the machine-readable contract and this file records conventions and the endpoint plan.

## 1. Conventions

- **Versioning:** breaking changes require `/api/v2`; additive changes stay in v1.
- **Auth:** mechanism decided in Phase 3 (D-010). Every protected route checks permission and scope on the server.
- **Tenancy:** tenant context derives from the authenticated principal and the resource being accessed. A client-supplied tenant id is only a filter that is authorized, never a grant.
- **Validation:** Pydantic models on every request body and parameter; unknown fields rejected on write endpoints.
- **Errors:** consistent envelope `{ "error": { "code", "message", "details?", "request_id" } }` with correct HTTP status codes. No stack traces or internal ids leaked.
- **Pagination:** cursor-based for large append-only lists; page/limit with a maximum for small admin lists (D-012).
- **Idempotency:** `Idempotency-Key` header required on scan/claim, purchase, and payment-related writes.
- **Time:** ISO 8601 UTC; dates for meal days in the campus timezone.
- **Rate limits:** applied to authentication, scan and purchase endpoints, with limits set in Phase 3/11.
- **Responses never return success before the corresponding transaction commits.**

## 2. Endpoint plan (names provisional; all Planned)

| Area | Examples | Phase |
|---|---|---|
| Auth and session | `POST /auth/login`, `POST /auth/logout`, `GET /me` | 3–4 |
| Tenancy | `GET /campuses`, `GET /hostels`, `GET /counters` | 3–4 |
| Users and roles | `GET/POST /users`, `POST /role-assignments` | 3–4 |
| Plans | `GET /meal-plans`, `POST /enrollments`, `POST /pauses/preview`, `POST /pauses` | 5 |
| Opt-in | `GET /meals/today`, `PUT /opt-ins/{date}/{service}` | 5–6 |
| Attendance | `POST /scans` (claim attempt), `GET /claims`, `GET /scan-attempts`, `POST /offline-batches` | 5 |
| Menus | `GET /menus`, `POST /menus`, `POST /menus/{id}/publish`, `PUT /variant-selections` | 6 |
| Feedback | `POST /reviews`, `POST /complaints`, `PATCH /complaints/{id}` | 6 |
| Inventory | `GET /ingredients`, `POST /stock-movements`, `POST /purchase-orders` | 7 |
| Finance | `GET /contracts`, `POST /reconciliation-runs`, `POST /invoices/{id}/approve` | 8 |
| Integrations | `POST /imports`, SSO callbacks | 9 |
| Analytics | `GET /reports/...` | 10 |

## 3. Scan endpoint requirements (Planned)

Request carries a short-lived credential (no personal data inside the QR), counter id, meal service and idempotency key. Responses distinguish: `accepted`, `rejected` with reason (not entitled, already claimed, expired credential, wrong service, wrong counter, etc.), and offline-reconciliation outcomes. Rejections are recorded as scan attempts.

## 4. Frontend contract

Until Phase 4 the frontend uses a mock-data layer with the same TypeScript types intended for the API client. Mock data is labeled in the UI. TypeScript types will be derived from or checked against the OpenAPI document when the backend exists.

## 5. Frontend/backend responsibility boundary

| Frontend (browser) | Backend (server) |
|---|---|
| Renders screens and states (loading, empty, error, success) | Authenticates users and enforces authorization and tenant scoping on every protected operation |
| Offers input hints and convenience validation | Validates every input as the authoritative check |
| Shows navigation by permission (usability only) | Decides eligibility, entitlement, cutoffs, pricing and variant availability |
| Calls the API only through a typed client | Executes the claim transaction and returns success only after commit |
| Displays server-computed previews (for example, the holiday pause effect) | Computes previews and final results with the same rules |
| Maps server reason codes to user-facing text | Returns stable reason codes and request ids; records rejected scans and audit events |
| Holds offline scanner data on the device (later phases) | Reconciles offline batches and marks outcomes verified or conflicting |

Rules:

1. The client never decides eligibility, cutoff, price or entitlement, and the client clock is never trusted for cutoffs.
2. Frontend validation is a convenience. The server re-validates everything.
3. Business rules are not duplicated in the frontend except as display hints derived from server responses.
4. Until Phase 4 the frontend talks to a mock-data layer that exposes the same typed interface the API client will use. Neither the mock layer nor the API client exists yet (Planned).