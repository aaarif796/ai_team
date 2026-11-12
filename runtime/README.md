# Runtime Adapter Contract

The core package is platform-neutral. A runtime adapter translates generic agent definitions, skills, model routing and tool requirements into a specific host.

## Required adapter responsibilities
1. Discover agent definitions in `agents/`.
2. Discover portable skills in `skills/<name>/SKILL.md`.
3. Read `config/models.yaml` and resolve tier → provider → model ID.
4. Resolve provider credentials from environment/secret manager.
5. Expose only authorized tools to each agent.
6. Execute sequential and parallel DAG nodes with bounded concurrency.
7. Persist task state, attempt counts, outputs and evidence.
8. Enforce retry, timeout, cost and approval policies.
9. Emit an audit trail and normalized task handoff objects.

## Adapter targets
- `adapters/opencode/`
- `adapters/claude-code/`
- `adapters/codex/`
- `adapters/custom-python/`

The folders describe integration mappings; the core agent and skill files remain unchanged.

## Runtime is not included as a finished scheduler
This package contains portable definitions and configuration contracts. A real runtime must implement provider calls, tool execution, DAG scheduling, persistence and permissions. Do not interpret YAML policy files as executable orchestration code.
