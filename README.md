# CI Context Detector

A GitHub Action that detects the git context for CI workflows and exposes normalized references.

## Overview

This action downloads and runs the `ci-context` CLI to determine the base and head references for the current change set. It is useful for workflows that need to inspect commits introduced by a pull request, a push, or other git events.

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `version` | Version of ci-context to download and run | no | `v0.1.0` |
| `base_ref` | Forced base reference. When empty, ci-context auto-detects the base | no | `""` |
| `since` | Minimum reference commit; base_ref will never be older than this commit | no | `""` |

## Outputs

| Output | Description |
|--------|-------------|
| `base_ref` | Base reference |
| `head_ref` | HEAD reference |
| `commit_count` | Number of commits in the change range |

## Usage

```yaml
name: Detect CI Context

on:
  pull_request:
  push:

jobs:
  detect:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7

      - name: Detect context
        uses: juli3nk/ci-context-action@main
        id: ci_context

      - name: Print context
        run: |
          echo "base_ref: ${{ steps.ci_context.outputs.base_ref }}"
          echo "head_ref: ${{ steps.ci_context.outputs.head_ref }}"
          echo "commit_count: ${{ steps.ci_context.outputs.commit_count }}"
```

## Requirements

- Linux runners (`x86_64` or `aarch64`/`arm64`)
- A published `ci-context` release matching the requested version on the [juli3nk/ci-context](https://github.com/juli3nk/ci-context) repository

## License

See [LICENSE](./LICENSE).
