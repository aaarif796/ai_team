# Project Instructions — SDLC + AI/GenAI Agent Framework

## Purpose
This repository contains an OpenCode-compatible library of specialist agent prompts and reusable skills for software engineering, AI/ML, GenAI, RAG, evaluation, MLOps and operations.

## Core architecture
- 1 primary Master Orchestrator.
- 43 specialist subagents.
- 3 configurable model tiers: High, Mid and Small.
- 10 functional clusters.
- Sequential and parallel workflows represented as dependency graphs.
- Bounded retries, escalation, persistent state and human approval gates.

## Operating principles
1. Research and discovery precede requirements and design.
2. Requirements must be testable and traceable.
3. Technology selection must include evidence, trade-offs, cost, licensing and operational fit.
4. TDD is the default: Red → Green → Refactor.
5. Bug fixes require reproduction and a regression test whenever feasible.
6. Testing includes functional, non-functional, security, compatibility, resilience and specialized testing.
7. RAG evaluation separates retrieval, generation and end-to-end quality.
8. ML/LLM releases require reproducible evaluation and approval gates.
9. Drift signals are not proof of model performance degradation; ground truth is needed to measure performance.
10. Production, destructive, external, security-sensitive and costly actions require explicit authorization.

## OpenCode compatibility
- Agent definitions live in `.opencode/agents/*.md` with YAML frontmatter.
- Skills live in `.opencode/skills/<skill-id>/SKILL.md`.
- `sdlc-orchestrator` is the primary agent; specialists are subagents.
- This package defines prompts and documentation. It does not by itself implement a persistent DAG scheduler, provider-aware model router, retry engine or shared database. Those require a separate runtime/configuration implementation.

## Model routing
The tier labels are logical aliases, not provider model IDs. Configure actual provider/model identifiers in the user's OpenCode configuration or an external router. Do not put guessed model IDs in agent frontmatter.

## Tool policy
Tools listed in agent files are recommended integrations. Availability depends on installation, MCP configuration, provider credentials, network access and permissions. Verify before use.
