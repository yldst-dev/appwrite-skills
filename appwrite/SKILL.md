---
name: appwrite-unified-platform
description: Unified Appwrite skill router for Next.js, Vite/React, React Native, Web TS/JS, fullstack architecture, Supabase migration, and production DB security design. Use when planning or implementing Appwrite across frontend, backend, and migration workflows.
---

# Appwrite Unified Platform

Use this skill as the entry point for Appwrite work. It routes to focused sub-skills by stack and objective.

## Scope

- Next.js fullstack with App Router and SSR auth.
- React with Vite.
- React Native and Expo.
- Plain Web JavaScript and TypeScript.
- End-to-end fullstack architecture.
- Supabase to Appwrite migration.
- Production DB security design.

## Skill Routing

- Next.js: read `frameworks/nextjs-fullstack/SKILL.md`.
- React or Vite: read `frameworks/react-vite/SKILL.md`.
- React Native or Expo: read `frameworks/react-native/SKILL.md`.
- Web TS/JS SDK usage: read `frameworks/web-ts-js/SKILL.md`.
- Multi-client system design: read `architecture/fullstack-architecture/SKILL.md`.
- Database and permission hardening: read `security/db-security-design/SKILL.md`.
- Supabase migration planning or execution: read `migrations/supabase-to-appwrite/SKILL.md`.
- General Appwrite primitives and API mode selection: read `core/development/SKILL.md`.

## Execution Order

1. Load `core/development/SKILL.md` for shared API and permissions baseline.
2. Load one framework skill matching the runtime.
3. If project is production or multi-tenant, also load security and architecture skills.
4. If migration is in scope, load migration skill before implementation changes.

## Non-Negotiable Rules

- API keys stay server-side only.
- Privileged writes run on Server SDK or Functions.
- Row security and least privilege are required for sensitive tables.
- Platforms must be allowlisted for web and mobile clients.
- Migration cutover requires data, auth, and permission validation before go-live.

## Official Doc Baseline

- Verified against Appwrite docs pages for quick starts, SSR auth, quick-start references, and Supabase migration.
- Verification date: 2026-02-18.

