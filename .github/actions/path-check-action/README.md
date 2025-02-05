<!--
# SPDX-License-Identifier: Apache-2.0
# SPDX-FileCopyrightText: 2024 The Linux Foundation
-->

# ✅ Check path exists in repository

Check if a given path exists in the repository; reports type as either:

    file
    directory
    invalid

Also, sets a flag to true or false if the given path is a symbolic link.

## Usage Example

<!-- markdownlint-disable MD046 -->

```yaml
steps:
    - name: "Test repository path: pyproject.toml"
      id: path-to-check
      uses: lfit/releng-reusable-workflows/.github/actions/path-check-action@main
      with:
          path: "pyproject.toml"
```

<!-- markdownlint-enable MD046 -->

## Inputs

<!-- markdownlint-disable MD013 -->

| Variable Name | Required | Description                       |
| ------------- | -------- | --------------------------------- |
| PATH          | True     | Path to check in local repository |

<!-- markdownlint-enable MD013 -->

## Outputs

<!-- markdownlint-disable MD013 -->

| Variable Name | Mandatory | Description                        |
| ------------- | --------- | ---------------------------------- |
| TYPE          | True      | Either file, directory, or invalid |
| SYMLINK       | True      | Set to either: true or false       |

<!-- markdownlint-enable MD013 -->
