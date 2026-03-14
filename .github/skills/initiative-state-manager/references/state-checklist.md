# State Checklist

## Read Before Acting
- `PHASE1_STATE.md` or `PHASE2_STATE.md`
- `TEAM_MEMORY.md`
- `DECISIONS.md`
- `AGENT_HANDOFFS.md`
- the current phase's authoritative artifacts

## Write Back After Acting
- update phase status and owner
- update blockers and next action
- record durable decisions only in `DECISIONS.md`
- add cross-agent facts and risks to `TEAM_MEMORY.md`
- update initiative index when phase or status changes materially

## Stale Artifact Signals
- downstream artifact contradicts upstream requirements
- missing dependency invalidates acceptance or release logic
- traceability no longer maps current implementation to requirements or tests
- release verification no longer matches the implementation or planned rollout
