# kueue-addons

`kueue-addons` hosts experimental controllers, ecosystem integrations, and scheduling evaluation tooling for [Kueue](https://github.com/kubernetes-sigs/kueue), the Kubernetes-native job queueing and batch workload orchestrator.

## Planned Components

- **Dynamic Quota Orchestration (DQO) capacity providers:** Experimental cluster and multi-cluster capacity controllers that dynamically adjust Kueue `ClusterQueue` quotas from live cluster and node state.
- **Maintenance and disruption integrations:** Controllers bridging node maintenance signals (`MaintenancePlan` and disruption budgets) with Kueue workload eviction and requeuing policies.
- **Accelerator slice controllers:** Experimental topology and slice lifecycle controllers for multi-node / multi-host AI training workloads on Kubernetes.
- **kueue-optimizer:** Deterministic [KWOK](https://kwok.sigs.k8s.io/) simulation chambers, trace-sanitization utilities, and A/B benchmark harnesses for evaluating Kueue and `kube-scheduler` / Workload-Aware Scheduling (WAS) configurations at scale.

## Contributing

See [`CONTRIBUTING.md`](CONTRIBUTING.md) for details on how to contribute and sign the Google Contributor License Agreement (CLA).

## License

Apache License 2.0 — see [`LICENSE`](LICENSE) for details.

## Disclaimer

This is not an officially supported Google product. This project is not eligible for the [Google Open Source Software Vulnerability Rewards Program](https://bughunters.google.com/open-source-security).
