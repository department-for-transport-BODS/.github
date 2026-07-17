# wait-for-ecs-task

A composite GitHub Action that waits for an ECS run-task execution to stop, then validates its exit code. Designed for use with `aws ecs run-task` (standalone tasks), as opposed to `wait-for-ecs-service-stability` which is for long-running services.

## What It Does

1. **Validates** that AWS credentials are configured before proceeding.
2. **Polls** the specified ECS task using `aws ecs wait tasks-stopped` until it reaches the `STOPPED` state.
3. **Retries up to 4 times** if the waiter times out — each attempt covers ~10 minutes, giving a maximum total wait of ~40 minutes.
4. **Reads the container exit code** via `aws ecs describe-tasks` once the task stops.
5. **Exits successfully** if the exit code is `0`, or **fails** with a non-zero exit code and logs the ECS `stoppedReason` for diagnosis.

## Usage

```yaml
- name: Wait for ECS task
  uses: department-for-transport-BODS/.github/actions/wait-for-ecs-task@main
  with:
    task_arn: ${{ steps.run-task.outputs.task_arn }}
    cluster_name: my-ecs-cluster
```

## Inputs

| Input | Description | Required |
|---|---|---|
| `task_arn` | The full ARN of the ECS task to wait for | Yes |
| `cluster_name` | The name of the ECS cluster the task is running in | Yes |

## Prerequisites

- AWS credentials must be configured in the workflow prior to using this action.
- The IAM role or user must have `ecs:DescribeTasks` permissions on the target cluster and task.
- The AWS CLI must be available in the runner environment (it is pre-installed on GitHub-hosted runners).

## Timeout Behaviour

This action delegates timeout to the AWS CLI waiter, which polls every **6 seconds** and gives up after **100 attempts** (~10 minutes per attempt). If the task has not stopped within that window, the action retries up to **4 times** before failing. The maximum total wait time is therefore approximately **40 minutes**.

## Difference from `wait-for-ecs-service-stability`

| | `wait-for-ecs-task` | `wait-for-ecs-service-stability` |
|---|---|---|
| Use case | Standalone `run-task` executions | Long-running ECS services |
| AWS CLI waiter | `tasks-stopped` | `services-stable` |
| Success condition | Container exit code `0` | Service desired/running count stable |
| Max wait | ~40 minutes | ~30 minutes |
