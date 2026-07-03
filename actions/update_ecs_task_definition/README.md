# update_ecs_task_definition

A composite GitHub Action that updates an ECS task definition with a new container image and deploys it to an ECS service using the official AWS-maintained GitHub Actions.

## What It Does

1. **Renders** a new ECS task definition by patching the specified container's image using [`aws-actions/amazon-ecs-render-task-definition`](https://github.com/aws-actions/amazon-ecs-render-task-definition).
2. **Deploys** the rendered task definition to the specified ECS service and cluster using [`aws-actions/amazon-ecs-deploy-task-definition`](https://github.com/aws-actions/amazon-ecs-deploy-task-definition).

## Usage

```yaml
- name: Update ECS Task Definition
  uses: ./.github/actions/update_ecs_task_definition
  with:
    service: my-ecs-service
    container: my-container
    task_definition_family: my-task-definition
    image_id: 123456789012.dkr.ecr.us-east-1.amazonaws.com/my-image:latest
    cluster_name: my-ecs-cluster
    wait_for_service_stability: 'true'
```

## Inputs

| Input | Description | Required | Default |
|---|---|---|---|
| `service` | The name of the ECS service to deploy to | Yes | — |
| `container` | The name of the container inside the task definition to patch with the new image | Yes | — |
| `task_definition_family` | The task definition family name to render and deploy | Yes | — |
| `image_id` | The updated container image URI | Yes | — |
| `cluster_name` | The name of the ECS cluster the service belongs to | Yes | — |
| `wait_for_service_stability` | Whether to wait for the ECS service to reach a stable state after deployment | No | `'false'` |

## Prerequisites

- AWS credentials must be configured in the workflow prior to using this action.
- The IAM role or user must have sufficient permissions to describe, register, and deploy ECS task definitions and update ECS services.

