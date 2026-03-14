---
description: Initialize a new initiative folder and shared markdown memory files from the workspace templates before discovery starts.
name: BootstrapIdea
argument-hint: feature, project, or initiative name to initialize
agent: agent
tools:
  - search
  - edit
---

# Bootstrap Initiative Workspace

The initiative to initialize is: ${input:initiative:feature, project, or initiative name}

Initialize a deterministic initiative workspace under `ai_docs/ideas/<initiative-slug>/` before discovery starts.

## Goals

- Resolve a stable lowercase kebab-case slug for the initiative.
- Create or reuse the initiative folder.
- Initialize the phase-1 shared memory files from the templates if they do not already exist.
- Preserve any existing files with meaningful content.
- Make the initiative ready for the `/Idea` prompt without starting discovery yet.

## Files To Initialize

Create or normalize these files from the template set in `ai_docs/templates/`:

- `PHASE1_STATE.md` from `PHASE1_STATE.template.md`
- `TEAM_MEMORY.md` from `TEAM_MEMORY.template.md`
- `DECISIONS.md` from `DECISIONS.template.md`
- `AGENT_HANDOFFS.md` from `AGENT_HANDOFFS.template.md`
- `CONTEXT.md` from `CONTEXT.template.md`
- `INTERVIEW.md` from `INTERVIEW.template.md`

Also ensure `ai_docs/ideas/INDEX.md` exists using `ai_docs/templates/IDEAS_INDEX.template.md` as the structure baseline.

## Initialization Rules

- If the initiative folder already exists, inspect existing files before changing them.
- Do not overwrite stronger existing content with empty template content.
- If a file is missing, create it from the matching template.
- If a file exists but is effectively blank or placeholder-only, normalize it to the latest template structure.
- Update `PHASE1_STATE.md` so the current stage is `Intake`, the overall status is `In Progress`, and the next required action points to running `/Idea`.
- Update `AGENT_HANDOFFS.md` so the latest handoff indicates the workspace has been initialized for discovery.
- Update `ai_docs/ideas/INDEX.md` with an entry for the initiative showing phase `Bootstrap`, status `In Progress`, latest artifact `PHASE1_STATE.md`, and next action `/Idea`.

## Output

- Report the resolved initiative folder path.
- Summarize which files were created, reused, or normalized.
- End by telling the user the initiative is ready for `/Idea`.

Do not start the interview. Stop after initialization is complete.