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
3. Map each requirement to concrete implementation evidence such as files, modules, migrations, or integration points changed during delivery.
4. Map each requirement to planned test coverage in `TEST_PLAN.md` and observed validation evidence in `TEST_REPORT.md` or `E2E_REPORT.md`.
5. Record the acceptance owner and any release, migration, rollout, or compatibility dependency tied to the requirement.
6. Map user-visible delivered scope to `RELEASE_NOTES.md` when relevant.
7. Record uncovered requirements, missing implementation evidence, and validation gaps explicitly.
8. Reopen the relevant upstream artifact if traceability can no longer be maintained honestly.

## References

- [Traceability checklist](./references/traceability-checklist.md)
