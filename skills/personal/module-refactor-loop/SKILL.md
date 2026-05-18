---
name: module-refactor-loop
description: Runs one module-sized refactor loop with boundary analysis, scoped implementation, verification, and durable memory updates. Use when refactoring a single module, reorganizing module ownership, reducing module-level design debt, or aligning code with documented project preferences.
---

# Module Refactor Loop

## Quick start

1. Identify the target module and read [REFERENCE.md](REFERENCE.md).
2. State the module boundary: responsibility, non-responsibility, public interface, internal interface, and current callers.
3. Present the fixed plan from [TEMPLATES.md](TEMPLATES.md) unless the user already asked to implement.
4. Implement only the agreed module goal.
5. Run risk-appropriate verification.
6. Update durable memory only when the memory gate requires it.
7. End with the fixed final response from [TEMPLATES.md](TEMPLATES.md).

## Workflow

Use [REFERENCE.md](REFERENCE.md) for:

- required reading;
- scope control and anti-bloat rules;
- requirement classification;
- implementation rules;
- verification selection;
- memory update gate.

If the refactor reveals function-level logic that cannot be understood or
diagnosed quickly, invoke `function-level-design` for that specific module or
workflow instead of expanding this refactor loop into detailed function docs.

Use [TEMPLATES.md](TEMPLATES.md) for:

- module plan format;
- `docs/refactor/modules/<module>.md` format;
- final response format.

## Guardrails

- Keep the work to one module unless source facts prove expansion is required.
- Inspect direct callers before changing or deleting public behavior.
- Keep the plan to at most five change items.
- Do not summarize whole directories, whole ADR sets, or unrelated call chains.
- Report exact commands run. Do not claim unrun tests passed.
