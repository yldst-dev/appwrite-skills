---
name: supabase-to-appwrite-migration
description: Supabase to Appwrite migration workflow and risk controls. Use when planning or executing migrations of auth users, database rows, storage objects, and post-migration permission hardening.
---

# Supabase To Appwrite Migration

Use this skill when moving an existing Supabase project to Appwrite.

## Verified Baseline

- Appwrite provides a Supabase migration flow in Console.
- Migration is initiated from Project Settings migration area.
- You provide source credentials, choose resources, then start migration.

## Migration Workflow

1. Read migration overview and source-specific limitations first.
2. Gather required Supabase credentials.
3. Create or choose target Appwrite project.
4. Start migration wizard and select Supabase.
5. Select resources and run migration.
6. Validate auth/data/storage integrity before production cutover.

## What To Validate After Import

- User account counts and key identity fields.
- Table/row counts and spot checks on critical entities.
- File/object accessibility and metadata integrity.
- Permission model on all migrated resources.
- App clients registered under Appwrite platforms.

## Known Limitation Areas

- Some source-specific features do not map one-to-one.
- Postgres-specific advanced features can require manual redesign.
- OAuth users can require re-authentication after migration.
- Functions are typically migrated manually due to runtime differences.
- Some metadata fields may not transfer exactly.

## Security Hardening After Migration

- Rebuild permissions with least privilege instead of trusting imported defaults.
- Re-enable row security where required.
- Remove broad read/write grants before opening production traffic.
- Move privileged write paths to server-only clients.
- Rotate all service keys before production launch.

## Cutover Strategy

1. Freeze source writes during final sync window.
2. Run final validation scripts and manual critical-path checks.
3. Switch client endpoints and environment settings.
4. Monitor auth failures, permission denials, and data consistency.
5. Keep rollback window and source backup until confidence threshold is met.

## Cost and Operations Notes

- Appwrite Cloud migration processing is not billed as normal project usage.
- Source provider transfer or egress charges may still apply.

## References

- https://appwrite.io/docs/advanced/migrations
- https://appwrite.io/docs/advanced/migrations/supabase
- https://appwrite.io/docs/products/databases/permissions
- https://appwrite.io/docs/advanced/platform/permissions
- https://appwrite.io/docs/advanced/platform/api-keys

