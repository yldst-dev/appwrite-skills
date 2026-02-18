---
name: appwrite-development
description: Appwrite backend and API integration patterns for Cloud or self-hosted deployments. Use when building or reviewing Appwrite Auth, TablesDB/Databases, Storage, Functions, Realtime, SDK usage, REST usage, and migration from collections/documents to tables/rows.
---

# Appwrite Development

Use this skill when building, reviewing, or debugging Appwrite-based backends and integrations with current Appwrite docs.

## Current Platform Notes

- Appwrite introduced `TablesDB` with relational terminology.
- Mapping is `Collections -> Tables`, `Documents -> Rows`, `Attributes -> Columns`.
- Old collection/document methods still work for maintenance and compatibility, but new database features land in TablesDB.
- Reference change date: August 26, 2025 changelog entry.

## Start Checklist

- Confirm deployment target: Cloud or self-hosted.
- Confirm endpoint format and region: `https://<REGION>.cloud.appwrite.io/v1` on Cloud.
- Separate project IDs and secrets across dev, stage, prod.
- Pick auth mode per call path: session, JWT, or API key.
- Prefer TablesDB APIs for all new database work.

## API Mode Selection

### Client SDK/API

- Use active user sessions and resource permissions.
- Use for user-facing flows.

### Server SDK/API

- Authenticate with API keys and scoped access.
- API key scope governs access and ignores user-level permissions.

### JWT Mode

- Use when server acts on behalf of a signed-in user.

### REST Headers To Remember

- `X-Appwrite-Project` and `Content-Type: application/json`.
- `X-Appwrite-Key` only on trusted server paths.
- `X-Appwrite-JWT` when using JWT auth.

## Database Workflow

1. Create database and tables.
2. Define columns and required constraints.
3. Create indexes for every frequent query path.
4. Configure permissions and row security before exposing endpoints.
5. Use `createRow`, `getRow`, `listRows`, `updateRow`, `deleteRow`.
6. Use `createTransaction` and `createOperations` for atomic multi-step writes.
7. Use `Query.select` to keep payloads minimal and relationship loading explicit.

## Permissions Rules That Matter

- Permissions are grant-based and private by default.
- Table-level and row-level permissions are additive; one matching grant is enough to access a row.
- Row-level controls require row security enabled on the table.
- If created from Server SDK or Console without explicit permissions, resources default to no user access.
- If created from Client SDK without explicit permissions, creator gets default read/update/delete on that resource.
- Avoid broad `Role.any()` on mutable resources.

## API Key Scope Hygiene

- Issue separate API keys per service boundary.
- Grant minimum scopes such as `rows.read`, `rows.write`, `tables.read`, `tables.write` only when required.
- Rotate keys and replace old keys after rollout.
- Never expose API keys in browser or mobile builds.

## Security Controls

- Add allowed platforms to enforce CORS boundaries.
- Assume client rate limits apply, but do not rely on rate limits as authorization.
- Remember: Server SDK with API key is not subject to client rate limits.
- Use encrypted text columns for sensitive values.
- Encrypted columns cannot be queried; design separate search-safe columns if filtering is required.
- Use audit logs for tables and rows, and export externally for longer retention.

## Functions and Realtime

- Keep function handlers idempotent and deterministic.
- Use strict timeouts for external calls.
- Return stable error codes and avoid leaking internals.
- Subscribe to narrow realtime channels and de-duplicate event handling.

## Troubleshooting

- `401/403`: Validate session/JWT, permission strings, and API key scopes.
- Empty reads: Check row security and both table-level plus row-level grants.
- Slow list queries: Add missing indexes and reduce selected fields.
- API failures: Verify endpoint region, project ID, and auth header type.
- Unexpected access: Audit role grants and inherited permissions.

## References

- https://appwrite.io/docs
- https://appwrite.io/docs/products/databases
- https://appwrite.io/docs/products/databases/tables
- https://appwrite.io/docs/products/databases/permissions
- https://appwrite.io/docs/products/databases/queries
- https://appwrite.io/docs/products/databases/quick-start
- https://appwrite.io/docs/references
- https://appwrite.io/docs/references/cloud/server-nodejs/tablesDB
- https://appwrite.io/docs/references/cloud/client-web/tablesDB
- https://appwrite.io/docs/apis/rest
- https://appwrite.io/docs/advanced/platform/permissions
- https://appwrite.io/docs/advanced/platform/api-keys
- https://appwrite.io/docs/advanced/platform/rate-limits
- https://appwrite.io/docs/advanced/security/encryption
- https://appwrite.io/docs/advanced/security/audit-logs
- https://appwrite.io/changelog/entry/2025-08-26-2
- https://appwrite.io/docs/products/auth
- https://appwrite.io/docs/products/functions
- https://appwrite.io/docs/products/storage
