---
name: verify
description: Run local generation, lint, and test checks in kueue-compute-domain-controller before pushing changes or opening or updating a pull request.
license: Apache-2.0
metadata:
  copyright: The Kubernetes Authors, Google LLC
---

# Skill: Run Controller Verification

Adapted from upstream Kueue's `kueue-verify` and `kueue-go-lint` skills.

Before pushing changes, opening or updating a pull request, or handing Go changes back to a user:

1. From inside `kueue-compute-domain-controller/`, run the local verification targets:
   ```bash
   make manifests
   make generate
   make lint-config
   make lint
   make test
   ```
   If `go.mod` or `go.sum` changed, also run `go mod tidy` and confirm no unexpected diff remains.
2. If Kustomize manifests under `config/` changed, verify they render cleanly (`kubectl kustomize config/default > /dev/null`).
3. Fix every reported error or lint issue and rerun the checks against the final diff.
4. Report every command executed and whether it passed. If a check could not run, report the exact reason.
