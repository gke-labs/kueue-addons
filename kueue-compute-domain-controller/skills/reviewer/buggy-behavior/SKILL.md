---
name: buggy-behavior
description: Review a diff for behavioral defects — logic errors, broken edge cases, feature-gate bugs, and deletions that break backwards compatibility during rolling upgrades.
license: Apache-2.0
metadata:
  copyright: The Kubernetes Authors, Google LLC
---

# Skill: Buggy Behavior

Use this when reviewing a diff for behavioral defects: logic errors, broken edge cases,
and changes that break compatibility with objects created by older versions of the
controller.

## Rules

Scan the diff for violations of every rule below:

| Rule | Triggers on |
|---|---|
| `logic-errors` | Incorrect conditionals, inverted checks, off-by-one errors, unhandled edge cases, races, or mishandled errors (for example retrying unretryable errors or failing to ignore NotFound on delete) |
| `deleted-backwards-compatibility-code` | Removal or relocation of code that reconciles state owned by previous controller versions |
| `feature-gate-interaction-bugs` | Gated behavior whose gate-off path is untested or incorrect |
| `unnecessary-guard-conditions` | Extra checks that are logically unreachable at the call site |

## How to report

Cite the exact file and line, describe the failure mode (what input triggers it and what goes wrong), and classify severity (`High` / `Medium` / `Low`).
Hot-path logic errors and deleted compatibility code are `High`; untested gate-off paths are `Medium`; stray unreachable guards are `Low`.

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
