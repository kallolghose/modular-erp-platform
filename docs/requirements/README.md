# Initial ERP Requirements Backlog

This is the initial, prioritized work breakdown derived from `docs/srs/ERP_SRS_Version_1.2.docx` (SRS v1.2, 1 November 2026). It covers the shared Core Platform and the CRM-first release; it is a requirements baseline, not a claim that the listed capabilities are implemented.

## Priority and ordering

- **P0** — required for the initial release or its stated acceptance gates.
- **P1** — important follow-on capability; not a prerequisite for the P0 release unless an owner confirms otherwise.

Tasks are listed by priority, then by prerequisite order. P0 tenant isolation and access controls come first because CRM and shared services depend on them. Subsequent tasks build the module lifecycle and Core services before shared data and CRM workflows. The task number is for sequencing, not a substitute for the SRS priority.

## Tasks

| Order | Priority | Workstream | Task |
|---|---|---|---|
| 1 | P0 | Tenant isolation, identity, authorization, security | [REQ-001](REQ-001-p0-tenant-isolation-and-access.md) |
| 2 | P0 | Module registry, entitlements, lifecycle | [REQ-002](REQ-002-p0-module-registry-and-lifecycle.md) |
| 3 | P0 | Shared Core services and operational foundation | [REQ-003](REQ-003-p0-core-services-and-operations.md) |
| 4 | P0 | Canonical party data and CRM lead workflows | [REQ-004](REQ-004-p0-party-data-and-crm-leads.md) |
| 5 | P0 | Deals, automation, and reporting | [REQ-005](REQ-005-p0-deals-automation-and-reporting.md) |
| 6 | P0 | Quotations, demo readiness, and release acceptance | [REQ-006](REQ-006-p0-quotations-demo-and-release-acceptance.md) |
| 7 | P1 | Post-MVP Core/CRM enhancements and deferred scope | [REQ-007](REQ-007-p1-post-mvp-enhancements.md) |

## Backlog conventions

Each task cites the originating SRS requirement IDs and keeps the SRS priority and acceptance intent. Dependencies are expressed as task IDs. Acceptance statements are intended to be verifiable; business-specific values and open decisions are identified rather than guessed. SRS requirements that have no formal ID cite their section.

This first cut groups related requirements into delivery workstreams. It does not assign owners, estimates, or dates; those require product and architecture planning.

## Explicitly deferred

The initial work breakdown does not include full Sales Orders/invoicing, Purchase, Inventory, Finance, HRM/Payroll, Projects, POS, Support, Manufacturing, or other later ERP modules. Provider-specific WhatsApp and telephony, broader advertising/marketplace lead-source connectors, and mobile/offline workflows require separate provider, product, legal, and acceptance decisions. Follow-on scope is listed in REQ-007 without treating it as part of the P0 CRM release.

## Confirmation items to resolve before commitment

- Approve the tenancy model, including shared-database/RLS details and any dedicated-tenant exception.
- Confirm target jurisdiction, privacy/retention rules, and quotation tax, rounding, currency, and numbering policies.
- Confirm the baseline performance/load profile and proposed availability, RPO, and RTO targets.
- Confirm which automatic assignment strategies and CRM custom fields are required for the first release.
- Confirm hosting region, provider accounts, and CRM mobile scope.
