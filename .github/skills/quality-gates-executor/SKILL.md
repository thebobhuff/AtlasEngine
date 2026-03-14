---
name: quality-gates-executor
description: 'Apply implementation quality gates consistently across tests, lint, typecheck, static analysis, security scans, performance checks, and accessibility checks. Use when deciding whether a wave passes, updating IMPLEMENTATION_STATE.md, or handing failed work back for fixes.'
argument-hint: 'implementation wave, quality gate scope, or test loop status'
user-invocable: true
---

# Quality Gates Executor

Use this skill when implementation work must be validated beyond basic test pass/fail.

## When To Use

- During implementation wave closeout
- When updating `IMPLEMENTATION_STATE.md`
- When deciding whether a wave may proceed
- When preparing failure feedback for developers

## Gate Families

- code tests
- lint
- typecheck
- static analysis
- security scan
- performance validation
- accessibility validation for user-facing work

## Procedure

1. Determine which gates are relevant for the current wave.
2. Record each gate as `Passed`, `Failed`, or `Not Applicable` with rationale when needed.
3. Treat the wave as incomplete until all relevant gates pass.
4. If a gate fails, route the work back with concrete failing items instead of generic "fix quality issues" guidance.
5. Update `TEST_REPORT.md` with failing tests and `IMPLEMENTATION_STATE.md` with non-test gate status.
6. Stop automatic retries when the workflow reaches the configured retry limit.
7. Ensure final acceptance and release verification reflect the latest gate outcomes.

## Failure Handoff Rules

- identify the exact failing gate
- identify the impacted task or subsystem
- describe the required repair clearly
- state whether retry budget remains

## References

- [Quality gate checklist](./references/quality-gate-checklist.md)
