# Changelog

All notable changes to CIT will be documented here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [1.0.0] - 2026-04-25

### Added
- CIT-Config: shared configuration skill for all three CIT skills
- CIT-Chat: cascade impact tracing for claude.ai conversations
- CIT-Code: cascade impact tracing for Claude Code projects
- CIT-Design: cascade impact tracing for UI/UX design work (Automated and Collaborative modes)
- Pre-packaged zip files in `downloads/` for Claude Desktop and claude.ai installs
- Full install guides for Windows, Mac, and Linux in INSTALL.md
- BLUEPRINT.md documenting every design decision, tradeoff, and honest limitation
- CONTRIBUTING.md with contribution philosophy and guidelines
- Persistent User Rules system in CIT-Chat
- Conversation Anchor System with five tracked anchor types
- Three-tier depth model in CIT-Code (Direct, Transitive, Environmental)
- Blind Spot Markers for unresolved references in CIT-Code
- Architectural Invariants system with user-confirmed discovery
- CIT-MAP self-documenting artifact blocks in CIT-Design
- Stale detection for CIT-MAP blocks modified outside Claude
- State and Breakpoint Matrix (8 states, 4 breakpoints) in CIT-Design
- Token Ownership Map in CIT-Design
- Triage system (Minor, Moderate, Major) across all three skills
- Loop detection with circular dependency resolution across all three skills
- Tree and Domino output formats with 8-node domino cap
- Runtime commands: `cit setup`, `cit on`, `cit off`, `cit status`, `cit settings`, `cit rules`
- Cold start declaration for first traces on new codebases
- Full honest disclosure of limitations in every skill

---

## What Counts as a Version

- **Patch (1.0.x):** Typo fixes, clarifications, wording improvements that do not change behavior
- **Minor (1.x.0):** New features, new options, improvements to trace depth or accuracy
- **Major (x.0.0):** Breaking changes to config format, skill interface, or core philosophy

---

*CIT follows its own principle: every change to this project should be traced for downstream impact before merging.*
