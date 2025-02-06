<!--
SPDX-License-Identifier: Apache-2.0
SPDX-FileCopyrightText: 2024 The Linux Foundation
-->

# 💬 GIT Commit Message

Action that returns the last/current commit message to a calling GitHub
action/workflow.

## git-commit-message-action

## Usage Example

Sets the account name/email used for Git operations inside other
actions/workflows.

```yaml
- name: "Retrieve GIT commit message"
  uses: lfit/releng-reusable-workflows/.github/actions/git-commit-message-action@main
```

## Inputs

This action takes/requires no explicit inputs.

## Outputs

| Variable Name  | Description                                        |
| -------------- | -------------------------------------------------- |
| COMMIT_MESSAGE | String of the last/current Git commit message body |

The action will exit with an error if the extracted message string is empty.
The action produces minimal output, but does write the value received in the
step output. The commit message string is both exported to the environment
and written explicitly as output from the action.

## Requirements/Dependencies

Perform a repository checkout step before using the action.

### Internal Implementation

The command used to capture the message is:

```console
git show -s --format='%B'
```
