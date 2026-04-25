# CIT: Cascade Impact Trace

> *"You cannot change one register without understanding what happens to all the others."*

CIT is a set of Claude skills built around one discipline: **before you change anything, map everything it touches, all the way to the last domino.**

Most AI coding and chat agents fix the bug you point at and call it a day. CIT exists because that's not enough. Every change has a blast radius. CIT maps it completely before a single line is written or a single word is changed.

---

## The Problem CIT Solves

Every recurring bug pattern looks like this:

```
Fix A introduces dependency on B
B was not designed to support A
B breaks
Fixing B requires changing C
C was not designed to support that change
...
```

This isn't a skill problem. It's a visibility problem. AI agents fix the source without mentally executing the interactions between components. CIT forces that execution to happen first, every time.

Think of it like the four registers in a CPU: MDR, MAR, PC, ACC. They all work together. Change one and the others are affected. CIT treats every codebase, conversation, and design system the same way.

---

## The Three Skills

| Skill | Platform | What It Does |
|---|---|---|
| **CIT-Code** | Claude Code | Traces across files, modules, and architecture before any code change |
| **CIT-Chat** | claude.ai | Tracks claims, assumptions, and recommendations across a conversation with no silent pivots |
| **CIT-Design** | Claude Code + claude.ai | Traces design token, component, state, and breakpoint dependencies before any visual change |

**CIT-Config** is the optional shared config that all three skills pull from. Install it once and all three skills stay in sync.

---

## How It Works

Every CIT response follows this structure. Claude cannot write an implementation without completing the trace first:

```
## CIT Trace [Minor / Moderate / Major]
[Full cascade map]
[Confidence levels per tier]
[Blind spots marked explicitly]

## Conflicts and Risks
[Circular dependencies, invariant violations, cross-path collisions]

## Implementation
[Only appears after the above is complete]
```

### Visibility Toggle
- `CIT_SHOW: ON` (default) - Claude shows you the trace before acting
- `CIT_SHOW: OFF` - Claude runs the trace silently and acts with full awareness
- Type `cit off` at any point to disable mid-conversation

### Output Formats
Choose at setup: **Tree view** or **Domino chain**

**Tree:**
```
settings.js [MODIFIED]
├── profile.js [Tier 1 - HIGH confidence]
│   └── user.js [Tier 2 - INFERRED ⚠️]
│       └── auth.js ⚠️ CIRCULAR DEPENDENCY
└── nickname.js [Tier 1 - HIGH confidence]
    └── display.js ✓
```

**Domino:**
```
[1] settings.js -> [2] profile.js -> [3] user.js -> [4] auth.js ⚠️ LOOP -> back to [1]
               -> [2b] nickname.js -> [3b] display.js ✓
```

---

## Installing CIT

### Recommended: Install all four (CIT-Config + all three skills)
This gives you shared config across all skills. Set up once and all three stay in sync.

### Installing individual skills
Each skill works standalone. If CIT-Config isn't present, the skill runs its own first-time setup and creates the config for future skills to find.

### First-time setup
On first run, CIT asks you three questions:
1. Show traces visually or run silently?
2. Tree view or Domino chain?
3. If you type `cit off` mid-task, should it finish the current trace first or stop immediately?

---

## What CIT Cannot Promise

CIT is honest about its limits. Every skill surfaces these at install:

- **Enforcement relies on Claude's discipline**, not a hard technical constraint. CIT significantly improves behavior but cannot guarantee full trace execution in every situation.
- **Long conversations degrade.** In very long claude.ai sessions, early context receives less attention. Periodically restate key constraints.
- **Stale CIT-MAP blocks are dangerous.** Files modified outside Claude after a trace need re-tracing before the map can be trusted.
- **Tier 2 traces without files present are inferences**, not verified analysis. Always labeled clearly.

This isn't a disclaimer to ignore. It's part of the philosophy: a system that knows its own limits and states them clearly is more trustworthy than one that claims perfection.

---

## Repo Structure

```
CIT/
├── README.md               <- You are here
├── BLUEPRINT.md            <- Full design decisions and reasoning
├── CONTRIBUTING.md         <- How to contribute
├── cit-config/
│   └── SKILL.md            <- Shared config skill
├── cit-chat/
│   └── SKILL.md            <- CIT for claude.ai conversations
├── cit-code/
│   └── SKILL.md            <- CIT for Claude Code
└── cit-design/
    └── SKILL.md            <- CIT for UI/UX design
```

---

## Status

| Skill | Status |
|---|---|
| CIT-Config | ✅ Live |
| CIT-Chat | ✅ Live |
| CIT-Code | ✅ Live |
| CIT-Design | ✅ Live |

---

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md). Ideas, edge cases, and real-world failure reports are the most valuable contributions. This project was born from exactly that kind of honest feedback.

---

## License

MIT. Use it, fork it, improve it.
