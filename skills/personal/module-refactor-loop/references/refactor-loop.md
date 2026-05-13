# Module Refactor Loop Protocol

## Required Reading

Before proposing code changes, read:

- `AGENTS.md`
- `docs/refactor/user-requirements.md` if present
- `docs/refactor/modules/<module>.md` if present
- relevant ADRs under `docs/adr/`
- the module source, direct callers, and nearby tests

If a required memory file does not exist, proceed from source facts and propose creating it after the plan is accepted.

## Scope Control

Keep discovery proportional to the module. Start with only:

- repo instructions and existing refactor memory;
- ADR titles plus ADRs that mention the module, its public API, or the current design question;
- the module entry points and implementation files;
- direct callers found by import/reference search;
- nearby tests and tests that exercise direct callers.

Expand beyond this set only when a source fact shows the current plan would otherwise be unsafe. Common expansion triggers are a public API change, unclear ownership across modules, failing focused tests outside the module, or a caller that forwards the module contract to another boundary.

Do not read or summarize whole directories, whole ADR sets, or unrelated call chains. For public contracts, inspect direct callers first; inspect second-hop callers only when the direct caller exposes the same contract externally. Keep the plan to at most five change items. If more are discovered, split the work and mark the rest `Not changing`.

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

Use this fixed plan structure:

```markdown
**Module Boundary**
- Responsibility:
- Non-responsibility:
- Public interface:
- Internal interface:
- Current callers:

**Goal This Round**
- <one module-level outcome>

**Change Plan**
- Must fix:
- Can fix now:
- Not changing:

**Files**
- Source:
- Tests:
- Docs/memory:

**Verification**
- Commands:
- Expected coverage:

**Commit**
- Commit this round: yes/no
- Reason:
```

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

After implementation, update durable memory by this gate:

- Update `docs/refactor/modules/<module>.md` when the loop changes or verifies responsibility, public interface, internal boundary, current callers, known debt, or the latest refactor summary.
- Update `docs/refactor/user-requirements.md` only when the user states a stable preference, rejected pattern, or workflow expectation that should apply beyond this module.
- Add or update an ADR only when the loop makes a long-lived architectural decision future agents are likely to reopen.
- Skip memory updates when the change is purely mechanical, test-only, reverted, or does not alter durable module knowledge.

If memory is skipped, say why in the final response. Do not put one-off tasks, temporary test instructions, speculative future work, raw investigation notes, or stale conclusions into memory.

Use this fixed structure for `docs/refactor/modules/<module>.md`:

```markdown
# <Module> Refactor Memory

## Responsibility
- Owns:
- Does not own:

## Public Interface
- Stable callers may depend on:
- Public contracts to preserve:

## Internal Boundary
- Private helpers:
- Internal state/config:
- Dependencies owned elsewhere:

## Current Callers
| Caller | Path | Contract relied on |
| --- | --- | --- |

## Design Rules
- Stable preferences:
- Rejected patterns:

## Known Debt
| Debt | Impact | Defer reason | Revisit when |
| --- | --- | --- | --- |

## Latest Refactor
- Date:
- Summary:
- Files changed:
- Verification:
- Commit:
```

Only create or update this file with source-backed facts from the current loop. Remove stale statements when the refactor invalidates them.

## Final Response

Use this fixed final response structure:

```markdown
Changed <1-3 lines describing the module-level result>.

Key files:
- `<path>`

Memory:
- Updated/skipped: <path or reason>

Verification:
- `<command>`: passed/failed/not run

Commit:
- `<hash>` or `Not committed`

Remaining debt:
- <only debt that affects the next step, or `None blocking`>
```
