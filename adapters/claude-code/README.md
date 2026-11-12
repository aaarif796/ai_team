# Claude Code adapter notes

Generate Claude Code subagent files from the generic agent definitions and install portable skills under `.claude/skills/<skill-name>/SKILL.md`. Model selection and provider access are controlled by the Claude Code environment/settings and any supported gateway; do not assume the generic `models.yaml` is read natively by Claude Code.

The generic model router must be implemented externally if you need dynamic per-task routing across providers.
