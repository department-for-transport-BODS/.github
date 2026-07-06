# build-and-push-image

A composite GitHub Action that builds a Docker image, checks whether the image already exists in the registry, and pushes the image if it does not.

## What It Does

1. Checks whether the specified image URI already exists in the target registry using `docker manifest inspect`.
2. Builds the Docker image using the provided Dockerfile and build context.
3. Pushes the built image to the provided image URI.

## Usage

```yaml
- name: Build, check, and push image
  uses: ./.github/actions/build-and-push-image
  with:
    image_uri: 123456789012.dkr.ecr.eu-west-2.amazonaws.com/my-repo:latest
    dockerfile: ./docker_images/bdd-automation/Dockerfile
    build_context: .
    check_existing_image: 'true'
```

## Inputs

| Input | Description | Required | Default |
|---|---|---|---|
| `image_uri` | Fully qualified image URI including registry, repository, and tag | Yes | — |
| `dockerfile` | Path to the Dockerfile | Yes | — |
| `build_context` | Docker build context directory | Yes | `.` |
| `check_existing_image` | Whether to fail if the image already exists in the registry | No | `'true'` |

## Outputs

| Output | Description |
|---|---|
| `image` | The image URI that was built and pushed |

## Prerequisites

- Docker must be installed and available on the runner.
- AWS credentials / ECR authorization must already be available in the environment or through previous workflow steps.
