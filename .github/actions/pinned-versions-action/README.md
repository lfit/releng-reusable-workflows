<!--
[comment]: # SPDX-License-Identifier: Apache-2.0
[comment]: # SPDX-FileCopyrightText: 2024 The Linux Foundation
-->

# 📌 Pinned Versions Action

Verifies action/workflow calls use SHA commit values

## pinned-versions-action

## Recommended Event Triggers

```yaml
on:
    workflow_dispatch:
    pull_request:
        branches:
            - main
            - master
        paths: [".github/**"]
```

## Usage Example

<!-- markdownlint-disable MD013 -->

```yaml
jobs:
    check-actions:
        name: Pinned Versions
        runs-on: ubuntu-24.04
        permissions:
            contents: read
        steps:
            - name: Check Pinned Versions
              # yamllint disable-line rule:line-length
              uses: os-climate/osc-github-devops/.github/actions/pinned-versions-action@ea8bbd5f4f817abe64b2498e0f1393ca15b86c0e # v1.0.0
```

<!-- markdownlint-enable MD013 -->

## Behaviour

### Pull Requests

When triggered against a pull request, will audit the change content for any
calls to GitHub actions/workflows that are not pinned to a SHA commit value.
This scans files changed in the pull request, and will NOT block merges
where GitHub actions elsewhere in the repository do not use SHA/commit values.

### Manual Invocation

Operates differently when explicitly called using "workflow_dispatch" trigger.
Will scan the entire repository for action/workflow calls and report results
for all GitHub actions/workflows found.
