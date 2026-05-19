# EarlyAI Regression Intelligence

GitHub Action that runs the EarlyAI CLI to generate code catalogs and impact analyses.

## Usage

Add a workflow file to your repository (e.g. `.github/workflows/early-regression-intelligence.yml`):

```yaml
name: Early Regression Intelligence

permissions:
  contents: read

on:
  workflow_dispatch:
    inputs:
      command:
        description: "catalog or impact"
        required: true
        type: choice
        options:
          - catalog
          - impact
      anchor_branch:
        description: "Baseline branch/tag"
        required: true
      compare_branch:
        description: "Target to analyze (impact only)"
        required: false
        default: ""
      label:
        description: "Display label for the run (impact only)"
        required: false
        default: ""
      catalog-id:
        description: "Existing catalog id to reuse/sync (optional)"
        required: false
        default: ""
      job-id:
        description: "Dispatch job identifier (set by backend)"
        required: false
        default: ""
      project-id:
        description: "Resolved project UUID (debug meta)"
        required: false
        default: ""
      project-root-path:
        description: "Project logical root path, e.g. ./apps/call-graph (debug meta)"
        required: false
        default: ""

jobs:
  run:
    runs-on: ubuntu-latest
    steps:
      - name: Run EarlyAI ${{ inputs.command }}
        uses: earlyai/regression-intelligence@v1
        with:
          command: ${{ inputs.command }}
          api-key: ${{ secrets.EARLY_AGENT_API_KEY }}
          anchor_branch: ${{ inputs.anchor_branch }}
          compare_branch: ${{ inputs.compare_branch }}
          label: ${{ inputs.label }}
          catalog-id: ${{ inputs.catalog-id }}
          job-id: ${{ inputs.job-id }}
          project-id: ${{ inputs.project-id }}
          project-root-path: ${{ inputs.project-root-path }}
```

## Commands

| Command | Description |
|---------|-------------|
| `catalog` | Generates a code catalog for the given branch or tag. |
| `impact` | Analyzes the impact of changes on a branch compared to an anchor (base) branch. |

Both shorthand (`catalog`, `impact`) and long-form (`generate-catalog`, `generate-impact`) values are accepted.

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `command` | Yes | | `catalog` or `impact` |
| `api-key` | Yes | | EarlyAI API key |
| `cli-build` | No | `prod` | `qa` or `prod`. QA installs from GitHub Packages (requires `github-token`); prod installs from public npm. |
| `github-token` | No | `github.token` | Token for GitHub Packages (`read:packages`). Only needed for `qa` builds. |
| `compare_branch` | No | | Branch to analyze. Required for `impact`. |
| `label` | No | | Human-readable label for the run. Used by `impact`. |
| `anchor_branch` | No | | `impact`: base branch to compare against (default: `master`). `catalog`: branch/tag to checkout (default: triggering ref). |
| `catalog-id` | No | | Existing catalog ID to pass to the CLI. |
| `job-id` | No | | Job ID passed to the CLI as `EARLY_JOB_ID`. |
| `project-id` | No | | Project UUID (debug metadata). |
| `project-root-path` | No | | Project logical root path (debug metadata). |
| `base-host` | No | | API base host override. Derived from `cli-build` if omitted. |
| `node-options` | No | `--max-old-space-size=5120` | `NODE_OPTIONS` passed to the CLI. |

## Outputs

| Output | Description |
|--------|-------------|
| `catalog-id` | Catalog ID captured from CLI output when running `catalog`. |

## Secrets

| Secret | Description |
|--------|-------------|
| `EARLY_AGENT_API_KEY` | EarlyAI API key for the production environment. |

## Versioning

This action uses [release-please](https://github.com/googleapis/release-please) for automated releases. Pin to the major version for automatic minor/patch updates:

```yaml
uses: earlyai/regression-intelligence@v1
```
