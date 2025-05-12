# 🔍 Check Cargo Version Matches Git Tag

This GitHub Action ensures that the version in your Rust project's `Cargo.toml` matches the version in the Git tag that triggered the workflow. It is useful for enforcing semantic versioning in release workflows.

## 🚀 Usage
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
This action expects your workflow to be triggered by a Git tag in the format vX.Y.Z (e.g. v1.2.3). It will:
 - Parse the version from the first package in Cargo.toml
 - Extract the version from the Git tag
 - Fail the workflow if the two versions do not match

## How It Works
This is a composite action, meaning it runs a shell script in your workflow. The script:
 1. Uses `cargo metadata` and `jq` to extract the version number from `Cargo.toml`
 2. Extracts the Git tag version from `GITHUB_REF`
 3. Compares the two
 4. Exits with an error if they differ

## 🔧 Required dependencies
The action requires:
 - cargo
 - jq
These are typically available on ubuntu-22.04 and ubuntu-24.04. If not, add:
```yaml
- name: Install jq
  run: sudo apt-get update && sudo apt-get install -y jq
```

## Fails If
 - The Git tag does not start with v
 - The version in `Cargo.toml` and Git tag do not match
