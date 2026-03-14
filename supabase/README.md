# Supabase Workspace

This folder is the repository home for Supabase-managed assets.

## Starter Layout

- `migrations/`: version-controlled SQL migration history
- `functions/`: Supabase Edge Functions when used
- `tests/`: pgTAP or SQL database tests when used
- `seed.sql`: deterministic local seed data

## First-Time Setup

Run these commands from the repository root when you are ready to activate the scaffold:

1. `supabase init`
2. `supabase start`
3. If you already have a hosted project with dashboard-created drift, run `supabase link --project-ref <project-ref>` and then `supabase db pull`

The GitHub workflow files under `.github/workflows/` intentionally skip execution until `supabase/config.toml` exists.

## Environment Secrets For GitHub Actions

Configure these repository or environment secrets before enabling remote deployment:

- `SUPABASE_ACCESS_TOKEN`
- `SUPABASE_STAGING_PROJECT_REF`
- `SUPABASE_STAGING_DB_PASSWORD`
- `SUPABASE_PRODUCTION_PROJECT_REF`
- `SUPABASE_PRODUCTION_DB_PASSWORD`

## Notes

- Do not add a manual markdown ledger of applied migrations.
- Use `supabase migration list` to compare local and remote state.
- Use `supabase db push --dry-run` before remote apply steps when release safety matters.