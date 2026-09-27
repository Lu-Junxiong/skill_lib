---
name: domain-modeling
description: Clarify domain terminology and boundaries, and document agreed terms or architectural decisions.
---

# Domain Modeling

Produce precise domain language and record consequential decisions for the topic being discussed. Ordinary implementation work that only uses existing terminology does not need a modeling session.

## Work from the project's conventions

Locate the relevant glossary and decision records using repository guidance or an existing context map. Keep their paths and formats; do not create a root `CONTEXT.md` or a second ADR directory when the project already has an equivalent location.

Read code and documentation needed to resolve the current question. Distinguish existing behavior from proposed behavior when they disagree.

## Resolve and record

- Use concrete scenarios to clarify ambiguous terms, ownership, and boundaries. Ask about consequential choices that the available evidence cannot resolve; recommend a term or boundary and explain the trade-off.
- Record agreed terms in the existing glossary. Keep hypotheses and unresolved choices out of accepted definitions. Capture related changes together when useful rather than interrupting every answer with a file write.
- Record an ADR for a consequential choice whose rationale would otherwise be lost, especially a non-obvious trade-off or costly boundary. Routine implementation details do not need a separate decision record.
- Documentation requested as part of the modeling task can be updated as decisions settle. Renaming code or changing behavior requires implementation scope from the user.

Finish with the agreed terminology, decisions, and any material open questions. Stop when the current topic is resolved; do not expand into unrelated domain design.

## Formats when needed

Read [CONTEXT-FORMAT.md](CONTEXT-FORMAT.md) when creating or restructuring a glossary, and [ADR-FORMAT.md](ADR-FORMAT.md) when writing an ADR. Existing repository conventions take precedence over these examples.
