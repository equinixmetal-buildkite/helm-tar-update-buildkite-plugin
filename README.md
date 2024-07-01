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
      github.com/equinixmetal-buildkite/helm-tar-update-buildkite-plugin#v0.0.2: {}
```

This will make sure that for any pull request that updates Helm dependencies,
the tarball for those dependencies will be downloaded and commited to appropriate
branch to come along the pull request.

Note that this will only happen if you keep a `charts` directory with your Helm
chart.

### Helm Registry Login

Some helm charts are stored in private registries that need to auth prior to pulling the updated dependencies.
Starting in `v0.0.2`, the plugin provides the ability to provide these credentials and endpoints.

```yaml
---
steps:
  - name: ":helm: Helm Tarball Update"
    plugins:
      github.com/equinixmetal-buildkite/helm-tar-update-buildkite-plugin#v0.0.2:
        parameters:
          HELM_TOKEN: "some-fake-token"
          HELM_USER: "some-helm-username"
          HELM_REGISTRY: "oci://ghcr.io/some-org"
```

More realistically (and recommended) usage is that you have a plugin that runs prior to this that pulls in the appropriate
`HELM_TOKEN`, sets it to an available environment variable and then you can set the `HELM_TOKEN` to that similar to the one
below.

```yaml
---
steps:
  - name: ":helm: Helm Tarball Update"
    plugins:
      github.com/equinixmetal-buildkite/helm-tar-update-buildkite-plugin#v0.0.2:
        parameters:
          HELM_TOKEN: SOME_ENV_VAR
          HELM_USER: "some-helm-username"
          HELM_REGISTRY: "oci://ghcr.io/some-org"
```
