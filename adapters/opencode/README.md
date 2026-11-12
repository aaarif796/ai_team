# OpenCode adapter notes

OpenCode-specific agent files/configuration are generated from `agents/` and `config/models.yaml`. OpenCode model identifiers use the provider/model form, and custom agent model selection is host-specific. See https://opencode.ai/docs/agents and https://opencode.ai/docs/config.

Example mapping concept:
- generic `default_model_tier: high`
- resolve `config/models.yaml` → `tiers.high.provider` + `tiers.high.model`
- emit OpenCode model string as `<provider>/<model-id>` where provider configuration supports it.

Do not put provider API keys in agent Markdown. Use OpenCode's provider configuration and environment-variable support.
