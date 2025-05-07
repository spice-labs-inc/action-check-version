# Check Cargo Version Matches Git Tag

This GitHub Action ensures that the version in your Rust project's `Cargo.toml` matches the version in the Git tag that triggered the workflow. It is useful for enforcing semantic versioning in release workflows.

## Usage

```yaml
jobs:
  check-version:
    runs-on: ubuntu-24.04
    steps:
      - name: Check out code
        uses: actions/checkout@v4

      - name: Check Cargo Version Matches Git Tag
        uses: spice-labs-inc/action-check-version@main

```
This action expects your workflow to be triggered by a Git tag in the format vX.Y.Z (e.g., v1.2.3). It will:
  *  Parse the version from the first package in Cargo.toml
  *  Extract the version from the Git tag
  *  Fail the workflow if the two versions do not match

## How It Works
This is a composite action, meaning it runs a small shell script within your workflow. It uses the following steps:
  1. Uses cargo metadata and jq to extract the version number from Cargo.toml
  2. Extracts the Git tag from GITHUB_REF (expects tags like v1.2.3)
  3. Compares the two
  4. Exits with an error if they differ

## Requirements
  *  Your workflow must check out the repository before this action runs
  *  `cargo` and `jq` must be available (they are preinstalled on standard GitHub runners)

## Fails If
  *  The Git tag does not start with v
  *  The version in `Cargo.toml` and Git tag do not match
