<!--
SPDX-License-Identifier: Apache-2.0
SPDX-FileCopyrightText: 2024 The Linux Foundation
-->

# 🛠️ Configure GIT User

Action to configure the Git username/email inside a GitHub action/workflow

## git-configure-action

## Usage Example

```yaml
- name: "Configure GIT user for commit"
  uses: lfit/releng-reusable-workflows/.github/actions/git-configure-action@main
  with:
      commit-user-name: "John Doe"
      commit-user-email: "john.doe@somedomain.org"
```

## Inputs

By default, the action will configure Git operations to use the Github actions
automation/bot account.

<!-- markdownlint-disable MD013 -->

| Variable Name     | Required | Default Value                                         | Description        |
| ----------------- | -------- | ----------------------------------------------------- | ------------------ |
| COMMIT_USER_NAME  | False    | github-actions[bot]                                   | User name          |
| COMMIT_USER_EMAIL | False    | 41898282+github-actions[bot]@users.noreply.github.com | User email address |

<!-- markdownlint-enable MD013 -->

## Outputs

The action has no outputs, but does provide summary step information when
invoked.

## Requirements/Dependencies

The git command-line tool must be available in the environment for the action
to succeed.
