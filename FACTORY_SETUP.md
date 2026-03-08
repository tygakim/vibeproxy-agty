# Using Factory Droid with VibeProxy

Use Factory Droid with the subscriptions you already pay for through VibeProxy on your Mac.

## What This Does

VibeProxy sits between Factory and your authenticated providers:

```text
Factory Droid -> VibeProxy -> Provider OAuth/API credentials -> Model APIs
```

VibeProxy handles:

- OAuth login for Claude Code, Codex, Gemini, Antigravity, Qwen, and Kimi Code
- API-key storage for Z.AI GLM
- local routing on `http://localhost:8317`
- Claude thinking model aliases like `-thinking-4000`

> [!NOTE]
> Older Factory guides often use `~/.factory/config.json` with `custom_models`. Current Factory config uses `~/.factory/settings.json` with `customModels`.

## Prerequisites

- macOS 13 or later
- VibeProxy installed and running
- Factory CLI installed:

```bash
curl -fsSL https://app.factory.ai/cli | sh
```

- At least one connected provider in VibeProxy:
  - Claude Code for Claude models
  - Codex for GPT models
  - Antigravity for Gemini 3 and Gemini-hosted Claude aliases
  - Gemini for Gemini 2.5 models
  - Qwen for Qwen coder models
  - Kimi Code for `kimi-k2.5`
  - Z.AI API key for GLM models

## Step 1: Connect Accounts in VibeProxy

1. Launch `VibeProxy.app`.
2. Open **Settings** from the menu bar icon.
3. Click **Add Account** for each provider you want to use.
4. Wait until the provider shows a connected account.

Recommended minimum setup:

- Claude Code
- Codex
- One Gemini source: Antigravity or Gemini

Optional:

- Qwen
- Kimi Code
- Z.AI GLM

## Step 2: Configure Factory

Edit `~/.factory/settings.json` and add a curated `customModels` list.

```json
{
  "customModels": [
    {
      "modelDisplayName": "Claude Opus 4.5",
      "model": "claude-opus-4-5-20251101",
      "baseURL": "http://localhost:8317",
      "apiKey": "dummy-not-used",
      "provider": "anthropic"
    },
    {
      "modelDisplayName": "Claude Opus 4.5 (Think Harder)",
      "model": "claude-opus-4-5-20251101-thinking-10000",
      "baseURL": "http://localhost:8317",
      "apiKey": "dummy-not-used",
      "provider": "anthropic"
    },
    {
      "modelDisplayName": "Claude Sonnet 4.5",
      "model": "claude-sonnet-4-5-20250929",
      "baseURL": "http://localhost:8317",
      "apiKey": "dummy-not-used",
      "provider": "anthropic"
    },
    {
      "modelDisplayName": "Claude Sonnet 4.5 (Think)",
      "model": "claude-sonnet-4-5-20250929-thinking-4000",
      "baseURL": "http://localhost:8317",
      "apiKey": "dummy-not-used",
      "provider": "anthropic"
    },
    {
      "modelDisplayName": "GPT-5.3 Codex",
      "model": "gpt-5.3-codex",
      "baseURL": "http://localhost:8317/v1",
      "apiKey": "dummy-not-used",
      "provider": "openai"
    },
    {
      "modelDisplayName": "GPT-5.3 Codex (High)",
      "model": "gpt-5.3-codex(high)",
      "baseURL": "http://localhost:8317/v1",
      "apiKey": "dummy-not-used",
      "provider": "openai"
    },
    {
      "modelDisplayName": "GPT-5.4",
      "model": "gpt-5.4",
      "baseURL": "http://localhost:8317/v1",
      "apiKey": "dummy-not-used",
      "provider": "openai"
    },
    {
      "modelDisplayName": "Gemini 3 Pro",
      "model": "gemini-3-pro-preview",
      "baseURL": "http://localhost:8317/v1",
      "apiKey": "dummy-not-used",
      "provider": "openai"
    },
    {
      "modelDisplayName": "Gemini 3 Pro Image",
      "model": "gemini-3-pro-image-preview",
      "baseURL": "http://localhost:8317/v1",
      "apiKey": "dummy-not-used",
      "provider": "openai"
    },
    {
      "modelDisplayName": "Gemini 2.5 Pro",
      "model": "gemini-2.5-pro",
      "baseURL": "http://localhost:8317/v1",
      "apiKey": "dummy-not-used",
      "provider": "openai"
    },
    {
      "modelDisplayName": "Gemini 2.5 Flash",
      "model": "gemini-2.5-flash",
      "baseURL": "http://localhost:8317/v1",
      "apiKey": "dummy-not-used",
      "provider": "openai"
    },
    {
      "modelDisplayName": "AG Claude Sonnet 4.5 Thinking",
      "model": "gemini-claude-sonnet-4-5-thinking",
      "baseURL": "http://localhost:8317/v1",
      "apiKey": "dummy-not-used",
      "provider": "openai"
    },
    {
      "modelDisplayName": "Qwen3 Coder Plus",
      "model": "qwen3-coder-plus",
      "baseURL": "http://localhost:8317/v1",
      "apiKey": "dummy-not-used",
      "provider": "openai"
    },
    {
      "modelDisplayName": "Qwen3 Coder Flash",
      "model": "qwen3-coder-flash",
      "baseURL": "http://localhost:8317/v1",
      "apiKey": "dummy-not-used",
      "provider": "openai"
    },
    {
      "modelDisplayName": "Kimi K2.5",
      "model": "kimi-k2.5",
      "baseURL": "http://localhost:8317/v1",
      "apiKey": "dummy-not-used",
      "provider": "openai"
    },
    {
      "modelDisplayName": "GLM-4.7",
      "model": "glm-4.7",
      "baseURL": "http://localhost:8317/v1",
      "apiKey": "dummy-not-used",
      "provider": "openai"
    }
  ]
}
```

### Why Two Base URLs?

- Use `http://localhost:8317` with `provider: "anthropic"` for direct Claude models.
- Use `http://localhost:8317/v1` with `provider: "openai"` for GPT, Gemini, Qwen, Kimi, GLM, and Antigravity-backed aliases.

## Step 3: Start Droid

Launch Factory:

```bash
droid
```

Inside Droid:

```text
/model
```

Pick one of the custom entries you added, then start working normally.

## Recommended Models

### Claude

- `claude-opus-4-5-20251101`
- `claude-opus-4-5-20251101-thinking-10000`
- `claude-sonnet-4-5-20250929`
- `claude-sonnet-4-5-20250929-thinking-4000`

### OpenAI / Codex

- `gpt-5.3-codex`
- `gpt-5.3-codex(high)`
- `gpt-5.4`

### Gemini

- `gemini-3-pro-preview`
- `gemini-3-pro-image-preview`
- `gemini-2.5-pro`
- `gemini-2.5-flash`
- `gemini-claude-sonnet-4-5-thinking`

### Qwen

- `qwen3-coder-plus`
- `qwen3-coder-flash`

### Kimi

- `kimi-k2.5`

### Z.AI GLM

- `glm-4.7`
- `glm-4-plus`
- `glm-4-air`
- `glm-4-flash`

## Thinking Models

VibeProxy supports Claude thinking aliases by suffix:

- `-thinking-4000`
- `-thinking-10000`
- `-thinking-32000`

Examples:

- `claude-sonnet-4-5-20250929-thinking-4000`
- `claude-opus-4-5-20251101-thinking-10000`

VibeProxy strips the suffix, adds the correct Claude thinking payload, and forwards the request automatically.

## Verification Checklist

1. VibeProxy menu bar status is green.
2. The providers you need show connected accounts in VibeProxy settings.
3. `~/.factory/settings.json` contains valid `customModels`.
4. `droid` shows your custom models in `/model`.
5. A simple prompt works without a 401 or 404.

## Troubleshooting

### Factory shows no custom models

- Confirm you edited `~/.factory/settings.json`, not an older `config.json`.
- Validate the JSON format.
- Restart Droid after saving changes.

### Factory shows 401 or unauthorized

- The matching provider is not connected in VibeProxy.
- Reconnect that provider in VibeProxy settings.
- If the provider is disabled, re-enable it.

### Factory shows 404

- Make sure VibeProxy is running.
- Check that Claude models use `http://localhost:8317`.
- Check that OpenAI-compatible models use `http://localhost:8317/v1`.

### Gemini 2.5 models fail

- Gemini 2.5 depends on the Gemini provider, not Antigravity.
- Reconnect Gemini if your Google Cloud project changed.

### Kimi model fails

- Make sure **Kimi Code** is connected in VibeProxy.
- Re-run the Kimi login from VibeProxy settings if the auth expired.

### GLM model fails

- Add a valid Z.AI API key in VibeProxy settings.
- Restart VibeProxy if you added the key while Factory was already running.

## Notes

- Provider toggles in VibeProxy immediately affect which model families are available.
- You can stay logged into multiple providers and selectively disable the ones you do not want Droid to use.
- Zen is not part of this guide yet.

## References

- [Factory CLI docs](https://docs.factory.ai/cli)
- [VibeProxy README](README.md)
- [Amp CLI setup](AMPCODE_SETUP.md)
