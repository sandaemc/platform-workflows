# platform-workflows

The shared GitHub Actions workflow that builds an app's image for the platform. It
reads everything from the app's `platform.yml`, so an app's own workflow is just:

```yaml
name: Image
on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  image:
    uses: sandaemc/platform-workflows/.github/workflows/image.yml@v1
    permissions:
      contents: read
      packages: write
```

Optional inputs: `manifest` (default `platform.yml`), `context` (the Docker build context;
default the manifest's folder), `prebuild` (a shell command run before the build, with the
`prebuild_token` secret as `GH_TOKEN`).

A second app in the same repo keeps its own manifest and Dockerfile in a folder, and its own
workflow that builds only when that folder changes:

```yaml
name: Gateway image
on:
  push:
    branches: [main]
    paths: [gateway/**, .github/workflows/gateway-image.yml]
  workflow_dispatch:

jobs:
  image:
    uses: sandaemc/platform-workflows/.github/workflows/image.yml@v1
    with:
      manifest: gateway/platform.yml
    permissions:
      contents: read
      packages: write
```

The first app's workflow then ignores that folder (`paths-ignore: [gateway/**]`).

Releases are tags: `v1` moves with compatible changes; a breaking change gets `v2`.

This repo is public on purpose and holds nothing about where or how apps run.
