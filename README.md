# .github

This repository contains shared resources such as reusable GitHub Actions used throughout the Department for Transport's Bus Open Data Service (BODS).

---
## Available Actions

| Action | Description |
|--------|-------------|
| [`update_ecs_task_definition`](actions/update_ecs_task_definition/action.yml) | Updates an ECS task definition with a new Docker image and deploys it to the specified ECS service and cluster using the AWS-maintained GitHub Actions. |
| [`wait_for_ecs_service_stability`](actions/wait_for_ecs_service_stability/action.yml) | Waits for one or more ECS services to become stable by polling every 15 seconds, timing out after 40 failed checks. |


---


## Using Reusable Actions in Another Repository

The actions in this repository are [composite actions](https://docs.github.com/en/actions/sharing-automations/creating-actions/creating-a-composite-action) and can be referenced from any other repository in the organisation using the `uses` keyword.

### Syntax

```yaml
steps:
  - name: My step
    uses: department-for-transport-BODS/.github/actions/<action-name>@<ref>
    with:
      input_name: value
```

- Replace `<action-name>` with the name of the action folder (e.g. `update_ecs_task_definition`).
- Replace `<ref>` with a branch name, tag, or commit SHA (e.g. `main`).