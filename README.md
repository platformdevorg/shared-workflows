# Reusable Workflows 

Contains reusable workflows that can be shared to an organization or other repositories to consume. 
 

## Example Python Lint, Test, Build

```yaml
---
name: PR Python Workflow
run-name: PR Python Workflow by @${{ github.actor }}

on:
  workflow_dispatch:
  pull_request:
    branches:
      - main
    paths:
      - 'src/*'
      - 'tests/*'
      - 'pyproject.toml'
      - 'setup.py'

jobs:
  pre_build:
    name: Pylint & Pytest
    uses: platformdevorg/shared-workflows/.github/workflows/py-lint-test.yml@v0.0.1
    permissions:
      contents: read
      issues: read
      checks: write
      pull-requests: write
    with:
      python_version: 3.8

  build:
    name: Python Build
    needs:
      - pre_build
    uses: platformdevorg/shared-workflows/.github/workflows/py-build.yml@v0.0.1
    permissions:
      contents: read
    with:
      release_version: ${{ github.event.pull_request.head.sha }}
      package_name: ${{ vars.PACKAGE_NAME }}
      python_version: 3.8

```