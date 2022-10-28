# `Custom GitHub Actions`

## `Build & Push Docker Image`

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
    uses: cryptoboyio/action-build-and-push/.github/workflows/build.yaml@master
    with:
      image_tag: ${{ github.ref_name }}-${{ github.run_number }}
      image_tag_latest: ${{ github.ref_name }}-latest
      push-image: true
      repository: ${{ github.event.repository.name }}
    secrets:
      cash-registry: ${{ secrets.ACTIONS_CASH_REGISTRY }}
      github-token: ${{ secrets.ACCESS_REPOS_TOKEN }}
      registry: ${{ secrets.ACTIONS_AWS_ECR_REPOSITORY }}
```
