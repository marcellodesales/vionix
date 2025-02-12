# 🗃️ load-mapped-repos-to-matrix action

Loads the list of github org-repo from a text file to be used in a matrix

# Use Cases

## Sync mirrored Github Actions from Upstream Github.com

Just provide the inputs for a job

```yaml
  repos:
    name: 🐙 repos
    runs-on: vionix
    outputs:
      mirroredReposMatrix: ${{ steps.repos-hash.outputs.mirroredReposMatrix }}
    steps:
      - name: Load the mirrored repos mapping
        id: repos-hash
        uses: seceng-devsecops-platform/devsecops-platform-github-workflows/actions/custom/load-mapped-repos-to-matrix@feature/SSEE-18202/add-workflow-sync-org-actions
        with:
          ### File containing the mapping between Viasat org/repo = Github upstream
          ### Example: seceng-devsecops-platform/docker-login-action-test=https://github.com/docker/login-action
          ###
          ### NOTE: If your mirrored action org is listed at https://git.viasat.com/seceng-devsecops-platform/sync-mirrored-org-actions/settings/actions
          ###       then you can consume the action directly, unless it requires to be patched to avoid Github API limits. 
          mirrored-repos-map-file: repos.txt
```

# Outputs

## mirroredReposMatrix

* The matrix to be used in a matrix job

```yaml
  github-repo-sync:
    name:  🏗️ git
    needs: [repos]
    strategy:
      fail-fast: false
      matrix: ${{ fromJSON(needs.repos.outputs.mirroredReposMatrix) }}
    uses: seceng-devsecops-platform/devsecops-platform-github-workflows/.github/workflows/github-sync-repository-workflow.yaml@feature/SSEE-18202/add-workflow-sync-org-actions
```

