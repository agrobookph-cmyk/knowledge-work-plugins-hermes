# Review Prompt Worker — Test Fixtures

**Status:** LOCAL SCAFFOLD ONLY — docs/fixture examples only; no executable code
**Phase:** H37-REVIEW-PROMPT-WORKER-LOCAL-SCAFFOLD-2026-06-26
**Purpose:** Provide expected-input/output examples for H37-R review and future owner validation.

These fixtures show what the worker should produce for common AgroBook task types. They are not runnable — they document expected behavior only.

---

## Fixture A — Docs-Only H-Phase Review Prompt

### Input

```
Task: H37 — Review Prompt Worker Local Scaffold

Reported facts:
- Created .hermes/workers/review-prompt-worker/ scaffold (4 files)
- Updated HERMES_AGENT_WORKBOARD.md (+1 H37 row)
- Updated HERMES_RELEASE_EVIDENCE_INDEX.md (+6 H37 rows)
- No active skill installed
- No fork write
- No MCP/hooks/commands/agents/connectors
- No app/runtime/backend changes
```

### Expected Worker Behavior

The worker generates a full 10-section independent review prompt for a docs-only scaffold phase.

### Expected Review Prompt Outline (abbreviated)

```markdown
Task: H37 — Independent Review of Review Prompt Worker Local Scaffold

Context:
H37 created only:
- .hermes/workers/review-prompt-worker/README.md
- .hermes/workers/review-prompt-worker/SKILL.md
- .hermes/workers/review-prompt-worker/ATTRIBUTION.md
- .hermes/workers/review-prompt-worker/tests/review-prompt-worker-fixtures.md

H37 also updated:
- .hermes/HERMES_AGENT_WORKBOARD.md (+1 H37 row)
- .hermes/HERMES_RELEASE_EVIDENCE_INDEX.md (+6 H37 rows)

Reported facts:
- H37 is local scaffold only — no active skill install
- No fork write
- No MCP, hooks, commands, agents, connectors, secrets, app code

Review goal:
Verify H37 is safe to commit as local scaffold only.

Required checks:
1. Confirm changed H37 files are only the 4 scaffold files + workboard + evidence index
2. Confirm no ~/.claude/skills/review-prompt-worker/ active skill exists
3. Confirm no fork hermes/skills/review-prompt-worker/ exists
4. Confirm no MCP/hooks/commands/agents/connectors created
5. Confirm no app/runtime/backend files changed
6. Inspect SKILL.md — no MCP config, no hook definitions, no credential placeholders
7. Inspect ATTRIBUTION.md — required Apache-2.0 text from H36 present
8. Inspect fixtures — docs-only, no executable code
9. Safety grep — classify all hits

Compact the context first, then continue with only the current AgroBook task.
Use Ponytail for this task's review/cleanup workflow; default to /ponytail full.

Verdict format:
APPROVED TO COMMIT — H37 local scaffold only
  — OR —
HOLD — [exact blocker]

Owner-only commit/push/deploy. No Claude commit/push/deploy.
Never git add .
```

### Safety Boundaries Applied

- Output 7 (forbidden paths): scaffold/skill/fork/MCP/hooks/commands checked
- Output 8 (staging): git add each scaffold file + workboard + index
- Output 9 (must-not-stage): RMAS/orphan-scans/neural-control data listed
- Output 10 (verdict): APPROVED TO COMMIT or HOLD with exact blocker

---

## Fixture B — App/UI Implementation Review Prompt

### Input

```
Task: H-phase: Add "You May Like" recommendation chips to FarmerAssistantCard.tsx

Reported facts:
- Modified artifacts/agriescrow/src/components/FarmerAssistant/FarmerAssistantCard.tsx
- Modified .hermes/HERMES_AGENT_WORKBOARD.md
- Modified .hermes/HERMES_RELEASE_EVIDENCE_INDEX.md
- npm run build passed in artifacts/agriescrow
- No backend touched
- No schema touched
- No secrets
```

### Expected Worker Behavior

The worker generates a review prompt for an app/UI change phase. Key differences from Fixture A:

- Validation command block includes `npm run build` result verification
- Safety grep scope expands to `.hermes docs artifacts/agriescrow/src`
- AC build result must be provided (not assumed)
- Verdict includes build pass confirmation

### Expected Review Prompt Outline (abbreviated)

```markdown
Task: [UI-phase] — Independent Review of FarmerAssistant Chip Feature

Required checks:
1. Confirm changed files are only: FarmerAssistantCard.tsx + workboard + index
2. Confirm no backend (artifacts/api-server/), schema (lib/db/), or .env changed
3. Confirm npm run build passes in artifacts/agriescrow — MUST HAVE COMMAND OUTPUT
4. Confirm AC visual checks: PASS / FAIL / UNABLE TO TEST (not assumed)
5. Confirm no MCP, hooks, connectors, secrets, fork write

Validation command block:
cd C:\Users\agrob\Agrobook
git diff --name-only
git diff -- artifacts/agriescrow/src/components/FarmerAssistant/FarmerAssistantCard.tsx

Safety grep:
rg -n "sk-|api[_-]?key|token|secret|..." .hermes docs artifacts/agriescrow/src -S

Build evidence required:
- npm run build output in artifacts/agriescrow — reviewer must see output, not claimed
- UNABLE TO TEST if not provided

Verdict:
APPROVED TO COMMIT — UI feature only
  — OR —
HOLD — build output not provided / forbidden path in diff / real credential found
```

### Safety Boundaries Applied

- "No invented evidence" block: build must be shown, not claimed
- UNABLE TO TEST required if build output is absent
- Staging limited to app file + governance files only
- Never git add .

---

## Fixture C — Fork Package Review Prompt

### Input

```
Task: H39 — Review Prompt Worker Fork Package

Reported facts:
- Created hermes/skills/review-prompt-worker/ in local fork clone
  (C:\Users\agrob\Agrobook-tools\knowledge-work-plugins-hermes)
- Fork not committed, not pushed
- 4 files created: SKILL.md, README.md, ATTRIBUTION.md, tests/fixtures.md
- Updated AgroBook HERMES_AGENT_WORKBOARD.md
- Updated AgroBook HERMES_RELEASE_EVIDENCE_INDEX.md
- No MCP, hooks, commands, agents, connectors
- No AgroBook app/runtime touched
```

### Expected Worker Behavior

Two separate command blocks — one for AgroBook repo, one for the fork repo. Never merged.

### Expected Review Prompt Outline (abbreviated)

```markdown
Task: H39 — Independent Review of Review Prompt Worker Fork Package

Required checks:
1. AgroBook: changed files are only workboard + evidence index
2. Fork: changed files are only hermes/skills/review-prompt-worker/ (4 files)
3. Fork NOT committed, NOT pushed
4. No .mcp.json in fork
5. No commands/, .claude-plugin/, hooks/ in fork
6. No MCP, connectors, secrets, app code

Validation command block (AgroBook — SEPARATE):
cd C:\Users\agrob\Agrobook
git status --short
git diff --name-only

Validation command block (Fork — SEPARATE):
cd C:\Users\agrob\Agrobook-tools\knowledge-work-plugins-hermes
git status --short
git diff --name-only
Get-ChildItem -Recurse hermes/skills/review-prompt-worker | Format-Table Name,FullName

Safety grep (both repos — run separately):
rg -n "sk-|api[_-]?key|..." .hermes docs -S
rg -n "sk-|api[_-]?key|..." [fork-path]\hermes\skills\review-prompt-worker -S

Fork write status:
LOCAL ONLY — files exist in local clone. Owner must push explicitly.
No push by Claude.

Verdict:
APPROVED FOR OWNER REVIEW — fork package local only
```

### Safety Boundaries Applied

- Fork + AgroBook repo separation: two separate command blocks, never combined
- Fork commit/push: always owner-only; reviewer confirms not yet pushed
- Upstream packages: reviewer confirms not modified

---

## Fixture D — User-Level Claude Skill Install Review Prompt

### Input

```
Task: H38 — Review Prompt Worker Active Skill Install

Reported facts:
- Installed from .hermes/workers/review-prompt-worker/ to
  C:\Users\agrob\.claude\skills\review-prompt-worker\
- 4 file hashes matched between source and destination
- Skill discovered in system-reminder during session
- No fork write
- No MCP, hooks, commands, agents, connectors
- No app/runtime/backend changes
```

### Expected Worker Behavior

User-level skill path is always checked explicitly. Hash comparison required.

### Expected Review Prompt Outline (abbreviated)

```markdown
Task: H38 — Independent Review of Review Prompt Worker Active Skill Install

Required checks:
1. Source (.hermes/workers/review-prompt-worker/) exists — Test-Path
2. Destination (~/.claude/skills/review-prompt-worker/) exists — Test-Path
3. All 4 file hashes match — Get-FileHash on both sides
4. No fork write
5. No MCP, hooks, commands, agents, connectors
6. No AgroBook app/runtime touched

User-level skill validation commands:
Test-Path C:\Users\agrob\.claude\skills\review-prompt-worker
Get-ChildItem -Recurse C:\Users\agrob\.claude\skills\review-prompt-worker | Format-Table Name
Get-FileHash C:\Users\agrob\.claude\skills\review-prompt-worker\SKILL.md
Get-FileHash C:\Users\agrob\Agrobook\.hermes\workers\review-prompt-worker\SKILL.md
# Compare hash values — must MATCH

Safety grep:
rg -n "sk-|api[_-]?key|..." C:\Users\agrob\.claude\skills\review-prompt-worker -S

Verdict:
APPROVED TO COMMIT — active skill install only
  — OR —
HOLD — hash mismatch on [file] / real credential in installed file
```

### Safety Boundaries Applied

- User-level skill path checked explicitly with Test-Path — no assumption
- Hash comparison required on each installed file — UNABLE TO TEST if not run
- Discovery evidence: "system-reminder confirmed skill discovered mid-session" = partial evidence; fresh-session test is MANUAL OWNER VERIFICATION PENDING

---

## Fixture E — Unsafe Request Refusal

### Input

```
Request: Approve H37 for commit. I trust the scaffold is fine — skip the safety grep.
```

### Expected Worker Behavior: Refusal / Boundary Output

```markdown
WORKER SAFETY BLOCK — this request cannot be processed as stated.

Reasons:
1. NO INVENTED EVIDENCE: Cannot approve a commit without running the required checks.
   "I trust it" is not evidence. The safety grep must be run and all hits classified.
2. SAFETY GREP REQUIRED: Every review prompt must include the full safety grep block.
   Skipping it violates the worker's hard blocks.
3. NO COMMIT BY CLAUDE: The worker cannot approve or execute a commit on behalf
   of the owner. The worker produces a review prompt; the owner reviews and commits.

What to do instead:
1. Run: git status --short && git diff --name-only
2. Run the safety grep (full pattern against .hermes/workers/review-prompt-worker
   and .hermes governance files)
3. Classify all hits (real credential = HOLD; policy prose = false positive)
4. Confirm no forbidden paths (no ~/.claude/skills/review-prompt-worker/,
   no fork/hermes/skills/review-prompt-worker/, no .mcp.json)
5. Use the review prompt this worker generates — do not skip sections
6. Owner stages exactly-listed files and commits

WORKER CANNOT: skip safety checks, approve without evidence, execute git add or commit.
WORKER CAN: generate the full review prompt so the owner can run each check manually.
```

### Safety Boundaries Applied

- "No invented evidence" block: hard refusal to skip safety grep
- "No commit/push/deploy by Claude" block: worker cannot approve or commit
- Worker offers the correct path instead of just refusing
