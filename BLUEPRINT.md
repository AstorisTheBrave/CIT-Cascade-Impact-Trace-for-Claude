# CIT - Full Design Blueprint v2.0

This document records every design decision made for CIT, why it was made, what alternatives were rejected, and what honest limitations each decision carries. This is a living document - it updates as the skills improve.

---

## Origin

CIT was born from a real frustration: AI agents fix the bug you point at without tracing what that fix breaks downstream. The pattern repeats endlessly:

```
Fix A introduces dependency on B
B was not designed to support A
B breaks
Fixing B requires changing C
C was not designed to support that change
...
```

The analogy that crystallized the solution: the four registers in a CPU - MDR, MAR, PC, ACC. They work together. Change one without understanding the others and the system breaks in ways that feel random but aren't. CIT applies this thinking to every domain Claude operates in.

---

## Core Protocol (All Three Skills)

Before any action, CIT enforces five steps:

1. **Node Identification** - every component involved in the proposed change
2. **Web Mapping** - every connection outward from each node
3. **Domino Simulation** - drop the first domino, follow every path to its end
4. **Conflict Surfacing** - where two cascade paths collide
5. **Implementation** - only now act, with the full web mapped

---

## Architecture Decisions

### Why Three Separate Skills, Not One

Each Claude superpower operates on fundamentally different node types:
- Code nodes are files, functions, modules, types, schemas
- Chat nodes are claims, assumptions, recommendations, constraints
- Design nodes are tokens, components, states, breakpoints

A single skill would require so many conditionals it would become unreliable. Three focused skills, one shared philosophy. The philosophy lives in CIT-Config so it never drifts.

### Why CIT-Config as a Separate Skill

Three skills sharing a philosophy means three places where that philosophy could drift independently over time. CIT-Config is the single source of truth. One edit propagates everywhere. CIT practices what it preaches - it does not allow its own components to go out of sync.

### The Enforcement Problem - Why We Cannot Fully Solve It

CIT cannot technically force Claude to complete a trace before acting. There is no compiler, no runtime error, no unit test for "did Claude actually trace." The structural response template - requiring CIT Trace and Conflicts sections before Implementation - is the closest available enforcement mechanism. It makes shortcuts visible rather than preventing them. This limitation is documented in every skill permanently.

### The Structural Response Template

```
## CIT Trace [Minor/Moderate/Major]
## Conflicts & Risks
## Implementation
```

Claude cannot write Implementation without completing the sections above. This works because the format is the enforcement - not a separate check. The limitation: Claude can technically fill in the template without doing real tracing. CIT cannot prevent this. It can only make it visible when it happens.

---

## Triage System

### Why Triage Exists

Applying maximum trace depth to a one-line typo fix destroys user trust through friction. Triage scales the trace to the change.

### Why Default One Level Higher

Claude has a natural bias toward Minor classification because Minor means less work and faster response. Defaulting one level higher when uncertain counteracts this bias explicitly.

### Triage Levels

| Level | Criteria | Trace Depth | Display Behavior |
|---|---|---|---|
| Minor | Isolated, no shared dependencies, cosmetic | Tier 1 only | Silent one-liner even when CIT_SHOW: ON |
| Moderate | Touches shared components or state | Tier 1 + 2 | Full trace display |
| Major | Architectural, core systems, shared contracts | All tiers + invariant check | Full trace display |

---

## Loop Detection

When a node is reached via a different path that was already visited, CIT does not just warn - it restructures the implementation plan:

1. Identify loop members as a single atomic unit
2. Find the weakest seam - the least coupled connection in the loop
3. Suggest breaking the loop before implementing
4. If unbreakable, plan changes to all loop members simultaneously

This turns a warning into architectural guidance.

---

## Domino Format Cap

The domino chain is capped at 8 nodes per path. Beyond 8, auto-switches to Tree with explicit notification:

> "Chain exceeds display limit at node 8. Switching to Tree. Remaining nodes: [list]."

Why 8: readability threshold. Why never silent: the user must know the chain continued - they cannot assume the domino format shows everything.

---

## CIT-Code Specific Decisions

### CLAUDE.md as Persistence

CLAUDE.md is the most reliable persistence mechanism available in Claude Code. It loads every session without exception. CIT-Code writes:
- Global rules to `~/.claude/CLAUDE.md`
- Project invariants to `./CLAUDE.md`

This is Claude Code only. claude.ai users fall back to memory with full honest disclosure.

### Tiered Confidence Model

Confidence labels were added after auditing that "Tier 2" in the original design was Claude guessing without files present. Labeling guesses as INFERRED rather than presenting them with the same authority as verified analysis was a critical integrity fix.

| Tier | Label | Meaning |
|---|---|---|
| Tier 1 | HIGH confidence | Files in context, directly analyzed |
| Tier 2 | INFERRED - verify before trusting | Reasoned from Tier 1, files not seen |
| Tier 3 | LOW confidence - human verification required | Environmental, cannot be auto-traced |

### File Request Protocol

Vague file requests ("can you share more context?") are replaced with specific targeted requests based on partial tracing. Claude identifies exactly where the trace goes blind, then asks for precisely the files that fill the gap.

### Blind Spot Markers

Unknown references are marked on the map rather than silently skipped:

```
settings.js
├── profile.js ✓
│   └── [BLIND SPOT: user.js not in context]
```

A map with clearly marked unknowns is more trustworthy than a map that silently omits them.

### Cold Start Declaration

First trace on any new codebase is always declared explicitly:

> "This is a cold start trace. Confidence is low until I've seen more of the codebase. Treat this as a preliminary map."

### Architectural Invariants

User-confirmed or user-provided only. Claude never silently writes inferred invariants. Discovered invariants are shown, labeled as inferred, and require explicit user confirmation before writing to CLAUDE.md.

---

## CIT-Chat Specific Decisions

### Why Not Cross-Session Registry

Cross-session claim tracking was considered and rejected. Most users don't use Claude memory. Token cost is high. The better solution: separate lightweight Persistent User Rules (things the user explicitly wants always honored) from the per-conversation Conversation Anchor System.

### Persistent User Rules

User-owned. User-written. Claude never auto-generates them. Stored in Claude Memory under a `[CIT-RULES]` tag. Kept short to resist memory summarization drift.

### Conversation Anchor System

Fresh re-scan each response, not maintained state. Honest framing: this is transformer attention over context, not a database. Early entries in long conversations genuinely receive less attention. The fresh-scan framing sounds reliable - it isn't guaranteed in very long sessions.

### Silent Pivot Prevention

Any change to a prior recommendation, fact, or assumption triggers explicit acknowledgment. No exceptions. This is the core integrity mechanism for CIT-Chat.

### Entry Types and Cascade Risk

| Type | Cascade Risk |
|---|---|
| Fact | High |
| Constraint | High |
| Recommendation | Medium |
| Assumption | Medium |
| Tone/Framing | Low |

---

## CIT-Design Specific Decisions

### Two Modes - Why They Are Not Equivalent

Automated Trace Mode (code-based design) and Collaborative Trace Mode (Figma/claude.ai) were initially described as equivalent options. Audit identified this as dishonest. Claude cannot read Figma files. Collaborative Mode is Claude providing an inspection framework and the user providing the eyes. Calling them equivalent would give users false confidence in Collaborative outputs.

Resolution: rename, honest disclosure on every Collaborative output, never imply parity.

### Self-Documenting Artifacts - CIT-MAP

The artifact carries its own dependency map. No external persistence needed for code-based design. The CIT-MAP block includes:
- Token dependencies
- Component connections
- State matrix risks
- Breakpoint risks
- Last traced date
- File last modified date (stale detection)

### Stale Detection

CIT-MAP compares Last Traced date against file's last modified date on every load. If file was modified after last trace, re-trace is triggered before trusting the map. Stale maps presented with authority are worse than no maps - they create false confidence.

### Mixed Team Disclosure

CIT-MAP works best in solo or Claude-centric workflows. In teams using multiple editors, CIT-MAP becomes stale within days. Embedded disclosure in every CIT-MAP block:

> "In mixed teams, treat CIT-MAP as Claude's working notes, not team documentation."

### Figma Collaborative Mode - Upfront Block Questions

Questions asked in one structured block, not one at a time. User answers everything, Claude synthesizes once. This reduces round-trip friction while maintaining rigor.

### State and Breakpoint Matrix

Every design change evaluated across:
- 8 interaction states: default, hover, focus, active, disabled, error, loading, empty
- 4 breakpoints: mobile (320–767px), tablet (768–1023px), desktop (1024–1439px), wide (1440px+)

Cascade traces flag specific states and breakpoints at risk, not just which component.

---

## Permanent Honest Disclosures

These live in every skill file. Never hidden:

| Limitation | Skill | Best Available Mitigation |
|---|---|---|
| Enforcement relies on Claude discipline | All | Structural template makes shortcuts visible |
| Long conversation attention degradation | CIT-Chat | Restate constraints periodically |
| Tier 2 without files is inference | CIT-Code | Labeled INFERRED, files requested |
| CIT-MAP goes stale after manual edits | CIT-Design | Stale detection on every load |
| Collaborative Mode inherits user error | CIT-Design | Mandatory disclosure on every output |
| Triage is Claude's judgment | All | Default one level higher rule |
| Three skills can drift | All | CIT-Config holds shared philosophy |
| CLAUDE.md is Claude Code only | CIT-Code | Memory fallback with full disclosure |
| Cold start traces are incomplete | CIT-Code | Cold start declaration on first run |
| Mixed team CIT-MAP drift | CIT-Design | Embedded team disclosure in every block |

---

## Audit History

CIT went through two full self-audits before any code was written. Both audits applied the CIT principle to CIT itself - tracing every design decision for downstream consequences. The flaws found and resolved are documented above. The two that cannot be engineered away are documented as permanent disclosures:

1. Enforcement relies on Claude's discipline, not a hard constraint
2. Long conversation attention degradation in CIT-Chat

Any future contribution that claims to fully solve either of these should be reviewed with extreme skepticism.

---

*This blueprint is the source of truth for all CIT design decisions. If a skill implementation conflicts with this document, the blueprint wins.*
