# Hermes Product Spec Worker

**Status:** PACKAGED IN HERMES FORK
**Package path:** `hermes/skills/product-spec-worker/` (this directory)
**Active skill source:** `~/.claude/skills/product-spec-worker/` (user-level, H32)
**Local scaffold source:** `.hermes/workers/product-spec-worker/` (AgroBook repo, H30)
**Phase:** H33-PRODUCT-SPEC-WORKER-FORK-PACKAGE-2026-06-26
**License:** Apache-2.0 (pattern-only adaptation — see ATTRIBUTION.md)
**Runtime:** NOT TOUCHED
**Connectors:** NOT ACTIVATED
**MCP:** NOT INCLUDED

---

## Purpose

The Product Spec Worker produces compact, bounded feature specifications before any AgroBook implementation task begins. It is an internal Hermes / owner governance tool consumed by implementation agents (Claude, Codex, Gemini, local Qwen).

**This worker is admin/Hermes governance only — never farmer-facing.**

---

## What This Package Contains

```
hermes/skills/product-spec-worker/
  SKILL.md                                ← active skill definition (frontmatter + workflow)
  README.md                               ← this file
  ATTRIBUTION.md                          ← Apache-2.0 attribution (H29)
  tests/
    product-spec-worker-fixtures.md       ← expected-input/output examples; docs-only
```

---

## What This Package Does NOT Include

- No `.mcp.json`
- No `CONNECTORS.md`
- No `commands/` directory
- No hooks, agents, or external connector configs
- No secrets or credentials
- No runtime/app/backend code
- No package dependency changes

---

## Activation History

| Phase | What happened |
|-------|--------------|
| H28 | Product Spec Worker proposal approved (AgroBook repo) |
| H29 | Apache-2.0 license gate passed |
| H30 | Local scaffold created (`.hermes/workers/product-spec-worker/`) |
| H30-R | Scaffold review — APPROVED TO COMMIT |
| H31 | Activation decision — template pointer added to Hermes prompt template |
| H31-R | Review — APPROVED TO COMMIT |
| H32 | Active Claude skill installed (`~/.claude/skills/product-spec-worker/`) |
| H32-R | Review — APPROVED TO COMMIT |
| **H33 (this)** | Packaged into Hermes fork (`hermes/skills/product-spec-worker/`) |
