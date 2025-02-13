<!--
# SPDX-License-Identifier: Apache-2.0
# SPDX-FileCopyrightText: 2025 The Linux Foundation
-->

# 🌍 Check URL for Validity with WGET

Checks if a remote web server returns a valid page for a given URL.

## Usage Example

<!-- markdownlint-disable MD013 -->

```yaml
steps:
    - name: "Check remote web server for URL"
      id: url-to-check
      uses: lfit/releng-reusable-workflows/.github/actions/url-validity-check-action@main
      with:
          url: "https://linuxfoundation.org"
```

<!-- markdownlint-enable MD013 -->

## Implementation

Uses the shell command:

`wget [URL] --spider -q`

## Inputs

<!-- markdownlint-disable MD013 -->

| Input | Required | Description  | Example                     |
| ----- | -------- | ------------ | --------------------------- |
| URL   | True     | URL to check | <https://linuxfoundation.org> |

<!-- markdownlint-enable MD013 -->

## Outputs

<!-- markdownlint-disable MD013 -->

| Variable Name | Mandatory | Description                                 |
| ------------- | --------- | ------------------------------------------- |
| VALID         | True      | Set true or false based on WGET exit status |

<!-- markdownlint-enable MD013 -->
