# Custom GitHub Actions

- [Build and Push Docker Image](#build-and-push-docker-image)

## Build and Push Docker Image

### Inputs

| Name               | Type    | Required | Default value                       | Description                |
| ------------------ | ------- | -------- | ----------------------------------- | -------------------------- |
| `CONTEXT`          | String  | false    | `.`                                 | Docker context             |
| `DOCKERFILE`       | String  | false    | `Dockerfile`                        | Dockerfile                 |
| `IMAGE_TAG`        | String  | false    | `github.ref_name-github.run_number` | Image tag                  |
| `IMAGE_TAG_LATEST` | String  | false    | `github.ref_name-latest`            | Image tag latest           |
| `NO_CACHE`         | Boolean | false    | `false`                             | Build without cache        |
| `PUSH_IMAGE`       | Boolean | false    | `true`                              | Push image to the registry |
| `REPOSITORY`       | String  | false    | `github.event.repository.name`      | Repository in the registry |

If `IMAGE_TAG` or `IMAGE_TAG_LATEST` contains a `/` character, then the default values ​​will be as follows:

| Name               | Value                            |
| ------------------ | -------------------------------- |
| `IMAGE_TAG`        | `build-${{ github.run_number }}` |
| `IMAGE_TAG_LATEST` | `build-latest`                   |

### Secrets

| Name             | Type   | Required | Description           |
| ---------------- | ------ | -------- | --------------------- |
| `ACCESS_TOKEN`   | String | true     | GitHub access token   |
| `BUILD_ARGS`     | List   | false    | Docker build args     |
| `CACHE_REGISTRY` | String | false    | Docker сache registry |
| `REGISTRY`       | String | false    | Docker registry       |

### Examples

```yaml
name: Build & Push Docker Image

on:
  workflow_dispatch:
  push:
    branches:
      - "master"
      - "develop"

jobs:
  build-and-push:
    name: Build ${{ github.ref_name }}-${{ github.run_number }}
    uses: cryptoboyio/github-actions/.github/workflows/build-and-push.yaml@v1.0.0
    secrets:
      ACCESS_TOKEN: ${{ secrets.ACCESS_REPOS_TOKEN }}
```

```yaml
name: Build Docker Image

on:
  pull_request:
    branches:
      - "master"
      - "develop"

jobs:
  build:
    name: Build PR-${{ github.run_number }}
    uses: cryptoboyio/github-actions/.github/workflows/build-and-push.yaml@v1.0.0
    with:
      PUSH_IMAGE: false
    secrets:
      ACCESS_TOKEN: ${{ secrets.ACCESS_REPOS_TOKEN }}
      BUILD_ARGS: |
        MAXMIND_LICENSE_KEY=${{ secrets.MAXMIND_LICENSE_KEY }}
```
