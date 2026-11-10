# replworks/.github

A collection of shared GitHub Actions reusable workflows for the Replworks organization.

## Workflows

| Workflow               | Purpose                                                                                   | Trigger (in the calling repo) |
| ---------------------- | ----------------------------------------------------------------------------------------- | ----------------------------- |
| `update-changelog.yml` | Updates `CHANGELOG.md` from the release notes and commits it to the release target branch | `release: published`          |

## Usage

Create `.github/workflows/changelog.yml` in each repository and use one of the two variants below, depending on the runner.

### 1. Ubuntu (GitHub-hosted)

```yaml
name: Changelog

on:
  release:
    types: [published]

jobs:
  changelog:
    uses: replworks/.github/.github/workflows/update-changelog.yml@v1
    permissions:
      contents: write
```

If `runs-on` is omitted, it defaults to `ubuntu-latest`.

### 2. Self-hosted (linux, arm64)

```yaml
name: Changelog

on:
  release:
    types: [published]

jobs:
  changelog:
    uses: replworks/.github/.github/workflows/update-changelog.yml@v1
    with:
      runs-on: '["self-hosted", "linux", "arm64"]'
    permissions:
      contents: write
```

### Inputs

| Name      | Type          | Default             | Description                                 |
| --------- | ------------- | ------------------- | ------------------------------------------- |
| `runs-on` | string (JSON) | `'"ubuntu-latest"'` | Runner selection. Pass it as a JSON string. |

`runs-on` examples:

- GitHub-hosted: omit it, or use `'"ubuntu-latest"'`
- Self-hosted: `'["self-hosted", "linux", "arm64"]'`

### Requirements for the calling repo

- The caller must grant `permissions: contents: write` itself. A reusable workflow cannot have more permissions than its caller.
- Only `GITHUB_TOKEN` is used, so `secrets: inherit` is not needed.
- If the release's `target_commitish` is not `main` or `master`, the major version of the tag must match the major version of the branch. If they differ, the workflow fails on purpose.

## Versioning

- Releases are created with concrete versions such as `v1.0.0` and `v1.1.0` (GitHub Releases).
- Callers reference the major tag `@v1`. `v1` is a **moving tag** that points to the latest `v1.x.y` release.
- Callers do not need to change their files when the workflow is updated. Only breaking changes bump the major version to `v2`.

### Releasing a new version

1. Push the change to `main` and verify it.
2. Create a `v1.x.y` Release on GitHub.
3. Move the major tag to the new release.

```bash
git fetch --tags
git tag -f v1 v1.x.y
git push origin v1 --force
```

For the very first release, create the `v1.0.0` Release, then run the commands above with `v1.0.0` in place of `v1.x.y`. If the `v1` tag does not exist, calls to `@v1` will fail.

> Creating a Release in this repo does not update any CHANGELOG, because this repo has no workflow that calls `update-changelog`.

## Setup checklist

- If this repo is private, enable "Accessible from repositories in the organization" under Settings → Actions → General → Access. (Not needed if the repo is public.)
- For repos using self-hosted runners, the runner must be registered at the organization level. If it is registered only at the repo level, only that repo can use it.
- `git` must be installed on the runner machine.
- The release date extraction uses GNU `date` (`date -d @...`), so it does not work on macOS (BSD `date`) runners. Use a Linux runner.

## Why it is built this way

- **Reusable workflow instead of copies**: Copying the yml into every repo means chasing each copy whenever it changes. Keeping it in one place and calling it with `uses` applies a change everywhere at once. (The `workflow-templates` approach creates copies, so changes to the original do not reach repos that already copied it.)
- **`runs-on` as an input**: Keeps the GitHub-hosted and self-hosted variants in a single file instead of two. The difference only shows up in the caller's `with` block.
- **`@v1` instead of `@main`**: Acts as a buffer so that a broken `main` does not immediately spread to every repo. Verify on `main`, then move `v1`.
- **Release + moving major tag**: Releases make the version name visible on the repo's main page, and moving `v1` lets updates reach callers without editing their files.
- **Actions referenced by tag, not SHA**: Upstream bug fixes are picked up automatically. The trade-off is that tags can be moved to a different commit, so a compromised third-party action would run with `contents: write` permission. We accept that risk.
- **Release date taken from the tag's commit**: The date in the changelog should be the UTC date of the commit the tag points to, so it is extracted directly with `git log`.
- **Values passed through `env`**: `${{ ... }}` expressions are not interpolated directly into shell scripts; they are passed via `env` instead.
