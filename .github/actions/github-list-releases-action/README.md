<!--
SPDX-License-Identifier: Apache-2.0
SPDX-FileCopyrightText: 2025 The Linux Foundation
-->

# 📦 GitHub List Releases

Lists the GitHub releases for a given repository

## github-list-releases-action

## Usage Example

When provided with a tag/version as input, will check if a release for that
tag/version exists in the repository. Provides the list of releases as an
output variable for other actions/workflows to consume.

```yaml
steps:
  - name: "Check if tag/version already released"
    id: release-check
    # yamllint disable-line rule:line-length
    uses: lfit/releng-reusable-workflows/.github/actions/github-list-releases-action@main
    with:
      tag: "v0.1.23"
```

## Inputs

<!-- markdownlint-disable MD013 -->

| Variable Name   | Required | Default   | Description                                  |
| --------------- | -------- | --------- | -------------------------------------------- |
| TAG             | False    | N/A       | Checks if this tag/version has released      |
| SUMMARY_OUTPUT  | False    | N/A       | Display summary output                       |
| PRODUCTION_ONLY | False    | See Below | Do not list pre-release or draft releases    |
| RETURN_TYPE     | False    | text      | Return type set to either: text/json         |
| JSON_FIELDS     | False    | False     | Fields to return as JSON                     |
| ORDER           | False    | False     | Sort order set to either: asc/desc           |

<!-- markdownlint-enable MD013 -->

Note: Specification of JSON fields is mandatory when JSON return type requested

## Outputs

<!-- markdownlint-disable MD013 -->

| Variable Name | Mandatory | Description                                    |
| ------------- | --------- | ---------------------------------------------- |
| RELEASES      | True      | List of releases as either text or JSON        |
| RELEASED      | False     | Set true when tag/version has already released |

<!-- markdownlint-enable MD013 -->
