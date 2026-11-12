# Three-Tier Model Routing

## Logical tiers
| Tier | Use for | Default policy |
|---|---|---|
| High | Ambiguous, high-impact, complex reasoning, architecture, root-cause analysis, safety/security review | Use the strongest available model that meets tool, context, privacy and budget requirements |
| Mid | Engineering implementation, test design, structured analysis, RAG/ML pipeline work | Use a capable coding/reasoning model with adequate context and tool calling |
| Small | Classification, formatting, deterministic summaries, routine checklist work | Use a low-latency/low-cost model; prefer deterministic code for exact checks |

## Routing signals
Evaluate:
- Task complexity and ambiguity.
- Impact/risk and reversibility.
- Required context length.
- Required tool/function-calling support.
- Language/code capability.
- Provider availability and rate limits.
- Latency and cost budgets.
- Recent benchmark performance on this task family.

## Escalation
Small → Mid → High when evidence indicates insufficient capability, repeated failure, high uncertainty or high impact. Do not escalate merely because an agent asks for a stronger model; use task evidence and retry history.

## Configuration
Tier names are aliases. Map them to real provider/model identifiers in a separate runtime config. Do not hardcode API keys or invent current model IDs. Keep provider fallback explicit and test it.

## Important limitation
OpenCode agent Markdown frontmatter can set an explicit model, but a true dynamic per-task router requires a separate orchestration layer or provider/router integration. This package intentionally leaves model IDs unconfigured.
