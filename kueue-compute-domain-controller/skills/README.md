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

# Skills

Reusable agent skills for developing, verifying, and reviewing `kueue-compute-domain-controller`.

| Skill | Purpose |
|---|---|
| [verify](verify/SKILL.md) | Run code generation, linting, and unit/envtest checks in `kueue-compute-domain-controller/` before pushing or opening a PR. |
| [reviewer](reviewer/README.md) | Review Go code and tests against Kueue coding, testing, architectural, behavioral, and security conventions. |
