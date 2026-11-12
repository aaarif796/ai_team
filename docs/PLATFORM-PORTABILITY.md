# Platform Portability

## Portable core
- Agent role and task instructions in `agents/*.md`.
- Reusable skill procedures in `skills/*/SKILL.md`.
- Logical model tiers and provider/model mapping in `config/`.
- Task handoff and workflow contracts in `docs/`.

## Host-specific layer
Agent loading, tool permission syntax, command syntax, model configuration and skill search paths vary by client. Keep these mappings in `adapters/<host>/`; never bake them into the canonical agent definitions.

## Portability caveat
The Agent Skills format is designed to be portable across compatible clients, but not every client supports every skill feature, script execution or resource loading behavior. Verify each target host's documentation.
