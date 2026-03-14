---
description: Turn IMPLEMENTATION_PLAN.md into repo-specific coding tasks, file targets, validation gates, and suggested agent handoffs for execution.
name: Execution
argument-hint: initiative slug, folder, or execution target
agent: senior-engineer
tools:
  - search
  - edit
  - agent
---

# Execution Planning Workflow

The initiative to execute is: ${input:initiative:initiative slug, folder name, or execution target}

Use the initiative folder under `ai_docs/ideas/<initiative-slug>/` as the working directory for execution planning.

## Preconditions

- Resolve the initiative folder from the provided input.
- Require `IMPLEMENTATION_PLAN.md` to exist before execution planning starts.
- Read `PLAN.md`, `PRD.md`, `CODEBASE_RESEARCH.md`, and `TEST_PLAN.md` as supporting context.
- Read `PHASE2_STATE.md`, `TEAM_MEMORY.md`, `DECISIONS.md`, and `AGENT_HANDOFFS.md` before creating execution tasks.
- If `EXECUTION_TASKS.md` already exists, resume from it instead of recreating it from scratch.

## Required Artifact

Create and maintain `EXECUTION_TASKS.md` using `ai_docs/templates/EXECUTION_TASKS.template.md` as the structure baseline.

## Workflow

### 1. Execution Review

Owner: `senior-engineer`

Inspect `IMPLEMENTATION_PLAN.md` and identify the most credible coding sequence, the likely file targets, and the main engineering dependencies.

### 2. Repo-Specific Task Breakdown

Owner: `senior-engineer`

Coordinate with these supporting agents when useful:

- `solution-architect` for system boundaries and integration points
- `qa-strategist` for validation gates and acceptance coverage
- `platform-release-engineer` for rollout and operational tasks
- `scrum-master` for sequencing and dependency clarity

Turn the implementation plan into:

- repo-specific coding tasks
- concrete target files or code areas where possible
- execution handoffs between engineering contributors or agents
- validation gates that should be completed before moving forward

### 3. Task Quality Rules

- Keep tasks grounded in the current codebase and `CODEBASE_RESEARCH.md`.
- Prefer concrete file paths, modules, or subsystems when evidence supports them.
- Do not invent nonexistent files or unsupported architectural assumptions.
- Separate coding work from validation, migration, observability, and release work.
- Organize the tasks in the order a delivery team should execute them.

### 4. Shared Memory Updates

After producing `EXECUTION_TASKS.md`:

- update `TEAM_MEMORY.md` with execution assumptions and risks
- add major execution decisions to `DECISIONS.md`
- update `AGENT_HANDOFFS.md` with the execution-planning handoff state
- update `PHASE2_STATE.md` to record that execution planning has completed or is blocked
- update `ai_docs/ideas/INDEX.md` with the initiative's current phase and next action

## Output Rules

- Produce or revise only `EXECUTION_TASKS.md`.
- Reuse strong prior content instead of replacing it with a generic rewrite.
- Keep the plan specific to the repository and initiative.
- Finish by summarizing the first build slice, main dependencies, and the recommended execution order.