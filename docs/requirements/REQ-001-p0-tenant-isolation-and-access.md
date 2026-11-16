# REQ-001 — P0 Tenant Isolation and Access

**Priority:** P0  
**Goal:** Establish a secure tenant-aware foundation before any tenant business data or module functionality is exposed.

## Source requirements

- `CORE-001` — tenant and organisation lifecycle management.
- `CORE-002` — authentication, logout, password reset, session management, account recovery.
- `CORE-003` — user, role, permission, and invitation management.
- `TEN-001`–`TEN-005` — tenant context and isolation across application and data services.
- `SEC-001`–`SEC-007` — server-side authorization, tenant context, privileged MFA/audit, secrets, encryption, file access, and critical/high security findings.
- SRS sections 3 and 16 — role/record access model and security baseline.

## Scope

- Register, configure, suspend, and reactivate tenants and organisations.
- Authenticate users, manage sessions and account recovery, and enforce role-based permissions on every protected server operation.
- Derive tenant context from authenticated identity and authorised membership; reject arbitrary client-supplied tenant identifiers.
- Apply tenant isolation consistently to database access, APIs, caches, files, search, exports, reports, queues, scheduled jobs, and background processing.
- Use tenant-aware records or equivalent enforced ownership, with PostgreSQL RLS or an approved equivalent as defense in depth.
- Support MFA for privileged accounts and audit privileged actions.
- Protect secrets from source code and client bundles; use TLS and configure production at-rest encryption for data stores and backups.
- Enforce tenant-scoped file authorization and expiring access links where applicable.

## Dependencies

None. This is the first P0 workstream and a prerequisite for REQ-002 through REQ-006.

## Acceptance criteria

- Tenant and organisation create/update/suspend/reactivate scenarios pass, with no cross-tenant access.
- Tenant context is validated from authenticated membership; tampered headers and request bodies cannot select another tenant.
- Negative authorization tests deny protected API, file, export, and background-job access without the required tenant membership and role.
- Isolation tests cover database queries, caches, files, search, reports/exports, queues, and scheduled jobs.
- Expired or revoked sessions are denied; account recovery and role/permission changes are tested.
- Privileged accounts can use MFA, and privileged actions record actor, tenant, timestamp, action, entity, and outcome.
- Secret scanning finds no committed secrets; production transport and at-rest encryption configuration is evidenced.
- No critical/high security finding remains open unless formally risk-accepted before release.

## Open decisions

The shared PostgreSQL plus RLS approach is a proposed SRS default, not an approved architecture decision. The Solution Architect and Security Lead must approve the tenancy model and any dedicated database/deployment exception before schema finalization.
