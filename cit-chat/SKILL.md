---
name: cit-chat
description: Cascade Impact Trace for Claude conversations. Before changing a recommendation, correcting a fact, or shifting a stance, CIT-Chat traces every downstream anchor that change affects — prior claims, user constraints, assumptions, artifacts already produced — and surfaces conflicts before acting. No silent pivots. Use this skill for any multi-turn conversation where consistency and reasoning integrity matter.
---

# CIT-Chat — Cascade Impact Trace for Conversations

> 🔧 **This skill is in development.** See the [CIT repository](https://github.com/) for status updates.

## What This Skill Does

CIT-Chat tracks the web of claims, constraints, recommendations, and assumptions across a conversation. Before Claude changes anything — a stance, a fact, a recommendation — it traces every downstream anchor that change affects and surfaces conflicts explicitly.

**No silent pivots. Ever.**

## Platform

claude.ai (all plans)

## Persistence — Read This Before Installing

CIT-Chat relies on Claude's memory system to persist your personal rules across conversations.

**This means:**
- Memory must be turned ON in your Claude settings
- Incognito conversations have no CIT rules loaded
- Claude summarizes memories over time — precise rules may lose detail
- Memory can be auto-edited by Claude — your rules are not guaranteed forever
- New conversation: Claude re-scans memory — small chance rules are missed

**Recommendation:** Keep your CIT rules short and specific. Verify them periodically. For guaranteed persistence, CIT-Code with Claude Code is the reliable version.

## What CIT-Chat Cannot Promise

- Enforcement relies on Claude's discipline, not a hard constraint
- In very long conversations, early context receives less attention — periodically restate key constraints
- The Conversation Anchor System is attention over context, not a database

---

## Core Concepts

### Conversation Anchor System
Fresh re-scan each response. Tracks:

| Type | Cascade Risk |
|---|---|
| Fact | High |
| Constraint | High |
| Recommendation | Medium |
| Assumption | Medium |
| Tone / Framing | Low |

### Persistent User Rules
User-defined rules that travel across all conversations. Examples:
- "Always show confidence level on factual claims"
- "Treat me as a senior engineer — skip basics"
- "Never suggest paid tools"

User-owned. User-written. Claude never auto-generates them.

### Silent Pivot Prevention
Any change to a prior recommendation, fact, or assumption triggers explicit acknowledgment:

> *"I'm about to revise my earlier recommendation. This affects [X]. Updated recommendation: [Y]. Do you want me to revisit anything built on the earlier recommendation?"*

### Response Structure
```
## CIT Trace [Minor/Moderate/Major]
[Cascade map of affected anchors]

## Conflicts & Risks
[Where cascade paths collide]

## Response
[Only after trace is complete]
```

---

*Full implementation coming. Track progress in BLUEPRINT.md.*
