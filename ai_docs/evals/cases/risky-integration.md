# Case: Risky Integration

## Scenario

A feature introduces a new external integration, sensitive data flow, or privileged action that raises security, reliability, and rollout risk.

## What This Should Stress

- threat modeling and security review rigor
- supply-chain and dependency scrutiny
- deployment, rollback, and migration planning
- post-release monitoring and incident response readiness

## Failure Signals

- security review is bypassed or treated as optional
- release safety artifacts remain generic
- no explicit rollback triggers are defined
- operate phase cannot tell whether the release is healthy
