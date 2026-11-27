---
name: security
description: Review a diff for security issues exploitable by a tenant, neighbouring controller, or malicious workload spec — input validation, injection, DoS, nil-safety, authz, info disclosure, supply chain, webhook safety.
license: Apache-2.0
metadata:
  copyright: The Kubernetes Authors, Google LLC
---

# Skill: Security

Use this when reviewing a diff for code patterns exploitable by an authenticated
low-privileged tenant, a compromised neighbouring controller, or a malicious workload
spec. Kueue addon controllers run with cluster-wide watch and write permissions and may
sit in the admission path — one insecure pattern can crash-loop the controller, leak
tenant data, bypass quota or isolation boundaries, or allow privilege escalation.

## Rules

Scan the diff for violations of every rule below:

| Rule | Triggers on |
|---|---|
| `input-validation` | Missing validation on user-settable CR fields, webhook payloads, labels, annotations, or untrusted deserialization |
| `injection` | Command exec, SSRF, template, query, or selector built from unsanitized user-supplied strings |
| `path-traversal` | File paths or archive extraction without root containment |
| `resource-bounds-dos` | Uncapped loops or allocations, missing timeouts, reconciler-wedging input, or unbounded metric label cardinality |
| `nil-safety` | Unguarded pointer dereferences from malformed CRs; goroutines without recover |
| `authn-authz-relaxation` | Wildcard RBAC (`*`), overly broad write verbs (`create`/`delete` where read-only suffices), webhook `failurePolicy: Ignore`, or privileged pod specs |
| `information-disclosure` | Full-object logging, request echoing, credentials in status/annotations, or kubeconfig logging |
| `supply-chain-hygiene` | Unpinned container images, TLS bypass, unjustified `replace` directives, `curl\|sh`, or unpinned moving-tag GitHub Actions |
| `annotation-label-namespace-abuse` | Generic annotation/label loops that copy or forward arbitrary user keys into privileged namespaces/prefixes |
| `feature-gated-insecure-paths` | Gated behavior whose security assumptions fail when accidentally enabled |
| `integration-adapter-trust-boundary` | Controllers reading credential-like or cross-namespace fields from third-party CRDs without treating them as untrusted |
| `webhook-safety-regressions` | Loosened `failurePolicy`, shortened timeouts, TLS bypass, or non-idempotent mutating webhooks |

## How to report

Cite the exact file and line, name the rule violated and the relevant CWE/CVE where
applicable, and classify severity (`High` / `Medium` / `Low`).
Anything that crashes the controller, leaks credentials, bypasses authz, or enables injection is `High`; missing caps, unbounded cardinality, or weakened supply-chain controls are `Medium`; stylistic security smells with no concrete exploit path are `Low`.

Return two sections:

### Findings

```
- <High/Medium/Low> | <Finding Title>: <one-sentence explanation grounded in the diff, with CWE/CVE where applicable>
```

### Recommendations

```
### Recommendation N: <short title>

**Problem**: Describe the specific issue in the diff.
**Reason**: Explain why this is a problem — what goes wrong or degrades over time.
**Solution**: Give a concrete, actionable fix. Where possible, show a before/after code snippet.
**Locations**: List every place this issue occurs, one per line, in `filepath:line` form.
```
