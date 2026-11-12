# OpenCode SDLC + AI/GenAI Engineering Framework

A proposed OpenCode-compatible prompt/skill library for software engineering and AI/ML/GenAI projects.

## Included
- 44 Markdown agent definitions (1 primary orchestrator + 43 specialist subagents).
- 110 reusable `SKILL.md` directories.
- Agent inventory, model-tier routing, cluster/workflow design, failure recovery, handoff contract and tools catalogue.
- Three logical LLM tiers: High, Mid and Small.

## Install into a project
Copy the `.opencode/` directory and `AGENTS.md` into the root of your target repository.

OpenCode discovers:
- Agents: `.opencode/agents/*.md`
- Skills: `.opencode/skills/<skill-id>/SKILL.md`

The primary agent is `sdlc-orchestrator`. Select it in OpenCode's agent picker. If you want it to be the default session agent, set `default_agent` to `sdlc-orchestrator` in your OpenCode config.

## Global installation
For global use, copy:
- `.opencode/agents/` to `~/.config/opencode/agents/`
- `.opencode/skills/` to `~/.config/opencode/skills/`

Do not overwrite existing files without reviewing conflicts.

## Model tiers
The agents contain logical tier labels in their instructions. They intentionally do not hardcode provider/model IDs. Configure actual provider/model names in OpenCode or an external router after benchmarking your available providers.

## Important: what this package does NOT implement by itself
Markdown agents and skills define behavior; they do not automatically provide:
- A persistent DAG scheduler.
- Dynamic per-task LLM routing across providers.
- A retry database or distributed worker queue.
- Automatic parallel execution of arbitrary independent agents.
- Installation or configuration of third-party tools.

Those require a separate runtime/configuration layer. The included docs specify the contracts and policies for implementing that layer. OpenCode's native subagent delegation can be used for agent delegation, but verify the installed OpenCode version and permissions.

## Suggested validation
1. Confirm OpenCode version and current config.
2. Copy the package into a test repository first.
3. Check that `sdlc-orchestrator` and specialist agents appear.
4. Check that skills are discoverable.
5. Test a read-only research workflow.
6. Test a small TDD feature in a disposable branch.
7. Only then enable write, shell, deployment or external integrations.

## Counts
- Agents: 44
- Specialist agents: 43
- Skills: 110
- Functional clusters: 10
- Logical LLM tiers: 3

## Official OpenCode references
- Agents: https://opencode.ai/docs/agents
- Skills: https://opencode.ai/docs/skills
