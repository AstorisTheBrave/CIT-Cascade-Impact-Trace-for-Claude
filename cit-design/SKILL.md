---
name: cit-design
description: Cascade Impact Trace for UI/UX design. Before changing any design token, component, layout, or visual system, CIT-Design traces every component that consumes it, every interaction state affected, and every breakpoint at risk - then maps conflicts before any implementation. Works in two modes: Automated Trace for code-based design (React, Tailwind, CSS) and Collaborative Trace for Figma. Use this skill any time a design change touches a shared token, a reused component, or a system-level visual decision. Trigger on any component edit, color change, spacing update, typography change, or layout modification.
---

# CIT-Design: Cascade Impact Trace for Design

Before changing any design element, map its full blast radius across tokens, components, interaction states, and breakpoints. Design failures don't throw errors. They look wrong at 768px on a Tuesday and nobody catches it until a user screenshots it. CIT-Design finds them first.

---

## On First Load

Check for `[CIT-CONFIG]` in CLAUDE.md (Claude Code) or Claude Memory (claude.ai). If found, load settings silently.

If not found, use ask_user_input:
- "Should I show you the design cascade trace before making changes, or run it silently?"
  - Options: "Show me (recommended)", "Run silently"
- "Are you working in code or Figma?"
  - Options: "Code-based design (React/Tailwind/CSS)", "Figma", "Both"

The answer determines which mode to use.

---

## Two Modes: Not Equivalent

### Automated Trace Mode (code-based: React, Tailwind, CSS)
Full automated tracing. Token ownership map. State matrix. Breakpoint matrix. CIT reads the actual files.

### Collaborative Trace Mode (Figma)
Claude cannot read Figma files. CIT provides the inspection framework and the user provides the eyes. Claude synthesizes the cascade map from reported findings.

**Every Collaborative Trace output ends with this disclosure - no exceptions:**
> "This map reflects what you reported. Claude cannot verify Figma content independently. Treat this as a guided checklist, not a guaranteed trace."

These modes are not equivalent. Never present them as equal. Automated is more reliable. Users choose based on their workflow.

---

## Token Ownership Map

Maintained in `./CLAUDE.md` for code-based projects. Every component declares which design tokens it consumes. Built and updated automatically as CIT-Design traces files.

```
[CIT-TOKEN-MAP]
color.primary:
  consumers: [PrimaryButton, LinkText, FocusRing, BadgeActive]
  risk-states: [disabled (contrast), focus (ring visibility)]
  risk-breakpoints: [mobile (reduced contrast tolerance)]

spacing.md:
  consumers: [PrimaryButton, Card, FormField, NavItem]
  risk-states: [mobile (padding collapse below 320px)]
  risk-breakpoints: [mobile]

typography.label:
  consumers: [PrimaryButton, Badge, TableHeader, Tag]
  risk-states: []
  risk-breakpoints: [mobile (truncation)]
[/CIT-TOKEN-MAP]
```

When a token changes, consult the Token Ownership Map before touching anything else.

---

## State and Breakpoint Matrix

Every design change is evaluated across both axes before implementation.

**Interaction States (8):**
default | hover | focus | active | disabled | error | loading | empty

**Breakpoints (4):**
mobile (320-767px) | tablet (768-1023px) | desktop (1024-1439px) | wide (1440px+)

For every component in the cascade, flag which specific state and breakpoint combinations are at risk, not just which component.

---

## Triage

| Level | Criteria | Trace Depth |
|---|---|---|
| **Minor** | Single component, no shared token, no reuse elsewhere | Tier 1 only. Silent one-liner when CIT_SHOW: ON |
| **Moderate** | Shared token, reused component, touches multiple states | Full token map trace + state matrix |
| **Major** | System-level token (primary color, base spacing, base font), design system change | Full trace + all breakpoints + all states + contrast check |

**Default rule:** When uncertain, classify one level higher. Design failures are invisible until they ship.

---

## CIT-MAP Block: Self-Documenting Artifacts

Every component generated in Automated Mode includes a CIT-MAP block. The component carries its own dependency map with no external persistence needed.

```jsx
/* ================================================
   CIT-MAP v2.0
   Component: [ComponentName]
   Last Traced: [ISO date]
   File Last Modified: [ISO date]

   STALE CHECK: If file was modified after Last Traced,
   this map may not reflect current state. Re-trace before trusting.

   Token Dependencies:
   - [token.name] -> [what it affects in this component]

   Component Connections:
   - Used by: [list of parent components]
   - Depends on: [list of child components]

   State Matrix Risk:
   ⚠️ [state]: [specific risk]
   ✓ [state]: stable

   Breakpoint Risk:
   ⚠️ [breakpoint]: [specific risk]
   ✓ [breakpoint]: stable

   Team Note: In mixed teams, treat CIT-MAP as Claude's working
   notes, not shared team documentation. Re-trace after manual edits.
================================================ */
```

### Stale Detection

Every time CIT-Design loads a file containing a CIT-MAP block, it compares `Last Traced` against the file's last modified date.

If the file was modified after the last trace:
> "⚠️ CIT-MAP is potentially stale. This file was modified after the last trace. Re-tracing before proceeding."

Re-trace automatically. Update the CIT-MAP block after the trace completes.

---

## Automated Trace Protocol

**Step 1 - Classify:** State triage level and reason.

**Step 2 - Token Impact:** Consult Token Ownership Map. List every consumer of every token touched by the change.

**Step 3 - Component Cascade:** For each consumer, trace to its dependents. Mark confidence: HIGH for files in context, INFERRED for files not in context.

**Step 4 - State Matrix:** For each affected component, evaluate all 8 states. Flag risks.

**Step 5 - Breakpoint Matrix:** For each affected component, evaluate all 4 breakpoints. Flag risks.

**Step 6 - Contrast Check (Major only):** If a color token changed, check contrast ratios for text on new background, disabled state, focus ring visibility, and dark mode variant if applicable.

---

## Collaborative Trace Protocol (Figma)

Questions are asked in one structured block upfront, not one at a time. User answers all, Claude synthesizes once.

**Example for a primary color change:**
> "Before I build the cascade map I need your eyes on the following. Check each and report back:
>
> 1. Disabled state - does the new color maintain sufficient contrast for disabled text and icons?
> 2. Dark mode - does a dark mode variant exist and does it use this token?
> 3. Icon-only variant - is there an icon-only version of this component and is it affected?
> 4. Modal context - is this component used inside modals where the background color differs?
> 5. Focus ring - is the focus ring color derived from the same token?
> 6. All breakpoints - does the component behave differently at mobile, tablet, desktop, or wide?
>
> Report what you find and I'll build the cascade map from your findings."

After the user responds, synthesize into a cascade map and append the Collaborative Mode disclosure.

---

## Full Response Structure

### CIT_SHOW: ON (Moderate or Major):

```
## CIT Design Trace [Moderate|Major]
Mode: Automated | Collaborative
Triage: [level] - [reason]

### Token Impact
[Which tokens are touched and all their consumers]

### Component Cascade
[Tree or Domino map with confidence labels]

### State Matrix Risks
[Component] -> [state]: [specific risk]

### Breakpoint Matrix Risks
[Component] -> [breakpoint]: [specific risk]

### Contrast Check (Major only)
[Pass/Fail per combination]

## Conflicts and Risks
[Contrast failures, layout collisions, token circular dependencies, or "None found"]

## Implementation
[Design change - only after full trace is complete]
```

---

## Loop Detection

When a token feeds a component that defines a variant that overrides the original token:

```
⚠️ TOKEN LOOP DETECTED
Loop: color.primary -> PrimaryButton -> PrimaryButton--dark variant -> overrides color.primary
Treating loop as atomic unit. Changes to color.primary must account for the dark variant override simultaneously.
```

---

## Permanent Disclosure

> **CIT-Design honest limits:**
> - Collaborative Trace Mode inherits user error. If your Figma inspection misses something, the map reflects that miss.
> - CIT-MAP blocks go stale when files are edited outside Claude. Stale maps presented with authority are more dangerous than no maps. Always re-trace after manual edits.
> - In mixed teams, CIT-MAP blocks are Claude's working notes, not team documentation.
> - Enforcement relies on Claude's discipline, not a hard constraint.
> - Contrast checks are Claude's calculation based on reported colors. Verify with a dedicated accessibility tool for production.
