---
description: Turn approved planning artifacts into implementation tasks, execute them in waves with testing loops, quality gates, traceability maintenance, and finish with a retro.
name: Implementation
argument-hint: initiative slug, folder, or implementation target
agent: project-manager
tools:
  - search
  - edit
  - agent
  - browser
---

# Implementation Workflow

The initiative to implement is: ${input:initiative:initiative slug, folder name, or implementation target}

Use the initiative folder under `ai_docs/ideas/<initiative-slug>/` as the working directory for the implementation workflow.

Relevant reusable skills for this workflow:

- [initiative-state-manager](../skills/initiative-state-manager/SKILL.md)
- [traceability-maintainer](../skills/traceability-maintainer/SKILL.md)
- [quality-gates-executor](../skills/quality-gates-executor/SKILL.md)
- [browser-validation-strategy](../skills/browser-validation-strategy/SKILL.md)
- [handoff-composer](../skills/handoff-composer/SKILL.md)
- [release-safety-pack](../skills/release-safety-pack/SKILL.md)

## Preconditions

- Resolve the initiative folder from the provided input.
- Require `PLAN.md` to exist before implementation planning starts.
- Read `PLAN.md`, `UI.md`, `UX.md`, and `TEST_PLAN.md` as primary implementation inputs.
- Read `PRD.md`, `CODEBASE_RESEARCH.md`, `IMPLEMENTATION_PLAN.md`, `TRACEABILITY.md`, `DEPLOYMENT_PLAN.md`, `ROLLBACK_PLAN.md`, `MIGRATION_PLAN.md`, and `RELEASE_VERIFICATION.md` when they exist as supporting context.
- Read `PHASE2_STATE.md`, `TEAM_MEMORY.md`, `DECISIONS.md`, and `AGENT_HANDOFFS.md` before planning.
- If `TASKS.md`, `IMPLEMENTATION_STATE.md`, or `RETRO.md` already exist, resume from them instead of restarting from scratch.

## Required Artifacts

Create and maintain these implementation artifacts:

- `TASKS.md` using `ai_docs/templates/TASKS.template.md`
- `IMPLEMENTATION_STATE.md` using `ai_docs/templates/IMPLEMENTATION_STATE.template.md`
- `TEST_REPORT.md` using `ai_docs/templates/TEST_REPORT.template.md` during wave testing
- `E2E_REPORT.md` using `ai_docs/templates/E2E_REPORT.template.md` during end-to-end validation
- `RETRO.md` using `ai_docs/templates/RETRO.template.md` after all waves are complete

Keep these release-safety and traceability artifacts current when implementation changes them materially:

- `TRACEABILITY.md`
- `DEPLOYMENT_PLAN.md`
- `ROLLBACK_PLAN.md`
- `MIGRATION_PLAN.md`
- `RELEASE_VERIFICATION.md`

## Workflow

### 1. Engineering Team

Core engineering team:

- `project-manager` as implementation orchestrator
- `developer` for scoped coding tasks
- `senior-engineer` for technical judgment and dependency analysis
- `solution-architect` for system boundaries and integration constraints
- `product-manager-orchestrator` for product intent and scope clarification
- `test-engineer` for test strategy and test-loop enforcement
- `e2e-tester` for browser-based end-to-end validation against user scenarios

Supporting specialists when needed:

- `qa-strategist`
- `platform-release-engineer`
- `scrum-master`

### 2. Task Synthesis

Owner: `project-manager`

Take `PLAN.md`, `UI.md`, `UX.md`, and `TEST_PLAN.md` and turn them into executable engineering tasks in `TASKS.md`.

Each task must include:

- a task ID
- a wave number
- a serial or parallel mode tag: `[S]` or `[P]`
- an owner agent
- dependencies
- target files, code areas, or subsystems when known
- validation expectations

Use engineering judgment and dependency analysis to determine whether each task is serial or parallel.

Also build a dependency graph in `TASKS.md` using Mermaid.

Use [traceability-maintainer](../skills/traceability-maintainer/SKILL.md) to preserve the mapping from requirements to tasks and planned validation.

### 3. Wave Planning

Owner: `project-manager`

Group tasks into waves based on the dependency graph.

Wave rules:

- Tasks in later waves must not start until predecessor waves satisfy their exit criteria.
- `[P]` tasks in the same wave may run concurrently when dependencies allow.
- `[S]` tasks must run in dependency order, even if they share a wave.

Update `IMPLEMENTATION_STATE.md` with the current wave plan and task execution status.

### 4. Build Execution Loop

Owner: `project-manager`

Coordinate with these agents during execution:

- `developer` for implementation work
- `senior-engineer` for task slicing and technical feasibility
- `solution-architect` for system boundaries and integration concerns
- `product-manager-orchestrator` for scope clarifications and product tradeoffs
- `test-engineer` for test-loop enforcement
- `qa-strategist` for acceptance coverage
- `platform-release-engineer` for rollout and operational tasks

Execution control flow:

1. Start the next eligible wave.
2. Dispatch `[P]` tasks in that wave to subagents in parallel where appropriate.
3. Dispatch `[S]` tasks in dependency order.
4. After each task completion, update `TASKS.md` and `IMPLEMENTATION_STATE.md`.
5. If a task is blocked, record the blocker and decide whether other parallel work can continue.
6. Do not mark a wave complete until its exit criteria and testing loop have passed.

### 5. Testing Loop

Owner: `test-engineer`

Testing is mandatory during implementation.

Testing rules:

- Run relevant tests after meaningful code changes and at minimum at the end of each wave.
- If tests fail, the implementation loop is not complete.
- Failed tests must return work to the appropriate implementation task owners.
- Continue the cycle of implement -> test -> fix -> retest until the wave passes or 3 attempts have been used.
- Maintain `TEST_REPORT.md` as the durable test artifact for the current wave.
- After each attempt, record test outcomes, failures, and required fixes in `TEST_REPORT.md`, `IMPLEMENTATION_STATE.md`, `TASKS.md`, and `TEAM_MEMORY.md`.
- If the wave passes within 3 attempts, mark the test loop passed and continue.
- If the wave still fails after 3 attempts, mark the wave blocked, stop automatic retries, and use `TEST_REPORT.md` as the repair guide for the failing items.
- The wave is not considered passed until required quality gates also pass or are explicitly marked not applicable with rationale.

Use `TEST_PLAN.md` as the baseline for what must be validated.

Use [quality-gates-executor](../skills/quality-gates-executor/SKILL.md) for the applicable-gate checklist, `Not Applicable` rules, and failure handoff format.

Use this control flow exactly:

1. Run the relevant tests for the completed tasks or current wave.
2. Write or update `TEST_REPORT.md` with the result of the attempt.
3. Run the required quality gates and record their status in `IMPLEMENTATION_STATE.md`.
4. If the tests and quality gates pass, mark the wave test status as passed.
5. If the tests or quality gates fail and fewer than 3 attempts have been used, send the failing items back for fixes and retest.
6. If the tests or quality gates fail and 3 attempts have been used, mark the wave blocked and stop the test loop.

### 6. End-to-End Validation Loop

Owner: `e2e-tester`

After all required waves have passed the code testing loop, run end-to-end validation in the browser using the scenarios from `TEST_PLAN.md` and the requirements from `PRD.md`.

Use [browser-validation-strategy](../skills/browser-validation-strategy/SKILL.md) for scenario selection, key-path prioritization, and failure evidence.

Use this control flow exactly:

1. Run the required browser scenarios derived from `TEST_PLAN.md` and `PRD.md`.
2. Write or update `E2E_REPORT.md` with scenario coverage and results.
3. If E2E passes, mark the initiative ready for retro.
4. If E2E fails and fewer than 3 attempts have been used, send the failing scenarios back for fixes and rerun E2E.
5. If E2E fails and 3 attempts have been used, mark the implementation blocked and stop the E2E loop.

### 7. Acceptance And Release-Readiness Check

Owners: `product-manager-orchestrator`, `project-manager`, and `platform-release-engineer`

After the implementation and E2E loops pass, perform a final acceptance and release-readiness check before declaring the initiative ready for `/Ship`.

Use [traceability-maintainer](../skills/traceability-maintainer/SKILL.md) to confirm requirement coverage and [release-safety-pack](../skills/release-safety-pack/SKILL.md) to confirm deployment, rollback, migration, and release-verification readiness before shipping.

If acceptance fails, reopen the necessary implementation tasks instead of advancing to shipping.

### 8. Completion Gate

All waves must be complete before the workflow can finish.

Completion requirements:

- every task in `TASKS.md` is complete or explicitly blocked with rationale
- all required waves have passed their test loop
- required quality gates are passed or explicitly marked not applicable with rationale
- no unresolved blocking failures remain
- `TEST_REPORT.md` reflects the final passing state or the blocked failures for the most recent wave
- `E2E_REPORT.md` reflects the final passing state or the blocked end-to-end failures
- `RELEASE_VERIFICATION.md` reflects the current implementation-side readiness state

### 9. Retro

Owner: `project-manager`

Once all waves are complete, hold an engineering retro with the team and produce `RETRO.md`.

The retro should capture:

- what went well
- what slowed the team down
- testing lessons
- wave execution lessons
- prompt and agent improvements
- recommended changes for future `/Implementation` runs

### 10. Task Quality Rules

- Tasks must be specific enough for engineering execution.
- Include dependencies and sequencing where they matter.
- Separate implementation, validation, migration, observability, and release work.
- Do not invent code locations that are not supported by the current codebase context.
- Use `CODEBASE_RESEARCH.md`, `PLAN.md`, `UI.md`, `UX.md`, and `TEST_PLAN.md` to keep the breakdown grounded.
- Do not leave task ordering implicit; encode it with waves and dependency edges.
- Keep `TRACEABILITY.md` current enough that every shipped requirement still maps to implementation and validation evidence.

### 11. Shared Memory Updates

Update the shared phase-2 memory files throughout implementation:

- add implementation findings and blockers to `TEAM_MEMORY.md`
- add important implementation decisions to `DECISIONS.md`
- update `AGENT_HANDOFFS.md` with wave and testing handoff state
- update `PHASE2_STATE.md` to record that implementation is in progress, complete, or blocked
- update `ai_docs/ideas/INDEX.md` with the initiative's current phase, status, latest artifact, and next action
- update `IMPLEMENTATION_STATE.md` with E2E attempt state and final validation outcome
- update `RELEASE_VERIFICATION.md` when implementation changes release readiness evidence

## Output Rules

- Produce or revise `TASKS.md`, `IMPLEMENTATION_STATE.md`, `TEST_REPORT.md`, `E2E_REPORT.md`, and `RETRO.md` as required by the current workflow state.
- Keep traceability and release-safety artifacts aligned with the implementation as work evolves.
- Reuse strong prior content instead of replacing it with a generic rewrite.
- Keep the plan specific to the current initiative and codebase.
- Organize the work around waves, `[S]` and `[P]` task modes, and explicit validation loops.
- Parallel tasks should be delegated to subagents when appropriate.
- Serial tasks should be worked in order.

If the implementation halts on test failures, `TEST_REPORT.md` must be complete enough to guide the next repair attempt.

If the implementation halts on end-to-end failures, `E2E_REPORT.md` must be complete enough to guide the next repair attempt.

The next workflow step after successful implementation closeout is `/Ship`.

Finish by summarizing the current wave state, the remaining blockers if any, whether the retro has been completed, and whether the work is ready for `/Ship`.