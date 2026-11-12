# Custom Python runtime adapter

This is the most direct route if you want one application to load the same agents and skills across providers.

Suggested components:
- YAML loader and schema validation.
- Provider clients/adapters (OpenAI-compatible, Google GenAI, Ollama, etc.).
- Agent prompt assembler (agent definition + selected skills + task context).
- Tool registry with permissions.
- DAG scheduler and bounded worker pool.
- State store (SQLite for local development, PostgreSQL for shared/multi-user execution).
- Retry/escalation manager.
- Tracing, token/cost accounting and audit log.

Use provider-specific SDKs only behind a common interface. Never assume every provider supports identical tool calls, structured output, multimodal input or context sizes.
