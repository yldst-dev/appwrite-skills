---
name: appwrite-db-security-design
description: Security architecture patterns for Appwrite TablesDB and Databases. Use when designing production data access models, multi-tenant isolation, permission schemes, API key scope boundaries, encrypted data handling, and audit-ready operations.
---

# Appwrite DB Security Design

Use this skill for security-first database architecture on Appwrite.
Use `security/operations-key-dependency-hardening/SKILL.md` together with this skill for runtime operations, key lifecycle, and dependency vulnerability governance.

## Security Objectives

- Enforce least privilege for every read and write path.
- Isolate tenants so no cross-tenant row access is possible.
- Keep all privileged operations on trusted server boundaries.
- Preserve auditability for sensitive data changes.
- Balance confidentiality, queryability, and operational simplicity.

## Core Facts To Design Around

- TablesDB is the current database API model for new features.
- Permissions are grant-based and default private.
- Table-level and row-level permissions are additive.
- Create permission for rows is controlled at table level; row-level controls are for existing-row access.
- Row-level control requires row security enabled on the table.
- Server API key access is scope-driven and bypasses user-level permissions.
- Client rate limits apply to client-authenticated access paths.

## Appwrite Row Security vs Supabase RLS

- Supabase RLS evaluates SQL policy predicates at query time.
- Appwrite Row Security evaluates permission grants attached to rows plus table-level grants.
- In Appwrite, a matching grant at either table level or row level can allow access to a row.
- Appwrite does not provide SQL policy clauses equivalent to `USING` and `WITH CHECK`.
- Business-rule checks that were in RLS predicates should move to trusted server logic or Functions.
- Server SDK with API key can access resources regardless of user-level permissions, so server-side checks remain mandatory.

## Threat Model Checklist

- Identify attacker profiles: anonymous internet user, authenticated user, compromised user account, compromised service key.
- Identify sensitive assets: PII, credentials, billing records, access-control metadata.
- Define trust boundaries: browser/mobile client, edge/backend, Appwrite project, third-party integrations.
- Define explicit abuse scenarios: horizontal data access, privilege escalation, mass scraping, key leakage.

## Recommended Architecture

1. Keep frontend on Client SDK with session-based auth only.
2. Put privileged workflows in Functions or backend services using Server SDK.
3. Split API keys by service and environment with minimum scopes.
4. Use one Appwrite project per environment and strict secret rotation.
5. Treat each table as private until permission requirements are fully tested.

## Permission Design Patterns

### Pattern A: User-Private Rows

- Table permissions: grant only `CREATE` to allowed authenticated users.
- Enable row security.
- On row creation, assign row-level `read/update/delete` only to `Role.user(<ownerId>)`.
- Do not grant table-level `READ/UPDATE/DELETE` to broad roles.

### Pattern B: Team Workspace

- Use team-scoped permissions for shared read access.
- Use team-role-scoped permissions for elevated write/delete.
- Keep admin overrides on server-only flows.

### Pattern C: Public Catalog With Restricted Writes

- Public read only on non-sensitive tables.
- Writes allowed only through server paths with strict validation.
- Never give direct client write permission to moderation-critical tables.

## Multi-Tenant Isolation Strategy

- Include `tenantId` on every tenant-bound row.
- Enforce tenant checks in server logic even when permissions look correct.
- Avoid wildcard permissions that span tenants.
- Keep tenant-admin actions in server-controlled functions.

## RLS Migration Pitfalls

- Migrating rows without recreating row-level permissions for each access pattern.
- Leaving broad table-level read or update grants that bypass intended per-row isolation.
- Assuming `tenantId` filters alone replace authorization.
- Migrating policy-driven write checks without equivalent server-side validation.

## API Key Security Model

- Create separate keys for runtime domains such as ingest, read-model, admin jobs.
- Grant only required scopes such as `rows.read` or `rows.write`.
- Avoid broad keys that include unrelated scopes.
- Rotate keys with staged cutover and immediate old-key revocation.
- Never place API keys in client bundles, public repos, or logs.

## Data Protection Decisions

- Use encrypted text columns for high-sensitivity fields.
- Plan around encrypted-column limitations: encrypted columns cannot be queried.
- For searchable sensitive attributes, store a derived index-safe value in a separate column and keep raw value encrypted.
- Minimize selected fields in queries and responses.

## Operational Security Controls

- Register only approved platform origins to enforce CORS boundaries.
- Apply anti-abuse controls and monitor 429 patterns.
- Track security-relevant events through Activity and audit logs.
- Export or mirror audit evidence outside Appwrite for retention requirements.

## Validation Plan Before Production

1. Run positive and negative permission tests for each role.
2. Verify cross-tenant access attempts fail for all endpoints.
3. Verify client paths fail on admin-only operations.
4. Verify leaked low-scope keys cannot perform sensitive operations.
5. Verify encrypted columns are not exposed through debug paths or logs.

## High-Risk Anti-Patterns

- Using `Role.any()` on mutable resources.
- Storing API keys in browser/mobile runtime.
- Granting table-level read access on tenant-sensitive tables.
- Depending on rate limits as a replacement for authorization.
- Building security solely in UI without server-side enforcement.

## References

- https://appwrite.io/docs/products/databases/permissions
- https://appwrite.io/docs/products/databases/rows
- https://appwrite.io/docs/advanced/platform/permissions
- https://appwrite.io/docs/advanced/platform/api-keys
- https://appwrite.io/docs/advanced/platform/rate-limits
- https://appwrite.io/docs/advanced/security/encryption
- https://appwrite.io/docs/advanced/security/audit-logs
- https://appwrite.io/docs/references/cloud/server-nodejs/tablesDB
- https://appwrite.io/changelog/entry/2025-08-26-2
