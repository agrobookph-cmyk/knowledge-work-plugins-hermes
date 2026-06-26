# Attribution

This Hermes worker scaffold was inspired by Anthropic's knowledge-work-plugins /
product-management plugin patterns.

Upstream project: https://github.com/anthropics/knowledge-work-plugins
License: Apache License 2.0 (SPDX: Apache-2.0)
Copyright holder: Anthropic

No upstream source code, prompt text, MCP server configurations, hooks, agents,
or runtime integrations were copied verbatim into this scaffold. All skill files
in this directory are original AgroBook-specific content written for the Hermes
governance layer.

The scaffold uses original AgroBook-specific wording.

---

## Adaptation Details

| Field | Value |
|-------|-------|
| Adaptation type | Pattern inspiration only |
| Upstream code copied | None |
| Upstream prompt text copied | None |
| MCP servers from upstream | None |
| Hooks from upstream | None |
| Commands from upstream | None |
| Agents from upstream | None |
| Connectors from upstream | None |
| Runtime integrations from upstream | None |
| Fork write executed | Not executed in H37 (H39 gate) |
| Active skill installed | Not executed in H37 (H38 gate) |

---

## What Was Adapted (Pattern Only)

The following structural patterns from the `product-management` plugin in
`knowledge-work-plugins` informed the layout of this scaffold:

- The `write-spec` skill's structured output approach (header → required checks → verdict)
- The general knowledge-worker discipline of producing a structured deliverable before action
- The `sprint-planning` skill's task-boundary + context-compact discipline

All content — trigger phrases, the 10 required output sections (review prompt header,
required checks, file-lock checklist, validation commands, safety grep block, false-positive
classification, forbidden activation path checks, owner staging commands, must-not-stage list,
verdict format), hard safety blocks, Hermes-specific H-phase terminology, AgroBook governance
conventions, Ponytail/Blueprint rules — is original AgroBook/Hermes content.

---

## Changes Made from the Upstream Pattern

- Rewritten for AgroBook governance review discipline (H-phase audit, not product spec)
- Output format adapted to Hermes H11 evidence pack + H12 index conventions
- 10 required output sections defined (none exist in upstream product-management skills)
- Hard safety blocks added (no invented evidence, no commit/push/deploy by Claude,
  no secrets, no MCP, no hooks, no connectors, no PII, no changes outside file lock,
  compact context first, Ponytail default, Blueprint Rule)
- Fork + AgroBook repo command separation rule
- User-level skill path checks rule
- False-positive classification table required
- Three-option verdict format (APPROVED TO COMMIT / APPROVED FOR OWNER REVIEW / HOLD)
- Owner decision package abbreviated format added
- Never `git add .` rule embedded in every staging section

---

## NOTICE File Status

No NOTICE file was found in the upstream `anthropics/knowledge-work-plugins`
repository as of 2026-06-26 (HTTP 404 — confirmed via `gh api` during H36
license review). No additional attribution text is required beyond the
standard Apache-2.0 attribution above.

H36 confirmed the same result as H29 (both checked 2026-06-26).

---

## License Gate Reference

This scaffold was created after H36 (`HERMES_REVIEW_PROMPT_WORKER_H36_LICENSE_DECISION.md`)
confirmed the Apache-2.0 license via live `gh` CLI queries and determined:
- `APPROVED_FOR_H37_LOCAL_SCAFFOLD_ONLY`
- No NOTICE file obligations
- Pattern-only use with original wording = minimal attribution obligation
- Attribution obligation satisfied by this file
