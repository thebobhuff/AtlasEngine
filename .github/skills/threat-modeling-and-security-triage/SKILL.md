---
name: threat-modeling-and-security-triage
description: 'Model trust boundaries, abuse cases, sensitive data flows, and security mitigations, then classify residual risk. Use when creating THREAT_MODEL.md, SECURITY_REVIEW.md, SUPPLY_CHAIN.md, or deciding whether security findings are blocking, accepted, or mitigated.'
argument-hint: 'initiative folder, threat model scope, or security triage target'
user-invocable: true
---

# Threat Modeling And Security Triage

Use this skill when an initiative introduces sensitive data, external integrations, privileged actions, or meaningful trust boundaries.

## When To Use

- Creating `THREAT_MODEL.md`
- Writing `SECURITY_REVIEW.md`
- Reviewing `SUPPLY_CHAIN.md`
- Deciding whether a security issue is blocking, accepted, or mitigated

## Procedure

1. Identify system boundaries, sensitive assets, and the actors involved.
2. Mark trust boundaries and externally reachable entry points.
3. Enumerate abuse cases, misuse paths, and attack surfaces.
4. Classify threats by impact, likelihood, and residual risk after mitigation.
5. Record required mitigations and whether they are implemented, planned, or missing.
6. Distinguish accepted risk from unresolved risk explicitly.
7. Block the workflow if the remaining risk is inconsistent with the initiative's expected safety posture.

## References

- [Security triage checklist](./references/security-triage-checklist.md)
