<!--
# SPDX-License-Identifier: Apache-2.0
# SPDX-FileCopyrightText: 2025 The Linux Foundation
-->

# #️⃣ GitHub Pull Request Metadata

For a specified pull request, provides a count of the commits it contains.
If no pull request specified, will attempt to identify the current pull
request number and will provide it as one of the action's outputs.

## github-pull-request-metadata-action

## Usage Example

<!-- markdownlint-disable MD013 -->

```yaml
steps:
  - name: "Get number of commits in PR"
    uses: lfit/releng-reusable-workflows/.github/actions/github-pull-request-metadata-action@main
    with:
      pull_request: 13
```

<!-- markdownlint-enable MD013 -->

## Implementation

Uses the GitHub CLI command below:

`gh pr view [Pull Request Number]`

Passing the output through JQ to provide a numerical value:

`gh pr view [Pull Request Number] --json commits | jq '.[] | length')`

## Inputs

<!-- markdownlint-disable MD013 -->

| Input        | Required | Default    | Description                     |
| ------------ | -------- | ---------- | ------------------------------- |
| PULL_REQUEST | False    | Enumerated | Pull request to query           |
| EXIT_ON_FAIL | False    | True       | Do not exit with error when set |

<!-- markdownlint-enable MD013 -->

In the absence of a specific pull request to query, will attempt to source the
current pull request from the GitHub workflow/action environment using:

`${{ github.event.pull_request.number }}`

If this doesn't provide a numerical value, the action will fail with an error.

## Outputs

<!-- markdownlint-disable MD013 -->

| Output       | Mandatory | Description                                                     |
| ------------ | --------- | --------------------------------------------------------------- |
| PULL_REQUEST | False     | Enumerated value when not supplied, otherwise matches the input |
| PR_COMMITS   | True      | The current number of commits in the pull request               |

<!-- markdownlint-enable MD013 -->
