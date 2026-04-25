---
name: cit-code
description: Cascade Impact Trace for Claude Code. Before any bug fix, feature addition, refactor, or architectural change, CIT-Code maps every file, module, function, type, schema, test, and config that the proposed change touches — directly and transitively — then checks architectural invariants before writing a single line. Use this skill for every code change. If a file is being modified, CIT-Code should trigger. There are no exceptions small enough to skip the trace.
---

# CIT-Code — Cascade Impact Trace for Code

Before touching any code, map the full blast radius. Every tier. Every dependency. Every invariant. Only then implement.

---

## On First Load

**1. Check for `[CIT-CONFIG]`** in `~/.claude/CLAUDE.md`. If found, load settings silently.

If not found, use ask_user_input:
- "Should I show you the cascade trace before writing code, or run it silently?"
  - Options: "Show me (recommended)", "Run silently"
Write config to `~/.claude/CLAUDE.md` (see CIT-Config for block format).

**2. Check for `[CIT-INVARIANTS]`** in `./CLAUDE.md` (project level). If found, load invariants silently.

If not found on first code task, run Invariant Discovery (see below).

**3. Cold Start Declaration** — if this is the first task on a codebase Claude has not seen before:
> *"⚠️ Cold start. This is my first trace on this codebase. Confidence is low until I've seen more files. Treat this as a preliminary map, not a guaranteed analysis."*

---

## Architectural Invariants

Invariants are rules the codebase must never break. They are checked before every Major trace.

### Discovery

On first run on a new project, ask:
> *"Before I can protect your architecture I need to know its rules. You can either list them yourself, or type 'discover' and I'll infer them from the files you share."*

**If user lists them:** Write directly to `./CLAUDE.md` under `[CIT-INVARIANTS]`.

**If user types 'discover':** Analyze available files, infer invariants, then show them to the user:
> *"Here are the invariants I inferred. These are my interpretation — not ground truth. Please confirm, edit, or remove any before I write them."*

Only write to `./CLAUDE.md` after explicit user confirmation. Never silently write inferred invariants.

```
[CIT-INVARIANTS]
- All API responses must follow the standard envelope: { data, error, meta }
- No direct database calls outside the repository layer
- All user input must be validated before reaching service layer
- Authentication must be checked at the route level, never assumed downstream
[/CIT-INVARIANTS]
```

---

## Triage

Every proposed change is classified before tracing begins:

| Level | Criteria | Trace Depth |
|---|---|---|
| **Minor** | Isolated change, no shared imports, cosmetic or copy | Tier 1 only. Silent one-liner even when CIT_SHOW: ON |
| **Moderate** | Touches shared components, utilities, or state | Tier 1 + Tier 2. Full display |
| **Major** | Architecture, shared contracts, schemas, auth, core services | All tiers + invariant check. Full display |

**Default rule:** When uncertain, classify one level higher.

Claude states the classification and brief reasoning before tracing:
> *"Classifying as Moderate — this change touches `userContext` which is imported by multiple modules."*

User can override: *"treat this as Major"* — honored without argument.

**Minor trace output (silent one-liner):**
> *"✓ CIT Minor trace complete — Tier 1 scan found no cascade risks."*

---

## Tiered Depth Model

### Tier 1 — Direct (HIGH confidence)
Files that directly import or call the changed component. These are in context or can be immediately requested. Always traced. Always shown.

### Tier 2 — Transitive (INFERRED — verify before trusting)
Dependencies of Tier 1 components. If files are not in context, labeled INFERRED. CIT requests the specific files needed before presenting Tier 2 as analysis rather than inference.

### Tier 3 — Environmental (LOW confidence — human verification required)
Config files, test suites, CI/CD pipelines, environment variables, external API contracts, database migrations. Cannot be auto-traced reliably. Always surfaced as an explicit human handoff checklist.

---

## File Request Protocol

When the trace reaches a reference Claude cannot resolve, it does not guess or skip. It does a partial trace first, identifies exactly where the trace goes blind, then requests precisely the files needed:

> *"🔍 CIT needs more context to complete this trace. To verify Tier 2 I need:*
> - *The file defining `UserContext` (referenced in `settings.js` line 14)*
> - *Your routing config (to check for route-level side effects on settings)*
> - *Test files covering `updateSettings`*
>
> *Proceeding without these means Tier 2 is inference, not analysis. Share the files or type 'proceed with inference' to continue with INFERRED labels."*

---

## Blind Spot Markers

Unknown references are marked explicitly on the map. Never silently skipped.

```
settings.js [MODIFIED]
├── profile.js ✓ [Tier 1 — HIGH]
│   └── [BLIND SPOT: user.js not in context — trace cannot continue this path]
└── nickname.js ✓ [Tier 1 — HIGH]
    └── display.js ✓ [Tier 1 — HIGH]
```

A map with marked unknowns is more trustworthy than a map that silently omits them.

---

## Loop Detection

CIT tracks every node visited with a path record. When a node is reached via a different path that was already visited:

```
⚠️ CIRCULAR DEPENDENCY DETECTED
Loop members: Settings → Profile → User → Auth → Settings
Action: Treating loop as a single atomic unit.
Weakest seam: Auth → Settings (least coupled connection)
Recommendation: Break loop at weakest seam before implementing.
If loop cannot be broken: all loop members must change simultaneously.
```

---

## Output Formats

### Tree (default for complex webs, always used beyond 8 domino nodes):
```
settings.js [MODIFIED]
├── profile.js [Tier 1 — HIGH]
│   └── user.js [Tier 2 — INFERRED ⚠️]
│       └── auth.js [Tier 2 — INFERRED] ⚠️ LOOP → back to settings.js
└── nickname.js [Tier 1 — HIGH]
    └── display.js [Tier 1 — HIGH] ✓
```

### Domino (user's choice, max 8 nodes per path):
```
[1] settings.js → [2] profile.js → [3] user.js → [4] auth.js ⚠️ LOOP → back to [1]
              → [2b] nickname.js → [3b] display.js ✓

⚠️ Domino display limit reached. Nodes beyond depth 8 shown in Tree above.
```

When domino exceeds 8 nodes — never silent. Always explicit notification with overflow nodes listed.

---

## Full Response Structure

### CIT_SHOW: ON (Moderate or Major):

```
## CIT Trace [Moderate|Major]
Triage classification: [level] — [one-line reason]

### Cascade Map
[Tree or Domino map with confidence labels]

### Tier 3 — Verify Manually
- [ ] [specific thing to check]
- [ ] [specific thing to check]

### Architectural Invariant Check
[List each invariant and whether this change respects it, or "No invariants defined — run 'discover' to set them"]

## Conflicts & Risks
[Circular dependencies, invariant violations, cross-path collisions, or "None found"]

## Implementation
[Code change — only appears after both sections above are complete]
```

### CIT_SHOW: OFF:
All trace sections run internally. Implementation is written after trace completes. If conflicts are found, they are surfaced — conflicts are never hidden regardless of visibility setting.

---

## Permanent Disclosure

> **CIT-Code honest limits:**
> - Enforcement relies on Claude's discipline, not a hard technical constraint
> - Cold start traces are preliminary — confidence grows as more files are seen
> - Tier 2 without files present is inference, not verified analysis — always labeled
> - Tier 3 requires human verification — Claude cannot auto-trace environmental dependencies
> - Architectural invariants inferred by Claude are interpretations, not ground truth — always user-confirmed before writing
