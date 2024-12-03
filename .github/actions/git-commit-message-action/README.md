# 💬 GIT Commit Message

Action that returns the last/current commit message to a calling GitHub
action/workflow.

## git-commit-message-action

## Usage Example

Sets the account name/email used for Git operations inside other
actions/workflows.

```yaml
- name: "Configure GIT user for commit"
  uses: lfit/releng-reusable-workflows/.github/actions/git-configure-action@main
  with:
      commit-user-name: "John Doe"
      commit-user-email: "john.doe@somedomain.org"
```

## Inputs

This action takes/requires no explicit inputs.

## Outputs

| Variable Name  | Description                                        |
| -------------- | -------------------------------------------------- |
| COMMIT_MESSAGE | String of the last/current Git commit message body |

The action provides summary step output when invoked, displaying the returned
commit message body. It also outputs the string value explicitly, and as an
environment export to the calling action/workflow.

## Requirements/Dependencies

Perform a repository checkout step before using the action.

### Command used

The command used to capture the message is:

```console
git show -s --format='%B'
```

<!--
[comment]: # SPDX-License-Identifier: Apache-2.0
[comment]: # SPDX-FileCopyrightText: 2024 The Linux Foundation
-->
