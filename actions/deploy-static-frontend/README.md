# deploy-static-frontend

Syncs a static export to an S3 bucket using `--delete`, while retaining the root `build-info.json`, then creates and waits for a CloudFront invalidation. When `html_prefix` is set, objects beneath that relative prefix are copied in place with `text/html; charset=utf-8` metadata before invalidation. This supports exports such as ABODS Freshdesk without embedding that application-specific path in the action.

```yaml
- uses: department-for-transport-BODS/.github/actions/deploy-static-frontend@<full-sha>
  with:
    export_directory: dist
    target_bucket: s3://frontend-bucket
    cloudfront_distribution_id: E1234567890
    html_prefix: freshdesk
```

## Inputs

| Input | Description | Required | Default |
|---|---|---|---|
| `export_directory` | Local directory containing the static export | Yes | — |
| `target_bucket` | S3 bucket URI | Yes | — |
| `cloudfront_distribution_id` | Distribution to invalidate | Yes | — |
| `html_prefix` | Optional relative prefix whose objects receive HTML content type before invalidation | No | Empty |

## Prerequisites

- AWS credentials with S3 sync/copy and CloudFront invalidation permissions.
- The AWS CLI must be available on the runner.