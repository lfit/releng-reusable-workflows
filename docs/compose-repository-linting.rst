.. _compose-repo-linting-docs:

###################################
Compose Repository Linting Workflow
###################################

This workflow runs when you submit a change request to Gerrit. It lints the repository by running linting tools such as `actionlint` and `pre-commit`. The workflow performs the following tasks:

1. Checks out the code from the specified Gerrit branch.
2. Runs `actionlint` to lint GitHub Actions workflow files.
3. Executes `pre-commit` hooks for static analysis and format checks.

:Required parameters:

    :GERRIT_BRANCH: The branch for the change.
        Default: None (Required)
    :GERRIT_CHANGE_ID: The unique identifier for the change.
        Default: None (Required)
    :GERRIT_CHANGE_NUMBER: The Gerrit number for the change.
        Default: None (Required)
    :GERRIT_CHANGE_URL: The URL to the change.
        Default: None (Required)
    :GERRIT_EVENT_TYPE: The Gerrit event triggering the workflow.
        Default: None (Required)
    :GERRIT_PATCHSET_NUMBER: The patch number for the change.
        Default: None (Required)
    :GERRIT_PATCHSET_REVISION: The revision SHA for the patchset.
        Default: None (Required)
    :GERRIT_PROJECT: The project in Gerrit for the change.
        Default: None (Required)
    :GERRIT_REFSPEC: The refspec of the change in Gerrit.
        Default: None (Required)
    :pre_commit_skips: Comma-separated list of pre-commit validators to skip,
      defaults to `actionlint`.
        Default: "actionlint" (Optional)

:Steps in the workflow:

1. **Gerrit Checkout**: The workflow begins by checking out the code from the
   specified Gerrit branch, including the changes needing linting.

2. **Download actionlint**: Download `actionlint` to lint the workflow files in the repository, checking for common issues in GitHub Actions workflows.

3. **Run actionlint**: Execute `actionlint` to ensure all GitHub Actions workflow files follow the correct syntax and structure.

4. **Run pre-commit Hooks**: In parallel, run `pre-commit` hooks for static analysis and format checks on the codebase. Control specific checks using the `pre_commit_skips` input to skip any pre-commit hooks if necessary.

:Expected environment variables:

    :vars.GERRIT_URL: The base URL for Gerrit.

:Example usage:

    - Trigger this workflow manually when you change the repository and need linting.
    - The workflow checks GitHub Actions workflow files with `actionlint` and runs static analysis/format checks using `pre-commit`.

:Comment Trigger: recheck|reverify

:Workflow Steps Summary:

    1. Checkout the Gerrit change to get the latest code.
    2. Download and run `actionlint` to check the workflow files.
    3. Run `pre-commit` hooks for static analysis and format issues.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
