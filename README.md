# EarlyAI Regression Intelligence

GitHub Action that runs the EarlyAI CLI to process regression jobs.

## Usage

Add a workflow file to your repository (e.g. `.github/workflows/early-regression-intelligence.yml`):

```yaml
name: Early Regression Intelligence

permissions:
  contents: read

on:
  workflow_dispatch:
    inputs:
      job-id:
        description: "Dispatch job identifier (set by backend)"
        required: false
        default: ""

jobs:
  run:
    runs-on: ubuntu-latest
    steps:
      - name: Run EarlyAI
        uses: earlyai/regression-intelligence@v1
        with:
          api-key: ${{ secrets.EARLY_AGENT_API_KEY }}
          job-id: ${{ inputs.job-id }}
```

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `api-key` | Yes | | EarlyAI API key |
| `job-id` | Yes | | Job ID to process. |
| `cli-build` | No | `prod` | `qa` or `prod`. Controls which CLI tag to install. |
| `github-token` | No | | Token for GitHub Packages (`read:packages`). Only needed when `cli-build` is `qa`. |
| `node-options` | No | `--max-old-space-size=5120` | `NODE_OPTIONS` passed to the CLI. |

## Secrets

| Secret | Description |
|--------|-------------|
| `EARLY_AGENT_API_KEY` | EarlyAI API key for the production environment. |

## Versioning

This action uses [release-please](https://github.com/googleapis/release-please) for automated releases. Pin to the major version for automatic minor/patch updates:

```yaml
uses: earlyai/regression-intelligence@v1
```
