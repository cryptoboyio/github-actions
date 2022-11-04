# Custom GitHub Actions

- [Build and Push Docker Image](#build-and-push-docker-image)

## Build and Push Docker Image

### Inputs

| Name               | Type    | Required | Default value                                     | Description                |
| ------------------ | ------- | -------- | ------------------------------------------------- | -------------------------- |
| `CONTEXT`          | String  | false    | `.`                                               | Docker context             |
| `DOCKERFILE`       | String  | false    | `Dockerfile`                                      | Dockerfile                 |
| `IMAGE_TAG`        | String  | false    | `${{ github.ref_name }}-${{ github.run_number }}` | Image tag                  |
| `IMAGE_TAG_LATEST` | String  | false    | `${{ github.ref_name }}-latest`                   | Image tag latest           |
| `NO_CACHE`         | Boolean | false    | `false`                                           | Build without cache        |
| `PUSH_IMAGE`       | Boolean | false    | `true`                                            | Push image to the registry |
| `REPOSITORY`       | String  | false    | `${{ github.event.repository.name }}`             | Repository in the registry |

### Secrets

| Name             | Type   | Required | Description           |
| ---------------- | ------ | -------- | --------------------- |
| `ACCESS_TOKEN`   | String | true     | GitHub access token   |
| `BUILD_ARGS`     | List   | false    | Docker build args     |
| `CACHE_REGISTRY` | String | false    | Docker Cache registry |
| `REGISTRY`       | String | false    | Docker registry       |

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
    name: Build ${{ github.ref_name }}-${{ github.run_number }}
    uses: cryptoboyio/github-actions/.github/workflows/build-and-push.yaml@develop
    secrets:
      ACCESS_TOKEN: ${{ secrets.ACCESS_REPOS_TOKEN }}
```
