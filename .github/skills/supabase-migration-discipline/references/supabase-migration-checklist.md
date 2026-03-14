# Supabase Migration Checklist

## Before Writing SQL
- confirm whether remote dashboard drift exists
- pull remote drift into a migration before starting new schema work
- decide whether the change needs schema, seed, policy, or type updates

## Before Merge
- new migration file exists when schema changed
- local reset-based validation completed
- migration plan updated when rollout or compatibility is non-trivial
- generated types and seed data updated when relevant

## Before Release
- staging and production project mapping is explicit
- `supabase db push` ownership is clear
- rollback and recovery constraints are documented
- no one is relying on a markdown record of executed migrations