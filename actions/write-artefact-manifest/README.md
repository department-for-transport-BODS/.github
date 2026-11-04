# write-artefact-manifest

Records deployed artefact versions in one DynamoDB transaction, so all supplied versions update together or none do. Call this action only after all deployments succeed and configure AWS credentials first. Other attributes on existing items are preserved.

```yaml
- uses: department-for-transport-BODS/.github/actions/write-artefact-manifest@v1
  with:
    table: bods-artefact-manifest
    environment: dev
    region: eu-west-2
    artefacts: |
      {
        "abods-backend": "abods_backend_sandbox_v1.2.3",
        "abods-frontend": "abods_frontend_sandbox_v1.2.3"
      }
```

## Inputs

| Input | Description | Required |
|---|---|---|
| `table` | DynamoDB manifest table | Yes |
| `artefacts` | JSON object mapping 1 to 100 non-empty service artefact keys to non-empty version strings | Yes |
| `environment` | Deployment environment to update | Yes |
| `region` | AWS region containing the table | Yes |

## Prerequisites

- AWS credentials with `dynamodb:UpdateItem` access to the manifest table for transactional updates.
- Bash, AWS CLI, and `jq` available on the runner.
- A table with string partition key `service_artefact`, string sort key `environment`, and string attribute `version`.

## Release Coordination

Release both manifest actions in this repository before merging consumers that reference them at `@v1`. For pre-release testing, reference the shared-repository commit SHA containing the actions instead.