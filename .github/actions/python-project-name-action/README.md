<!--
SPDX-License-Identifier: Apache-2.0
SPDX-FileCopyrightText: 2024 The Linux Foundation
-->

# 🐍 Python Project/Package Names

Extracts the Python project name and derives the Python package name.

## python-project-name-action

## Usage Example

An example workflow step using this action:

```yaml
steps:
    # Code checkout performed in earlier step
    - name: Check for Python Project
      uses: lfit/releng-reusable-workflows/.github/actions/python-project-name-action@main
```

## Outputs

<!-- markdownlint-disable MD013 -->

| Output Variable     | Mandatory | Value                                                     |
| ------------------- | --------- | --------------------------------------------------------- |
| PYTHON_PACKAGE_FILE | Yes       | File used to extract metadata: pyproject.toml or setup.py |
| PYTHON_PROJECT_NAME | Yes       | Extracted from pyproject.toml/setup.py                    |
| PYTHON_PACKAGE_NAME | Yes       | Derived from value in pyproject.toml/setup.py             |

<!-- markdownlint-enable MD013 -->
