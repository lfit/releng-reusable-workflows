<!--
# SPDX-License-Identifier: Apache-2.0
# SPDX-FileCopyrightText: 2024 The Linux Foundation
-->

# 🛠️ Repository Metadata

Gathers metadata about the Github repository for use in other actions.

Also prints some useful GitHub environment variables to the console.

## repository-metadata-action

## Usage Example

<!-- markdownlint-disable MD013 -->

```yaml
steps:
    - name: "Repository metadata"
      id: repository-metadata
      # yamllint disable-line rule:line-length
      uses: lfit/releng-reusable-workflows/.github/actions/repository-metadata-action@main
```

<!-- markdownlint-enable MD013 -->

## Outputs

<!-- markdownlint-disable MD013 -->

| Variable Name    | Description                     |
| ---------------- | ------------------------------- |
| REPOSITORY_OWNER | Name of the GitHub organization |
| REPOSITORY_NAME  | Name of the GitHub repository   |

<!-- markdownlint-enable MD013 -->
