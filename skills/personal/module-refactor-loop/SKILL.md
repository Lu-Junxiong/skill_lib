---
name: module-refactor-loop
description: Use when refactoring one project module through multi-turn design review, user-requirement alignment, boundary definition, implementation, focused verification, commit, and reusable documentation updates.
---

# Module Refactor Loop

Use this skill to run one module-sized refactor loop. The goal is to move one module closer to stable design preferences while preserving correctness and making future agent work easier.

Before proposing or making code changes, read [references/refactor-loop.md](references/refactor-loop.md) and follow its execution protocol.

Core rules:

- Keep the work to one module boundary unless source facts prove expansion is required.
- State responsibility, non-responsibility, public interface, internal interface, and current callers before editing.
- Present the fixed module-level plan unless the user already gave explicit permission to implement.
- Keep discovery proportional; do not read or summarize whole directories, whole ADR sets, or unrelated call chains.
- Update durable memory only when the reference gate says to, and explain skipped memory updates in the final response.
- Report exact verification commands and results. Do not claim unrun tests passed.
