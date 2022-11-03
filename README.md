# Custom GitHub Actions

- [Build and Push Docker Image](#build-and-push-docker-image)

## Build and Push Docker Image

### Inputs

| Name               | Type    | Required | Default value | Description                |
| ------------------ | ------- | -------- | ------------- | -------------------------- |
| `CONTEXT`          | String  | false    | `.`           | Docker context             |
| `DOCKERFILE`       | String  | false    | `Dockerfile`  | Dockerfile                 |
| `IMAGE_TAG`        | String  | true     | `–`           | Image tag                  |
| `IMAGE_TAG_LATEST` | String  | true     | `–`           | Image tag latest           |
| `NO_CACHE`         | Boolean | false    | `false`       | Build without cache        |
| `PUSH_IMAGE`       | Boolean | false    | `true`        | Push image to the registry |
| `REPOSITORY`       | String  | true     | `–`           | Repository in the registry |

### Secrets

| Name             | Type   | Required | Description           |
| ---------------- | ------ | -------- | --------------------- |
| `ACCESS_TOKEN`   | String | true     | GitHub access token   |
| `BUILD_ARGS`     | List   | false    | Docker build args     |
| `CACHE_REGISTRY` | String | true     | Docker Cache registry |
| `REGISTRY`       | String | true     | Docker registry       |

### Example

```yaml
name: Build & Push Docker Image

on:
  workflow_dispatch:
  push:
    branches:
      - "master"
      - "develop"
      - "blockbook"
      - "release[0-9]+"
      - "release[0-9]+-[0-9]+"

jobs:
  build-and-push:
    name: Build PR-${{ github.run_number }}
    uses: cryptoboyio/github-actions/.github/workflows/build-and-push.yaml@master
    with:
      IMAGE_TAG: pr-${{ github.run_number }}
      IMAGE_TAG_LATEST: pr-${{ github.run_number }}-latest
      PUSH_IMAGE: false
      REPOSITORY: ${{ github.event.repository.name }}
    secrets:
      ACCESS_TOKEN: ${{ secrets.ACCESS_REPOS_TOKEN }}
      BUILD_ARGS: |
        MAXMIND_LICENSE_KEY=${{ secrets.MAXMIND_LICENSE_KEY }}
      CACHE_REGISTRY: ${{ secrets.ACTIONS_CACHE_REGISTRY }}
      REGISTRY: ${{ secrets.ACTIONS_REGISTRY }}
```
