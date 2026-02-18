---
name: appwrite-web-ts-js
description: Appwrite web SDK patterns for JavaScript and TypeScript projects. Use when implementing browser apps with Appwrite client initialization, auth flows, typed TablesDB usage, and incremental TS adoption.
---

# Appwrite Web TS JS

Use this skill for Appwrite integration in plain JavaScript or TypeScript web apps.

## Verified Baseline

- Web quick start supports npm package setup and CDN usage.
- SDK initialization requires endpoint and project ID.
- Docs include a TypeScript-focused section for typed row modeling.

## Setup Workflow

1. Register web platform domains in Appwrite Console.
2. Install `appwrite` package for module-based projects.
3. Create a dedicated client initializer.
4. Implement auth bootstrap and user session checks.
5. Add typed data-access layer for TablesDB operations.

## JavaScript Pattern

- Keep SDK usage in small service modules.
- Normalize error handling for auth and data APIs.
- Keep write paths behind explicit permission checks.

## TypeScript Pattern

- Extend Appwrite models for row typing where needed.
- Cast query results in one data-access layer, not across UI.
- Centralize collection/table IDs and query constants.
- Prefer narrow response projections with `Query.select`.

## API Boundary Rules

- Browser apps use Client SDK and user permissions.
- Privileged operations move to Server SDK or Functions.
- Never expose API keys to JS or TS browser runtime.

## Hardening Checklist

- Confirm row security on sensitive tables.
- Confirm least-privilege permissions for all roles.
- Confirm all write operations validate ownership server-side.
- Confirm no sensitive columns are overfetched.

## References

- https://appwrite.io/docs/quick-starts/web
- https://appwrite.io/docs/references/quick-start
- https://appwrite.io/docs/products/databases/tables
- https://appwrite.io/docs/products/databases/queries
- https://appwrite.io/docs/products/databases/permissions

