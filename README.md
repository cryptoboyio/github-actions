# `GitHub Action – Build & Push Docker Image`

```yaml
name: Build and Push Docker Image

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
    name: Build & Push Docker Image
    runs-on: self-hosted-general
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Registry login
        uses: docker/login-action@v1
        with:
          registry: ${{ secrets.registry }}
      - name: Build image
        run: |
          docker build \
            --build-arg GITHUB_TOKEN="${{ secrets.github-token }}" \
            -t ${{ secrets.registry }}/${{ inputs.repository }}:${{ inputs.image_tag }} \
            -t ${{ secrets.registry }}/${{ inputs.repository }}:${{ inputs.image_tag_latest }} \
            -f ${{ inputs.dockerfile }} .
      - name: Push image
        run: |
          docker push ${{ secrets.registry }}/${{ inputs.repository }}:${{ inputs.image_tag }}
          docker push ${{ secrets.registry }}/${{ inputs.repository }}:${{ inputs.image_tag_latest }}
```
