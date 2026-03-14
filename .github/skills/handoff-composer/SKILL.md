---
name: handoff-composer
description: 'Compose precise, durable handoffs between agents with current owner, next owner, required inputs, blockers, and next action. Use when updating AGENT_HANDOFFS.md, creating workflow handoff summaries, or preparing repair guidance for another agent.'
argument-hint: 'initiative folder, handoff target, or transition summary'
user-invocable: true
---

# Handoff Composer

Use this skill whenever work moves from one agent, phase, or retry loop to another.

## When To Use

- Updating `AGENT_HANDOFFS.md`
- Preparing a specialist review handoff
- Sending failed work back for repair
- Handing a release into operate or operate findings back upstream

## Procedure

1. Identify the current owner and next owner.
2. Summarize the current state in a few concrete lines.
3. Include the exact files or artifacts the next owner must read first.
4. State unresolved blockers, accepted risks, and missing evidence.
5. End with the next required action, not a vague suggestion.
6. If the handoff is for failed work, include repair guidance and retry status.

## Required Handoff Fields

- current owner
- next owner
- current status
- required inputs
- blockers or risks
- next action

## References

- [Handoff checklist](./references/handoff-checklist.md)
