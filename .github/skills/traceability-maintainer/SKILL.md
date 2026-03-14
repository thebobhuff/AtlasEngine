---
name: traceability-maintainer
description: 'Maintain requirement traceability from PRD to tasks, tests, end-to-end scenarios, and release outputs. Use when updating TRACEABILITY.md or checking whether requirements, validation, and release notes still align.'
argument-hint: 'initiative folder or traceability maintenance task'
user-invocable: true
---

# Traceability Maintainer

Use this skill whenever requirements, implementation tasks, validation artifacts, or release outputs need to stay linked.

## When To Use

- Updating `TRACEABILITY.md`
- Creating implementation tasks from requirements
- Aligning `TEST_PLAN.md` and `E2E_REPORT.md` to requirements
- Checking that release notes still map to delivered scope

## Procedure

1. Read `PRD.md` and extract requirement IDs or stable requirement statements.
2. Map each requirement to one or more task IDs in `TASKS.md`.
3. Map each requirement to planned test coverage in `TEST_PLAN.md` and observed validation in `TEST_REPORT.md` or `E2E_REPORT.md`.
4. Map user-visible delivered scope to `RELEASE_NOTES.md` when relevant.
5. Record uncovered requirements and validation gaps explicitly.
6. Reopen the relevant upstream artifact if traceability can no longer be maintained honestly.

## References

- [Traceability checklist](./references/traceability-checklist.md)
