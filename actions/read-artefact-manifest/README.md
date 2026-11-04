# read-artefact-manifest

Reads a deployed artefact version from DynamoDB using a strongly consistent read. Missing, empty, or multiline versions fail the action. The caller must configure AWS credentials first and remains responsible for service-specific version validation.

```yaml
- uses: department-for-transport-BODS/.github/actions/read-artefact-manifest@v1
  id: manifest
  with:
    table: bods-artefact-manifest
    service_artefact: abods-backend
    environment: sandbox
    region: eu-west-2
```

## Inputs

| Input | Description | Required |
|---|---|---|
| `table` | DynamoDB manifest table | Yes |
| `service_artefact` | Service artefact key to read | Yes |
| `environment` | Deployment environment to read | Yes |
| `region` | AWS region containing the table | Yes |

## Outputs

| Output | Description |
|---|---|
| `version` | Deployed artefact version, available as `steps.manifest.outputs.version` |

## Prerequisites

- AWS credentials with `dynamodb:GetItem` access to the manifest table.
- Bash, AWS CLI, and `jq` available on the runner.
- A table with string partition key `service_artefact`, string sort key `environment`, and string attribute `version`.