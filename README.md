# camp-workflows

Reusable GitHub Actions workflows for publishing plugin releases to
[camp](https://camp-registry.org). Calling these instead of copying the
templates from camp-index means fixes and version bumps land here once,
not in every plugin repository.

## Publish releases

Add `.github/workflows/camp-release.yml` to your plugin repository:

```yaml
name: Publish release to camp
on:
  push:
    tags: ["v*"]
  workflow_dispatch: {}
permissions:
  contents: read
  id-token: write
jobs:
  camp:
    uses: camp-registry/camp-workflows/.github/workflows/release.yml@v1
```

Publishing a release is then `git tag v1.2.3 && git push --tags`. No
token, no secrets, no fork: the workflow proves its identity to the camp
publish service with a signed OIDC token, and authorization is your
claimed listing (checked by permanent repository id). See
[AUTHORS.md](https://github.com/camp-registry/camp-docs/blob/main/AUTHORS.md).

Organizations with shared workflows all plugins inherit: add the same
`uses:` job to your shared workflow instead — no per-repository changes.
The OIDC token always identifies the plugin repository that triggered
the run, so per-plugin authorization is unchanged.

`@v1` follows the latest v1.x.y release; exact tags exist if you prefer
pinning. The copy-paste templates in camp-index remain supported.
