.. _gerrit-compose-ci-management-verify-docs:

############################################
Gerrit Compose ci-management Verify Workflow
############################################

The Compose ci-management Verify Workflow is a group of jobs required to verify a ci-management change.
It verifies changes in a Gerrit repository through checks to ensure the change meets required standards before merging.

The workflow performs the following tasks:

1. **Notify Job Start**: Clears any previous votes on the Gerrit change and notifies the start of the job.
2. **Actionlint**: Validates the GitHub Actions workflow files.
3. **Pre-commit**: Runs static analysis and format checkers using pre-commit hooks.
4. **Jenkins Job Builder (JJB)**: Verifies the correctness of Jenkins job configurations.
5. **Report Status**: The workflow reports the result of the checks to Gerrit.

:Required parameters:

    :GERRIT_BRANCH: The branch that the change targets.
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

:Required secrets:

    :secrets.GERRIT_SSH_PRIVKEY: The SSH private key for the authorized user account used to interact with Gerrit.

:Steps in the workflow:

1. **Notify Job Start**: The workflow clears any existing votes and notifies the start of the job on the Gerrit change.

2. **Actionlint Verification**: The workflow checks the validity of the GitHub Actions workflow files using `actionlint`, ensuring no issues in the configuration.

3. **Pre-commit Hooks**: The workflow runs `pre-commit` hooks to perform static analysis and check for formatting issues in the code.

4. **Jenkins Job Builder Verification**: The workflow verifies the correctness of Jenkins job configurations using `jenkins-job-builder`, ensuring that the configuration changes are valid.

5. **Workflow Conclusion**: The workflow concludes and reports the result back to Gerrit, updating the change status with the success or failure of the workflow.

:Example usage:

    - Trigger this workflow manually when a change in Gerrit requires validation. The workflow performs checks, including GitHub Actions workflow linting, code analysis with `pre-commit`, and Jenkins Job Builder configuration checks.

    - The workflow assesses the change and provides feedback to Gerrit.

:Comment Trigger: recheck|reverify

:Workflow Steps Summary:

    1. Notify the job start and clear any previous votes in Gerrit.
    2. Check GitHub Actions workflows with `actionlint`.
    3. Run `pre-commit` hooks for static analysis and formatting checks.
    4. Verify Jenkins job configurations using `jenkins-job-builder`.
    5. Report the workflow conclusion back to Gerrit.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
