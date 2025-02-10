<!--
SPDX-License-Identifier: Apache-2.0
SPDX-FileCopyrightText: 2025 The Linux Foundation
-->

# 📦 Project Name Action

Extract the project name for supported project types. Compares the project name
to the repository name and sets a flag when they are identical.

## project-name-action

## Supported Project Types

Supports:

- Python projects using: pyproject.toml, setup.py

## Usage Example

An example workflow step using this action:

```yaml
steps:
    # Code checkout performed in earlier step
    - name: Get project name and compare with repository name
      uses: lfit/releng-reusable-workflows/.github/actions/project-name-action@main
```

## Outputs

<!-- markdownlint-disable MD013 -->

| Output Variable | Mandatory | Value                                                |
| --------------- | --------- | ---------------------------------------------------- |
| PROJECT_NAME    | Yes       | Extracted from files for supported project types     |
| MATCH           | Yes       | Flag set when project name and repository name match |

<!-- markdownlint-enable MD013 -->

## Future Enhancements

Add support for projects of other types, e.g.:

- Maven projects
- Gradle projects
- Go projects
