---
name: cit-design
description: Cascade Impact Trace for UI/UX design. Before changing any design token, component, layout, or visual system, CIT-Design traces every component that consumes it, every interaction state affected, and every breakpoint at risk — then maps conflicts before any implementation. Works in two modes: Automated Trace for code-based design (React, Tailwind, CSS) and Collaborative Trace for Figma. Use this skill any time a design change touches a shared token, a reused component, or a system-level visual decision.
---

# CIT-Design — Cascade Impact Trace for Design

> 🔧 **This skill is in development.** See the [CIT repository](https://github.com/) for status updates.

## What This Skill Does

CIT-Design maps the full cascade of any design change across tokens, components, interaction states, and breakpoints before implementation. Every change is evaluated across a full state and breakpoint matrix — not just "which component" but "which states and which breakpoints are at risk."

## Platform

Two modes depending on workflow:

- **Automated Trace Mode** — Claude Code, code-based design (React, Tailwind, CSS)
- **Collaborative Trace Mode** — claude.ai, Figma-based design

**These modes are not equivalent.** Automated traces components directly. Collaborative provides the inspection framework — the user provides the eyes. Never treat them as the same level of reliability.

## What CIT-Design Cannot Promise

- Collaborative Trace Mode inherits user error — if you miss something in your Figma inspection, the map reflects that miss
- CIT-MAP blocks go stale when files are edited outside Claude — re-trace after any manual edits
- In mixed teams, CIT-MAP blocks are Claude's working notes, not team documentation
- Enforcement relies on Claude's discipline, not a hard constraint

---

## Core Concepts

### Two Modes

**Automated Trace Mode** (code-based)
Full token ownership mapping, state matrix, breakpoint matrix. Same rigor as CIT-Code.

**Collaborative Trace Mode** (Figma)
Claude presents a structured inspection checklist upfront. User reports findings. Claude synthesizes the cascade map from those findings.

Every Collaborative output ends with:
> *"This map reflects what you reported. Claude cannot verify Figma content independently. Treat this as a guided checklist, not a guaranteed trace."*

### CIT-MAP — Self-Documenting Artifacts

Every component generated in Automated Mode carries a CIT-MAP block:

```jsx
/* ================================================
   CIT-MAP v2.0
   Component: PrimaryButton
   Last Traced: [date]
   File Last Modified: [date]

   ⚠️ STALE WARNING: If file was modified after Last Traced,
   re-trace before trusting this map.

   Token Dependencies:
   - color.primary → background, hover state, focus ring
   - spacing.md → padding
   - typography.label → font-size, weight

   Component Connections:
   - Used by: Modal, Form, Navbar, CardAction
   - Depends on: IconWrapper (optional)

   State Matrix Risk:
   ⚠️ disabled: contrast ratio sensitive to color.primary
   ⚠️ mobile (<320px): padding collapses
   ✓ hover, focus, active, loading, empty: stable

   Breakpoint Risk:
   ⚠️ 768px: layout reflow if parent is flex
   ✓ 1024px, 1440px: stable

   Team Note: In mixed teams, treat as Claude's notes,
   not shared team documentation.
================================================ */
```

Stale detection: CIT compares Last Traced against file last modified on every load. If file was touched after last trace, re-trace is triggered automatically.

### State and Breakpoint Matrix

Every change evaluated across:

**States:** default, hover, focus, active, disabled, error, loading, empty

**Breakpoints:** mobile (320–767px), tablet (768–1023px), desktop (1024–1439px), wide (1440px+)

### Token Ownership Map

Maintained in `./CLAUDE.md` for code-based projects:

```
color.primary:
  consumers: PrimaryButton, LinkText, FocusRing, BadgeActive
  risk states: disabled (contrast), focus (ring visibility)
  risk breakpoints: mobile (reduced contrast tolerance)
```

Change a token → instantly know every component, state, and breakpoint at risk.

### Response Structure
```
## CIT Trace [Minor/Moderate/Major]
[Token ownership trace]
[Component cascade map]
[State matrix risks]
[Breakpoint matrix risks]

## Conflicts & Risks
[Contrast failures, layout collisions, circular token dependencies]

## Implementation
[Only after trace is complete]
```

---

*Full implementation coming. Track progress in BLUEPRINT.md.*
