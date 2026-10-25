# UI Design System

**Status:** Planned (tokens and components defined in Phase 1) · **Last updated:** 2026-10-25
**Related:** [PRODUCT_REQUIREMENTS](PRODUCT_REQUIREMENTS.md), [ROLE_PERMISSION_MATRIX](ROLE_PERMISSION_MATRIX.md), [TEST_STRATEGY](TEST_STRATEGY.md)

## 1. Goals

Consistent, accessible, responsive UI across desktop, tablet and mobile, with role-specific experiences built from shared components.

## 2. Foundations (values chosen in Phase 1)

- **Tokens:** typography scale, spacing scale, color roles, status colors (success, warning, error, info, neutral), radius, elevation. Implemented through Tailwind configuration.
- **Status never relies on color alone:** pair color with icon and text (essential at counters).
- **Accessibility target (Proposed):** WCAG 2.2 AA; visible focus; full keyboard operation; labeled form controls; sufficient contrast; screen-reader names for icon buttons.
- **Responsive:** mobile-first; Tailwind breakpoints; tables adapt (cards or horizontal scroll) on small screens.

## 3. Component inventory (all Planned, Phase 1)

Buttons, inputs, selects, checkboxes, radio groups, switches, form fields with labels and validation messages, cards, tables (search, filter, sort, pagination), dialogs (confirmation pattern), dropdown menus, tabs, toasts, badges, banners, skeletons, empty-state, error-state, pagination, date/time pickers, layout shell (header, sidebar, bottom navigation on mobile).

## 4. Required states

Every data view defines loading, empty, error, and success states. Destructive or consequential actions use a confirmation dialog stating the effect first (for example, the holiday pause preview).

## 5. Role experiences (Planned, Phases 1–2)

| Experience | Priorities |
|---|---|
| Student / diner | Today's menu; opt-in; passes; history; collected/missed/remaining; holiday pause with effect preview; festival variant choice; queue status; feedback and complaints |
| Counter operator | Large scan area; unmistakable accepted/rejected states with reasons; duplicate feedback; entitlement status; authorized manual fallback; offline and sync indicator |
| Contractor | Attendance and meal counts; opt-ins vs collections; walk-ins and missed meals; demand; reviews/complaints; operations and finance |
| Institution admin | Users, plans, menus oversight, exceptions, reports |
| Finance | Reconciliation, adjustments, disputes, approvals |
| SaaS | Tenant lifecycle, support, billing |

## 6. Navigation and permissions

Navigation items are shown by permission for usability only. Hidden items are never the security control (ROLE_PERMISSION_MATRIX §1).

## 7. Mock data rule

During Phases 1–2 all data is mock. Screens show a visible "Demo data" indicator. No screen implies a live backend or real people.

## 8. Testing

Component tests with React Testing Library, keyboard-navigation checks, and accessibility checks on core components (TEST_STRATEGY §4).