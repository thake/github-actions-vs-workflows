---
paginate: true
---
<!-- _class: invert -->
# GitHub Actions vs Reusable Workflows
---
# Actions

- Can be one of
    - Javascript (e.g. gatekeeper-check)
    - Composite action (e.g. create-tls-cert)
    - Docker container (not used by our team)
- Used as a step in a workflow job
- Accessible values
    - Inputs
    - Environment variables
- Caveats:
  - Variables and Secrets are not accessible
  - Environment is shared with calling job

---
# Reusable Workflows
<style>
  section {
    font-size: 25px;
  }
</style>
- A regular github workflow with a `workflow_call` trigger.
- Can be executed standalone if another trigger exists (e.g. `workflow_dispatch`)
- Can be used as a job in another workflow.
- Caveats:
    - Environment variables are not passed.
    - Secrets are not passed. Must be passed explicitly.
    - All required permissions of the called workflow must be present in the calling workflow.
    - The `github` context is always the context of the calling workflow. Meaning that all actions using this context for their defaults, are taking the values from the calling workflow `github` variable.

---
# Comparison overview

<!--
style: |
  table {
    font-size: 19px;
  }
-->
|     | Actions | Reusable Workflows |
| --- | --- | --- |
| Used as | Step in a workflow job | a workflow job |
| Inputs | ✅   | ✅   |
| Outputs | ✅ (step output) | ✅ (job output) |
| Secrets | 🚫  | ✅ (need to be passed explicitly using `secrets: inherit`) |
| Environment variables from calling side | ✅   | 🚫  |
| Modifying Environment variables of calling side | ✅   | 🚫  |
| Jobs can be defined (concurrency possible) | 🚫  | ✅   |
| Contexts from calling side | all | only `github` |