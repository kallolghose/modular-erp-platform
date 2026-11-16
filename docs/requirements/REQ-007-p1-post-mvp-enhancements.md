# REQ-007 — P1 Post-MVP Enhancements and Deferred Scope

**Priority:** P1 and later roadmap  
**Goal:** Record important follow-on requirements without making them hidden prerequisites for the P0 Core + CRM release.

## P1 Core and lifecycle improvements

- `CORE-011`, `CORE-012`, `CORE-014` — shared workflow/approval, subscription/plan/renewal/usage limits, and explicitly owned common reference data.
- `MOD-008`–`MOD-010` — upgrade recovery, release compatibility/regression scope, module health and lifecycle history.
- `TEN-006`–`TEN-007` — approved/time-bound/audited support access and tenant export/retention/deletion.
- `SEC-008`–`SEC-009` — protected audit logs/retention and jurisdiction-approved personal-data workflows.
- `OPS-006`–`OPS-007` — centralized observability/alerting and operational runbooks.

## P1 CRM and mobile improvements

- `CRM-LEAD-004` — permission-controlled duplicate merge with reference and history preservation.
- `CRM-AUTO-001`, `CRM-AUTO-004` — configurable assignment-rule administration and overdue/uncontacted lead reminders/escalations.
- `RPT-004` — audited exports with the same permissions as on-screen results.
- `QUO-008` — idempotent conversion of accepted quotations to approved downstream documents.
- `MOB-003`–`MOB-004` — agreed mobile CRM workflows and any explicitly approved offline behavior.
- `MDM-005` — permissioned/audited master-data merge, correction, and deactivation.

The SRS requires mobile clients to use shared APIs and server-side business rules (`MOB-001`–`MOB-002`); this is a platform constraint, not permission for mobile clients to bypass REQ-001 authorization. The mobile app workflows and offline scope remain subject to product approval.

## Later roadmap (not part of this P1 task)

- Provider-specific WhatsApp Business and telephony adapters, website forms, email, advertising, and lead marketplace connectors (SRS sections 9–11).
- Full Sales Orders, invoices/returns, Purchase, Inventory, Finance, HRM/Payroll, Projects, POS, Support, Manufacturing, and other catalogue modules (SRS sections 6, 8, and 12).
- Independent deployment, plugin marketplace, advanced analytics, and any unapproved offline mobile functionality.

Each follow-on feature requires an approved provider/business scope, dependency and data-ownership contract, security/privacy review as applicable, and measurable acceptance criteria before implementation is scheduled. Provider API availability, consent requirements, service fees, jurisdictional rules, and mobile app-store ownership are external decisions, not assumed deliverables here.
