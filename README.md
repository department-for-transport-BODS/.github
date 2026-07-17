# .github

This repository contains shared resources such as reusable GitHub Actions used throughout the Department for Transport's Bus Open Data Service (BODS).

---
## Available Actions

| Action | Description |
|--------|-------------|
| [`build-and-push-image`](actions/build-and-push-image/action.yml) | Builds a Docker image, checks whether the image already exists in ECR, and pushes it. |
| [`update_ecs_task_definition`](actions/update-ecs-task-definition/action.yml) | Updates an ECS task definition with a new Docker image and deploys it to the specified ECS service and cluster using the AWS-maintained GitHub Actions. |
| [`wait-for-ecs-task`](actions/wait-for-ecs-task/action.yml) | Waits for a standalone ECS `run-task` execution to stop, then validates its exit code. Retries up to 4 times (~40 minutes total) before failing. |
| [`wait_for_ecs_service_stability`](actions/wait-for-ecs-service-stability/action.yml) | Waits for one or more ECS services to become stable by polling every 15 seconds, timing out after 40 failed checks. |


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
- Replace `<ref>` with a major version tag (e.g. `v1`). This ensures you automatically receive bug fixes and non-breaking enhancements while remaining protected from breaking changes.

---

## Versioning

Actions in this repository follow [Semantic Versioning](https://semver.org/).

| Version | Purpose |
|---|---|
| `v1.0.1` | Bug fixes |
| `v1.1.0` | New functionality (non-breaking) |
| `v2.0.0` | Breaking changes |

A **moving major version tag** (e.g. `v1`) is maintained alongside full release tags and always points to the latest compatible release.


Consumers referencing `@v1` automatically receive compatible updates. Breaking changes are released under a new major version (e.g. `v2`), so existing consumers are never affected until they choose to migrate.

---

## Development and Release Workflow

### 1. Develop

Create a feature branch and raise a PR against `main`:

```text
feature/my-improvement
```

### 2. Test

Before releasing, validate changes in a consuming repository using a specific **commit SHA** for an immutable reference:

```yaml
uses: department-for-transport-BODS/.github/actions/<action-name>@a1b2c3d4e5f6
```

### 3. Release

Once testing is complete, create a semantic version tag and publish a GitHub Release with a changelog describing what has changed.

The [`update-major-version-tag`](.github/workflows/update-major-version-tag.yml) workflow will automatically update the corresponding major tag (e.g. `v1`) to point to the new release.

### 4. Breaking Changes

Breaking changes require a new major version. Create a new major tag (e.g. `v2`). Consumers on `@v1` are unaffected and can migrate to `@v2` when ready.
