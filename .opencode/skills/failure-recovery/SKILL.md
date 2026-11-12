---
name: failure-recovery
description: Recover from failed tasks without infinite loops.
---

# Failure Recovery

## Purpose
Recover from failed tasks without infinite loops.

## Procedure
Classify failure, preserve evidence, retry transient errors with backoff, escalate reasoning, cap attempts/cost/time and require human intervention when blocked.

## Required outputs
- State the work performed and the evidence used.
- List files, commands, versions and results where applicable.
- Identify assumptions, limitations and unresolved questions.
- Recommend the next step without claiming it has been completed.

## Safety and quality rules
- Follow repository instructions and the user's approved scope.
- Never invent tool results, metrics, citations or successful execution.
- Protect secrets and sensitive data.
- Ask for approval before destructive, external, production or security-sensitive actions.
