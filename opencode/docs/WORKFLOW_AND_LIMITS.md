# Workflow and execution boundaries

## Standard feature workflow

1. Requirements validator identifies ambiguity and acceptance criteria.
2. Architect proposes design and records trade-offs.
3. TDD agent writes failing tests before implementation.
4. Developer implements the smallest useful change.
5. Test engineer runs targeted, integration, regression, and relevant non-functional tests.
6. Reviewer checks correctness, maintainability, security, and test coverage.
7. Orchestrator summarizes evidence, remaining risks, and changed files.

## Parallel work

Use parallel subagents only for independent tasks (for example, API contract review and test
plan drafting). Avoid parallel edits to the same files. Have one agent integrate and resolve
conflicts.

## Failure handling

The agents are instructed to:
- distinguish transient tool/provider failures from code/test failures;
- retry transient failures only a small bounded number of times;
- avoid repeating an identical failed command without changing the cause;
- escalate from small to mid or high reasoning when needed;
- stop and ask for human input on destructive, security-sensitive, or ambiguous actions.

These are instruction-level policies. They are not a guaranteed automatic retry engine.

## Testing before implementation

For feature work, write acceptance criteria and tests first where practical. For bug fixes,
reproduce the bug with a failing regression test before changing production code. If a test
cannot be written first, explain why and create the closest verifiable check.

## Human approval

Review shell commands, dependency changes, migrations, deployments, external network access,
and destructive operations before execution.
