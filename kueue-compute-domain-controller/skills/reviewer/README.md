<!--
Copyright The Kubernetes Authors.
Copyright 2026 Google LLC

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->

# Reviewer Skills

Curated code-review skills adapted from [kubernetes-sigs/kueue](https://github.com/kubernetes-sigs/kueue/tree/main/cmd/experimental/skills/reviewer) to keep `kueue-addons` aligned with upstream Kueue conventions.

## Selection Guide

| Review Pass | Skills | When to use |
|---|---|---|
| 1. Tests | [map-table-tests](map-table-tests/SKILL.md), [table-driven-tests](table-driven-tests/SKILL.md), [cmp-diff-assertions](cmp-diff-assertions/SKILL.md), [test-fixture-wrappers](test-fixture-wrappers/SKILL.md), [inline-test-steps](inline-test-steps/SKILL.md) | The diff adds or modifies unit, integration, or E2E tests |
| 2. Go conventions | [ctx-structured-logging](ctx-structured-logging/SKILL.md), [visibility-exports](visibility-exports/SKILL.md), [code-style](code-style/SKILL.md) | The diff adds or modifies Go product code |
| 3. Architecture & correctness | [architectural-decisions](architectural-decisions/SKILL.md), [buggy-behavior](buggy-behavior/SKILL.md) | Substantive controller, reconciler, or state-machine changes |
| 4. Security | [security](security/SKILL.md) | Changes touching CRDs, RBAC, webhooks, annotations/labels, or untrusted input |

## Skills Index

| Skill | Triggers on |
|---|---|
| [map-table-tests](map-table-tests/SKILL.md) | New table-driven test or benchmark ranges over a slice of cases instead of a map |
| [table-driven-tests](table-driven-tests/SKILL.md) | Independent unit scenarios duplicate setup or bypass a suitable existing table |
| [cmp-diff-assertions](cmp-diff-assertions/SKILL.md) | New or changed unit test compares values with `==`, `!=`, or `reflect.DeepEqual` instead of `cmp.Diff` |
| [test-fixture-wrappers](test-fixture-wrappers/SKILL.md) | New test builds fixture objects with callbacks, closures, or helper constructors instead of wrappers with `Clone()` |
| [inline-test-steps](inline-test-steps/SKILL.md) | New or changed test moves steps or assertions into a local closure, callback, or test-only helper |
| [ctx-structured-logging](ctx-structured-logging/SKILL.md) | `context.Context` silenced with `_ = ctx`, `var _ = ctx`, or equivalent discard |
| [visibility-exports](visibility-exports/SKILL.md) | New exported function or method with no callers outside its package |
| [code-style](code-style/SKILL.md) | Imprecise names, convention drift, reinvented helpers, wrong log verbosity, misaligned test names, typos |
| [architectural-decisions](architectural-decisions/SKILL.md) | Unjustified complexity, duplicated logic, scope creep, or misplaced code |
| [buggy-behavior](buggy-behavior/SKILL.md) | Logic errors, broken edge cases, feature-gate bugs, or backwards-compatibility regressions |
| [security](security/SKILL.md) | Input-validation gaps, injection, DoS, nil-safety crashes, authz relaxation, info disclosure, supply-chain weakening, webhook safety regressions |
