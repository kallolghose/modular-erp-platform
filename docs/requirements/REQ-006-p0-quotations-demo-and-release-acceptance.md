# REQ-006 — P0 Quotations, Demo, and Release Acceptance

**Priority:** P0  
**Goal:** Make CRM usable in an end-to-end commercial demonstration and verify its release-critical acceptance scenarios.

## Source requirements

- `QUO-001`–`QUO-007` — quotation numbering, CRM party links, line items/totals, approved tax/rounding, status/branding, PDF, version history.
- `CRM-DEMO-001`–`CRM-DEMO-003`, `CRM-DEMO-005`–`CRM-DEMO-006` — isolated demo tenant, synthetic data, reset, end-to-end lead-to-quotation journey, data-backed dashboards.
- `UAT-CRM-001`–`UAT-CRM-010` — CRM release acceptance scenarios.
- SRS sections 7.4–7.5, 8, 18.1–18.4 — commercial demonstration, screens, quotation, platform demonstration, release gates.

## Scope

- Create tenant-scoped, uniquely numbered quotations using existing CRM account/contact identities.
- Support products/services, quantity, unit rate, discounts, taxes, calculated totals, validity, terms, notes, branding, controlled status transitions, and PDF generation.
- Preserve quotation revisions and change history.
- Provide an isolated resettable demo tenant with realistic synthetic leads, accounts, contacts, deals, activities, and quotations; never use hard-coded dashboard KPIs.
- Demonstrate the prospect path from lead creation through assignment, qualification, deal, follow-up, and quotation.
- Record pass/fail evidence for CRM UAT and core platform acceptance; verify direct API/job access denial for unlicensed modules and cross-tenant denial.

## Dependencies

- REQ-001 through REQ-005 for security, lifecycle, shared services, party/lead workflows, and pipeline.

## Acceptance criteria

- Concurrent quotation creation produces unique tenant-scoped numbers.
- Quotation party links reuse CRM identities; approved calculation examples reconcile line items, discounts, tax, rounding, and grand totals.
- Only permitted status transitions succeed; PDF output matches approved values, currency, layout, and version; earlier revisions remain inspectable.
- Business owners approve jurisdiction-specific tax, rounding, currency, and numbering rules before customer use.
- Sales users complete the lead-to-quotation scenario in the isolated demo tenant; resetting demo data does not affect other tenants.
- Demo dashboards reconcile to demo records and are not backed by hard-coded figures.
- All `UAT-CRM-001`–`UAT-CRM-010` scenarios have recorded pass/fail results, including CRM permissions, conversion, reminders, import, reports, demo flow, tenant isolation, and unlicensed module denial.
- Platform demonstrations cover module registration/activation, deactivation without disrupting unrelated modules, tenant isolation, quotation PDF, backup restoration, and deployment from client-controlled source.
- No critical/high defect remains open unless formally risk-accepted; Product Owner and QA sign-off is recorded.

## Open decisions

Confirm jurisdiction, tax rules, currency/rounding policy, quotation terms/branding, demo access policy, and client-owned deployment environment before production acceptance.

## P1 follow-on

Conversion of accepted quotations to downstream documents (`QUO-008`) is not required for this initial quotation task.
