# Generic Multi-Agent Software + AI Engineering Framework

Platform-neutral source of truth for a multi-agent engineering system covering the full SDLC, traditional ML, GenAI, RAG, evaluation, AI safety, deployment, monitoring, drift and retraining.

## Package contents
- `agents/`: 44 platform-neutral agent definitions.
- `skills/`: reusable Agent Skills (`SKILL.md`) in the portable folder format.
- `config/models.yaml`: the place to enter actual provider and model IDs for High/Mid/Small.
- `config/providers.example.yaml`: provider connection templates.
- `config/routing.yaml`: model selection, fallback and bounded escalation policy.
- `runtime/`: contract for a real execution engine.
- `adapters/`: host-specific integration notes for OpenCode, Claude Code, Codex and custom Python.
- `docs/`: inventory, workflows, tool catalogue, handoffs and failure recovery.

## Where do I enter model names?
Edit `config/models.yaml`. For each tier, set:
- `provider`: provider key from `config/providers.example.yaml`.
- `model`: exact model ID accepted by that provider/API.
- `fallback`: optional provider/model alternatives.

Example:
```yaml
tiers:
  high:
    provider: nvidia
    model: "YOUR_EXACT_NVIDIA_MODEL_ID"
  mid:
    provider: groq
    model: "YOUR_EXACT_GROQ_MODEL_ID"
  small:
    provider: google
    model: "YOUR_EXACT_GOOGLE_MODEL_ID"
```
Use exact IDs shown in the provider console/API docs. Do not use display names unless the provider explicitly accepts them. Keep credentials in environment variables or a secret manager.

## How an agent gets its model
An agent declares `default_model_tier` in its metadata. The runtime:
1. Checks `agent_overrides` in `config/models.yaml`.
2. Otherwise reads the agent's `default_model_tier`.
3. Resolves that tier to provider + model ID.
4. Checks provider capability, context window, permissions, budget and availability.
5. Calls the provider through an adapter.

An agent can therefore be moved from High to Mid by changing one config entry, without editing its role instructions.

## Is this generic?
Yes. `agents/` and `skills/` are the portable source of truth. Host-specific adapters translate them for a supported agent client. The portable Agent Skills folder structure is an open format; host-specific agent configuration is not universally portable.

## Important runtime boundary
This package is a configuration/specification and prompt library. It does not yet include a fully working multi-provider execution engine, DAG scheduler, shared state database, cost meter or retry worker. Those are runtime components to implement. YAML files describe the policy; they do not execute it automatically.

## Quick start
1. Edit `config/models.yaml` with real model IDs.
2. Configure provider credentials in your local environment.
3. Choose a host adapter or build `runtime/custom-python`.
4. Install/copy `skills/` into the host's supported skills directory.
5. Map agents to host-native agent definitions.
6. Test one read-only research workflow.
7. Test a small TDD workflow.
8. Enable write/deploy tools only after permission and approval tests.

## Official references
- OpenCode agents: https://opencode.ai/docs/agents
- OpenCode model/provider config: https://opencode.ai/docs/config
- Agent Skills format: https://agentskills.io/
