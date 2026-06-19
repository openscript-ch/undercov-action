# undercov-action

Don't go undercov! Track your test coverage and check for under-coverage with this Forgejo, Gitea and GitHub Action. 🕵️

## Usage

You can use undercov in your workflow to check for under-coverage. Here's an example of how to set it up:

```yaml
name: Coverage check

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  coverage:
    runs-on: ubuntu-latest
    steps:
      - name: Check out repository
        uses: actions/checkout@v6

      - name: Run tests and generate lcov file
        run: |
          # Run your tests and generate the lcov file

      - name: Check coverage with undercov
        uses: openscript-ch/undercov-action@v1
        with:
          threshold: 80
          files: '**/coverage/lcov.info'
          branch: coverage
          # Optional: push updated coverage data to the remote branch.
          # push: true
          # remote: origin
          # push-force-with-lease: true
          # Optional: pin to a specific release tag.
          # version: v0.2.0
          # Optional: verify the downloaded binary checksum.
          # sha256: 0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef
```

## Inputs

- `files`: Glob pattern for LCOV files. Default: `**/coverage/lcov.info`.
- `branch`: Branch used to store coverage data. Default: `coverage`.
- `threshold`: Minimum coverage percentage. Default: `0`.
- `check-regression`: Fail if coverage regresses compared to previously stored data. Default: `false`.
- `push`: Push the updated coverage branch to the configured remote. Default: `false`.
- `remote`: Remote used when `push` is enabled. Default: `origin`.
- `push-force-with-lease`: Use `--force-with-lease` when pushing the coverage branch. Default: `false`.
- `version`: Undercov release tag to use (for example `v0.2.0`). Default: empty, which resolves to the latest release.
- `sha256`: Optional SHA-256 checksum for the resolved undercov binary asset. If set, the action verifies the downloaded (or cached) binary and fails on mismatch.

When `push` is enabled, ensure the workflow has permission to push to the configured remote branch and the checkout step uses credentials that can write.

## Binary Download, Caching and PATH

- The action downloads a platform-specific undercov binary from releases.
- Release source order depends on where the action runs:
  - On GitHub runners: GitHub first, then Forgejo (Codeberg).
  - On Gitea/Forgejo runners: Forgejo (Codeberg) first, then GitHub.
- The action resolves the correct binary asset for the current runner OS and architecture.
- The binary is cached using `actions/cache` and reused in subsequent runs.
- If `sha256` is provided, the action validates the binary checksum before execution.
- The binary directory is added to `PATH`, and the action runs `undercov` directly.
