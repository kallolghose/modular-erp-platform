# REQ-003 — P0 Shared Core Services and Operations

**Priority:** P0  
**Goal:** Deliver the reusable Core capabilities and operational baseline required by CRM and future modules.

## Source requirements

- `CORE-006`–`CORE-010` — notifications, files, configuration/settings, audit, common search/list patterns, integration error/activity history, responsive layouts.
- `CORE-013` — tenant-aware document storage, access, metadata, retention.
- `OPS-001`–`OPS-005` — separate environments, reproducible deployment, release recovery, encrypted/monitored backups, restore against approved RPO/RTO.
- SRS sections 5.7, 15, 17, 18 — API/event conventions, measurable quality targets, operations, testing and release gates.

## Scope

- Provide shared notification, file/document, configuration, settings, and audit services rather than duplicating them in business modules.
- Provide consistent search, filtering, sorting, pagination, export, and integration activity/error patterns.
- Expose versioned documented APIs for web, mobile, and approved integrations; validate input and correlate requests.
- Use published module contracts for cross-module communication; define reliable event delivery, idempotency, retries, and dead-letter handling when asynchronous work is introduced.
- Support responsive desktop, tablet, and mobile-browser layouts.
- Establish development, QA, staging, and production environments; automate reproducible build/deployment from the client-controlled source repository.
- Automate, encrypt, and monitor database and file backups; rehearse restore and release recovery procedures.

## Dependencies

- REQ-001 for tenant-scoped authorization and file access.
- REQ-002 for module registration and consumers of shared services.

## Acceptance criteria

- At least one consuming module uses each applicable shared Core service.
- Cross-tenant file/document access is denied and retention metadata is applied according to an approved policy.
- Audit entries include actor, tenant, timestamp, action, entity, and outcome for important actions.
- Common list/search/filter/sort/pagination/export behavior is verified; exports obey the same authorization as on-screen data.
- API contracts are versioned and documented; integration failures and activity history can be inspected.
- All four environments are documented and access-reviewed; a clean deployment can be reproduced from client-controlled source.
- Database and file backups are automated, encrypted, and monitored; a recorded restore exercise meets the approved recovery objectives.
- Production changes follow an approved release and rollback/forward-recovery procedure.

## Baseline targets to validate

The SRS proposes API p95 latency of at most 2 seconds for standard read/write operations, 10 seconds for complex reports, an initial planning baseline of 100 tenants/500 concurrent active users, 99.5% monthly availability, RPO of at most 15 minutes, and RTO of at most 4 hours. Confirm workload, cost, and service targets with Product and Operations before treating them as contractual acceptance thresholds.
