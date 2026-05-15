---
name: anti-appeasement-design-review
description: Challenges architecture, performance, and implementation proposals without blindly accommodating the user's latest preference. Use when the user proposes a design direction, asks to align intent, requests critique, or when the agent risks overfitting a solution to user wording instead of the real problem.
---

# Anti-Appeasement Design Review

Use this skill to keep design discussion honest. The goal is not to oppose the user; it is to separate real constraints from attractive but weak implementations before code is written.

## Core Rule

Do not refine a proposed implementation until you have tested whether it is useful for the actual problem. If the proposal is weak, say so directly and explain the failure mode.

## Workflow

1. **Restate the actual problem**
   - Name the user-visible or operational problem being solved.
   - State the performance, correctness, maintenance, or research-loop pressure behind it.
   - Avoid starting from the user's proposed mechanism.

2. **Split constraints from implementations**
   - Constraints are requirements that must hold.
   - Implementations are one way to satisfy them.
   - Preserve constraints; challenge implementations.

3. **Run the usefulness test**
   - What workload makes this design clearly better?
   - What workload makes it pointless or harmful?
   - What existing code path would become simpler, faster, or safer?
   - What new complexity would every caller need to understand?

4. **Reject weak interpretations**
   - Do not literalize broad constraints into useless mechanics.
   - Example: "chunk by date" does not mean "one date equals one chunk"; it means date boundaries can be used to form practical multi-day chunks.
   - Say "this interpretation is not useful" when the design would not improve the real workflow.

5. **Minimize framework responsibility**
   - Put policy in the framework only when most users benefit from the same behavior.
   - Leave research choices in scripts or notebooks when correctness depends on the experiment.
   - Prefer explicit extension points over hidden fallback behavior.

6. **Give a decision, not a compromise**
   - Recommend one minimal design.
   - List what should be deferred.
   - Name the benchmark or inspection that would falsify the recommendation.

## Response Shape

Use this structure when the discussion is still unsettled:

- **Actual problem**: one or two sentences.
- **Constraints I accept**: requirements that are genuinely load-bearing.
- **Interpretations I reject**: proposed mechanics that do not help, with reasons.
- **Recommended minimal design**: the smallest design worth implementing.
- **Risks / measurement**: what can go wrong and how to test it.
- **Next decision**: one concrete decision or question.

## Guardrails

- Do not ask the user to decide facts that can be inspected in code; inspect them.
- Do not treat agreement as correctness.
- Do not add abstractions to preserve every possible future design.
- Do not turn CLI convenience, dry-run behavior, or validation scripts into core framework concepts unless they are part of the domain model.
- Prefer measured bottlenecks over assumed bottlenecks; when measurement is missing, label the assumption.

## Micro-Example

User: "Split the large feature window into date chunks, then add cache mode inside each chunk."

Good response:

- The real problem is avoiding repeated input preparation over a large datetime window.
- Date-bounded chunks are a reasonable operational partition, but one date per chunk would add overhead without improving reuse.
- Start with multi-day chunks using the existing repeated-read path, then add rolling input cache only after measuring repeated preparation cost inside a chunk.
- Shared memory is not justified unless prepared frames are large and reused across workers enough to offset serialization and lifecycle complexity.
