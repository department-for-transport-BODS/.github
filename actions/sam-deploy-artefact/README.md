# sam-deploy-artefact

Deploys a local packaged SAM template using the supplied config file and environment. It reads and validates that environment's `deploy.parameters.stack_name`, preserves the deployment flags and tags used by the ABODS workflow, and exposes the validated stack name. It does not download artefacts or configure credentials.

```yaml
- uses: department-for-transport-BODS/.github/actions/sam-deploy-artefact@<full-sha>
  id: backend
  with:
    template_file: packaged.yaml
    config_file: samconfig.yaml
    config_environment: sandbox
    aws_region: eu-west-2
    deployment_bucket: deployment-artefacts
    deployment_prefix: app/deployments/sandbox
    project_name: app
    release_version: v1.2.3
```

## Inputs

| Input | Description | Required |
|---|---|---|
| `template_file` | Path to the packaged SAM template | Yes |
| `config_file` | Path to the SAM config file | Yes |
| `config_environment` | Environment key in the SAM config file | Yes |
| `aws_region` | AWS region for deployment | Yes |
| `deployment_bucket` | SAM deployment-artifact bucket | Yes |
| `deployment_prefix` | SAM deployment-artifact key prefix | Yes |
| `project_name` | `ProjectName` stack tag | Yes |
| `release_version` | `BuiltFrom` stack tag | Yes |

## Outputs

| Output | Description |
|---|---|
| `stack_name` | Validated stack name selected from the SAM config |

## Prerequisites

- AWS credentials with permissions to deploy the stack and read any referenced code objects.
- AWS SAM CLI and Mike Farah `yq` v4 must be available on the runner.