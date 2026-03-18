---
description: Audit initiative, bug, review, eval, or repo-customization workflow state for missing artifacts, invalid transitions, stale downstream outputs, owner mismatches, and repair actions.
name: WorkflowHealth
argument-hint: initiative slug, bug run, review run, eval run, or workflow scope
agent: workflow-evaluator
tools:
  - search
  - edit
---

# Workflow Health

The workflow target to audit is: ${input:target:initiative slug, bug run, review run, eval run, or workflow scope}

Use the relevant workflow folder for the target as the working directory for this health pass.

Relevant reusable skills for this workflow:

- [initiative-state-manager](../skills/initiative-state-manager/SKILL.md)
- [handoff-composer](../skills/handoff-composer/SKILL.md)
- [workflow-evaluation-rubric](../skills/workflow-evaluation-rubric/SKILL.md)

## Preconditions

- Resolve whether the target is an initiative folder, bug run folder, review folder, eval folder, or repo-level customization scope.
- Read `ai_docs/WORKFLOW.md` first.
- Read `ai_docs/WORKFLOW_PROFILE.md` when it exists.
- Read the relevant state, memory, handoff, and artifact files before issuing findings.
- Reuse `ai_docs/templates/WORKFLOW_HEALTH_REPORT.template.md` for the report structure.

## Required Artifact

Create or update `WORKFLOW_HEALTH_REPORT.md` in the audited target folder.

For repo-level customization audits that are not tied to a single run folder, write the report under `ai_docs/evals/workflow-health/` using a dated run subfolder.

## Workflow

1. Resolve the target folder and artifact family.
2. Validate that the expected state and memory files exist for that workflow family.
3. Check for invalid stage transitions, stale downstream artifacts, contradictory owners, missing required handoffs, and missing index updates.
4. Classify each issue as Healthy, Needs Repair, or Blocked.
5. Write `WORKFLOW_HEALTH_REPORT.md` with concrete repair actions.

## Audit Rules

- Focus on state drift, resumability, and unsafe transitions rather than style preferences.
- Prefer repairable findings with explicit next actions over generic criticism.
- If a downstream artifact is stale because an upstream assumption changed, recommend reopening the upstream phase instead of patching around the inconsistency.
- Do not invent missing evidence. Mark it unknown or missing.
- Do not introduce a new runtime system or alternate state store. AtlasEngine remains markdown-first.

## Output Rules

- Update only the health report and any target state files that must be normalized to record the finding honestly.
- End by summarizing the overall status, highest-severity findings, and the next repair step.