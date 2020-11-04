# grype-action
A github action to scan a repository with grype

## Inputs
### grype-options
**Optional** The options to pass to grype.

## Outputs
**None**

## Example usage

```
name: Scan repo for security flaws in container images.
on:
  release:
    types: [published]
jobs:
  scan-repository:
    name: Scan the repository with the Grype Scan Repo action.
    runs-on: ubuntu-latest
    steps:
      - name: Login to GHCR
        id: ghcr_login
        uses: docker/login-action@v1
        with:
            registry: ghcr.io
            username: ${{ secrets.DEREK_GHCR_UNAME }}
            password: ${{ secrets.DEREK_GHCR_PAT }}

      - name: Check out the repo
        uses: actions/checkout@v2

      - name: Do the Scan
        uses: jhu-library-operations/grype-action
        with: -o -s '(Critical|High)' -k
```