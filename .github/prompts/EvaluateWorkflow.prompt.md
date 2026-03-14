---
description: Evaluate the workflow prompts, agents, and templates against sample initiative scenarios using a scoring rubric for coverage, artifacts, handoffs, gating, resumability, and practicality, and produce a scored report with concrete findings.
name: EvaluateWorkflow
argument-hint: workflow scope, phase, or eval target
agent: workflow-evaluator
tools:
  - search
  - edit
  - agent
---

# Workflow Evaluation

The workflow scope to evaluate is: ${input:scope:full workflow, a phase, or a specific prompt/agent set}

Use `ai_docs/evals/` as the working directory for this evaluation run.

Relevant reusable skills for this workflow:

- [workflow-evaluation-rubric](../skills/workflow-evaluation-rubric/SKILL.md)
- [initiative-state-manager](../skills/initiative-state-manager/SKILL.md)
- [handoff-composer](../skills/handoff-composer/SKILL.md)

Use [workflow-evaluation-rubric](../skills/workflow-evaluation-rubric/SKILL.md) for scoring dimensions, severity logic, and pass/pass-with-risk/fail criteria instead of inventing a custom rubric per eval run.

## Preconditions

- Read `ai_docs/WORKFLOW.md` first.
- Read the prompt files, agent files, templates, and any workflow artifacts relevant to the requested scope.
- Reuse `ai_docs/evals/README.md` and the sample cases under `ai_docs/evals/cases/`.

## Required Eval Artifacts

Create or maintain these files for each evaluation run under `ai_docs/evals/<eval-run-slug>/`:

- `EVAL_PLAN.md` using `ai_docs/templates/EVAL_PLAN.template.md`
- `EVAL_REPORT.md` using `ai_docs/templates/EVAL_REPORT.template.md`
- one `CASE_<case-slug>.md` result file per evaluated scenario using `ai_docs/templates/EVAL_CASE_RESULT.template.md`

## Workflow

1. Define the eval scope, cases, scoring dimensions, and success criteria in `EVAL_PLAN.md`.
2. Select at least 2 relevant cases from `ai_docs/evals/cases/` unless the user asked for a narrower targeted eval.
3. Evaluate the workflow against each case and write a separate case-result file.
4. Score the workflow using the standard dimensions from [workflow-evaluation-rubric](../skills/workflow-evaluation-rubric/SKILL.md).
5. Summarize the results in `EVAL_REPORT.md` with findings ordered by severity and recommended fixes.

## Evaluation Rules

- Be strict about missing gates, stale-state risks, contradictory agent ownership, and unverifiable transitions.
- Prefer a small number of strong findings over a long list of weak preferences.
- If the evaluated workflow passes, still record residual risks and blind spots.
- Keep the eval artifacts specific to the actual prompts, agents, and templates in this workspace.

## Output Rules

- Update only eval artifacts and any shared workflow docs explicitly needed to record the eval result.
- End by summarizing the eval scope, overall result, highest-severity findings, and the most important next fixes.