---
description: Run a test-first end-to-end feature workflow.
agent: sdlc-orchestrator
---

Deliver the requested feature using this sequence:
1. Inspect repository instructions and relevant code.
2. Clarify requirements and define acceptance criteria.
3. Ask the TDD agent to propose tests before implementation.
4. Ask the architect to review design if scope warrants it.
5. Implement in small steps.
6. Run targeted tests, then relevant integration/regression checks.
7. Request code and acceptance review.
8. Summarize changed files, tests run, outcomes, and risks.
Do not claim tests passed unless they were actually run.
