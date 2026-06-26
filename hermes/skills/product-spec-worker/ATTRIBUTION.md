# Attribution

This Hermes worker was inspired by Anthropic's knowledge-work-plugins /
product-management plugin patterns.

Upstream project: https://github.com/anthropics/knowledge-work-plugins
License: Apache License 2.0 (SPDX: Apache-2.0)
Copyright holder: Anthropic

No upstream source code, prompt text, MCP server configurations, hooks, agents,
or runtime integrations were copied verbatim. All skill files in this directory
are original AgroBook-specific content written for the Hermes governance layer.

---

## Adaptation Details

| Field | Value |
|-------|-------|
| Adaptation type | Pattern inspiration only |
| Upstream code copied | None |
| Upstream prompt text copied | None |
| MCP servers from upstream | None |
| Hooks from upstream | None |
| Agents from upstream | None |
| Runtime integrations from upstream | None |
| Fork write executed | H33 — local package only; push is owner-only |

---

## What Was Adapted (Pattern Only)

The following structural patterns from the `product-management` plugin informed the layout:

- The `write-spec` skill's field-layout pattern (title → problem → acceptance
  criteria → out-of-scope → validation steps)
- The `sprint-planning` skill's context-compact + task-boundary discipline
- The `stakeholder-update` skill's "what changed and what's next" summary format

All field names, wording, section content, safety rules, file-lock behavior,
AgroBook-specific entities (farmer, listing, harvest, verification, H-phase,
Ponytail review), and governance conventions are **original AgroBook content**.

---

## Changes Made from the Upstream Pattern

- Field names rewritten for AgroBook entities (farmer, listing, harvest,
  verification, H-phase, Ponytail review)
- Output format adapted to Hermes H7/H11 evidence pack conventions
- Hard safety blocks added (no secrets, no production DB, no external connectors,
  no commit/push/deploy by Claude, no invented evidence, no changes outside
  file lock)
- Lessons for Hermes / MemPalace section added
- Ponytail review instruction embedded as default
- Owner commit/deploy boundary made explicit in every spec output
- AgroBook blueprint mapping (ICM folder contract) added as required field
- YAML frontmatter added for Claude Code active skill discovery (H32)
- No CONNECTORS.md reference — Hermes workers do not use external connectors
- Packaged into fork `hermes/` domain (H33) — no `.mcp.json`, no `commands/`

---

## NOTICE File Status

No NOTICE file was found in the upstream `anthropics/knowledge-work-plugins`
repository as of 2026-06-26 (HTTP 404 — confirmed via `gh api` during H29
license review). No additional attribution text is required beyond the
standard Apache-2.0 attribution above.

---

## License Gate Reference

H29 (`HERMES_PRODUCT_SPEC_WORKER_H29_LICENSE_DECISION.md` in AgroBook repo)
confirmed Apache-2.0 via live `gh` CLI queries:
- No NOTICE file obligations
- Pattern-only use with original wording = minimal attribution obligation
- Attribution obligation satisfied by this file

H33 fork packaging does not change the license status.
