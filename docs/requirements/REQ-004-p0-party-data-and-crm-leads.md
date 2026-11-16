# REQ-004 — P0 Canonical Party Data and CRM Leads

**Priority:** P0  
**Goal:** Implement CRM lead workflows on a stable, non-duplicated party identity and preserve activity and conversion history.

## Source requirements

- `MDM-001`–`MDM-004` — canonical Party identity, ownership, canonical references, and traceable lead conversion.
- `CRM-LEAD-001`–`CRM-LEAD-003` — lead lifecycle, configurable capture fields, duplicate detection.
- `CRM-LEAD-005`–`CRM-LEAD-008` — import, timeline, status/ownership history, controlled lead conversion.
- SRS sections 7.1 and 12.1–12.2 — lead/contact management, Party model, master-data ownership.

## Scope

- Define stable Party, Organisation, and Person identifiers and declare a single owner/system of record for each master entity.
- Allow authorised users to create, view, edit, assign, qualify, disqualify, and convert leads.
- Capture configurable source, campaign, status, priority, owner, qualification data, and approved custom fields.
- Detect likely duplicates using configurable matching criteria.
- Import CSV/XLSX data with field mapping, preview, validation, row-level errors, and retry of failed rows.
- Preserve chronological notes, tasks, meetings, reminders, attachments, and activity history; retain actor/time and old/new values for status and ownership changes.
- Convert qualified leads to linked Party, Organisation, Contact, and Deal records without uncontrolled duplication.

## Dependencies

- REQ-001 for tenant isolation and authorization.
- REQ-002 for CRM module activation and permission registration.
- REQ-003 for shared files, audit, search/list patterns, and export.

## Acceptance criteria

- The Party model and master-data ownership register are approved before cross-module consumers are added.
- Lead lifecycle and field validation scenarios pass with role and tenant checks.
- Duplicate warnings cover agreed exact/near-match scenarios.
- Valid import rows are created, invalid rows include row-level reasons, and failed rows can be retried without duplicating successful records.
- Lead detail shows the required chronological activity timeline and attachments.
- Status/owner history records actor, timestamp, and old/new values.
- Lead conversion preserves the source lead and links the resulting Party, Contact, and Deal identities without duplicate Party creation.

## P1 follow-on

Permission-controlled duplicate merge with preserved references and history (`CRM-LEAD-004`) is tracked in REQ-007.
