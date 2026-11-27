---
name: architectural-decisions
description: Review a diff for small-scale structural problems — unjustified complexity, duplicated logic, scope creep, or misplaced code.
license: Apache-2.0
metadata:
  copyright: The Kubernetes Authors, Google LLC
---

# Skill: Architectural Decisions

Use this when reviewing a diff for small-scale structural problems: how code is organized,
how parts connect, and whether complexity is justified. Complex problems may need complex
solutions, but every unit of complexity must earn its keep.

## Rules

Scan the diff for violations of every rule below:

| Rule | Triggers on |
|---|---|
| `illogical-structure` | Code a future maintainer will struggle to follow, modify, or extend |
| `nonsensical-decisions` | Unnecessary indirection, mismatched abstractions, or confusing data flow |
| `avoidable-complexity` | Solutions more elaborate than the problem requires |
| `pointless-intermediate-variables` | Redundant local variables that add noise without clarity |
| `duplicated-logic` | Identical blocks across types/adapters/call sites that should be shared — or new helpers with only one caller |
| `scope-creep` | Diffs bundling a bugfix with an unrelated refactor, or over-generalizing a change |
| `misplaced-logic` | Code placed somewhere a reader would not expect to find it |

## What not to include

Do not report personal preferences or architectural taste. Do not flag:

- A design choice simply because you would have organized the code differently.
- Small helpers, wrappers, locals, or indirections that are reasonable in context.
- Broad redesign ideas unless the current structure creates a concrete maintenance risk, ambiguous ownership, duplicated change burden, or extension hazard.

## How to report

For each finding, cite the exact file and line, name which rule it violates, and classify severity (`High` / `Medium` / `Low`).
Decisions that make the code materially harder to extend are `High`; cosmetic structural nits are `Low`.

Return two sections:

### Findings

```
- <High/Medium/Low> | <Finding Title>: <one-sentence explanation grounded in the diff>
```

### Recommendations

```
### Recommendation N: <short title>

**Problem**: Describe the specific issue in the diff.
**Reason**: Explain why this is a problem — what goes wrong or degrades over time.
**Solution**: Give a concrete, actionable fix. Where possible, show a before/after code snippet.
**Locations**: List every place this issue occurs, one per line, in `filepath:line` form.
```
