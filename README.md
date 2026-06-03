# 🔍 Check Cargo Version Matches Git Tag

This GitHub Action ensures that the version in your Rust project's `Cargo.toml` matches the version in the Git tag that triggered the workflow. It is useful for enforcing semantic versioning in release workflows.

## 🚀 Usage

```yaml
jobs:
  check-version:
    name: Ensure Tag Matches Cargo.toml
    if: startsWith(github.ref, 'refs/tags/v')
    runs-on: ubuntu-24.04
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v5

      - name: Check Cargo Version Matches Tag
        uses: spice-labs-inc/action-check-version@main
```

> **Note:** Pinning to `@main` means you always get the latest changes. For production workflows, consider pinning to a specific release tag (e.g. `@v1.0.0`) for reproducibility.

This action expects your workflow to be triggered by a Git tag in the format `vX.Y.Z` (e.g. `v1.2.3`). It will:

- Parse the version from the first package resolved by `cargo metadata` in `Cargo.toml`
- Extract the version from the Git tag
- Fail the workflow if the two versions do not match

> **Workspace note:** In a Cargo workspace, `cargo metadata` resolves packages in dependency order, which may not match the order they appear in `Cargo.toml`. If your workspace contains multiple packages, ensure the intended package is the first resolved package, or consider specifying a sub-directory via the `working-directory` input.

## How It Works

This is a composite action, meaning it runs a shell script in your workflow. The script:

1. Uses `cargo metadata` to extract the workspace package list as JSON, then uses `jq` to parse the version of the first package
2. Extracts the Git tag version from `GITHUB_REF`
3. Compares the two
4. Exits with an error if they differ

## 🔧 Required Dependencies

The action requires:

- `cargo`
- `jq`

These are typically available on `ubuntu-22.04` and `ubuntu-24.04`. If not, add:

```yaml
- name: Install jq
  run: sudo apt-get update && sudo apt-get install -y jq
```

The action passes `--frozen` to `cargo metadata`, which means it will not attempt any network fetches or update `Cargo.lock`. Your `Cargo.lock` must be committed and up to date before this action runs.

## Inputs

| Input               | Required | Default | Description                              |
|---------------------|----------|---------|------------------------------------------|
| `working-directory` | No       | `.`     | Path to the directory containing `Cargo.toml` |

## Fails If

- The Git tag does not start with `v`
- The version in `Cargo.toml` and the Git tag do not match
- `Cargo.lock` is missing or out of date (due to `--frozen`)
