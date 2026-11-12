# Model setup in OpenCode

OpenCode model IDs are written as `provider/model`.

1. Configure the provider in `opencode.json` (or your user-level OpenCode config).
2. Put the provider's model ID in each agent's `model` field.
3. Set API credentials through environment variables.
4. Verify provider and model availability using your installed OpenCode version.

Example pattern (replace all placeholders with real IDs):
```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "nvidia": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "NVIDIA",
      "options": {
        "baseURL": "https://integrate.api.nvidia.com/v1",
        "apiKey": "{env:NVIDIA_API_KEY}"
      },
      "models": {
        "YOUR_HIGH_MODEL_ID": { "name": "Your selected high-tier model" },
        "YOUR_MID_MODEL_ID": { "name": "Your selected mid-tier model" }
      }
    }
  }
}
```

The provider example above is a template, not a guarantee that every model endpoint is
compatible with every OpenCode/AI SDK version. Confirm the provider's current API format
and the model ID before use.

Tier mapping:
| Tier | Typical work | Suggested agents |
|---|---|---|
| High | architecture, complex reasoning, security, hard debugging | orchestrator, architect, debugging, security reviewer |
| Mid | implementation, test design, refactoring | developer, TDD, test engineer, code reviewer |
| Small | classification, routine checks, concise summaries | requirements validator, documentation formatter |

To change a tier, edit the `model` field in the relevant agent file. The `tier` metadata is
documentation for humans; OpenCode does not dynamically resolve a custom tier alias.
