# REQ-002 — P0 Module Registry and Lifecycle

**Priority:** P0  
**Goal:** Allow modules to be registered and activated per tenant only when their manifest, entitlement, dependencies, and Core compatibility are valid.

## Source requirements

- `CORE-004` — module catalogue, registry, versions, dependencies, tenant entitlements.
- `CORE-005` — navigation based on tenant entitlements and user permissions.
- `MOD-001`–`MOD-007` — validated manifest, activation checks, migrations, registration, backend enforcement, safe deactivation, and reactivation.
- SRS sections 5.3–5.6 — module boundaries, manifest, and lifecycle requirements.

## Scope

- Register modules with a unique ID and validated manifest including version, compatible Core range, dependencies, owned data/migrations, permissions, APIs/events, navigation, configuration, and entitlement keys.
- Validate tenant entitlement, licensing, Core compatibility, and required/optional dependencies before activation.
- Run repeatable, versioned migrations with status and clear failure reporting.
- Register approved APIs, permissions, routes, navigation, and event subscriptions on activation.
- Enforce entitlement on backend APIs and background jobs, not only in the user interface.
- On deactivation, block new access and processing without silently deleting retained customer data; allow authorised reactivation of retained data.

## Dependencies

- REQ-001 for authenticated tenant context and server-side authorization.

## Acceptance criteria

- A valid sample module registers; missing or invalid required manifest attributes are rejected with actionable errors.
- Activation succeeds only for valid entitlement, dependency, and Core compatibility combinations; invalid combinations are denied.
- Clean-install and upgrade migrations run repeatably, report status, and fail visibly without reporting success.
- A module is visible only to entitled and authorised users; direct API and job attempts are denied when it is unlicensed or deactivated.
- Deactivation retains data and leaves unrelated licensed modules usable; reactivation restores only permitted access to retained data.
- Registry, activation, and deactivation outcomes are inspectable and auditable.

## Out of scope for this task

Independent deployment of every module is not a Release 1 requirement. The SRS prefers a modular monolith initially; service extraction requires separate justification and approval.
