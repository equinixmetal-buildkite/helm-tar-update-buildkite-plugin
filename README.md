# helm-tar-update-buildkite-plugin

The helm-tar-update-buildkite-plugin allows for automatically fetching and cleaning up helm tarballs

## Context

There exist multiple solutions for automatic version updates for helm.
While this is great, there are cases where folks keep a copy of the
release tarball. Either for caching optimization or to ensure that
it stays available... Regardless of the reason, this re-usable workflow
ensures that when a dependency update happens, a tarball is downloaded.

## Usage

In your workflow, simply add the following:

```yaml
---
steps:
  - name: ":helm: Helm Tarball Update"
    plugins:
      github.com/equinixmetal-buildkite/helm-tar-update-buildkite-plugin#v0.0.1: {}
```

This will make sure that for any pull request that updates Helm dependencies,
the tarball for those dependencies will be downloaded and commited to appropriate
branch to come along the pull request.

Note that this will only happen if you keep a `charts` directory with your Helm
chart.
