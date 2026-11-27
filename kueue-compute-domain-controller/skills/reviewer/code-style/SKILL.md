---
name: code-style
description: Review a diff for naming precision, convention drift, reinvented helpers, log-verbosity mistakes, misaligned test names, and typos.
license: Apache-2.0
metadata:
  copyright: The Kubernetes Authors, Google LLC
---

# Skill: Code Style

Use this when reviewing a diff for naming, convention, and stylistic violations against
the surrounding code. The goal is consistency with the existing package and
[Kueue Coding Guidelines](https://github.com/kubernetes-sigs/kueue/blob/main/site/content/en/community/contribution_guidelines/coding_guidelines.md),
not the reviewer's personal taste.

## Rules

Scan the diff for violations of every rule below:

| Rule | Triggers on |
|---|---|
| `imprecise-names` | Identifier whose name does not accurately describe what it contains or returns |
| `convention-drift` | New code that breaks naming, error-wrapping (`%w`), or structural conventions established in the same file or package |
| `reinvented-helpers` | Logic that duplicates an existing utility/helper function (or standard library/Kueue util) instead of reusing it |
| `wrong-log-verbosity` | Log levels violating Kubernetes/Kueue logging conventions (for example per-object reconcile logs at `V(2)` or below, or `V(n).Error`) |
| `misaligned-test-names` | Test function or subtest names that do not reflect the function or behavior under test |
| `code-style-typos` | Typos in identifiers, strings, comments, or docs introduced by the diff |

## What not to include

Do not report personal preferences or taste-based objections. Do not flag:

- Code that is consistent with the surrounding file or package, even if you would have named or structured it differently.
- Short helpers, direct expressions, or naming choices that are reasonable and accurate.
- Harmless wording differences or cosmetic style opinions with no maintenance or readability impact.

## How to report

Cite the exact file and line, name the rule being violated, and classify severity (`High` / `Medium` / `Low`).
Naming-precision issues that introduce semantic ambiguity are `High`; convention drift and reinvented helpers are `Medium`; stray typos are `Low`.

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
