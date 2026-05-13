# Module Refactor Loop Reference

## Required Reading

Before proposing code changes, read:

- `AGENTS.md`
- `docs/refactor/user-requirements.md` if present
- `docs/refactor/modules/<module>.md` if present
- relevant ADRs under `docs/adr/`
- module source, direct callers, and nearby tests

If a required memory file is missing, proceed from source facts and propose creating it after the plan is accepted.

## Scope Control

Start with only:

- repo instructions and existing refactor memory;
- ADR titles plus ADRs that mention the module, public API, or current design question;
- module entry points and implementation files;
- direct callers found by import/reference search;
- nearby tests and tests that exercise direct callers.

Expand only when source facts show the plan would otherwise be unsafe. Expansion triggers include public API changes, unclear ownership across modules, focused test failures outside the module, or a caller that forwards the module contract to another boundary.

For public contracts, inspect direct callers first. Inspect second-hop callers only when a direct caller exposes the same contract externally. Keep the plan to at most five change items; split anything larger and mark the rest `Not changing`.

## Boundary First

Start every loop by stating:

- Responsibility: what this module owns.
- Non-responsibility: what belongs elsewhere.
- Public interface: what external callers may depend on.
- Internal interface: package-private helpers and implementation details.
- Current callers: main code paths constraining the change.

Challenge vague conclusions against source facts. Do not accept a proposed deletion or abstraction until references and callers are checked.

## Requirement Classification

Merge the user's current request, stable preferences from `user-requirements.md`, and friction found in source/tests.

- Must fix: wrong ownership, duplicate sources of truth, hidden side effects, dead code, parameter redundancy, repeated validation/copying in runtime paths.
- Can fix now: small duplicate helpers, fake lazy state, thin wrappers, stale docs, unclear naming when it reflects design.
- Do not fix this round: cross-module redesign, unmeasured performance work, future capability placeholders, directory churn without leverage.

## Planning

Present one module-level plan before editing unless the user already gave explicit permission to implement. Use the plan template in [TEMPLATES.md](TEMPLATES.md).

Prefer one coherent module commit over many small reactive commits.

## Implementation Rules

- Keep edits scoped to the agreed module goal.
- Remove obsolete code created by the refactor.
- Preserve BaseModel or other explicitly stable public contracts.
- Avoid compatibility shims for internal APIs unless the user explicitly asks.
- Prefer typed config/context objects over parameter tunneling.
- Keep internal helper modules private with a leading underscore when they are not public interface.
- Do not add a directory layer unless it removes real coupling or improves locality.

## Verification

Choose tests by risk:

- Schema/config/helper refactor: focused unit tests for the module and direct consumers.
- Runner/trainer/orchestration changes: relevant path tests plus smoke tests.
- Wide public contract changes: full unit suite.

Report exactly what was run. Do not claim unrun tests passed.

## Memory Gate

After implementation:

- Update `docs/refactor/modules/<module>.md` when the loop changes or verifies responsibility, public interface, internal boundary, current callers, known debt, or latest refactor summary.
- Update `docs/refactor/user-requirements.md` only when the user states a stable preference, rejected pattern, or workflow expectation that applies beyond this module.
- Add or update an ADR only for a long-lived architectural decision future agents are likely to reopen.
- Skip memory updates when the change is purely mechanical, test-only, reverted, or does not alter durable module knowledge.

If memory is skipped, say why in the final response. Do not store one-off tasks, temporary test instructions, speculative future work, raw investigation notes, or stale conclusions.

Use the module memory template in [TEMPLATES.md](TEMPLATES.md). Only write source-backed facts from the current loop and remove stale statements invalidated by the refactor.
