---
name: appwrite-react-vite
description: Appwrite integration for React projects created with Vite. Use when implementing Appwrite Auth, TablesDB access, session flows, and secure frontend-to-backend boundaries in React SPA setups.
---

# Appwrite React Vite

Use this skill for React SPA projects using Vite and Appwrite.

## Verified Baseline

- React quick start uses `npm create vite@latest my-app -- --template react`.
- Install SDK with `npm install appwrite`.
- Configure endpoint as `https://<REGION>.cloud.appwrite.io/v1` and set project ID.

## Setup Workflow

1. Create Appwrite project and register Web platform for local/prod origins.
2. Scaffold React app with Vite.
3. Add a dedicated Appwrite client module for browser-safe SDK usage.
4. Implement signup, login, logout, and current-user bootstrapping.
5. Add server-side path for privileged actions instead of API key use in browser.

## Environment Rules

- Keep endpoint and project ID in Vite env variables.
- Use only `VITE_` prefixed variables in client runtime.
- Keep secrets and API keys outside Vite client env.

## Auth Pattern

- Use `Account.create`, `createEmailPasswordSession`, `get`, `deleteSession`.
- On app bootstrap, attempt `account.get()` and hydrate auth state.
- Handle session expiration and unauthorized responses centrally.

## Data Pattern

- Use client-side reads only for user-permitted resources.
- Use backend or Appwrite Functions for admin operations.
- Prefer TablesDB APIs for new database development.

## Security Guardrails

- Never ship API keys to browser bundles.
- Avoid `Role.any()` on mutable resources.
- Enforce permission checks server-side for critical writes.
- Reduce overfetching with `Query.select`.

## Common Pitfalls

- CORS errors from missing domain in Appwrite platforms list.
- Broken auth state when startup user-check is skipped.
- Permission confusion when table grants exist but row grants do not.
- Large payloads from list queries without select and pagination.

## References

- https://appwrite.io/docs/quick-starts/react
- https://appwrite.io/docs/references/quick-start
- https://appwrite.io/docs/products/auth
- https://appwrite.io/docs/products/databases/tables
- https://appwrite.io/docs/products/databases/permissions

