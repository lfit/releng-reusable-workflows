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
steps:
    - name: Linting action
      # yamllint disable-line rule:line-length
      uses: lfit/releng-reusable-workflows/.github/actions/standalone-linting-action@main
```

An example reusable workflow implementation:

```yaml
---
# SPDX-License-Identifier: Apache-2.0
# SPDX-FileCopyrightText: 2024 The Linux Foundation

name: "⛔️ [R] Standalone Linting"

# yamllint disable-line rule:truthy
on:
    workflow_call:

permissions: {}

jobs:
    linting:
        name: "Standalone linting check"
        runs-on: "ubuntu-24.04"
        permissions:
            contents: read
        steps:
            - name: Linting action
              # yamllint disable-line rule:line-length
              uses: lfit/releng-reusable-workflows/.github/actions/standalone-linting-action@main
```

## Inputs

<!-- markdownlint-disable MD013 -->

| Variable Name    | Required | Default                                                                                                                                                                     |
| ---------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CONFIG_URL       | False    | [pre-commit-config.yaml](https://raw.githubusercontent.com/lfit/releng-reusable-workflows/refs/heads/main/.github/actions/standalone-linting-action/pre-commit-config.yaml) |
| DEPENDENCIES_URL | False    | [requirements.txt](https://raw.githubusercontent.com/lfit/releng-reusable-workflow/refs/heads/main/linting/requirements.txt)                                                |

<!-- markdownlint-enable MD013 -->
