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

Optional inputs: `manifest` (default `platform.yml`), `prebuild` (a shell command run
before the build, with the `prebuild_token` secret as `GH_TOKEN`).

Releases are tags: `v1` moves with compatible changes; a breaking change gets `v2`.

This repo is public on purpose and holds nothing about where or how apps run.
