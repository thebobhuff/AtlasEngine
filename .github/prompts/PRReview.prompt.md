---
description: Review a pull request using repository evidence, summarize what changed, run an iterative code-review loop, and recommend merge or fixes with concrete repair guidance.
name: PRReview
argument-hint: PR number, URL, branch comparison, or review scope
agent: pr-reviewer
tools:
  - search
  - edit
  - agent
  - fetch
---

# PR Review Workflow

The pull request to review is: ${input:pr:PR number, URL, branch comparison, or review scope}

Use a review folder under `ai_docs/reviews/<pr-review-slug>/` as the working directory for PR review artifacts.

Relevant reusable skills for this workflow:

- [handoff-composer](../skills/handoff-composer/SKILL.md)
- [quality-gates-executor](../skills/quality-gates-executor/SKILL.md)
- [release-safety-pack](../skills/release-safety-pack/SKILL.md)
- [github-hardening-branch-airgap](../skills/github-hardening-branch-airgap/SKILL.md)

## Preconditions

- Resolve the pull request from the provided input before starting review.
- Use repository evidence first: changed files, diff context, linked artifacts, CI or validation evidence, and PR metadata when available.
- If the review folder already exists, resume from it instead of restarting.
- Maintain all generated files inside the review folder.
- Use the markdown templates in `ai_docs/templates/` when creating or normalizing review artifacts.
- If the PR cannot be resolved from the available environment, mark the workflow blocked in `PR_REVIEW_STATE.md`, record the exact blocker in `PR_REVIEW_REPORT.md`, and stop before specialist review.

## Required Artifacts

Create and maintain these files during the workflow:

- `PR_REVIEW_STATE.md` using `ai_docs/templates/PR_REVIEW_STATE.template.md`
- `PR_REVIEW_REPORT.md` using `ai_docs/templates/PR_REVIEW_REPORT.template.md`
- `TEAM_MEMORY.md` using `ai_docs/templates/TEAM_MEMORY.template.md`
- `DECISIONS.md` using `ai_docs/templates/DECISIONS.template.md`
- `AGENT_HANDOFFS.md` using `ai_docs/templates/AGENT_HANDOFFS.template.md`

## Workflow

### 1. Review Initialization

Owner: `pr-reviewer`

Resolve or create the review folder, initialize the shared memory files, and record the PR identifier, branches when known, and current owner.

Update these files immediately:

- `PR_REVIEW_STATE.md`
- `TEAM_MEMORY.md`
- `DECISIONS.md`
- `AGENT_HANDOFFS.md`

Record the current phase as `PR Intake`, the active owner, and the evidence sources to inspect.

### 2. PR Intake And Summary

Owners: `pr-reviewer` and `github-devops-engineer` when GitHub metadata or required-check context is needed

Gather the core review evidence:

- PR title, identifier, URL, source branch, and target branch when available
- summary of changed files or areas
- stated intent of the PR
- linked issue, initiative, or bug context when available
- current validation evidence, required checks, and review status when available

Write the concise summary into `PR_REVIEW_REPORT.md` before detailed findings begin.

### 3. Specialist Review Pass

Owner: `pr-reviewer`

Use specialist reviewers when relevant to the changed surface area:

- `senior-engineer` for correctness, maintainability, and implementation tradeoffs
- `solution-architect` for boundaries, integration risk, and architectural fit
- `security-reviewer` for trust, abuse, secrets, and residual-risk concerns
- `qa-strategist` or `test-engineer` for validation gaps, regression risk, and test sufficiency
- `platform-release-engineer` for rollout, migration, rollback, or operational-readiness implications
- `github-devops-engineer` for required checks, branch-policy, or PR-control concerns

Capture all material findings in `TEAM_MEMORY.md` and synthesize them into `PR_REVIEW_REPORT.md`.

### 4. Review Loop

Loop owners:

- Synthesis and recommendation owner: `pr-reviewer`
- Specialist contributors: relevant review specialists from step 3

This is a required loop when blockers or missing evidence remain.

Rules:

- The review must perform at least 1 full synthesis cycle.
- It may go through up to 3 cycles when the PR is revised or when missing evidence is later supplied.
- Stop early only if the PR is clearly merge-ready after a full review cycle.
- If blocking findings remain after 3 cycles, stop and clearly mark the PR `Fix Before Merge` or `Blocked`.
- Do not recommend `Merge` if material findings, missing validation evidence, or unresolved release-safety concerns remain.

Use this control flow exactly:

1. Review the current PR evidence and changed code.
  Owner: `pr-reviewer`
2. Update `PR_REVIEW_REPORT.md` with current findings, severity ordering, summary, and recommendation.
  Owner: `pr-reviewer`
3. Update `PR_REVIEW_STATE.md` with:
  Owner: `pr-reviewer`
  - current cycle number
  - current status
  - recommendation
  - blocking findings
  - missing evidence
  - next required action
4. If the PR is merge-ready, exit the loop with recommendation `Merge`.
5. If the PR needs fixes or more evidence and fewer than 3 cycles have completed, return a concrete repair handoff using [handoff-composer](../skills/handoff-composer/SKILL.md) and wait for revision or evidence updates.
6. If 3 cycles have completed and blockers remain, stop and mark the outcome `Fix Before Merge` or `Blocked`.

### 5. Merge Recommendation Rules

Owner: `pr-reviewer`

Choose one of these outcomes:

- `Merge`: the PR is clear, materially correct, adequately validated, and safe to merge
- `Fix Before Merge`: the PR has actionable blocking issues that should be addressed before merge
- `Blocked`: the review cannot be completed because critical evidence, access, or validation context is missing

The recommendation must include:

- a concise summary of what the PR does
- the most important findings ordered by severity
- validation status and missing evidence
- exact suggestions on how to fix the blocking or risky issues
- release-safety implications when relevant

Use [quality-gates-executor](../skills/quality-gates-executor/SKILL.md) to judge whether the available test and validation evidence is sufficient for merge readiness.

Use [release-safety-pack](../skills/release-safety-pack/SKILL.md) when the PR changes deployment, migration, rollback, or release verification expectations.

Use [github-hardening-branch-airgap](../skills/github-hardening-branch-airgap/SKILL.md) when required checks, approvals, or branch-protection expectations affect the review decision.

### 6. Completion Gate

The workflow is complete only when:

- `PR_REVIEW_REPORT.md` includes the PR summary, findings, validation status, recommendation, and fix suggestions when needed
- `PR_REVIEW_STATE.md` reflects the final cycle status and next action
- `TEAM_MEMORY.md` captures the important findings and unresolved concerns
- `AGENT_HANDOFFS.md` records the final repair or merge handoff

## Output Rules

- Ground the review in repository evidence and available PR metadata.
- Prefer behavioral, safety, validation, and maintainability findings over cosmetic comments.
- Keep findings specific enough that the author can act on them immediately.
- If no material findings are discovered, say so explicitly and still note any residual validation risk.

Finish by summarizing the PR, listing the key findings in severity order, stating whether you recommend merge or fixes, and giving the exact next action for the PR author or reviewer.