### cargo-version-check
Checks GitHib Release tag matches Cargo.toml version number and fails if they mismatch

#### Check Cargo Version Matches Tag
This GitHub Action ensures that the Git tag (e.g. `v1.2.3`) matches the version in `Cargo.toml`.

#### Usage
```yaml
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Check Cargo version matches tag
        uses: spice-labs-inc/actions/cargo_version_check@main
```
#### Jobs Section in Workflow for Publishing Images
```yaml
jobs:   
  check-version:
    if: startsWith(github.ref, 'refs/tags/v')  # Run only on tag pushes
    name: Ensure GitHub Release & Cargo.toml Version Numbers Match
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Check Cargo version matches tag
        uses: spice-labs-inc/actions/cargo_version_check@main

  push_to_registry:
    name: Push Docker image to Docker Hub and GHCR
    runs-on: ubuntu-latest
    needs: check-version
```
Rest of workflow ...
