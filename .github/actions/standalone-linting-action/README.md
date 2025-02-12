<!--
SPDX-License-Identifier: Apache-2.0
SPDX-FileCopyrightText: 2024 The Linux Foundation
-->

# ⛔️ Standalone Linting Action

This action runs linting checks as a standalone step, which is helpful
for tools that do not run well (or cannot run) under the GitHub marketplace
pre-commit.ci application.

## standalone-linting-action

## Usage Example

An example workflow step using this action:

```yaml
jobs:
  linting:
    name: "Standalone linting checks"
    runs-on: "ubuntu-24.04"
    permissions:
      contents: read
    steps:
      # yamllint disable-line rule:line-length
      - uses: lfit/releng-reusable-workflows/.github/actions/standalone-linting-action@main
```

## Inputs

<!-- markdownlint-disable MD013 -->

| Variable Name    | Required | Description                                                    | Default                                                                                                                                                                     |
| ---------------- | -------- | -------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CONFIG_URL       | False    | Download location for pre-commit configuration                 | [pre-commit-config.yaml](https://raw.githubusercontent.com/lfit/releng-reusable-workflows/refs/heads/main/.github/actions/standalone-linting-action/pre-commit-config.yaml) |
| DEPENDENCIES_URL | False    | Download location for Python dependencies                      | None                                                                                                                                                                        |
| REMOTE_CONFIG    | False    | Ignore local linting configuration; always install from remote | true                                                                                                                                                                        |
| PYTHON-VERSION   | False    | Python version used to run linting tools                       | 3.12                                                                                                                                                                        |

<!-- markdownlint-enable MD013 -->
