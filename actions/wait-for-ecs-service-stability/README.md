# wait_for_ecs_service_stability

A composite GitHub Action that polls one or more ECS services until they reach a stable state, using the AWS CLI `ecs wait services-stable` command.

## What It Does

1. **Validates** that AWS credentials are configured before proceeding.
2. **Polls** the specified ECS services in the given cluster using `aws ecs wait services-stable`.
3. **Retries up to 3 times** if the waiter times out before the services become stable.
4. **Exits successfully** if all services become stable, or **fails** with a non-zero exit code if all 3 attempts are exhausted.

## Usage

```yaml
- name: Wait for ECS Service Stability
  uses: ./.github/actions/wait-for-ecs-service-stability
  with:
    service_names: my-service-a my-service-b
    cluster_name: my-ecs-cluster
```

## Inputs

| Input | Description | Required |
|---|---|---|
| `service_names` | A space-separated list of ECS service names to monitor (e.g. `"service-a service-b"`) | Yes |
| `cluster_name` | The name of the ECS cluster the services belong to | Yes |

## Prerequisites

- AWS credentials must be configured in the workflow prior to using this action.
- The IAM role or user must have `ecs:DescribeServices` permissions on the target cluster and services.
- The AWS CLI must be available in the runner environment (it is pre-installed on GitHub-hosted runners).

## Timeout Behaviour

This action delegates timeout behaviour to the AWS CLI waiter, which polls every **15 seconds** and gives up after **40 attempts** (~10 minutes per attempt). If the services have not stabilised within that window, the action retries up to **3 times** before exiting with a non-zero code and failing the workflow. The maximum total wait time is therefore approximately **30 minutes**.

