# Hermes — AgroBook Native Workers

This directory contains **Hermes-native workers** built specifically for the AgroBook project. These are not upstream `knowledge-work-plugins` skills — they are original AgroBook-specific governance tools adapted from the upstream pattern library.

## What Is This

`hermes/` is a new plugin domain in the AgroBook fork of `knowledge-work-plugins`. It holds workers for the Hermes internal operations assistant, designed for:

- AgroBook feature spec production
- Hermes H-phase governance workflows
- Owner decision packages and review prompts

**These workers are admin/Hermes governance only — never farmer-facing.**

## What This Directory Does NOT Include

- No `.mcp.json` — Hermes-native workers do not activate external MCP connectors
- No `commands/` — no slash-command auto-triggers beyond the `description` field
- No `.claude-plugin/` — these workers are not listed in the marketplace
- No external connectors, hooks, agents, or secrets

## Skills

| Skill | Path | Description |
|-------|------|-------------|
| Product Spec Worker | `skills/product-spec-worker/` | Produces bounded AgroBook feature specs before any implementation begins |

## Attribution

All content in `hermes/` is original AgroBook-specific work.
Structural patterns were inspired by the upstream `product-management` plugin (Apache-2.0, Anthropic).
See `skills/product-spec-worker/ATTRIBUTION.md` for full attribution.

## Activation History

| Phase | What happened |
|-------|--------------|
| H28 | Product Spec Worker proposal approved (AgroBook repo) |
| H29 | Apache-2.0 license gate passed |
| H30 | Local scaffold created (`.hermes/workers/product-spec-worker/`) |
| H31 | Template pointer activated |
| H32 | Active Claude skill installed (`~/.claude/skills/product-spec-worker/`) |
| **H33 (this)** | Packaged into Hermes fork (`hermes/skills/product-spec-worker/`) |
