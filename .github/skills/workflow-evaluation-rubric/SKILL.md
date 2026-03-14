---
name: workflow-evaluation-rubric
description: 'Score workflow quality consistently across coverage, artifacts, handoffs, gating, resumability, and practicality. Use when creating EVAL_PLAN.md, CASE_<case>.md, EVAL_REPORT.md, or judging whether the prompt-and-agent system is structurally ready.'
argument-hint: 'eval scope, workflow phase, or scoring task'
user-invocable: true
---

# Workflow Evaluation Rubric

Use this skill when evaluating whether the workflow is structurally sound across sample scenarios.

## When To Use

- Creating `EVAL_PLAN.md`
- Scoring a case result
- Writing `EVAL_REPORT.md`
- Comparing workflow revisions over time

## Scoring Dimensions

- coverage
- artifact completeness
- handoff quality
- safety and gating
- resumability and state transfer
- practicality for humans and agents

## Procedure

1. Choose representative cases for the targeted workflow scope.
2. Define scoring expectations per dimension.
3. Evaluate the workflow end state, not just whether the prompts mention relevant concepts.
4. Record findings ordered by severity and tied to concrete prompts, agents, or artifacts.
5. Distinguish structural failures from optional improvements.
6. Produce an overall result of `Pass`, `Pass With Risk`, or `Fail`.

## References

- [Eval scoring checklist](./references/eval-scoring-checklist.md)
