# AtlasEngine Workspace Instructions

## Purpose

This repository is a Copilot customization workspace for AI-driven product and delivery workflows, not an application codebase.

Start with [ai_docs/WORKFLOW.md](../ai_docs/WORKFLOW.md) for the end-to-end phase model and artifact expectations.

Use [ai_docs/VERSIONING.md](../ai_docs/VERSIONING.md), [.github/pull_request_template.md](pull_request_template.md), and [.github/release.yml](release.yml) for release and PR conventions instead of restating them in prompts.

## Repository Map

- [.github/prompts](prompts): workflow entry points such as Idea, PRD, Implementation, Ship, Operate, DevOpsAudit, EvaluateWorkflow, and Bug
- [.github/agents](agents): role-specific agents with responsibilities, handoffs, and explicit reusable-skill references
- [.github/skills](skills): reusable methods and rubrics; put repeatable logic here instead of duplicating it in prompts
- [ai_docs/templates](../ai_docs/templates): baseline markdown structures for initiative, bug, shipping, evaluation, and operational artifacts
- [ai_docs/ideas](../ai_docs/ideas): durable initiative state and artifacts
- [ai_docs/evals](../ai_docs/evals): workflow evaluation cases and per-run reports

## Working Model

- Prompts own workflow orchestration and phase control flow.
- Agents own role behavior, responsibilities, and handoffs.
- Skills own reusable rubrics, checklists, and cross-agent methods.
- Templates provide artifact structure only; do not move rubric logic out of skills and back into prompt prose.

## Repository-Specific Rules

- Treat this as a documentation-first repo. There are no application build, test, or runtime commands to discover here.
- Preserve phase boundaries. Do not collapse discovery, planning, implementation, shipping, operate, evaluation, or bug remediation into one prompt unless the workflow explicitly calls for it.
- Keep durable state in the correct working folder:
  - initiative work in `ai_docs/ideas/<initiative-slug>/`
  - bug remediation runs in `ai_docs/bugs/<bug-run-slug>/`
- Reuse existing slug folders when they already match the target instead of creating duplicates.
- Shared memory files such as `TEAM_MEMORY.md`, `DECISIONS.md`, and `AGENT_HANDOFFS.md` are durable state. Extend them; do not wipe or casually replace strong existing content.
- If information is missing, record it as unknown or blocked. Do not invent repository behavior, code paths, issue details, or validation evidence.

## Editing Conventions

- When editing prompts, agents, or skills, keep YAML frontmatter valid and minimal.
- When a prompt or agent depends on a reusable method, add an explicit markdown link to the relevant skill in the body. Do not rely only on description-based discovery.
- When creating new artifact families, add templates under [ai_docs/templates](../ai_docs/templates) and document the workflow in [ai_docs/WORKFLOW.md](../ai_docs/WORKFLOW.md).
- Prefer linking to the workflow guide, templates, or skills instead of embedding large duplicated checklists in multiple files.
- Keep changes structurally consistent with the existing customization style: concise frontmatter, explicit owners, durable artifact names, and exact stop-gates.

## Validation

- After editing customization files, validate the changed files for errors.
- If you add a new prompt, agent, skill, or template, also update the relevant workflow documentation or repo memory when appropriate.

## High-Value References

- [ai_docs/WORKFLOW.md](../ai_docs/WORKFLOW.md)
- [ai_docs/SUPABASE.md](../ai_docs/SUPABASE.md)
- [ai_docs/templates](../ai_docs/templates)
- [.github/prompts](prompts)
- [.github/agents](agents)
- [.github/skills](skills)