# .github

This repository contains shared GitHub Actions and organisation-wide resources for the Department for Transport's Bus Open Data Service (BODS).

## Actions

### `aws-login-assumed-role`

**Path:** `.github/actions/aws-login-assumed-role`

Encapsulates the two-step AWS authentication pattern used across all BODS pipelines:

1. Authenticate with the **BODS Shared-Services** AWS account via OIDC
2. Chain to **assume a role** in the target account (dev, test, uat, prod, etc.)

This pattern is required because the OIDC identity provider and the initial trust relationship are hosted in Shared-Services. Each target account grants access by trusting the Shared-Services role.

#### Inputs

| Input | Required | Default | Description |
|---|---|---|---|
| `aws-region` | yes | — | AWS region to configure credentials for |
| `shared-services-role-arn` | yes | — | ARN of the OIDC role in the Shared-Services account. Maps to the `BODS_DEFAULT_GITHUB_ACTIONS_ASSUME_ROLE_ARN` repository variable |
| `target-role-arn` | yes | — | ARN of the role to assume in the target account (e.g. `PROD_ASSUME_ROLE_ARN`, `ASSUME_ROLE_ARN`) |
| `role-session-name` | yes | — | Session name for AWS CloudTrail attribution (e.g. `my-service-dev-deployment`) |
| `role-duration-seconds` | no | `3600` | Duration in seconds for each assumed role session |

#### Outputs

None. AWS credentials are set as environment variables on the runner by `aws-actions/configure-aws-credentials`, making them available to all subsequent steps in the job.

#### Usage

The calling workflow must have `id-token: write` permission for the OIDC exchange.

```yaml
permissions:
  id-token: write
  contents: read

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: dev

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Configure AWS credentials
        uses: department-for-transport-BODS/.github/.github/actions/aws-login-assumed-role@main
        with:
          aws-region: ${{ vars.AWS_REGION }}
          shared-services-role-arn: ${{ vars.BODS_DEFAULT_GITHUB_ACTIONS_ASSUME_ROLE_ARN }}
          target-role-arn: ${{ vars.ASSUME_ROLE_ARN }}
          role-session-name: my-service-dev-deployment

      # Subsequent steps now have credentials scoped to the target account
      - name: Login to Amazon ECR
        uses: aws-actions/amazon-ecr-login@v2
```

#### Notes

- `role-skip-session-tagging` is set to `true` on both credential steps. This is consistent with all existing BODS workflows and avoids the `sts:TagSession` permission requirement.
- For workflows that need to switch accounts mid-job (e.g. authenticate to prod for ECR, then switch to dev for deployment), call the action a second time with `unset-current-credentials: true` handled via the underlying `aws-actions/configure-aws-credentials` action directly. This less-common pattern is intentionally out of scope for this action.
