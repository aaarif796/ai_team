# Runtime adapters

Each adapter maps the generic core into a host's native conventions. The generic agent and skill sources are canonical; adapter-specific files should be generated or maintained separately.

- OpenCode: maps agents to its agent definitions/configuration and skills to its supported skill directory.
- Claude Code: maps agent definitions to Claude Code subagents and skills to `.claude/skills/`.
- Codex: maps agent instructions and supported skills to Codex conventions.
- Custom Python: loads generic YAML/Markdown and executes agents through your own orchestration runtime.

Do not copy one host's frontmatter into the generic source of truth.
