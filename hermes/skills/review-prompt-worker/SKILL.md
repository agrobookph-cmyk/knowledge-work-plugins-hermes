---
name: review-prompt-worker
description: Use when drafting independent review prompts for AgroBook H-phases, PR audits, skill installs, fork packages, safety-boundary checks, and owner commit decision packages before any commit/push/merge. Triggers on /review, Hxx-R, "independent review", "verify safe to commit", "owner decision package", "pre-merge review", "audit this phase".
---

# Hermes Review Prompt Worker — Skill Definition

## Name

`Hermes Review Prompt Worker`
Short ID: `review-prompt-worker`
Suggested trigger: `/review`
Status: ACTIVE — installed H38-REVIEW-PROMPT-WORKER-ACTIVE-SKILL-INSTALL-2026-06-26

---

## Status

ACTIVE — installed as a user-level Claude Code skill at:
`C:\Users\agrob\.claude\skills\review-prompt-worker\`

Source scaffold: `.hermes/workers/review-prompt-worker/` (H37-approved)
Install phase: H38 (H37-R-approved scaffold; Apache-2.0 pattern-only per H36)
Next gate: H38-R independent review required before use in production commit decisions.

---

## Purpose

The Review Prompt Worker generates complete, consistent independent review prompts before any AgroBook H-phase commit, fork push, active skill install, or PR merge. Every review prompt produced by this worker covers all required checks in the same order and format, eliminating the drift that occurs when review prompts are written from scratch each time.

**It is admin / Hermes governance only. It is never farmer-facing.**

---

## Step 0 — Compact First

Every invocation begins with:

> "Compact the context first, then continue with only the current AgroBook task."

This instruction is always emitted first, before any review prompt content.

---

## Triggers

| Trigger | Type |
|---------|------|
| `/review` | Primary slash command (when activated as skill) |
| `"write review prompt"` | Plain text |
| `"Hxx-R"` | Phase review shorthand |
| `"independent review"` | Plain text |
| `"pre-merge review"` | Plain text |
| `"audit this phase"` | Plain text |
| `"verify safe to commit"` | Plain text |
| `"owner decision package"` | Owner decision format only (abbreviated output) |

---

## Inputs

| Input | Format | Required |
|-------|--------|----------|
| Task description / phase name | Free text (e.g. "H37 local scaffold") | Required |
| Reported facts | Bullet list of what the task claimed to do | Required |
| Files changed | List of exact file paths | Required |
| Task type | docs-only / scaffold / skill-install / fork-package / app-change | Required |
| Fork touched? | YES / NO | Required when fork may be in scope |
| Active skill touched? | YES / NO | Required when active skill may be in scope |

---

## Default Workflow

When invoked, the worker runs these steps in order:

1. **Emit compact instruction.** "Compact the context first, then continue with only the current AgroBook task."
2. **Classify the task.** Identify: docs-only / scaffold / skill-install / fork-package / app-change / governance. This determines which output sections are required.
3. **Build the review prompt header.** Task ID, context summary, reported facts, review goal.
4. **List required checks.** File scope check, no-runtime check, no-secrets check, task-specific checks.
5. **Build the file-lock checklist.** Each expected file with what change is expected.
6. **Build the validation command block.** AgroBook repo first; fork repo separately if touched; user-level skill path separately if touched.
7. **Build the safety grep block.** Full pattern. All scopes that were touched.
8. **Build the false-positive classification section.** Table with all expected hits classified.
9. **Build the forbidden activation path checks.** MCP, hooks, commands, agents, connectors, .claude-plugin, active skill, fork write.
10. **Build the owner staging commands.** `git add [exact-file]` only. Never `git add .`
11. **Build the must-not-stage list.** All pre-existing dirty files and other-lane files.
12. **Build the verdict format.** Three options only (see §Verdict Format).
13. **Apply Ponytail default.** Append: "Use Ponytail for this task's review/cleanup workflow where applicable; default to /ponytail full unless the task scope requires a smaller safe check."
14. **Apply Blueprint Rule.** When reviewing tasks that touch docs or governance: check ICM folder contract alignment.
15. **State the owner boundary.** End every prompt with: "Owner-only commit/push/deploy. No Claude commit/push/deploy."

---

## Required Output Sections (all 10 required — write N/A if not applicable)

### Output 1 — Review Prompt Header

```markdown
Task: [Task ID] — Independent Review of [Task Name]

Context:
[Task description] created only:
- [file 1]
- [file 2]

[Task description] also updated:
- [governance file 1]

Reported facts:
- [key fact 1]
- [key fact 2]

Review goal:
Verify [Task ID] is safe to commit as [task type] only.
```

### Output 2 — Required Checks Block

```markdown
Required checks:
1. Confirm changed files are only: [exact list]
2. Confirm no app/runtime/backend/source files changed.
3. Confirm no .env, secrets, package files, DB/schema, or production config changed.
4. [task-specific check: scaffold exists / active skill absent / fork absent]
5. [task-specific check: MCP/hooks/commands/agents/connectors absent]
6. [task-specific check: content audit of created files]
```

### Output 3 — File-Lock Checklist

```markdown
File-lock checklist:
- [ ] [expected file 1] — [what change is expected]
- [ ] [expected file 2] — [what change is expected]
- [ ] No file outside this list appears in git diff --name-only
```

### Output 4 — Validation Command Block

```powershell
# AgroBook repo
cd C:\Users\agrob\Agrobook
git status --short
git diff --stat
git diff --name-only
git diff -- [file1]
git diff -- [file2]

# Fork repo (ONLY if fork was touched)
cd C:\Users\agrob\Agrobook-tools\knowledge-work-plugins-hermes
git status --short
git diff --stat
git diff --name-only

# User-level skill (ONLY if active skill was touched)
Test-Path C:\Users\agrob\.claude\skills\[skill-name]
Get-ChildItem -Recurse C:\Users\agrob\.claude\skills\[skill-name]
Get-FileHash C:\Users\agrob\.claude\skills\[skill-name]\SKILL.md
```

### Output 5 — Safety Grep Block

```powershell
# AgroBook governance
rg -n "sk-|api[_-]?key|token|secret|password|DATABASE_URL|SUPABASE|PRIVATE|BEGIN RSA|BEGIN OPENSSH|refresh_token|client_secret|GMAIL_API_KEY" .hermes docs -S

# Scaffold files (if created)
rg -n "sk-|api[_-]?key|token|secret|password|DATABASE_URL|SUPABASE|PRIVATE|BEGIN RSA|BEGIN OPENSSH|refresh_token|client_secret|GMAIL_API_KEY" .hermes/workers/[skill-name] -S

# Fork path (if touched)
rg -n "sk-|api[_-]?key|token|secret|password|DATABASE_URL|SUPABASE|PRIVATE|BEGIN RSA|BEGIN OPENSSH|refresh_token|client_secret|GMAIL_API_KEY" [fork-path] -S

# Active skill path (if touched)
rg -n "sk-|api[_-]?key|token|secret|password|DATABASE_URL|SUPABASE|PRIVATE|BEGIN RSA|BEGIN OPENSSH|refresh_token|client_secret|GMAIL_API_KEY" C:\Users\agrob\.claude\skills\[skill-name] -S
```

### Output 6 — False-Positive Classification

```markdown
Classify all safety grep hits:

| Pattern match | File | Line | Classification |
|--------------|------|------|---------------|
| [pattern] | [file] | [line] | [policy prose / boundary list / established false positive] ✅ |

GMAIL_API_KEY in product-spec-worker fixtures Fixture D: established false positive (refusal example) — do not re-raise as HOLD.

Real credential found: HOLD — [exact file:line]
No real credentials: CONFIRMED ✅
```

### Output 7 — Forbidden Activation Path Checks

```markdown
Forbidden path checks:
- MCP: [NO_MCP_JSON / FOUND — HOLD]
- commands/: [NO_COMMANDS / FOUND — HOLD]
- hooks/: [NO_HOOKS / FOUND — HOLD]
- agents/: [NO_AGENTS / FOUND — HOLD]
- .claude-plugin/: [NO_CLAUDE_PLUGIN / FOUND — HOLD]
- .env/secrets: [NONE / FOUND — HOLD]
- Active skill (~/.claude/skills/[name]/): [ABSENT / FOUND — check if expected]
- Fork (hermes/skills/[name]/): [NOT EXECUTED / FOUND — check if expected]
- upstream packages modified: [NONE / FOUND — HOLD]
```

### Output 8 — Owner Staging Commands

```markdown
### Exact owner staging commands

cd C:\Users\agrob\Agrobook
git add [exact file 1]
git add [exact file 2]
git diff --cached --name-only   # confirm exact file list
git commit -m "[commit message]"
```

### Output 9 — Files Owner Must NOT Stage

```markdown
### Files owner must NOT stage

[file or pattern]: [reason]

Never git add . in AgroBook or fork repo.
```

### Output 10 — Verdict Format

```markdown
## [Task ID]-R Verdict

[One of exactly three options:]

APPROVED TO COMMIT — [task type] only
  — OR —
APPROVED FOR OWNER REVIEW — [condition]
  — OR —
HOLD — [exact blocker: file:line or path]

Changed files: [list]
Safety grep: [ZERO REAL CREDENTIALS / HOLD — file:line]
Runtime impact: [NOT TOUCHED]
Connector status: [NONE ACTIVATED]
Fork write: [NOT EXECUTED / LOCAL ONLY — not pushed]
Active skill: [NOT INSTALLED / INSTALLED — expected in H3x]
Build required: [NOT REQUIRED / PASS / FAIL]
```

---

## Ponytail Default

Include in every output:

> "Use Ponytail for this task's review/cleanup workflow where applicable; default to /ponytail full unless the task scope requires a smaller safe check."

---

## Blueprint Rule

When reviewing any task that touches `.hermes/`, `docs/`, or governance files:

> "Verify ICM folder contract alignment: check that changed files are in the correct ICM lane as defined in `docs/governance/agrobook-icm-folder-contract.md`."

---

## Safety Boundaries (Hard Blocks)

These are hard blocks. The worker must refuse or halt if any of the following is present:

| Block | Rule |
|-------|------|
| No invented evidence | Never assert "build passes", "tests pass", "hash matches", or "discovery confirmed" without observed command output. Mark untestable items UNABLE TO TEST. Never invent screenshots or CLI output. |
| No commit/push/deploy by Claude | Every output must end with "Owner-only commit/push/deploy. No Claude commit/push/deploy." Every staging section must use `git add [exact-file]`, never `git add .` |
| No secrets | Never include, reference, or request API keys, tokens, passwords, DATABASE_URL, Replit Secrets, or credentials in any output |
| No MCP | Never reference, activate, or configure MCP server in any output |
| No hooks | Never create or invoke hooks in any output |
| No connectors | Never reference, activate, or configure external connectors in any output |
| No production DB | Never query or reference live AgroBook database |
| No PII | Never include farmer names, GPS, phone, or financial data in review prompts |
| No changes outside file lock | Every review prompt must list expected files and name at least one forbidden path category |
| Compact context first | Every output begins with or notes: "Compact the context first, then continue with only the current AgroBook task." |
| Ponytail default | Append Ponytail instruction for every task that involved implementation |
| Blueprint Rule | Apply for tasks that touch docs or governance paths |
| Fork + AgroBook repo separation | If both repos were touched, provide separate command blocks for each |
| User-level skill path explicit | If active skill is in scope: require `Test-Path ~/.claude/skills/[name]` and hash comparison |
| No unsafe MemPalace content | Never auto-promote to approved lessons; only capture lesson candidates |

---

## When to Produce an Abbreviated "Owner Decision Package"

If the trigger is `"owner decision package"` or the task is low-complexity docs-only, produce the abbreviated format (Output 8 + 9 + 10 only):

```markdown
# Owner Decision Package — [Task Name]

**Task ID:** [ID]
**Date:** [YYYY-MM-DD]
**Status:** READY FOR OWNER DECISION

## What was done
- [File]: [what changed and why, one line]

## What must not be staged
- [file path]: [reason]

## Exact staging commands
git add [exact file path]
git diff --cached --name-only
git commit -m "[message]"

## Rollback
git rm [new files]
git checkout -- [modified files]

## Verdict options
- APPROVED — proceed with commit
- HOLD — [specify what must change]
- BLOCKED — [specify what is unsafe]

Owner-only commit/push/deploy. No Claude commit/push/deploy.
Never git add .
```

---

## Non-Goals

This worker does **not**:

- Execute builds or tests
- Write application code
- Run git commands on behalf of the owner
- Connect to external systems
- Access the production AgroBook database
- Access farmer PII
- Make commits, pushes, or deployments
- Activate MCP servers, hooks, agents, or connectors
- Replace the owner's judgment on commit/push decisions
- Auto-approve any H-phase
- Write to the Hermes fork
- Install itself as an active skill
- Guarantee that a phase is safe — only the reviewer can do that
- Auto-promote to MemPalace approved lessons
