# Bump Version and Tags

This action will find the last version tag made on the current branch, bump it and tag the current commit with the new version. As tags are commit specific and not branch specific, these version tags are, by default, prefixed with the current branch name, e.g. main/v1.0.0.

## Usage

### Example Pipeline

```yaml
name: Bump Version and Tag
on:
  push:
    branches:
      - 'test'
      - 'staging'
      - 'production'
jobs:
  build-and-push:
    env:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
    name: Bump and Tag
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v1
      - name: Bump and Tag
        id: bump_and_tag
        uses: bricklane/action.bump-version-and-tag@v2
```

## Environment Variable

- `GITHUB_TOKEN`: GitHub provides a token that you can use to authenticate on behalf of GitHub Actions as described in [Authenticating with the GITHUB_TOKEN](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets).

## Optional Arguments

- `prefix`: Set a custom prefix for the tag name (default: _{branch-name}_/v)

## Outputs

- `tag`: The name of the new tag created

## Versioning

Versioning is performed with the SemVer format (_major_._minor_._patch_). By default the _patch_ number will be incremented on merge. _Major_ and _minor_ bumps are be controlled by  keywords found in the commit messages created since the last tag created on the same branch. The action understands both Conventional Commit style messages and also `#major` and `#minor` keywords to indicate _major_ and _minor_ version changes.

In the case of multiple keywords across commits, only a single update is performed, with _major_ taking priority, followed by _minor_ and then _patch_.

As tags will be specific to the branch they are on, tags will be in the form of `[BRANCH_NAME]/1.0.0`.

## Hotfixes

In the case of merging a _hotfix_ branch, the previous tag on that particular branch will be moved to the current commit.
