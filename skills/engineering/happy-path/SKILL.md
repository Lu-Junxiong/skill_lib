---
name: happy-path
description: Keep Python application code centered on a readable domain happy path while preventing glue code, repeated defensive checks, and validation abstractions from becoming syntax noise. Use when writing, editing, reviewing, or refactoring Python code, especially when isinstance/hasattr/callable/null/shape/state checks obscure normal execution, wrappers merely forward to parsers, or helper/class/framework proliferation reduces readability.
---

# Happy Path

Make valid input read as a short sequence of domain actions. Treat rejection logic as a boundary concern. Preserve fail-fast behavior and useful errors while reducing syntax noise and concept count.

## Establish the boundary

Before editing:

1. Read repository instructions, the target code, direct callers, public tests, and relevant developer documentation.
2. Identify the owner boundary that first knows the complete input semantics: config loading, file decoding, a public API, persistence loading, or an extension point.
3. Write the intended valid-input flow as domain actions:

```python
def execute(raw_request):
    request = parse_request(raw_request)
    plan = build_plan(request)
    return run_plan(plan)
```

4. Classify surrounding code as domain action, normalization, rejection, or glue.
5. Preserve public behavior and error contracts unless the user explicitly authorizes a behavior change.

## Apply the smallest sufficient move

Evaluate each noisy check in this order:

1. Delete it when the caller, constructor, earlier parser, or library operation already established the fact.
2. Perform the real operation when Python's protocol error already proves the required capability.
3. Keep one or two local failures as flat guard clauses.
4. Reuse an existing scalar or collection parser when it exactly owns the contract.
5. Normalize an external value once and pass the normalized value downstream.
6. Extract one cohesive validation function only when several rejection conditions protect one complete concept and return no transformed value.
7. Split a long function only along real domain stages.

Stop at the first option that solves the readability problem.

## Keep the happy path visible

Prefer orchestration that can be read from top to bottom:

```python
def publish(raw_config):
    config = parse_config(raw_config)
    frame = build_frame(config)
    return write_frame(frame, config.output)
```

Keep short rejection conditions near the operation they protect. Move a larger rejection block behind one meaningful boundary call only when doing so reveals the domain flow.

Do not optimize for the fewest lines. Optimize for fewer concepts, fewer jumps, and a visible valid-input path.

## Validate once at the owner

Validate untrusted input at the earliest layer that knows its full meaning. Trust that contract inside the boundary.

- Let type annotations document internal contracts.
- Do not repeat `isinstance`, null, shape, or state checks in every downstream function.
- Fix internal call structure when callers can bypass the intended boundary.
- Keep explicit identity checks only when identity itself matters, such as rejecting `bool` as an integer or requiring a concrete dataframe semantic.

## Reuse parsers directly

Call an existing parser directly when it already expresses the constraint:

```python
params = mapping_value(raw_params, "model section params")
name = text_value(raw_name, "model name")
columns = unique_text_list_value(raw_columns, "selected columns")
```

Do not add a helper that performs one type check, one copy, or one parser call:

```python
def _parse_text_field(value, label):
    if not isinstance(value, str):
        raise TypeError(f"{label} must be a string")
    return text_value(value, label)
```

Name shared parsers after their complete reusable contract, such as `unique_text_list_value`, not after the first field that needed them, such as `_parse_selected_columns`.

Create a shared parser only when multiple independent owners require the same accepted inputs, normalized result, and error semantics. Keep one stable contract per parser; do not add boolean mode switches to make one validator serve unrelated policies.

## Let duck typing carry behavior

Call the behavior the code needs:

```python
def render(writer, report):
    writer.write(report)
```

Avoid speculative `hasattr`, `callable`, and concrete-container checks. Use narrow EAFP only when the boundary must add context, and catch only the exception raised by the proving operation.

Do not catch `TypeError` around a callback or constructor and relabel every failure as "not callable"; the error may come from inside the called code.

## Make helpers earn their names

Keep a helper only when its name reveals a stable concept or domain action that is clearer than its body at the call site.

Reject these by default:

- forwarding wrappers with no semantic translation;
- `_validate_*` functions containing one obvious condition;
- field-specific wrappers around generic structure checks;
- predicates used once for a readable expression;
- classes, dataclasses, protocols, or context objects created only to hide a few checks;
- `validators.py`, validator objects, decorators, schemas, or validation DSLs introduced for local cleanup;
- `Pipeline`, `Stage`, or registry abstractions for one ordinary sequence;
- comments and docstrings that restate obvious code.

Keep adapters only when they translate a real external protocol, representation, ownership rule, or error boundary.

## Preserve useful rejection behavior

- Use `KeyError` for missing or unknown fields, `TypeError` for type/shape/protocol errors, and `ValueError` for invalid values or cross-field relationships unless the interface already promises something more specific.
- Preserve failure ordering when callers or tests rely on it.
- Keep field paths or action context in boundary errors.
- Never use `assert` for external input.
- Never hide broken behavior with fallback values, broad exception handling, or `typing.cast`.
- Treat behavior changes as separate decisions that require explicit authorization and public tests.

## Review the result

Before finishing, verify:

- The normal path reads as domain actions without mentally executing rejection branches.
- Each external input is parsed or validated once by its owner.
- Every remaining defensive check protects a concrete semantic, not a hypothetical misuse.
- Every new helper or type removes more concepts than it adds.
- Error categories and useful context remain intact.
- Focused tests cover the happy path and the boundary errors still promised.
- The change stops at the requested scope.

If the refactor only moved noise elsewhere, added more code than meaning, or made readers chase thin helpers, simplify again.
