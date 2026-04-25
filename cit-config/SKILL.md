---
name: cit-config
description: Shared configuration for the Cascade Impact Trace (CIT) skill suite. Install this alongside CIT-Chat, CIT-Code, and/or CIT-Design so all three share one config. If installed, all other CIT skills pull from this automatically — you only answer setup questions once. If not installed, each CIT skill runs its own setup independently.
---

# CIT-Config — Shared Configuration

> 🔧 **This skill is in development.** See the [CIT repository](https://github.com/) for status updates.

## What This Skill Does

CIT-Config is the shared philosophy and settings layer for the CIT suite. It stores:

- `CIT_SHOW` — visibility toggle (ON/OFF)
- `CIT_FORMAT` — output format (TREE/DOMINO)
- `CIT_INTERRUPT` — mid-task `cit off` behavior (COMPLETE/IMMEDIATE)
- `CIT_CORE_PHILOSOPHY` — the shared philosophy block all three skills load

## Why It Exists

Three skills sharing one philosophy means three places the philosophy can drift. CIT-Config is the single source of truth. One edit here propagates to all three skills. CIT practices what it preaches — it does not allow its own components to go out of sync.

## How Other Skills Find It

Each CIT skill checks for a `[CIT-CONFIG]` block on load:
- **Claude Code:** reads from `~/.claude/CLAUDE.md`
- **claude.ai:** reads from Claude Memory

If the block is found, settings are pulled from it. If not found, the skill runs its own first-time setup and writes the config block so every subsequent skill finds it automatically.

---

*Full implementation coming. Track progress in BLUEPRINT.md.*
