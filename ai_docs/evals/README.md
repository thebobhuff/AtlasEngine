# Workflow Evals

This folder stores lightweight workflow evaluations so the prompt-and-agent factory can be tested against representative scenarios before teams rely on it for real delivery.

## Structure

- `cases/`: reusable sample initiative scenarios
- `<eval-run-slug>/EVAL_PLAN.md`: the plan for a specific evaluation run
- `<eval-run-slug>/CASE_<case-slug>.md`: per-case scored results
- `<eval-run-slug>/EVAL_REPORT.md`: final report for the run

## Eval Approach

- Start with at least 2 sample cases for the targeted scope.
- Score coverage, artifacts, handoffs, safety and gating, resumability, and practicality.
- Record only concrete workflow findings that would matter in real use.
- Prefer repeating the same small case set over time so workflow changes are comparable.

## Suggested First Cases

- `greenfield-feature.md`
- `brownfield-feature.md`
- `risky-integration.md`
