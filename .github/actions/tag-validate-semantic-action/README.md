<!--
SPDX-License-Identifier: Apache-2.0
SPDX-FileCopyrightText: 2025 The Linux Foundation
-->

# 🏷️ Check Tag/Version String is Semantic

Checks a given tag/version string for compliance with semantic versioning.

Refer to the web site below:

[https://semver.org/](https://semver.org/)

## tag-validate-semantic-action

## Usage Example

Pass the string to check as input to the action:

```yaml
steps:
  - name: "Check pushed tag is semantic"
    if: startsWith(github.ref, 'refs/tags/')
    id: tag-validate
    # yamllint disable-line rule:line-length
    uses: lfit/releng-reusable-workflows/.github/actions/tag-validate-semantic-action@main
    with:
      tag: ${{ github.ref_name }}
```

## Inputs

<!-- markdownlint-disable MD013 -->

| Variable Name | Required | Default   | Description                              |
| ------------- | -------- | --------- | ---------------------------------------- |
| TAG           | True     | N/A       | Tag/version string to check for validity |

<!-- markdownlint-enable MD013 -->

## Outputs

<!-- markdownlint-disable MD013 -->

| Variable Name | Mandatory | Description                           |
| ------------- | --------- | ------------------------------------- |
| SEMANTIC      | True      | Set to either true/false if semantic  |

<!-- markdownlint-enable MD013 -->
