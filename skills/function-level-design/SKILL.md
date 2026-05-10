---
name: function-level-design
description: Use when the user explicitly asks to extract, design, or write function-level documentation for a module, clarify code logic, document inputs/outputs/control flow, or produce docs that make problems easy to locate. Also use for phrases like "提取函数级文档", "梳理模块逻辑", "落实到每个函数", or "让我能看清哪里有问题".
---

# Function-Level Design Docs

## Overview

Produce a clear function-level document for one module or workflow. The output must be written to a doc file and make the code logic, dependencies, edge cases, and likely problem points easy to inspect.

This skill is for documentation and design clarity. Do not modify production code unless the user separately asks for implementation.

## Output Location

Follow existing repo documentation conventions. If none exist, write to:

```text
docs/function-level/<module-or-workflow-name>.md
```

Use a stable, descriptive filename such as `order-pricing.md`, `csv-import.md`, or `auth-session.md`.

## Workflow

1. Identify the module/workflow boundary from the user request.
2. Inspect relevant files before writing. Use existing code as source of truth when documenting current behavior.
3. If designing a new module, mark proposed functions as proposed instead of implying they already exist.
4. Build a file/function inventory.
5. Trace main call paths, data flow, state changes, and external dependencies.
6. Identify unclear logic, risky branches, hidden side effects, duplicated responsibility, and likely bug locations.
7. Write the function-level doc to disk.
8. Report the doc path and the highest-signal findings.

## Required Document Structure

Every generated doc must use this structure.

```markdown
# <Module or Workflow> Function-Level Design

## Purpose
One paragraph explaining what this module/workflow is responsible for and what it is not responsible for.

## File Map
| Path | Role | Why it matters |
| --- | --- | --- |

## Function Index
| Path | Function/Class/Method | Status | Responsibility | Inputs | Outputs/Errors | Side Effects | Callers | Dependencies | Problem Signals |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

## Main Flows
### <Flow Name>
1. `caller()` receives/creates ...
2. `callee()` validates/transforms ...
3. `writer()` persists/emits ...

## Function Details
### `<symbol>` in `<path>`
- Responsibility:
- Inputs:
- Outputs/errors:
- Core logic:
- Side effects:
- Called by:
- Calls:
- Edge cases:
- Problem signals:

## Cross-Function Invariants
- Invariant:
- Where enforced:
- What breaks if violated:

## Problem-Finding Notes
| Area | Why suspicious | Where to inspect first |
| --- | --- | --- |

## Open Questions
- Question:
- Why it matters:
```

## Function Detail Requirements

For each important function, method, class, hook, endpoint, command handler, or test helper, document:

- exact path and symbol name
- current/proposed status: existing / proposed / should-split / should-merge / unclear
- responsibility in one sentence
- inputs: params, types, sources, constraints
- outputs/errors: return value, exceptions, error object, null/undefined cases
- core logic: ordered steps, not vague summaries
- side effects: I/O, network, database, mutation, cache, logging, state
- callers and dependencies called
- edge cases and boundary conditions
- problem signals: where bugs or design confusion are likely to hide

If any item cannot be verified from code, write `Unknown from current code` and explain what evidence is missing. Do not invent certainty.

## Clarity Rules

- Prefer tables for scanability and short bullets for details.
- Use exact symbol names and paths.
- Keep responsibilities narrow. If a function does multiple unrelated things, call that out.
- Separate observed behavior from proposed design.
- Make problem locations actionable: name the function, branch, state transition, or dependency to inspect.
- Do not write generic statements like "handles validation", "processes data", or "manages errors" without explaining the actual logic.

## Problem Signals Checklist

Look for and document:

- unclear input assumptions
- inconsistent return shapes
- hidden mutation or shared state
- validation split across multiple functions
- error handling that changes type or loses context
- duplicated business rules
- functions with unrelated responsibilities
- dependencies called in surprising order
- edge cases handled in one path but not another
- tests that do not cover the riskiest branches

## Completion Criteria

Before finishing:

- The doc is written to disk.
- Every important touched or inspected file appears in the File Map.
- Every important function appears in the Function Index.
- Main flows connect function calls in execution order.
- Problem-Finding Notes identify concrete places to inspect first.
- Unknowns are marked explicitly instead of guessed.
