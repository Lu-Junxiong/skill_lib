---
name: module-refactor-loop
description: Use when refactoring one project module through multi-turn design review, user-requirement alignment, boundary definition, implementation, focused verification, commit, and reusable documentation updates.
---

# Module Refactor Loop

Use this skill to run one module-sized refactor loop. The goal is not isolated cleanup; the goal is to move one module closer to the user's stable design preferences while preserving code correctness and making future agent work easier.

## Required Reading

Before proposing code changes, read:

- `AGENTS.md`
- `docs/refactor/user-requirements.md` if present
- `docs/refactor/modules/<module>.md` if present
- relevant ADRs under `docs/adr/`
- the module source, direct callers, and nearby tests

If a required memory file does not exist, proceed from source facts and propose creating it after the plan is accepted.

## Module Boundary First

Start every loop by stating:

- Responsibility: what this module owns.
- Non-responsibility: what belongs elsewhere.
- Public interface: what external callers may depend on.
- Internal interface: package-private helpers and implementation details.
- Current callers: the main code paths that constrain the change.

Challenge vague user or agent conclusions against source facts. Do not accept a proposed deletion or abstraction until references and callers are checked.

## Requirement Collection

Merge three inputs into one plan:

- the user's current request;
- stable preferences from `user-requirements.md`;
- actual code friction found in source and tests.

Classify each candidate:

- Must fix: wrong module ownership, duplicate sources of truth, hidden side effects, dead code, parameter redundancy, repeated validation/copying in runtime paths.
- Can fix now: small duplicate helpers, fake lazy state, thin wrappers, stale docs, unclear naming when it reflects design.
- Do not fix this round: cross-module redesign, unmeasured performance work, future capability placeholders, directory churn without leverage.

## Plan Before Editing

Present one module-level plan before editing unless the user already gave explicit permission to implement.

The plan should include:

- module goal for this round;
- exact changes grouped by module boundary;
- items intentionally not changed and why;
- verification scope;
- memory files or ADRs to update.

Prefer one coherent module commit over many small reactive commits.

## Implementation Rules

- Keep edits scoped to the agreed module goal.
- Remove obsolete code created by the refactor.
- Preserve BaseModel or other explicitly stable public contracts.
- Avoid compatibility shims for internal APIs unless the user explicitly asks.
- Prefer typed config/context objects over parameter tunneling.
- Keep internal helper modules private with a leading underscore when they are not a public interface.
- Do not add a new directory layer unless it removes real coupling or improves locality.

## Verification

Choose tests by risk:

- Schema/config/helper refactor: focused unit tests for the module and direct consumers.
- Runner/trainer/orchestration changes: relevant path tests plus smoke tests.
- Wide public contract changes: full unit suite.

Report exactly what was run. Do not claim unrun tests passed.

## Memory Updates

After implementation, update durable memory when useful:

- `docs/refactor/user-requirements.md`: only stable user preferences, rejected patterns, and workflow expectations.
- `docs/refactor/modules/<module>.md`: current responsibility, public interface, internal boundary, known debt, and latest refactor summary.
- ADRs: only long-lived decisions future agents are likely to reopen.

Do not put one-off tasks, temporary test instructions, or stale conclusions into memory.

## Final Response

End with:

- 1-3 lines of what changed;
- key files;
- verification commands and results;
- commit hash if committed;
- remaining known debt only when it affects the next step.
