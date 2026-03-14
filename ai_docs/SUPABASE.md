# Supabase Workflow

This repository should treat Supabase database changes as code, not as ad hoc dashboard state.

## Source Of Truth

- `supabase/migrations/*.sql` is the version-controlled source of truth for schema changes.
- `supabase/config.toml` and other committed Supabase project files define local project behavior.
- The applied-migration source of truth is the database migration history managed by Supabase plus `supabase migration list`.
- `MIGRATION_PLAN.md` is the workflow artifact for rollout, compatibility, backfill, and rollback planning when a specific initiative changes data or schema.

Do not create a markdown ledger to remember which migrations have already run in shared environments. That state already exists in Supabase. A markdown file would drift and become a second, weaker source of truth.

## Recommended Repository Layout

Keep the standard Supabase folder under the repository root:

- `supabase/config.toml`
- `supabase/migrations/`
- `supabase/seed.sql` when deterministic seed data is useful
- `supabase/functions/` when Edge Functions are part of the product
- `supabase/tests/` when database tests are part of the product

Commit the `supabase/` folder to git. Supabase documents this as the normal workflow for local development, migration history, and environment promotion.

This repository now includes a starter `supabase/` scaffold plus GitHub Actions patterns under `.github/workflows/` for pull-request validation, staging apply, and production apply. The workflows are intentionally dormant until `supabase/config.toml` exists.

## Recommended Working Flow

For a new repository or first-time setup:

1. Run `supabase init`.
2. Run `supabase start` for local development.
3. If a hosted project already exists and has dashboard-created drift, run `supabase db pull` before introducing new local changes.

For every schema-affecting change:

1. Create a migration with `supabase migration new <name>` when writing SQL directly, or `supabase db diff -f <name>` when diffing from local changes.
2. Validate the full migration chain locally with `supabase db reset`.
3. Review the result with `supabase migration list` when environment state needs confirmation.
4. Commit the migration files, supporting config changes, and seed updates together.

For shared environments:

1. Prefer CI/CD or controlled release automation for `supabase db push`.
2. Map environments clearly: local database for current work, staging Supabase project for pre-production validation, production Supabase project for release.
3. Keep secrets and project references in CI secrets, not in markdown artifacts.

## GitHub Actions Pattern

The repository includes these workflow files:

- `.github/workflows/supabase-migration-check.yml`: validates local replay on pull requests
- `.github/workflows/supabase-staging.yml`: links to the staging project and runs `supabase db push` on `staging`
- `.github/workflows/supabase-production.yml`: links to the production project and runs `supabase db push` on `main`

Expected secrets:

- `SUPABASE_ACCESS_TOKEN`
- `SUPABASE_STAGING_PROJECT_REF`
- `SUPABASE_STAGING_DB_PASSWORD`
- `SUPABASE_PRODUCTION_PROJECT_REF`
- `SUPABASE_PRODUCTION_DB_PASSWORD`

## Cleanup And Drift Rules

- Do not delete or rewrite committed migrations that may already have reached a shared environment.
- Fix mistakes with a new forward migration unless the migration is strictly local and unshared.
- If someone changed the remote schema in the Supabase dashboard, capture that drift with `supabase db pull` and commit the generated migration before continuing.
- Keep `supabase/seed.sql` deterministic and safe for reset-based local validation. Do not treat it as a production snapshot.
- If generated types are part of the product, regenerate and commit them in the same change as the migration.

## How This Fits The AtlasEngine Workflow

- Use `MIGRATION_PLAN.md` only when an initiative needs rollout, compatibility, backfill, or rollback planning for a database change.
- Keep long-lived Supabase conventions in this file, not in initiative-specific memory.
- Use the reusable Supabase migration skill when prompts or agents need to reason about schema changes, migration validation, or Supabase environment promotion.

## Decision

- Add a stable repository guide for Supabase conventions: yes.
- Add a markdown file to remember executed migrations: no.
- Add a dedicated agent just for migrations: no.
- Add a reusable skill for consistent migration discipline across planning, implementation, review, and shipping: yes.