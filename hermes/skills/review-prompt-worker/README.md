# Hermes Review Prompt Worker

**Status:** FORK PACKAGE MIRROR — packaged for reusable Hermes skill distribution
**Phase:** H39-REVIEW-PROMPT-WORKER-FORK-PACKAGE-2026-06-26
**Source:** Active skill at `C:\Users\agrob\.claude\skills\review-prompt-worker\` (H38-installed)
**Proposal:** H35 (`PROP-20260626-002`) — `PROPOSED_FOR_OWNER_REVIEW_ONLY`
**License gate:** H36 — `APPROVED_FOR_H37_LOCAL_SCAFFOLD_ONLY` (Apache-2.0, no NOTICE file upstream)
**Active install gate:** H38 — installed to `~/.claude/skills/review-prompt-worker/`; H38-R approved
**Runtime:** NOT TOUCHED
**Connectors:** NOT ACTIVATED
**Fork write:** EXECUTED H39 — `hermes/skills/review-prompt-worker/` only (local; not pushed)
**Build required:** NO
**Install note:** This is a fork package mirror. Installing from this path requires explicit owner approval (H40 gate).

---

## Purpose

The Review Prompt Worker is an internal Hermes / owner tool that generates complete, consistent independent review prompts before any AgroBook commit, fork push, active skill install, or PR merge.

It addresses a recurring problem: every H-phase requires writing a review prompt from scratch. Without a worker, prompts drift — some miss the safety grep block, some omit the fork-write-status check, some omit "Never `git add .`". This worker generates all required sections in one step, guaranteeing consistency across H-phase reviews.

**This worker is never farmer-facing.** It is admin / Hermes governance layer only.

---

## What This Scaffold Is

This directory contains:

- A skill definition (`SKILL.md`) describing the worker's inputs, outputs, default workflow, and 10 required output sections
- Attribution documentation (`ATTRIBUTION.md`) covering the Apache-2.0 upstream pattern reference (H36)
- Fixture examples (`tests/review-prompt-worker-fixtures.md`) showing expected inputs and outputs

**This scaffold is not:**

- An active Claude Code skill installation
- A MCP server or connector
- A hook or automated trigger
- A command definition
- An agent definition
- Connected to any external system

**Owner approval is required before this scaffold is activated** as an installed skill. That activation step belongs to H38 with independent review and Gate 5 owner approval.

---

## What This Scaffold Is Not

- No `.mcp.json` exists or is planned before H38+ approval
- No `commands/` directory
- No `hooks/` configuration
- No `agents/` configuration
- No external connectors
- No secrets or credentials
- No runtime, app, or backend impact

---

## File List

```
.hermes/workers/review-prompt-worker/
  README.md                                   ← this file
  SKILL.md                                    ← worker definition: inputs, outputs, workflow, safety
  ATTRIBUTION.md                              ← Apache-2.0 upstream attribution (H36 gate)
  tests/
    review-prompt-worker-fixtures.md          ← expected-input/output examples; no runtime
```

---

## How to Use (current state — scaffold only)

The owner may read `SKILL.md` and paste its context block into a new Claude or Codex session when drafting an independent H-phase review prompt. This is a manual invocation — the worker is not auto-triggered.

Future activation as an installed skill requires:

1. H37-R — independent review of this scaffold (Gate 4)
2. Owner Gate 5 explicit approval for activation
3. H38 — a separate H-phase that installs the skill file into `~/.claude/skills/review-prompt-worker/`

---

## Relationship to Prior Phases

| Phase | What it did |
|-------|-------------|
| H35 | Created Review Prompt Worker proposal (`PROP-20260626-002`); no scaffold |
| H35-R | Independent review — APPROVED TO COMMIT |
| H36 | Confirmed Apache-2.0 license; confirmed no NOTICE file upstream; approved H37 scaffold |
| H36-R | Independent review — APPROVED TO COMMIT |
| **H37 (this)** | Creates local scaffold only — `README.md`, `SKILL.md`, `ATTRIBUTION.md`, `tests/` |
| H37-R | Independent review of H37 scaffold (Gate 4) |
| H38 | Owner activation decision (Gate 5) → active skill install → validation |
| H39 | Fork package mirror to `hermes/skills/review-prompt-worker/` |
| H40 | Fork install-path test (same pattern as H34) |

---

## Relationship to Product Spec Worker

| Dimension | Product Spec Worker | Review Prompt Worker |
|-----------|--------------------|--------------------|
| Purpose | Draft implementation spec before work begins | Draft independent review prompt before commit |
| Used when | About to start a task | About to commit/push/merge |
| Output | Feature spec with file lock, AC, validation checklist | Review prompt with commands, grep, verdict format |
| Trigger | `/spec`, "plan this", "spec this" | `/review`, "Hxx-R", "verify safe to commit" |
| Complementary | YES | YES |

Correct order for every AgroBook H-phase:

```
1. Product Spec Worker → spec + file lock
2. Implementation agent → execute within file lock
3. Review Prompt Worker → review prompt
4. Independent reviewer → verify
5. Owner → commit / push
```

---

## How H37-R Should Validate This Scaffold

An independent H37-R review must verify:

1. `git status --short` shows only `.hermes/workers/review-prompt-worker/` new files + workboard/evidence index updates
2. No `artifacts/`, `tools/neural-control/src/`, `lib/`, or production path was touched
3. No `~/.claude/skills/review-prompt-worker/` active skill was created
4. No fork `hermes/skills/review-prompt-worker/` was created
5. No `.env`, `package.json`, lockfile, or secret was modified
6. `SKILL.md` contains no MCP server config, no hook definitions, no external connector references, no credential placeholders
7. `ATTRIBUTION.md` contains the required Apache-2.0 attribution text from H36
8. `tests/review-prompt-worker-fixtures.md` contains only fixture examples — no executable code
9. Safety grep finds no real credentials in any new file
10. Workboard row and evidence index entries are accurate and do not include unrelated lanes
11. No commit was made by Claude
