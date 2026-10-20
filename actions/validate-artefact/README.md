# validate-artefact

Checks that a list of required files exists beneath an S3 prefix and that each object is non-empty. Missing objects receive a clear error annotation; permission and AWS service errors are reported separately. The caller must configure AWS credentials before using this action.

```yaml
- uses: department-for-transport-BODS/.github/actions/validate-artefact@<full-sha>
  with:
    bucket: release-artefacts
    prefix: app_v1.2.3
    required_files: |
      packaged.yaml
      build-info.json
```

## Inputs

| Input | Description | Required |
|---|---|---|
| `bucket` | S3 bucket containing the artefact | Yes |
| `prefix` | S3 key prefix, without a leading slash | Yes |
| `required_files` | Newline-separated relative paths that must exist and be non-empty | Yes |

## Prerequisites

- AWS credentials with `s3:HeadObject` access to the requested objects.
- The AWS CLI must be available on the runner.