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

## Example

When providing the example inputs above to the action, the checked URL would be:

`https://pypi.org/my-project/1.0.1`

The full wget command in the shell:

`wget https://pypi.org/my-project/1.0.1 --spider -q`

## Inputs

<!-- markdownlint-disable MD013 -->

| Variable Name | Required | Description                       | Example            |
| ------------- | -------- | --------------------------------- | ------------------ |
| URL           | True     | Primary URL to check              | <https://pypi.org> |
| STRING        | False    | Intermediate string to add to URL | /my-project        |
| SUFFIX        | False    | Suffix to add to URL              | /1.0.1             |

<!-- markdownlint-enable MD013 -->

## Outputs

<!-- markdownlint-disable MD013 -->

| Variable Name | Mandatory | Description                                 |
| ------------- | --------- | ------------------------------------------- |
| VALID         | True      | Set true or false based on WGET exit status |

<!-- markdownlint-enable MD013 -->
