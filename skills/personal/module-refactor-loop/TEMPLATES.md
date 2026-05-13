# Module Refactor Loop Templates

## Plan

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

## Module Memory

Use for `docs/refactor/modules/<module>.md`:

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

## Final Response

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
