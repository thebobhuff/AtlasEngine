# Versioning And Release Policy

## Semantic Versioning

This repository uses semantic versioning with the format `MAJOR.MINOR.PATCH`.

- Increment `MAJOR` for breaking changes.
- Increment `MINOR` for backward-compatible feature additions.
- Increment `PATCH` for backward-compatible fixes.

The source of truth for the current repository version is the root `VERSION` file.

## Release Tags

Git tags for releases should use the format `vX.Y.Z`.

Examples:

- `v1.2.0`
- `v2.0.0`

## Branch Promotion Flow

The intended promotion flow is:

1. current work -> `dev`
2. `dev` -> `staging`
3. `staging` -> `main`

The final `staging -> main` pull request requires human approval and must not be auto-merged.

## GitHub Releases

The repository uses GitHub Releases for formal release publication.

Release notes should be prepared before the final production merge is completed.
