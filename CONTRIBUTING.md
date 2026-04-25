# Contributing to CIT

CIT was built from real frustration with a real failure pattern. The best contributions come from the same place — actual experience where the cascade tracing helped, where it fell short, or where an edge case wasn't handled.

---

## What We Need Most

### 1. Real-world failure reports
If CIT ran a trace that missed something important — what was the change, what did CIT miss, what broke? These are the most valuable contributions. They improve the trace logic directly.

### 2. Edge cases
Situations where the triage system misclassified a change. Where loop detection failed. Where a Blind Spot Marker should have appeared but didn't.

### 3. Improvements to the honest disclosures
If you find a better mitigation for a known limitation — especially the two that currently have no full solution (enforcement and long-conversation degradation) — that's worth a PR with a detailed explanation.

### 4. New node types
CIT-Chat currently tracks: facts, constraints, recommendations, assumptions, tone/framing. Are there node types missing? Real examples help.

---

## What We Are Not Looking For

- Features that claim to fully solve enforcement or long-conversation degradation without a concrete technical mechanism. These are fundamental architecture limitations — optimism without mechanism makes the disclosures dishonest.
- Changes that remove or soften the permanent honest disclosures. The transparency is not a weakness to fix — it's a design principle.
- Scope expansion to other AI agents (Gemini, ChatGPT, Copilot) without someone who uses those tools daily owning that implementation. CIT for Claude is built by someone who uses Claude daily. Ports should be the same.

---

## How to Contribute

1. Open an issue first for anything beyond a small fix — describe the real-world case that motivated it
2. Reference the relevant section of `BLUEPRINT.md` — every change should trace back to a design decision
3. If you're changing behavior, update `BLUEPRINT.md` too — the blueprint is the source of truth
4. PRs that add features without updating the blueprint will be asked to update it before merging

---

## Philosophy

CIT applies its own principle to itself. Before any contribution is merged, the question is: what does this change connect to, and what does changing that break? If a contribution improves one skill without considering the others, that's a CIT violation in the CIT repo. The irony would be noted.

---

*Read `BLUEPRINT.md` before contributing. It explains every decision and why.*
