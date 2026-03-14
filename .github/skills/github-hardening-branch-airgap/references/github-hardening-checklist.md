# GitHub Hardening Checklist

## Check For Repository Evidence
- `.github/workflows/`
- `CODEOWNERS`
- `.github/pull_request_template.md`
- `.github/release.yml`
- `VERSION`
- versioning and release policy docs

## Mark As Unverified If Repo Evidence Cannot Prove
- branch protection rules
- rulesets
- required reviewers
- required status checks
- bypass permissions
- merge restrictions

## High-Severity Gaps
- production branch can be pushed directly
- no PR requirement for `main`
- no required approval before production merge
- no required checks for production merge
- no release traceability to version and release notes
