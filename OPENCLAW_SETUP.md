# OpenClaw Agent Guide

Use OpenClaw against VibeProxy on your Mac so OpenClaw agents default to Antigravity-backed Claude 4.6 aliases first, while keeping direct Claude overrides available when you explicitly want them.

## Goal

The intended setup is:

- OpenClaw agents use `ag-claude-opus-4-6` first
- cheaper or fallback runs use `ag-claude-sonnet-4-6`
- Claude Code and intentional direct-Claude work can still use `cc-claude-opus-4-6` or `cc-claude-sonnet-4-6`

This protects your direct Anthropic Claude quota by spending the Antigravity-backed route first inside OpenClaw.

## What The OpenClaw Preset Adds

When you enable the **OpenClaw** section in VibeProxy Settings, VibeProxy generates:

- a localhost-only API key for OpenClaw
- Antigravity-first Claude aliases:
  - `ag-claude-opus-4-6`
  - `ag-claude-sonnet-4-6`
- direct Claude override aliases:
  - `cc-claude-opus-4-6`
  - `cc-claude-sonnet-4-6`

This keeps the normal Claude provider enabled. OpenClaw can prefer the `ag-*` models, while Claude Code and any intentional direct overrides can still use `cc-*`.

## Endpoints

- OpenAI-compatible endpoint: `http://localhost:8317/v1`
- Anthropic-compatible endpoint: `http://localhost:8317`

## Recommended Routing Strategy

Use these model IDs inside OpenClaw:

- Primary agent model: `ag-claude-opus-4-6`
- Lower-cost fallback: `ag-claude-sonnet-4-6`
- Direct Anthropic override: `cc-claude-opus-4-6`
- Direct Anthropic fallback: `cc-claude-sonnet-4-6`

That setup lets OpenClaw spend your Antigravity-backed Claude quota first, so your direct Anthropic Claude Code quota stays available for Claude Code and work coding sessions.

## Agent Setup Pattern

If you want the cleanest OpenClaw setup, create two custom providers inside OpenClaw:

1. A default OpenAI-compatible provider pointed at `http://localhost:8317/v1`
2. A direct-override Anthropic-compatible provider pointed at `http://localhost:8317`

Then use them like this:

- Everyday coding agent: `ag-claude-opus-4-6`
- Lower-cost coding agent: `ag-claude-sonnet-4-6`
- Emergency or intentional direct-Claude agent: `cc-claude-opus-4-6`
- Lower-cost direct-Claude agent: `cc-claude-sonnet-4-6`

In practice, most people only need to keep one agent defaulted to `ag-claude-opus-4-6` and switch models only when they deliberately want a cheaper run or a direct-Claude override.

## Step 1: Enable The Preset In VibeProxy

1. Launch `VibeProxy.app`.
2. Open **Settings** from the menu bar.
3. Scroll to **OpenClaw**.
4. Turn on **Enable OpenClaw preset**.
5. Copy the generated local API key.

## Step 2: Add Your Main OpenClaw Agent Provider

Point this provider at VibeProxy's OpenAI-compatible localhost endpoint:

- Base URL: `http://localhost:8317/v1`
- API key: the generated key from VibeProxy
- Recommended default model: `ag-claude-opus-4-6`

If you prefer the OpenClaw onboarding CLI, the equivalent command is:

```bash
openclaw onboard \
  --auth-choice custom-api-key \
  --custom-compatibility openai \
  --custom-base-url http://localhost:8317/v1 \
  --custom-api-key "$VIBEPROXY_OPENCLAW_KEY" \
  --custom-model-id ag-claude-opus-4-6 \
  --custom-provider-id vibeproxy-openai
```

Use this provider for most agentic work. It exposes the Antigravity-first aliases along with the rest of VibeProxy's OpenAI-compatible model surface.

## Step 3: Add A Direct-Claude Override Provider

Point this provider at VibeProxy's Anthropic-compatible localhost endpoint:

- Base URL: `http://localhost:8317`
- API key: the same generated key
- Recommended direct override model: `cc-claude-opus-4-6`

CLI equivalent:

```bash
openclaw onboard \
  --auth-choice custom-api-key \
  --custom-compatibility anthropic \
  --custom-base-url http://localhost:8317 \
  --custom-api-key "$VIBEPROXY_OPENCLAW_KEY" \
  --custom-model-id cc-claude-opus-4-6 \
  --custom-provider-id vibeproxy-anthropic
```

Only switch to the `cc-*` models when you explicitly want direct Claude routing instead of Antigravity-first routing.

## Recommended Agent Defaults

Use this playbook if you want OpenClaw to route however it wants while still protecting direct Claude quota:

| Use case | Provider type | Model |
| --- | --- | --- |
| Main coding agent | OpenAI-compatible via VibeProxy | `ag-claude-opus-4-6` |
| Cheaper fallback agent | OpenAI-compatible via VibeProxy | `ag-claude-sonnet-4-6` |
| Direct Claude override | Anthropic-compatible via VibeProxy | `cc-claude-opus-4-6` |
| Direct Claude cheap override | Anthropic-compatible via VibeProxy | `cc-claude-sonnet-4-6` |

If OpenClaw supports multiple agent profiles in your workflow, make the `ag-*` models the defaults and keep the `cc-*` models as explicit secondary choices rather than the default route.

## Model Notes

- `ag-claude-opus-4-6` aliases `claude-opus-4-6-thinking` from the Antigravity provider.
- `ag-claude-sonnet-4-6` aliases Antigravity `claude-sonnet-4-6`.
- `cc-claude-opus-4-6` aliases direct Claude `claude-opus-4-6`.
- `cc-claude-sonnet-4-6` aliases direct Claude `claude-sonnet-4-6`.

VibeProxy does not expose Kimi's first-party product modes like `K2.5 Instant`, `K2.5 Agent`, or `K2.5 Agent Swarm` as separate OpenClaw model IDs. Through VibeProxy, Kimi is exposed as raw model IDs such as `kimi-k2.5`.

## Verify It Works

With VibeProxy running:

```bash
curl -s http://localhost:8317/v1/models \
  -H "Authorization: Bearer $VIBEPROXY_OPENCLAW_KEY"
```

Look for:

- `ag-claude-opus-4-6`
- `ag-claude-sonnet-4-6`
- `cc-claude-opus-4-6`
- `cc-claude-sonnet-4-6`

If those aliases appear, OpenClaw can route through them.

## Quick Start

Once the preset is enabled and both providers are added in OpenClaw:

1. Set your main agent to `ag-claude-opus-4-6`
2. Keep `ag-claude-sonnet-4-6` as the cheaper alternative
3. Only switch to `cc-claude-opus-4-6` or `cc-claude-sonnet-4-6` when you explicitly want direct Claude routing

That gives you the behavior you asked for: OpenClaw spends Antigravity-backed Claude quota first, while your direct Anthropic Claude quota stays available for Claude Code and work.

## Troubleshooting

- If the aliases do not appear, make sure the OpenClaw preset is enabled in VibeProxy Settings.
- If OpenClaw cannot connect, confirm VibeProxy is running and listening on `http://localhost:8317`.
- If Antigravity-backed aliases fail, verify you have an active Antigravity account connected in VibeProxy.
- If `cc-*` aliases fail, verify you have an active Claude account connected in VibeProxy.
