# grype-scan-repo-action
A github action to scan a repository with grype.  This uses the dockerized version of the [grype_scan_repo](https://github.com/jhu-library-operations/grype_scan_repo) script that we created.  It runs a a github action and will fail if vulnerablilities are round.  This can allow for a discussion about if they can be an accepted risk.  The PR can still be merged with failed checks, so this can be overriden.

## Inputs
### grype-options
**Optional** The options to pass to grype.  For the severity, because of quote passing and such, do not quote the severity filter.  It may fail to catch things if it winds up quoted.

## Outputs
**None**

## Example usage

```
name: Scan repo for security flaws in container images.
on: [pull_request]
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
        with: 
          grype-options: "-o -s (Critical|High) -k"
```
