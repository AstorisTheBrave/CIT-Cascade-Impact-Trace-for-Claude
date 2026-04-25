---
name: cit-config
description: Shared configuration for the Cascade Impact Trace (CIT) skill suite. Install this alongside CIT-Chat, CIT-Code, and/or CIT-Design so all three share one config and you only answer setup questions once. When installed, all other CIT skills pull from this automatically. Use this skill whenever the user mentions CIT setup, CIT settings, changing CIT behavior, or installing any CIT skill for the first time.
---

# CIT-Config — Shared Configuration

CIT-Config is the single source of truth for all CIT settings and the shared philosophy that all three skills load. One edit here propagates everywhere. CIT does not allow its own components to drift out of sync.

---

## On First Load

Check for an existing `[CIT-CONFIG]` block:

**Claude Code:** Read `~/.claude/CLAUDE.md` and search for `[CIT-CONFIG]`.
**claude.ai:** Search Claude Memory for `[CIT-CONFIG]`.

If a `[CIT-CONFIG]` block is found, load it silently and confirm to the user:
> *"CIT-Config loaded. Type `cit settings` to review or change your configuration."*

If no block is found, run First-Time Setup below.

---

## First-Time Setup

Use the ask_user_input tool to ask all three questions at once:

**Question 1:** "When CIT runs a cascade trace, should I show it to you before acting, or run it silently in the background?"
- Options: "Show me the trace (recommended)", "Run silently"

**Question 2:** "Which format would you like for cascade maps?"
- Options: "Tree view", "Domino chain"
- Note shown: *Tree handles complex webs better. Domino is more visual but auto-switches to Tree beyond 8 nodes.*

**Question 3:** "If you type `cit off` while I'm mid-task, what should I do?"
- Options: "Finish the current trace, then turn off", "Stop immediately"
- After user selects, show the relevant disclosure:
  - Finish current: *"Safer — the action proceeds with full cascade awareness. Takes slightly longer."*
  - Stop immediately: *"Faster — but the current action proceeds without a complete trace. Use with caution."*

After all three answers are collected, write the config block (see Writing Config below) and confirm:
> *"CIT configured. All CIT skills will use these settings. Type `cit settings` to change anything at any time."*

---

## Writing Config

**Claude Code** — append to `~/.claude/CLAUDE.md`:

```
[CIT-CONFIG]
CIT_SHOW: ON|OFF
CIT_FORMAT: TREE|DOMINO
CIT_INTERRUPT: COMPLETE|IMMEDIATE
CIT_VERSION: 2.0

[CIT-CORE-PHILOSOPHY]
Before any change, map every node it touches.
Follow every cascade path to its end.
Mark blind spots explicitly — never skip silently.
Classify triage level before tracing. When uncertain, go one level higher.
Surface all circular dependencies as atomic units.
Implement only after the full trace is complete.
Enforce no silent pivots — acknowledge every revision to prior output.
[/CIT-CORE-PHILOSOPHY]
[/CIT-CONFIG]
```

**claude.ai** — write the same block to Claude Memory with the tag `[CIT-CONFIG]`. Inform the user:
> *"Config saved to memory. Note: CIT relies on memory being ON. If you turn memory off, CIT rules will not load in new conversations."*

---

## Runtime Commands

These commands work in any conversation where a CIT skill is active:

| Command | Effect |
|---|---|
| `cit off` | Disable CIT for this conversation (behavior per CIT_INTERRUPT setting) |
| `cit on` | Re-enable CIT |
| `cit settings` | Show current config, offer to change any setting |
| `cit status` | Show whether CIT is ON/OFF and current format |
| `cit rules` | Show Persistent User Rules (CIT-Chat) |

When `cit off` is received mid-task:
- If `CIT_INTERRUPT: COMPLETE` → finish current trace, then disable. Notify: *"CIT will turn off after this trace completes."*
- If `CIT_INTERRUPT: IMMEDIATE` → stop trace immediately. Notify: *"CIT disabled. Proceeding without cascade trace."*

---

## Config Sync Check

When any CIT skill loads, it checks:
1. Is `[CIT-CONFIG]` present?
2. Is `CIT_VERSION` current?

If version mismatch or block missing, prompt user to run `cit settings` to update.

---

## Permanent Disclosure

Shown once at first-time setup, and available via `cit settings` at any time:

> **What CIT cannot guarantee:**
> - CIT relies on Claude's discipline, not a hard technical constraint. It significantly improves behavior but cannot enforce full trace execution in every situation.
> - In long claude.ai conversations, early context receives less attention. Periodically restate key constraints in critical sessions.
> - Stale CIT-MAP blocks in design files are dangerous. Re-trace after any manual edits outside Claude.
> - Tier 2 traces without files present in Claude Code are inferences, not verified analysis.
> - CIT-Chat config on claude.ai requires memory to be ON. Incognito conversations have no CIT rules.
