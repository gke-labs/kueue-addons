---
name: inline-test-steps
description: Review that new tests write their steps and assertions inline instead of sharing them through local closures or callbacks.
license: Apache-2.0
metadata:
  copyright: The Kubernetes Authors, Google LLC
---

# Skill: Inline Test Steps

Follow [Kueue Writing Tests](https://github.com/kubernetes-sigs/kueue/blob/main/site/content/en/community/contribution_guidelines/writing_tests.md).
Tests follow the DAMP principle (Descriptive And Meaningful Phrases): each spec
or case shows its own steps, even when that repeats code.

**Flag:** Test code that the diff adds, and that moves test steps or assertions
out of the specs into a local closure, a callback, or a test-only helper in the
same file. The steps include creating objects, running the action under test,
and `Eventually` or `Consistently` checks. For example:

```go
ginkgo.When("Reconciling a Workload", func() {
    expectState := func(wl *kueue.Workload, want kueue.CheckState) {
        ginkgo.GinkgoHelper()
        gomega.Eventually(func(g gomega.Gomega) {
            g.Expect(k8sClient.Get(ctx, client.ObjectKeyFromObject(wl), wl)).To(gomega.Succeed())
            g.Expect(checkState(wl)).To(gomega.Equal(want))
        }, timeout, interval).Should(gomega.Succeed())
    }

    ginkgo.It("Should mark the admission check ready", func() {
        wl := makeWorkload("wl", ns.Name).Obj()
        gomega.Expect(k8sClient.Create(ctx, wl)).To(gomega.Succeed())
        expectState(wl, kueue.CheckStateReady)
    })
})
```

This includes closures that create fixtures for a spec, `func` fields in test
cases, and callbacks passed to a shared runner.

**Ask:** Drop the closure and write its steps directly in each spec or case:

```go
ginkgo.It("Should mark the admission check ready", func() {
    wl := makeWorkload("wl", ns.Name).Obj()
    gomega.Expect(k8sClient.Create(ctx, wl)).To(gomega.Succeed())
    gomega.Eventually(func(g gomega.Gomega) {
        g.Expect(k8sClient.Get(ctx, client.ObjectKeyFromObject(wl), wl)).To(gomega.Succeed())
        g.Expect(checkState(wl)).To(gomega.Equal(kueue.CheckStateReady))
    }, timeout, interval).Should(gomega.Succeed())
})
```

A reader sees what each spec does without jumping to the closure, and a change
to one spec does not change the others. Build objects with the addon's testing
wrappers, and promote a helper to a shared test-utility package only when it is
generic and used across many test files.

## Do not flag

- Reuse of existing shared test-utility helpers and suite cleanup helpers.
- Function literals that the framework or an API requires, such as
  `ginkgo.It`, `ginkgo.By`, `BeforeEach`, `DeferCleanup`,
  `Eventually(func(g gomega.Gomega) {...})`, client interceptor funcs, and
  sort or `cmp` functions.
- The runner of a unit-test table, either the subtest body or a function in
  the same file that runs each case. Each case still declares its own inputs
  and expectations.
- A closure or helper that exists before the diff and that the diff only
  calls. Do not ask to inline it in the same PR.
