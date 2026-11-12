# OpenCode Multi-Agent Engineering Framework

A ready-to-customize OpenCode project structure for software engineering, testing, debugging,
AI/ML, GenAI, RAG, model lifecycle, and code review workflows.

## Install

1. Extract this folder into your project root, or copy its contents into an existing project.
2. Edit `opencode.json` and `.opencode/agents/*.md` to select provider/model IDs available to you.
3. Add provider credentials using environment variables or your secret manager. Never commit API keys.
4. Start OpenCode from the project root.
5. Try `/workflow-feature`, `/workflow-bugfix`, `/workflow-rag`, or `/workflow-ml`.

## Model configuration

OpenCode's native model references use `provider/model`. Update the `model` fields in
`.opencode/agents/*.md` and provider definitions in `opencode.json`.

This package deliberately uses clearly marked placeholders rather than pretending a model
ID is available on your account. Example environment variables are in `.env.example`.

Suggested logical tiers:
- High: architecture, complex debugging, security review, system design, final decisions.
- Mid: implementation, test creation, refactoring, code analysis.
- Small: classification, summaries, routine checks, formatting.

## What is included

- 44 OpenCode agent definitions
- 110 reusable Agent Skills (`SKILL.md`)
- OpenCode commands for common workflows
- Native OpenCode config example
- Model/provider setup guide and routing matrix
- Workflow and failure-recovery documentation

## Important runtime boundary

This is an OpenCode-native configuration and instruction pack, not a new orchestration
server. OpenCode executes the configured agents and tools. The markdown instructions
describe bounded retries, escalation, handoffs, and parallel work; they do not create a
custom durable DAG scheduler, shared state database, cost controller, or automatic retry
worker. Those require a separate runtime implementation.

## Recommended first run

Start with `/workflow-feature`. It asks the orchestrator to clarify requirements, create
acceptance criteria, plan tests before implementation, implement in small steps, run tests,
and request review. Review proposed commands and tool permissions before allowing changes.
