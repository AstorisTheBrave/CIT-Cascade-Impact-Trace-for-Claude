# CIT: Cascade Impact Trace

![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg) ![Works with Claude](https://img.shields.io/badge/Works%20with-Claude-orange.svg) ![Platform](https://img.shields.io/badge/Platform-Claude%20Desktop%20%7C%20claude.ai%20%7C%20Claude%20Code-blueviolet.svg) ![Version](https://img.shields.io/badge/Version-1.0.0-brightgreen.svg)

> *"You cannot change one register without understanding what happens to all the others."*

CIT is a set of Claude skills built around one discipline: **before you change anything, map everything it touches, all the way to the last domino.**

Most AI coding and chat agents fix the bug you point at and call it a day. CIT exists because that is not enough. Every change has a blast radius. CIT maps it completely before a single line is written or a single word is changed.

---

## Quick Links

- [**INSTALL.md**](./INSTALL.md) - Full step-by-step setup for Windows, Mac, and Linux
- [**BLUEPRINT.md**](./BLUEPRINT.md) - Every design decision, tradeoff, and honest limitation. Read this before contributing.
- [**CHANGELOG.md**](./CHANGELOG.md) - Full version history

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

This is not a skill problem. It is a visibility problem. AI agents fix the source without mentally executing the interactions between components. CIT forces that execution to happen first, every time.

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

## Platform Support

| Platform | Mac | Windows | Linux |
|---|---|---|---|
| Claude Desktop | ✅ | ✅ | Not available |
| claude.ai (browser) | ✅ | ✅ | ✅ |
| Claude Code | ✅ | ✅ (via WSL2) | ✅ |

Full install instructions for every platform and OS are in [INSTALL.md](./INSTALL.md).

---

## Installing CIT

### Short version

**Claude Desktop or claude.ai (Mac, Windows):**
Download the zip files from the `downloads/` folder in this repo and upload them through Settings > Customize > Skills.

**Claude Code on Mac or Linux:**
```bash
mkdir -p ~/.claude/skills
cp -r cit-config cit-chat cit-code cit-design ~/.claude/skills/
```

**Claude Code on Windows (WSL2):**
Run the above commands inside your Ubuntu/WSL terminal.

Then start a new conversation and type:
```
cit setup
```

Full instructions with troubleshooting for every OS are in [INSTALL.md](./INSTALL.md).

---

## What CIT Cannot Promise

CIT is honest about its limits. Every skill surfaces these at install:

- **Enforcement relies on Claude's discipline**, not a hard technical constraint. CIT significantly improves behavior but cannot guarantee full trace execution in every situation.
- **Long conversations degrade.** In very long claude.ai sessions, early context receives less attention. Periodically restate key constraints.
- **Stale CIT-MAP blocks are dangerous.** Files modified outside Claude after a trace need re-tracing before the map can be trusted.
- **Tier 2 traces without files present are inferences**, not verified analysis. Always labeled clearly.

This is not a disclaimer to ignore. It is part of the philosophy: a system that knows its own limits and states them clearly is more trustworthy than one that claims perfection.

The full reasoning behind every decision is in [BLUEPRINT.md](./BLUEPRINT.md).

---

## Repo Structure

```
CIT/
├── README.md               <- You are here
├── INSTALL.md              <- Full install guide for Windows, Mac, and Linux
├── BLUEPRINT.md            <- Full design decisions and reasoning
├── CHANGELOG.md            <- Version history
├── CONTRIBUTING.md         <- How to contribute
├── downloads/
│   ├── cit-config.zip      <- Upload this first
│   ├── cit-chat.zip
│   ├── cit-code.zip
│   └── cit-design.zip
├── cit-config/
│   └── SKILL.md
├── cit-chat/
│   └── SKILL.md
├── cit-code/
│   └── SKILL.md
└── cit-design/
    └── SKILL.md
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

See [CONTRIBUTING.md](./CONTRIBUTING.md). Real-world failure reports and edge cases are the most valuable contributions. This project was born from exactly that kind of honest feedback.

Before opening a PR, read [BLUEPRINT.md](./BLUEPRINT.md). Every change should trace back to a design decision documented there.

---

## License

MIT. Use it, fork it, improve it.
