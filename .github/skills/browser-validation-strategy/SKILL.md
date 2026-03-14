---
name: browser-validation-strategy
description: 'Convert PRD and TEST_PLAN scenarios into structured browser-based validation with key paths, edge cases, and failure evidence. Use when planning or running E2E validation, maintaining E2E_REPORT.md, or deciding what browser scenarios matter most.'
argument-hint: 'initiative folder, E2E scope, or browser validation target'
user-invocable: true
---

# Browser Validation Strategy

Use this skill when end-to-end browser validation must be systematic rather than ad hoc.

## When To Use

- Planning browser-based validation from `PRD.md` and `TEST_PLAN.md`
- Writing or updating `E2E_REPORT.md`
- Selecting key user journeys and edge cases for end-to-end checks

## Procedure

1. Extract the highest-value user journeys from `PRD.md` and `TEST_PLAN.md`.
2. Separate core happy paths from edge or failure paths.
3. Prioritize scenarios whose failure would block release or invalidate core requirements.
4. Run or plan browser steps with clear expected results.
5. Record failures as scenario-specific repair guidance, not generic browser problems.
6. Update `E2E_REPORT.md` with coverage, results, failed scenarios, and retry status.

## References

- [Browser validation checklist](./references/browser-validation-checklist.md)
