<!--
SPDX-License-Identifier: Apache-2.0
SPDX-FileCopyrightText: 2024 The Linux Foundation
-->

# 💬 GIT Commit Message

Returns the commit SHA and message for a given (numbered) commit. Defaults to
the latest commit if no commit number supplied as input. Also checks the commit
message body for a Gerrit Change-Id and DCO line.

## git-commit-message-action

## Usage Example

Sets the account name/email used for Git operations inside other
actions/workflows.

```yaml
- name: "Retrieve GIT commit message"
  uses: lfit/releng-reusable-workflows/.github/actions/git-commit-message-action@main
  with:
    commit_number: 2
```

## Inputs

<!-- markdownlint-disable MD013 -->

| Variable Name  | Mandatory  | Default | Description                                        |
| -------------- | -----------| ------- | -------------------------------------------------- |
| COMMIT_NUMBER  | False      | 1       | String of the last/current Git commit message body |

<!-- markdownlint-enable MD013 -->

## Outputs

<!-- markdownlint-disable MD013 -->

| Variable Name   | Description                                            |
| --------------- | ------------------------------------------------------ |
| COMMIT_SHA      | String of the last/current Git commit message body     |
| COMMIT_MESSAGE  | String of the last/current Git commit message body     |
| CHANGE_ID       | Whether the commit message contains a Gerrit Change-Id |
| CHANGE_ID_VALUE | When Change-Id present, returns the string/value       |
| DCO_SIGNED_OFF  | Whether the commit message body contains a DCO line    |
| DCO_NAME        | Name provided in DCO statement, if present             |
| DCO_EMAIL       | E-mail address from DCO statement, if present          |

<!-- markdownlint-enable MD013 -->

The action will provide a warning if the extracted message string is empty.

## Requirements/Dependencies

Perform a repository checkout before using the action.

The fetch depth must cover the range that includes the requested commit number.

### Internal Implementation

The commands used to capture the SHA and message are:

```console
COMMIT_SHA=$(git log --pretty=format:"%H" | awk "NR==$COMMIT_NUMBER")
COMMIT_MESSAGE=$(git log --format=%B -n 1 "$COMMIT_SHA")
```
