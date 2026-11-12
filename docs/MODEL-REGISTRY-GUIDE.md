# Model Registry Guide

## The single source of truth
`config/models.yaml` stores logical tiers and maps each tier to a provider and exact model ID.

## Example with placeholders
```yaml
tiers:
  high:
    provider: nvidia
    model: "YOUR_MODEL_ID_FROM_NVIDIA"
  mid:
    provider: openrouter
    model: "YOUR_MODEL_ID_FROM_OPENROUTER"
  small:
    provider: groq
    model: "YOUR_MODEL_ID_FROM_GROQ"
```

## What is a provider and what is a model?
- Provider: the API vendor or gateway, e.g. NVIDIA, OpenRouter, Groq, Google AI Studio, Ollama.
- Model ID: the exact identifier the provider expects in the request.
- Tier: your own logical quality/cost class (High, Mid, Small). It is not a model name.

## Per-agent assignment
If `agents/developer.md` has `default_model_tier: mid`, it uses the Mid mapping unless an override exists:
```yaml
agent_overrides:
  developer:
    tier: high
```

## Multiple providers
Fallbacks are ordered. The runtime must check:
- provider is available and credentials are configured;
- model supports the required context and tool-calling mode;
- task data is allowed to leave the current environment;
- budget and latency limits;
- provider rate limits and retry-after headers.

## Security
Never commit real API keys. Use environment variables or a secret manager. Do not print secrets in logs. Use separate credentials and permissions per environment.
