---
name: appwrite-nextjs-fullstack
description: Appwrite integration patterns for Next.js App Router and SSR auth. Use when building Next.js fullstack apps with Appwrite Auth, TablesDB, Server Actions, route handlers, session cookies, and secure server/client API boundaries.
---

# Appwrite Next.js Fullstack

Use this skill for Next.js projects that integrate Appwrite with App Router and server-side auth.

## Verified Baseline

- Quick start uses `npx create-next-app@latest`.
- Recommended setup includes App Router.
- Install SDK with `npm install appwrite`.
- Add `localhost` platform in Appwrite Console for local web development.

## Architecture Split

- Browser paths use Client SDK with user session context.
- Privileged paths use Server SDK in trusted server runtime only.
- Never expose API keys in browser bundles or client components.

## Setup Workflow

1. Create Appwrite project and register Web platform for local and production domains.
2. Create a shared client initializer for browser-safe calls.
3. Create a server-only Appwrite client for privileged operations.
4. Keep environment values in server env vars, and only expose safe public values when required.
5. Build auth flow first, then data access with explicit permission model.

## SSR Auth Pattern

- Use an admin client for session creation flows.
- Use a session client per request by reading `a_session_<PROJECT_ID>` cookie.
- Ensure API key includes `sessions.write` scope for server-side session creation.
- Set secure cookie attributes (`httpOnly`, `secure`, `sameSite`) and short expiry aligned with risk profile.
- Forward user-agent when needed for SSR request consistency.

## Next.js Implementation Rules

- Keep Appwrite server client creation in server-only modules.
- Use route handlers or server actions for operations requiring API keys.
- Keep client components limited to user-scoped operations.
- Use middleware only for lightweight session checks, not heavy data operations.
- Enforce authorization again on server even if UI already checks roles.

## TablesDB In Next.js

- Prefer TablesDB (`createRow`, `listRows`, `updateRow`, `deleteRow`) for new work.
- Enable row security on sensitive tables.
- Use `Query.select` to reduce payload and accidental data leakage.
- Add indexes for query paths before traffic scales.

## Security Guardrails

- One API key per service boundary and environment.
- No write access from public client for admin-critical data.
- Keep tenant checks server-side for every multi-tenant query.
- Log denied access attempts and abnormal query volume.

## Troubleshooting

- `401/403`: verify cookie name, session propagation, key scopes, table/row permissions.
- Empty rows: verify row security and row-level grants.
- CORS errors: verify platform hostnames in Appwrite Console.
- Session mismatch in SSR: verify cookie domain/path and `secure` behavior by environment.

## References

- https://appwrite.io/docs/quick-starts/nextjs
- https://appwrite.io/docs/products/auth/server-side-rendering
- https://appwrite.io/docs/tutorials/nextjs-ssr-auth
- https://appwrite.io/docs/references/quick-start
- https://appwrite.io/docs/products/databases/tables
- https://appwrite.io/docs/products/databases/permissions

