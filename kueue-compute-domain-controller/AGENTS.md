<!--
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

# kueue-compute-domain-controller Agent Instructions

`kueue-compute-domain-controller` is a Kubebuilder (`go/v4`) controller in `gke-labs/kueue-addons` that integrates [Kueue](https://github.com/kubernetes-sigs/kueue) workload admission with GPU compute domain lifecycle management.

## Project Layout & Kubebuilder Rules

- Run `go`, `make`, and `kubebuilder` commands from this directory (`kueue-compute-domain-controller/`).
- GitHub Actions CI for this controller lives at the repo root: `../.github/workflows/kueue-compute-domain-controller.yml`.
- **Never edit auto-generated files manually:** `config/rbac/role.yaml`, `config/crd/bases/*.yaml`, `config/webhook/manifests.yaml`, `**/zz_generated.*.go`, and `PROJECT`. Regenerate them with `make manifests` and `make generate`.
- **Never remove `// +kubebuilder:scaffold:*` markers**; Kubebuilder CLI commands inject code at those markers.
- Use `kubebuilder create api` (with `--resource=false --controller=true --external-api-path ...` for external Kueue / NVIDIA CRDs) and `kubebuilder create webhook` to scaffold new components.
- E2E tests (`make test-e2e`) run against an isolated Kind cluster, never a shared dev/prod cluster.

## Contribution Workflow

1. Search open and closed PRs for overlapping work before starting implementation.
2. Confirm scope, API compatibility expectations, and required verification checks before editing code.
3. Prefix PR titles and commit messages with the component name (for example `compute-domain: ...`).
4. Before pushing or opening a PR, run the local verification steps in [skills/verify/SKILL.md](skills/verify/SKILL.md) and report exact commands and results (including any checks that could not run).
5. Fill in [../.github/PULL_REQUEST_TEMPLATE.md](../.github/PULL_REQUEST_TEMPLATE.md) clearly in English.

## AI Contribution Policy

Follow the [Kubernetes AI Tool Usage Policy](https://www.kubernetes.dev/docs/guide/pull-requests/#ai-guidance):

- **Disclose AI usage** in PR descriptions, issue descriptions, and comments.
- **No AI authorship trailers.** Do not add AI `Co-authored-by`, `Assisted-by`, or similar trailers to commit messages.

## Coding and Testing Conventions

Follow upstream Kueue's conventions so code stays consistent and remains easy to upstream:

- [Kueue Coding Guidelines](https://github.com/kubernetes-sigs/kueue/blob/main/site/content/en/community/contribution_guidelines/coding_guidelines.md)
- [Kueue Writing Tests](https://github.com/kubernetes-sigs/kueue/blob/main/site/content/en/community/contribution_guidelines/writing_tests.md)

Key rules for Go code in this project:

- **Controllers:** Implement `reconcile.Reconciler` with injected dependencies, verify interface compliance (`var _ reconcile.Reconciler = (*MyReconciler)(nil)`), prefer functional options, and use typed predicates/event handlers where available.
- **Reconcile & errors:** Use `client.IgnoreNotFound(err)` for deleted objects, wrap errors with `%w`, check error types with `errors.Is`/`errors.As` and `apierrors.IsNotFound`/`IsConflict`, and avoid retrying unretryable errors.
- **Logging:** Obtain the logger via `ctrl.LoggerFrom(ctx)` and log with structured key-value pairs; do not log full objects or secrets.
- **Copyright headers:** New Google-authored source files must use the Apache-2.0 header in `hack/boilerplate.go.txt` (`Copyright 2026 Google LLC`, no trailing period). Preserve existing third-party copyright notices.
- **Unit tests:** Use Go's `testing` package with map-based table-driven tests (`map[string]struct{...}`), `google/go-cmp` (`cmp.Diff` with `(-want,+got)`), and fixture builder wrappers with `.Clone()`. Avoid assertion libraries and local closure helpers (keep test steps DAMP and inline).
- **Integration & E2E tests:** Use Ginkgo/Gomega with `envtest` or Kind, rely on event handlers rather than calling `Reconcile` directly, and use `Eventually`/`Consistently` instead of fixed sleeps.

## Skills

- Operational and verification skills live in [skills/README.md](skills/README.md).
- Code review skills live in [skills/reviewer/README.md](skills/reviewer/README.md). Apply them proactively when reviewing diffs or self-reviewing changes before opening a PR.
