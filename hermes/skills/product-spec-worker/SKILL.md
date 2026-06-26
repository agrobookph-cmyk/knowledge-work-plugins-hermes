---
name: product-spec-worker
description: "Use when drafting AgroBook feature specs, acceptance criteria, file locks, validation plans, review prompts, or owner decision packages — before any implementation task begins. Trigger: /spec or any request to plan, spec, or scope an AgroBook task."
---

# Hermes Product Spec Worker

Use this skill when:
- The owner asks to spec, plan, or scope an AgroBook feature or bug fix
- A task needs bounded acceptance criteria before implementation begins
- An implementation agent needs explicit file locks and forbidden-path rules
- An H-phase or governance task needs a docs-only spec
- A review prompt is needed for independent H-style validation
- An owner decision package is needed before a gate-approval commit

This is an **admin/Hermes governance skill only** — never farmer-facing.

> This worker does not use external connectors. It does not reference CONNECTORS.md.
> No MCP servers, hooks, commands, agents, or external APIs are activated by this skill.

---

## Step 0 — Compact and Frame

Before anything else, emit:

> "Compact the context first, then continue with only the current AgroBook task."

Then classify the task type: feature / bug fix / UI polish / governance / docs-only.

---

## Default Workflow

Run these steps in order for every invocation:

1. **Compact and frame.** Emit the compact instruction above.
2. **Classify the task.** Determine: feature / bug fix / UI polish / governance / docs-only. Note which AgroBook subsystem is affected.
3. **Propose a file lock.** List only the files that *must* change. Note files that *must not* change (forbidden paths). Check against the ICM folder contract (`docs/governance/agrobook-icm-folder-contract.md`).
4. **Draft acceptance criteria.** Each criterion must be pass/fail testable. Assign a test type: visual / build / runtime / docs-review.
5. **Draft the feature spec.** See Spec Output Format below.
6. **Draft the validation checklist.** Steps the implementation agent runs before handing off.
7. **Apply Ponytail review instruction.** Append: "Use Ponytail for this task's review/cleanup workflow. Default: `/ponytail full` unless the task scope requires a smaller safe check."
8. **Add Lessons candidate section.** Append a placeholder: "Lessons for Hermes / MemPalace: [capture what was non-obvious about this task after it completes]."
9. **State the owner boundary.** End every spec with: "Owner-only commit/push/deploy. No Claude commit/push/deploy."

---

## Spec Output Format

Every spec must include all sections below. Blank sections are not permitted — write N/A if not applicable.

```markdown
# [Task Title]

**AgroBook area:** [Farmer Assistant / Map UI / Neural Control / Hermes H-phase / etc.]
**Priority:** [P1 / P2 / P3]
**Type:** [Feature / Bug fix / UI polish / Governance / Docs-only]
**Status:** SPEC DRAFT — owner approval required before implementation

---

## Problem

[One paragraph. What breaks or is missing?]

---

## Scope

### In scope
- [Specific change 1]

### Out of scope (explicit)
- [What this task must NOT do]

---

## File Lock

### Files this task may create or modify
- `path/to/file.ts` — [reason]

### Forbidden paths (must not touch)
- `artifacts/` — production app; separate gated H-phase required
- `lib/db/src/schema/` — schema changes require migration approval
- `.env` / Replit Secrets — secrets are owner-only
- `tools/neural-control/data/*.json` — runtime data; not in scope

---

## Blueprint Mapping

[Which section of docs/governance/agrobook-icm-folder-contract.md does this change touch?]
[Which ICM folder or lane owns this area?]

---

## Acceptance Criteria

| # | Criterion | Test type | Pass condition |
|---|-----------|-----------|----------------|
| AC-1 | [criterion] | [visual / build / runtime / docs-review] | [what must be true] |

---

## Validation Checklist

Before producing a review packet, the implementation agent must run:

- [ ] `git status --short` — confirm only file-locked paths changed
- [ ] `git diff --name-only` — no forbidden paths in output
- [ ] `npm run build` in `artifacts/agriescrow` if frontend touched — must PASS
- [ ] `npm run build` in `artifacts/api-server` if backend touched — must PASS
- [ ] Safety grep: `rg -n "sk-|api[_-]?key|token|secret|password|DATABASE_URL" .hermes docs -S` — zero real credentials
- [ ] All acceptance criteria checked — PASS / FAIL / UNABLE TO TEST
- [ ] No connector activated, no fork write, no commit/push/deploy by Claude

---

## Ponytail Review Instruction

Use Ponytail for this task's review/cleanup workflow. Default: `/ponytail full` unless the task scope requires a smaller safe check.

---

## Owner Boundary

Owner-only commit/push/deploy. No Claude commit/push/deploy.

Implementation agent produces a review packet (H7 format). Owner reviews, approves, and commits.

---

## Lessons for Hermes / MemPalace

[After this task completes, capture what was non-obvious: a hidden constraint, a subtle invariant, a design decision that would surprise a future agent. Only durable lessons go here — not task-specific steps.]
```

---

## Safety Boundaries

These are hard blocks. Refuse or halt if any of the following is requested:

| Block | Rule |
|-------|------|
| Secrets / credentials | Never include, reference, or request API keys, tokens, passwords, DATABASE_URL, Replit Secrets, or any credential |
| Production DB | Never write to or read from the live AgroBook database |
| External connectors | Never activate MCP servers, OAuth flows, external APIs, or webhooks |
| Commit / push / deploy | Never commit, push, or deploy. Always emit exact staging commands for the owner |
| Runtime changes | Never modify `artifacts/`, `tools/neural-control/src/`, `lib/`, or any production source unless the task explicitly requests it |
| Changes outside file lock | Never touch files not listed in the spec's File Lock section |
| Invented evidence | Never assert "build passes" or "mobile verified" without running the check. Mark untestable items UNABLE TO TEST |
| Farmer PII | Never include farmer names, GPS, phone numbers, transaction records, or any PII |

---

## File-Lock Rules

1. **Smallest safe set.** Only list files that must change. One file is better than five.
2. **Forbidden paths are explicit.** Every spec must name at least one forbidden path category.
3. **No glob locks.** Lock specific files, not `artifacts/**`.
4. **Blueprint check.** Verify proposed files are in the correct ICM lane.
5. **Flag lane crossings.** Multiple ICM lanes = propose task split.

---

## Validation Requirements

An implementation agent receiving a spec from this worker must satisfy all of the following before producing an H7 review packet:

1. **Build passes** if any source file in `artifacts/` was touched.
2. **File lock respected** — `git diff --name-only` shows only file-locked paths.
3. **Forbidden paths absent** — no forbidden file appears in diff.
4. **Safety grep clean** — zero real credentials.
5. **Acceptance criteria checked** — each AC has a result: PASS, FAIL, or UNABLE TO TEST.
6. **No commit by Claude** — review packet includes exact staging commands for the owner.

---

## Review Prompt Format

When a review prompt is requested:

```markdown
Task: [Task ID] — Independent Review of [Task Name]

Context:
[Task name] created only: [list of files created]
[Task name] also updated: [list of governance files updated]

Reported facts:
- [key fact 1]
- [key fact 2]

Review goal:
Verify [Task ID] is safe to commit as [task type] only.

Required checks:
1. Confirm changed files are only: [list]
2. Confirm no app/runtime/backend/source files changed.
3. Confirm no .env, secrets, packages, DB/schema, or production config changed.
4. [additional task-specific checks]

Run:
git status --short
git diff --stat
git diff --name-only
[task-specific diffs]

Safety grep:
rg -n "sk-|api[_-]?key|token|secret|password|DATABASE_URL|SUPABASE|PRIVATE|BEGIN RSA|BEGIN OPENSSH|refresh_token|client_secret" .hermes docs -S

Final output:
- Verdict: APPROVED TO COMMIT or HOLD — exact blocker
- Changed files / Safety grep / Runtime impact / Connector status / Fork write status
- Exact owner staging commands / Exact files owner must not stage
```

---

## Owner Decision Package Format

```markdown
# Owner Decision Package — [Task Name]

**Task ID:** [ID]
**Date:** [YYYY-MM-DD]
**Status:** READY FOR OWNER DECISION

## What was done
- [File]: [what changed and why]

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
```

---

## Non-Goals

This skill does **not**:
- Execute implementation steps
- Write application code
- Run builds or tests
- Connect to external systems
- Access the production AgroBook database
- Access farmer PII
- Make commits, pushes, or deployments
- Activate MCP servers, hooks, agents, or connectors
- Replace the owner's judgment on scope
- Guarantee acceptance criteria will pass
- Auto-capture Lessons for Hermes / MemPalace
