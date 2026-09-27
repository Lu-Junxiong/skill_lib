# Architecture decision record

Use the repository's existing ADR location, numbering, and template. Create a decision directory only when a consequential decision needs a record and no suitable location exists.

For a new record without an established template:

```md
# Decision title

State the constraint or problem, the chosen approach, and why it was chosen over the relevant alternative.
```

Add consequences or rejected alternatives when they explain a non-obvious cost or boundary. A paragraph is enough for a small decision; do not add empty sections.

If the repository numbers ADRs, use the next number in the relevant directory. Follow its convention for recording superseded decisions.

Record settled choices as decisions and identify proposals as proposals. The record should explain reasoning a future maintainer cannot recover from the code alone.
