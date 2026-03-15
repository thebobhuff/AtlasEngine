# Product Workflow Guide

## Purpose

This workspace uses a markdown-based workflow so agents and humans can resume planning work without losing context.

## Installing AtlasEngine Into Another Repository

AtlasEngine is copied into a target repository as repo-local Copilot customization. The target repository should import the workflow files it needs, then adapt the instructions to the target repo's real commands, architecture, release controls, and deployment model.

Core assets to import:

- `.github/copilot-instructions.md`
- `.github/prompts/`
- `.github/agents/`
- `.github/skills/`
- `ai_docs/WORKFLOW.md`
- `ai_docs/VERSIONING.md`
- `ai_docs/templates/`
- `ai_docs/ideas/INDEX.md`

Recommended additional assets:

- `ai_docs/evals/README.md`
- `ai_docs/evals/cases/`
- `.github/pull_request_template.md`
- `.github/release.yml`
- `VERSION`

Optional Supabase assets for repos that actually use Supabase:

- `ai_docs/SUPABASE.md`
- `supabase/`
- `.github/workflows/supabase-migration-check.yml`
- `.github/workflows/supabase-staging.yml`
- `.github/workflows/supabase-production.yml`

New repository import example:

```powershell
git clone <atlasengine-repo-url> AtlasEngine
git clone <target-repo-url> <target-repo>

$source = "C:\work\AtlasEngine"
$target = "C:\work\target-repo"

Copy-Item "$source\.github\copilot-instructions.md" "$target\.github\copilot-instructions.md" -Force
Copy-Item "$source\.github\prompts" "$target\.github" -Recurse -Force
Copy-Item "$source\.github\agents" "$target\.github" -Recurse -Force
Copy-Item "$source\.github\skills" "$target\.github" -Recurse -Force
Copy-Item "$source\ai_docs\WORKFLOW.md" "$target\ai_docs\WORKFLOW.md" -Force
Copy-Item "$source\ai_docs\VERSIONING.md" "$target\ai_docs\VERSIONING.md" -Force
Copy-Item "$source\ai_docs\templates" "$target\ai_docs" -Recurse -Force
Copy-Item "$source\ai_docs\ideas\INDEX.md" "$target\ai_docs\ideas\INDEX.md" -Force
```

```bash
source_repo="$HOME/work/AtlasEngine"
target_repo="$HOME/work/target-repo"

mkdir -p "$target_repo/.github" "$target_repo/ai_docs/ideas"
cp "$source_repo/.github/copilot-instructions.md" "$target_repo/.github/copilot-instructions.md"
cp -R "$source_repo/.github/prompts" "$target_repo/.github/"
cp -R "$source_repo/.github/agents" "$target_repo/.github/"
cp -R "$source_repo/.github/skills" "$target_repo/.github/"
cp "$source_repo/ai_docs/WORKFLOW.md" "$target_repo/ai_docs/WORKFLOW.md"
cp "$source_repo/ai_docs/VERSIONING.md" "$target_repo/ai_docs/VERSIONING.md"
cp -R "$source_repo/ai_docs/templates" "$target_repo/ai_docs/"
cp "$source_repo/ai_docs/ideas/INDEX.md" "$target_repo/ai_docs/ideas/INDEX.md"
```

Established repository merge rules:

- Review the existing `.github/`, documentation, CI, and release files before importing AtlasEngine assets.
- Merge `.github/copilot-instructions.md` with repo-native build, test, runtime, security, and deployment guidance instead of overwriting it blindly.
- Keep the target repository's actual commands authoritative; AtlasEngine orchestrates process, not fake repo behavior.
- Preserve existing UI tokens, CSS, Tailwind, and theme sources for established products and derive `STYLE.md` from those sources.
- Only import Supabase assets if Supabase is part of the target architecture.
- Validate prompt names, agent names, skill references, and copied workflow files after the merge.
- Run `/EvaluateWorkflow` after the first meaningful customization pass to confirm the imported workflow still behaves correctly in the new repo.

## Phase 1: Idea Discovery

Optional first step:

- Run `/BootstrapIdea` to initialize a deterministic initiative folder from the shared templates before discovery begins.
- `ai_docs/ideas/INDEX.md` tracks all active initiatives and their current state.

Authoritative files in `ai_docs/ideas/<initiative-slug>/`:

- `PHASE1_STATE.md`: current discovery stage, confidence, blockers, and next actions
- `TEAM_MEMORY.md`: shared facts, assumptions, risks, and unresolved questions
- `DECISIONS.md`: important decisions and rationale made during discovery
- `AGENT_HANDOFFS.md`: handoff context between discovery and planning work
- `CONTEXT.md`: workspace findings and codebase context
- `INTERVIEW.md`: final discovery handoff into phase 2

Rules:

- Discovery stays open until confidence is at least 90%.
- `/Idea` runs as a one-question-at-a-time interview loop and must emit a visible confidence checkpoint after every user answer.
- `/Idea` should ask the next single highest-value question after each checkpoint until all critical categories are Clear or remaining Partial items are explicitly non-blocking.
- Discovery should inspect existing CSS, Tailwind, theme, or design-token sources when the project already exists and only ask style-direction questions when those sources do not establish a clear visual language.
- `INTERVIEW.md` is the primary handoff document to phase 2.
- Shared memory files continue into phase 2 and must not be discarded.
- `/Idea` is the main discovery prompt after initialization.

## Phase 2: Product Planning

Additional authoritative files in the same initiative folder:

- `PHASE2_STATE.md`: overall planning workflow state and artifact status
- `RESEARCH.md`: external product and market research
- `CODEBASE_RESEARCH.md`: codebase implementation research
- `PRD.md`: evolving requirements document
- `PRD_NOTES.md`: product-manager review notes per PRD cycle
- `PRD_REVIEW_STATE.md`: PRD loop state and scoring
- `THREAT_MODEL.md`: threat boundaries, abuse cases, and mitigations
- `SECURITY_REVIEW.md`: security findings, required controls, and residual-risk decision
- `SUPPLY_CHAIN.md`: dependency, integration, secrets, and provenance review
- `STYLE.md`: visual system guidance including fonts, colors, tokens, and design constraints
- `UX.md`: UX specification with Mermaid flows
- `UI.md`: UI specification with ASCII layouts
- `TEST_PLAN.md`: validation and test strategy
- `PLAN.md`: delivery plan
- `TRACEABILITY.md`: requirement-to-task-to-test-to-release mapping
- `DEPLOYMENT_PLAN.md`: planned rollout sequence and release approach
- `ROLLBACK_PLAN.md`: rollback triggers and recovery steps
- `MIGRATION_PLAN.md`: data or schema migration plan when relevant
- `RELEASE_VERIFICATION.md`: pre-release, release-time, and post-release readiness checks
- `IMPLEMENTATION_PLAN.md`: engineering epics and executable task breakdown
- `TASKS.md`: wave-based engineering tasks tagged `[S]` or `[P]`
- `IMPLEMENTATION_STATE.md`: implementation wave and testing state
- `TEST_REPORT.md`: code testing results and required fixes during implementation
- `E2E_REPORT.md`: browser-based end-to-end validation results and required repairs
- `RETRO.md`: implementation retrospective and workflow improvements

Rules:

- The PRD loop must run for at least two cycles and up to five.
- Security review, release-safety planning, and traceability are required phase-2 outputs.
- `STYLE.md` should be created by the design workflow before `UI.md`; for established products it should be derived from existing CSS, Tailwind, token, or theme sources, and for greenfield work it should be derived from discovery answers rather than guesswork.
- If UX, UI, or testing finds a material contradiction, reopen the upstream phase rather than proceeding with stale artifacts.
- `PHASE2_STATE.md` is the stage-level source of truth for what is complete, stale, blocked, or ready.
- `/PRD` runs the product-planning workflow.
- `/Implementation` turns the approved planning artifacts into `TASKS.md`, executes implementation in waves, runs testing loops and quality gates, performs a final acceptance check, and ends with `RETRO.md`.
- The implementation testing loop retries up to 3 times per wave before marking the wave blocked.
- After code testing passes, `/Implementation` runs a browser-based E2E loop against `TEST_PLAN.md` and `PRD.md`, retrying up to 3 times before blocking the initiative.

## Phase 3: Execution Planning

Additional authoritative file in the same initiative folder:

- `EXECUTION_TASKS.md`: repo-specific coding tasks, file targets, validation gates, and suggested execution handoffs

Rules:

- `/Execution` turns `IMPLEMENTATION_PLAN.md` into concrete execution tasks.
- `/Execution` is optional lower-level execution planning if the team wants a separate repo-task refinement step.
- Execution planning must stay grounded in `CODEBASE_RESEARCH.md` and the current repository context.
- The initiative entry in `ai_docs/ideas/INDEX.md` should be updated as each phase changes state.

## Bug Remediation Workflow

Bug remediation runs can live under `ai_docs/bugs/<bug-run-slug>/` when the work starts from repository GitHub issues instead of a product initiative.

Authoritative files in a bug-run folder:

- `BUG_STATE.md`: issue intake status, current remediation phase, wave state, quality-gate status, and blockers
- `BUG_INTAKE.md`: retrieved GitHub bug issue inventory, triage disposition, severity, and wave assignment
- `TEAM_MEMORY.md`: shared reproduction findings, root-cause notes, risks, and unresolved questions
- `DECISIONS.md`: durable bug remediation decisions and tradeoffs
- `AGENT_HANDOFFS.md`: issue intake, repair, testing, and closure handoffs
- `TASKS.md`: wave-based engineering tasks tagged `[S]` or `[P]`
- `IMPLEMENTATION_STATE.md`: implementation wave and retry-loop state for bug fixing
- `TEST_REPORT.md`: code testing results and repair guidance during bug remediation
- `E2E_REPORT.md`: browser-based validation results when the bug affects user-facing flows
- `BUG_FIX_REPORT.md`: per-issue fix summary, validation evidence, and closure recommendation
- `RETRO.md`: remediation retrospective and workflow improvements

Rules:

- `/Bug` pulls open GitHub issues labeled `bug` by default unless the user provides a narrower or broader issue filter.
- If GitHub issue intake cannot be completed, the workflow stops in a blocked state instead of guessing from incomplete issue data.
- Actionable bugs are triaged into waves and fixed through the same implementation, test, quality-gate, and optional E2E loops used by `/Implementation`.
- Bugs must not be marked ready to close without recorded fix and validation evidence.
- `github-devops-engineer` supports GitHub issue intake and closure context while `project-manager` orchestrates remediation execution.

## Pull Request Review Workflow

PR review runs can live under `ai_docs/reviews/<pr-review-slug>/` when the team wants a durable review summary, iterative findings, and a merge-or-fix recommendation.

Authoritative files in a review folder:

- `PR_REVIEW_STATE.md`: review phase, cycle count, recommendation state, blockers, and next action
- `PR_REVIEW_REPORT.md`: PR summary, findings, validation status, recommendation, and suggested fixes
- `TEAM_MEMORY.md`: shared review findings, unresolved risks, and evidence gaps
- `DECISIONS.md`: durable review decisions or accepted risks
- `AGENT_HANDOFFS.md`: review-to-author or specialist handoffs

Rules:

- `/PRReview` reviews a PR from repository evidence and available GitHub metadata.
- The workflow must summarize the PR before issuing detailed findings.
- The review loop can run up to 3 cycles when fixes or missing evidence require re-review.
- Final outcomes are `Merge`, `Fix Before Merge`, or `Blocked`.
- Review findings should focus on correctness, safety, validation sufficiency, and maintainability instead of cosmetic comments.

## Phase 4: Shipping

Additional authoritative files in the same initiative folder:

- `SHIP_REPORT.md`: shipping readiness, PR promotion state, release state, and blockers
- `RELEASE_NOTES.md`: release notes for the semantic version being shipped

Post-release operating files in the same initiative folder:

- `POST_RELEASE_CHECKS.md`: production smoke checks, alert review, and rollout verification
- `OPERATIONS_REPORT.md`: observed production state, incidents, and actions taken
- `OUTCOME_REVIEW.md`: business outcomes, customer feedback, and follow-up recommendations

Repository-level supporting files:

- `VERSION`: semantic version source of truth
- `.github/pull_request_template.md`: repository pull request template
- `.github/release.yml`: GitHub release categorization
- `ai_docs/VERSIONING.md`: repository versioning and release policy

Rules:

- `/Ship` runs the shipping workflow.
- Branch promotion follows `current work -> dev -> staging -> main`.
- The final `staging -> main` PR requires human approval.
- GitHub Releases are used for formal release publication.
- Semantic versioning is required and should be updated through the `VERSION` file.
- `release-manager` coordinates shipping while `github-devops-engineer` manages GitHub-native promotion, PR, and release execution details.
- Shipping also requires release-safety artifacts and must hand off into `/Operate`.

## Phase 5: Operate

Rules:

- `/Operate` runs the post-release operate-and-observe workflow.
- Production health, smoke checks, alerts, and outcome signals must be captured explicitly.
- If post-release checks fail or outcomes materially miss expectations, the workflow should recommend whether to roll back, stabilize, or reopen an upstream phase.
- `operations-analyst` owns the operating phase and closes the loop back into future planning.

## Continuous Evaluation

Supporting eval files live under `ai_docs/evals/`:

- `README.md`: eval workflow overview
- `cases/`: reusable sample initiative scenarios
- `<eval-run>/EVAL_PLAN.md`: eval scope, cases, scoring, and success criteria
- `<eval-run>/CASE_<case-slug>.md`: per-case scored result
- `<eval-run>/EVAL_REPORT.md`: aggregate findings and recommendations

Rules:

- `/EvaluateWorkflow` runs the workflow evaluation process.
- The eval suite should be used after meaningful prompt, agent, or template changes.
- Each eval run should cover at least 2 representative cases unless the user explicitly scopes it narrower.
- Eval findings should focus on structural workflow gaps, unsafe transitions, missing gates, and weak state transfer.
- Reuse the same sample cases over time so workflow changes can be compared.

## Shared Memory Rules

- Every agent reads shared memory before acting.
- Every agent updates shared memory after acting.
- `TEAM_MEMORY.md`, `DECISIONS.md`, and `AGENT_HANDOFFS.md` persist across phases.
- If later work invalidates earlier assumptions, update the shared memory files and reopen the required phase.

## Templates

Shared templates live in `ai_docs/templates/` and define the baseline structure for phase states, handoffs, pull-request review artifacts, bug remediation artifacts, security artifacts, planning artifacts including style guidance, release-safety artifacts, shipping artifacts, post-release operating artifacts, and workflow eval artifacts.

## Shared Skills

Reusable agent skills live under `.github/skills/` and should be used when the work calls for repeatable cross-agent methods instead of role-specific reasoning.

- `initiative-state-manager`: resume initiative state, update shared memory, normalize artifacts, and reopen stale phases safely
- `prd-review-loop-rubric`: score and drive PRD review cycles consistently
- `release-safety-pack`: maintain deployment, rollback, migration, verification, and operate handoff controls
- `supabase-migration-discipline`: keep Supabase schema changes, drift handling, local validation, and environment promotion consistent
- `github-hardening-branch-airgap`: audit and harden GitHub branch protections, PR controls, and release-safety settings
- `quality-gates-executor`: enforce implementation quality gates beyond basic test pass/fail
- `threat-modeling-and-security-triage`: model trust boundaries, abuse cases, sensitive flows, and residual risk
- `traceability-maintainer`: keep requirements, implementation, validation, and release outputs linked
- `browser-validation-strategy`: convert PRD and TEST_PLAN scenarios into structured browser validation
- `workflow-evaluation-rubric`: score workflow quality consistently across cases and revisions
- `handoff-composer`: create precise, durable handoffs with blockers, inputs, and next actions

For repositories that use Supabase, keep long-lived conventions in `ai_docs/SUPABASE.md`. Do not use markdown files to track which migrations have already been applied; Supabase migration history is the source of truth for that state.
