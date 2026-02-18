---
name: appwrite-fullstack-architecture
description: Fullstack Appwrite architecture playbook across Next.js, React/Vite, React Native, and backend services. Use when designing end-to-end Appwrite auth, data, security boundaries, and production rollout strategy.
---

# Appwrite Fullstack Architecture

Use this skill for multi-client, production Appwrite systems.

## Scope

- Next.js web app with SSR and route handlers.
- React/Vite SPA clients.
- React Native mobile clients.
- Backend services and Appwrite Functions.

## API Mode Decision Model

- Client SDK: user-context operations in browser/mobile.
- Server SDK with API key: privileged backend/admin workflows.
- Session client on server: user-context SSR operations using session cookie.

## Standard Topology

1. Clients call backend routes for privileged operations.
2. Backend routes call Appwrite with scoped server keys.
3. User-level reads/writes stay on Client SDK when permissions are sufficient.
4. High-risk domain logic is centralized in backend or Functions.

## Environment Separation

- One Appwrite project per environment.
- One API key set per environment and service boundary.
- No cross-environment secrets reuse.
- Release promotion uses schema/permissions parity checks.

## Database and Permissions

- Prefer TablesDB for new implementations.
- Enable row security on sensitive tables.
- Design table-level and row-level grants intentionally.
- Add tenant boundary checks in server code for multi-tenant systems.
- Use transactions for multi-step consistency-critical writes.

## Fullstack Security Baseline

- API keys never leave trusted server runtime.
- Public clients never get admin-write capability.
- Sensitive columns use encryption strategy plus separate query-safe derivatives.
- Access anomalies are logged and reviewed.
- Platform allowlists are enforced for web and mobile apps.

## Reliability Patterns

- Idempotent write endpoints for retries.
- Deterministic error mapping between Appwrite errors and app-level errors.
- Request correlation IDs from client to backend logs.
- Pagination and index-first query design for list endpoints.

## Migration-Friendly Design

- Keep domain logic isolated from vendor-specific SDK wrappers.
- Centralize auth/session abstraction to reduce lock-in.
- Normalize data-access contracts in backend service layer.

## References

- https://appwrite.io/docs/references/quick-start
- https://appwrite.io/docs/products/auth/server-side-rendering
- https://appwrite.io/docs/products/databases/tables
- https://appwrite.io/docs/products/databases/permissions
- https://appwrite.io/docs/products/databases/transactions
- https://appwrite.io/docs/advanced/platform/api-keys
- https://appwrite.io/docs/advanced/platform/permissions

