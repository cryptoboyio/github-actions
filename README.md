# Custom GitHub Actions

- [Build and Push Docker Image](#build-and-push-docker-image)

## Build and Push Docker Image

### Inputs

| Name               | Type    | Required | Default value         | Description                      |
| ------------------ | ------- | -------- | --------------------- | -------------------------------- |
| `CACHE_REGISTRY`   | String  | false    | `***`                 | Docker layers cache registry     |
| `CONTEXT`          | String  | false    | `.`                   | Docker context                   |
| `DEFAULT_RUNNER`   | String  | false    | `self-hosted`         | Default runner                   |
| `DOCKERFILE`       | String  | false    | `Dockerfile`          | Dockerfile                       |
| `IMAGE_TAG`        | String  | false    | `BRANCH-RUN_NUMBER`   | Image tag                        |
| `IMAGE_TAG_LATEST` | String  | false    | `BRANCH-latest`       | Image tag latest                 |
| `NO_CACHE`         | Boolean | false    | `false`               | Build without cache              |
| `PUSH_IMAGE`       | Boolean | false    | `true`                | Push image to the registry       |
| `REGISTRY`         | String  | false    | `***`                 | Docker registry                  |
| `REPOSITORY`       | String  | false    | `GITHUB_REPOSITORY`   | Repository in the registry       |
| `RUNNERS`          | String  | false    | `self-hosted-general` | Runner types for different archs |

If `IMAGE_TAG` or `IMAGE_TAG_LATEST` contains a `/` character, then the default values ​​will be as follows:

| Name               | Value              |
| ------------------ | ------------------ |
| `IMAGE_TAG`        | `build-RUN_NUMBER` |
| `IMAGE_TAG_LATEST` | `build-latest`     |

### Secrets

| Name              | Type   | Required | Description                       |
| ----------------- | ------ | -------- | --------------------------------- |
| `ACCESS_TOKEN`    | String | true     | GitHub access token               |
| `BUILD_ARGS`      | List   | false    | Docker build args                 |
| `SLACK_BOT_TOKEN` | String | false    | Slack bot token for notifications |

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
    uses: cryptoboyio/github-actions/.github/workflows/build-and-push.yaml@v1.0.2
    with:
      RUNNERS: self-hosted-general,self-hosted-arm
    secrets:
      ACCESS_TOKEN: ${{ secrets.ACCESS_REPOS_TOKEN }}
      SLACK_BOT_TOKEN: ${{ secrets.ACTIONS_SLACK_BOT_TOKEN }}
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
    uses: cryptoboyio/github-actions/.github/workflows/build-and-push.yaml@v1.0.2
    with:
      PUSH_IMAGE: false
    secrets:
      ACCESS_TOKEN: ${{ secrets.ACCESS_REPOS_TOKEN }}
      BUILD_ARGS: |
        MAXMIND_LICENSE_KEY=${{ secrets.MAXMIND_LICENSE_KEY }}
```
