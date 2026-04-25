---
name: cit-code
description: Cascade Impact Trace for Claude Code. Before any bug fix, feature addition, or refactor, CIT-Code maps every file, module, function, type, schema, test, and config that the proposed change touches — directly and transitively — then checks architectural invariants before writing a single line. Use this skill for any code change where downstream breakage is a risk, which is every code change.
---

# CIT-Code — Cascade Impact Trace for Code

> 🔧 **This skill is in development.** See the [CIT repository](https://github.com/) for status updates.

## What This Skill Does

Before touching any code, CIT-Code maps the full blast radius of the proposed change across three tiers of dependency, checks architectural invariants, requests missing files when context is insufficient, and marks blind spots explicitly rather than silently skipping them.

**No incomplete maps. No silent unknowns.**

## Platform

Claude Code exclusively. Requires CLAUDE.md support.

## Persistence

- Global rules → `~/.claude/CLAUDE.md` (loads every session)
- Project invariants → `./CLAUDE.md` (loads per project)

## What CIT-Code Cannot Promise

- Enforcement relies on Claude's discipline, not a hard constraint
- Cold start traces are preliminary — confidence grows as more files are seen
- Tier 2 without files present is inference, not verified analysis
- Tier 3 requires human verification — Claude cannot auto-trace environmental dependencies

---

## Core Concepts

### Tiered Depth Model

| Tier | Label | Meaning |
|---|---|---|
| Tier 1 | HIGH confidence | Files in context, directly analyzed |
| Tier 2 | INFERRED — verify before trusting | Reasoned from Tier 1, files not in context |
| Tier 3 | LOW confidence — human verification required | Config, tests, CI, environment variables |

### Triage System

| Level | Criteria | Trace Depth |
|---|---|---|
| Minor | Isolated, no shared dependencies | Tier 1 only, silent one-liner |
| Moderate | Touches shared components or state | Tier 1 + 2, full display |
| Major | Architectural, core systems | All tiers + invariant check |

Default: when uncertain, classify one level higher.

### File Request Protocol
Partial trace first → identify exactly where trace goes blind → specific targeted file request:

> *🔍 CIT needs more context. To complete the Tier 2 trace I need:*
> - *The file defining `UserContext` (referenced in settings.js line 14)*
> - *Test files covering `updateSettings`*

Never vague. Always specific.

### Blind Spot Markers
```
settings.js
├── profile.js ✓
│   └── [BLIND SPOT: user.js not in context — trace cannot continue]
└── nickname.js ✓
```

### Architectural Invariants
On first run, Claude asks for invariants or offers to discover them. Discovered invariants are shown to user for confirmation before writing to `./CLAUDE.md`. Never silently written.

### Loop Detection
```
⚠️ CIRCULAR DEPENDENCY DETECTED
Loop: Settings → Profile → User → Auth → Settings
Weakest seam: Auth → Settings
Recommendation: Break loop at weakest seam before implementing.
```

### Response Structure
```
## CIT Trace [Minor/Moderate/Major]
[Cascade map — Tree or Domino format]
[Confidence levels per tier]
[Blind spot markers]

## Conflicts & Risks
[Circular dependencies, invariant violations]

## Implementation
[Only after trace is complete]
```

---

*Full implementation coming. Track progress in BLUEPRINT.md.*
