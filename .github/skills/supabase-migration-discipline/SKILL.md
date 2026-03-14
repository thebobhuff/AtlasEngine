---
name: supabase-migration-discipline
description: 'Apply Supabase migration discipline across planning, implementation, review, and shipping. Use when changes affect the supabase folder, database schema, RLS policies, SQL functions, seed data, or remote schema drift.'
argument-hint: 'initiative folder, Supabase schema change, migration review, or release scope'
user-invocable: true
---

# Supabase Migration Discipline

Use this skill whenever work touches Supabase-managed database structure, RLS policies, SQL functions, seed data, or release sequencing for database changes.

See `ai_docs/SUPABASE.md` for repository-wide conventions.

## When To Use

- Planning an initiative that changes Supabase schema or data compatibility
- Implementing a feature that changes `supabase/migrations/`, `supabase/seed.sql`, or other Supabase project files
- Reviewing a PR that modifies Supabase SQL, policies, functions, or generated database types
- Shipping a change that requires `supabase db push`, staged promotion, backfill, or rollback coordination
- Cleaning up remote drift created outside version-controlled migrations

## Procedure

1. Decide whether the change is schema-affecting, data-affecting, policy-affecting, or operational-only.
2. If a remote Supabase project already exists and may contain dashboard-created drift, require `supabase db pull` before adding new migrations.
3. Create a forward migration with `supabase migration new <name>` or `supabase db diff -f <name>`.
4. Validate the full chain locally with `supabase db reset` before treating the migration as ready.
5. Update `MIGRATION_PLAN.md` when rollout order, compatibility windows, backfill steps, or rollback constraints matter.
6. Keep generated types, seed data, and application code aligned with the new schema when those assets exist.
7. During review or shipping, confirm the environment promotion path and whether `supabase db push` is handled manually or by CI.

## Rules

- Treat `supabase/migrations/*.sql` as the version-controlled schema history.
- Treat Supabase migration history and `supabase migration list` as the source of truth for what has been applied.
- Do not maintain a markdown ledger of executed migrations.
- Do not rewrite or delete migrations that may already exist in shared environments.
- Prefer forward-fix migrations over history edits.
- Require deterministic local validation for reset-based workflows.
- Treat dashboard-only schema edits as drift that must be pulled back into git.

## Review Questions

- Does the change need a new migration file?
- Was remote drift pulled into version control before new work started?
- Can the entire migration chain be replayed cleanly on a reset database?
- Are seed data and generated types still accurate?
- Does `MIGRATION_PLAN.md` explain backfill, compatibility, and rollback behavior when needed?
- Is release execution using a controlled environment path instead of an undocumented local push?

## References

- [Supabase migration checklist](./references/supabase-migration-checklist.md)
- `ai_docs/SUPABASE.md`