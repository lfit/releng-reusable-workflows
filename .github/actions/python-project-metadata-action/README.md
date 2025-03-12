<!--
# SPDX-License-Identifier: Apache-2.0
# SPDX-FileCopyrightText: 2024 The Linux Foundation
-->

# 🛠️ Python Project Metadata

Extracts Python project metadata from a repository.

## python-project-metadata-action

## Usage Example

<!-- markdownlint-disable MD013 -->

```yaml
  - name: "Get Python project metadata"
    uses: lfit/releng-reusable-workflows/.github/actions/python-project-metadata-action@main
```

<!-- markdownlint-enable MD013 -->

## Outputs

<!-- markdownlint-disable MD013 -->

| Variable Name         | Description                                              |
| --------------------- | -------------------------------------------------------- |
| PYTHON_PROJECT_FILE   | File used to define/describe the project                 |
| PYTHON_PROJECT_NAME   | The name of the Python project                           |
| PYTHON_PACKAGE_NAME   | The name of the Python package                           |
| PROJECT_MATCH_PACKAGE | Set true when the project and package name match         |
| PROJECT_MATCH_REPO    | Set true when the project name and repository name match |
| VERSIONING_TYPE       | Can be either static or dynamic (determined by tags)     |
| BUILD_PYTHON          | Most recent Python version supported by project          |
| MATRIX_JSON           | All Python versions supported by project as JSON string  |

<!-- markdownlint-enable MD013 -->

Python package names can be different from project names.

Note: dashes in the project name typically get replaced with underscores
