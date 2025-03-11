<!--
SPDX-License-Identifier: Apache-2.0
SPDX-FileCopyrightText: 2024 The Linux Foundation
-->

# Check Python Project for Dynamic Versioning

Parses pyproject.toml to check if dynamic versioning enabled.

## python-dynamic-version-action

## Usage Example

```yaml
- name: "Check for dynamic project versioning"
  uses: lfit/releng-reusable-workflows/.github/actions/python-dynamic-version-action@main
```

## Outputs

<!-- markdownlint-disable MD013 -->

| Variable Name   | Description                              |
| --------------- | ---------------------------------------- |
| DYNAMIC_VERSION | Set true when dynamic versioning enabled |

<!-- markdownlint-enable MD013 -->
