---
description: Evaluate the repository against DevOps and release standards, identify deficiencies, and produce a markdown report with GitHub hardening, production branch airgap, PR review, CI/CD, and release-management recommendations.
name: DevOpsAudit
argument-hint: repository scope, branch policy target, or audit focus
agent: github-devops-engineer
tools:
  - search
  - edit
  - agent
---

# DevOps Audit

The repository scope to audit is: ${input:scope:full repository, production branch safety, release management, PR review controls, or other DevOps focus}

Your job is to evaluate the current repository against this workspace's DevOps and release standards, identify deficiencies, and write a markdown report of the gaps and recommended fixes.

Relevant reusable skills for this workflow:

- [github-hardening-branch-airgap](../skills/github-hardening-branch-airgap/SKILL.md)
- [release-safety-pack](../skills/release-safety-pack/SKILL.md)

Use [github-hardening-branch-airgap](../skills/github-hardening-branch-airgap/SKILL.md) for production branch airgap expectations, GitHub control classification, and unverified-settings guidance.

Create or update `ai_docs/DEVOPS_DEFICIENCIES.md` using `ai_docs/templates/DEVOPS_DEFICIENCIES.template.md`.

## Standards To Evaluate Against

Use these files as the repository standards baseline:

- `ai_docs/VERSIONING.md`
- `ai_docs/WORKFLOW.md`
- `.github/pull_request_template.md`
- `.github/release.yml`
- `.github/prompts/Ship.prompt.md`
- `.github/agents/github-devops-engineer.agent.md`

If those files imply a standard but the repository does not currently provide evidence for it, record the gap explicitly instead of assuming the control exists.

## Repository Areas To Inspect

Inspect the repository for evidence of the following:

- GitHub Actions workflows under `.github/workflows/`
- `CODEOWNERS`
- branch-protection and production-airgap evidence stored in repository docs or config
- release automation and semantic-version evidence
- PR review standards and required-check evidence
- rollback, deployment, and release verification support files
- security and DevOps hygiene files such as Dependabot, security policy, secret-scanning guidance, or other repository-level controls when present

## Required Audit Categories

Evaluate production branch airgap, PR review controls, CI/CD coverage, release management, GitHub Releases discipline, deployment and rollback controls, security-sensitive DevOps controls, and release traceability. Mark any GitHub setting that cannot be proven from repository evidence as `Unverified` and recommend the exact ruleset or branch-protection control needed.

## Severity Model

Use this severity model in the audit report:

- Critical: production or release safety is materially exposed
- High: a major control is missing or unverifiable
- Medium: the process is workable but weak or manual
- Low: useful hardening or clarity improvement

## Output Rules

- Ground all findings in repository evidence or clearly mark them as unverified platform settings.
- Prefer specific, actionable findings over generic DevOps advice.
- Include concrete recommendations for airgapping `main`, tightening PR review, and improving release management.
- End with the top-priority hardening actions in execution order.

Begin by inspecting the repository's GitHub and release-management surface area, then produce `ai_docs/DEVOPS_DEFICIENCIES.md`.