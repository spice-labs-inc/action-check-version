### cargo-version-check
Checks GitHib Release tag matches Cargo.toml version number and fails if they mismatch

#### Check Cargo Version Matches Tag
This GitHub Action ensures that the Git tag (e.g. `v1.2.3`) matches the version in `Cargo.toml`.

#### Usage
```yaml
jobs:
  check-version:
    uses: spice-labs-inc/action-check-version/.github/workflows/check-version.yml@main
```
