# REQ-005 — P0 Deals, Automation, and Reporting

**Priority:** P0  
**Goal:** Support an auditable CRM pipeline and dependable follow-up/reporting on authorised records.

## Source requirements

- `CRM-DEAL-001`–`CRM-DEAL-005` — deal relationships, configurable stages/probability, deal data, audit history, pipeline reporting.
- `CRM-AUTO-002`–`CRM-AUTO-003` — manual/approved automatic assignment and idempotent automation.
- `RPT-001`–`RPT-003` — report metric definitions, permission enforcement, data-backed CRM dashboards.
- SRS sections 7.2–7.3, 7.5, and 14 — pipeline, assignment/follow-up, CRM screens, and reports.

## Scope

- Create deals linked to relevant CRM parties and contacts.
- Capture value, currency, owner, source, expected close date, products/services, next action, stage, and probability.
- Let authorised administrators configure pipeline stages and probability; retain stage, value, and ownership history.
- Support manual assignment and only the automatic assignment strategies approved for Release 1.
- Ensure repeated event/job processing cannot create duplicate assignments, tasks, or notifications.
- Provide permission-aware pipeline value, stage distribution, conversion, team, lead source, activity, and overdue follow-up reporting.
- Define each KPI/report's source entities, calculation, filters, timezone, currency, and refresh behavior.

## Dependencies

- REQ-001 through REQ-004 for tenant/security, module, Core, and canonical CRM records.

## Acceptance criteria

- Deal creation and links to parties/contacts pass authorization and referential-integrity checks.
- Stage and probability configuration is permission-controlled; stage/value/ownership changes preserve an audit history.
- Pipeline and team reports reconcile to the underlying records for the selected filters.
- A repeated automation event does not create duplicate tasks, notifications, or other side effects.
- Reports and dashboards return only tenant-, role-, and record-authorized information.
- Dashboard KPI values are calculated from authorized records, not hard-coded; report metric definitions are approved.

## Open decision

The SRS leaves the exact automatic assignment strategies to agreement. Confirm the Release 1 strategy set before implementing strategy-specific behavior. Advanced assignment rule administration and overdue-lead escalation are P1 (`CRM-AUTO-001`, `CRM-AUTO-004`) and tracked in REQ-007.
