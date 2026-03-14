---
name: github-hardening-branch-airgap
description: 'Audit and harden GitHub repository controls for production branch airgaps, PR reviews, CODEOWNERS, required checks, and release safety. Use when evaluating main branch protection, PR review policy, GitHub settings, CI/CD controls, or DevOps deficiencies.'
argument-hint: 'repository scope, branch hardening target, or GitHub controls audit'
user-invocable: true
---

# GitHub Hardening And Branch Airgap

Use this skill when auditing or strengthening GitHub-native controls around the production branch and release pipeline.

## When To Use

- Running a DevOps audit
- Reviewing `main` branch safety
- Checking PR review and CODEOWNERS posture
- Reviewing required checks and release-traceability controls

## Production Airgap Standard

- no direct pushes to `main`
- no force pushes to `main`
- no deletion of `main`
- PRs required for `main`
- human approval required before merging to `main`
- required status checks before merge
- limited bypass permissions
- release changes traceable to PRs, version updates, and release notes

## Procedure

1. Inspect repository evidence such as `.github/workflows/`, `CODEOWNERS`, PR templates, release config, and versioning docs.
2. Classify each control as `Present`, `Missing`, or `Unverified`.
3. Record `Unverified` when the control likely lives in GitHub settings rather than repo files.
4. For each missing or unverified production control, recommend the exact GitHub ruleset or branch-protection setting needed.
5. Review whether PR review expectations, required checks, and release artifacts create an auditable path from change to production.
6. Prioritize hardening actions in the order that most reduces production risk.

## References

- [GitHub hardening checklist](./references/github-hardening-checklist.md)
