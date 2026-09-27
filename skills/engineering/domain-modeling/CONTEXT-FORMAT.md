# Domain glossary

Use the repository's existing glossary location and structure. If none exists and this task needs one, place it with the project's developer documentation. Create only the file needed for the resolved terms.

A compact entry can look like this:

```md
# Ordering terminology

## Language

**Order**: A customer's accepted request for specified goods.
_Avoid_: Using "transaction" for an order; a payment transaction is a separate concept.

**Invoice**: A request for payment associated with delivered goods.
```

- Define project-specific concepts in a sentence or two. Add aliases or discouraged terms only when they resolve real ambiguity.
- Capture meaning and boundaries; put implementation details, plans, and decision rationale in their appropriate documents.
- Group related terms when it improves navigation. Preserve terminology from distinct domains instead of forcing one definition across unrelated contexts.
- If a context map already exists, use it to locate the relevant glossary. Add a map only when multiple actual contexts need one; a new term alone does not justify new document structure.
