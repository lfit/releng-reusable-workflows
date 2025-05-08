.. _gerrit-ci-management-verify-docs:

####################################
Gerrit ci-management Verify Workflow
####################################

The ci-management Verify Workflow verifies ci-management change with the Jenkins Job Builder (JJB) verify to ensure they meet quality standards before merging.

The workflow performs the following tasks:

1. Notifies the job start and clears any existing votes in Gerrit.
2. Runs the following verifications on the Gerrit change:
    - **Actionlint**: Validates the GitHub Actions workflow files.
    - **Pre-commit**: Runs static analysis and format checkers via `pre-commit` hooks.
    - **Jenkins Job Builder (JJB)**: Verifies the correctness of Jenkins job configurations.
3. Reports the workflow result back to Gerrit.

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

    :secrets.GERRIT_SSH_PRIVKEY: The SSH private key for the authorized user account to interact with Gerrit.

:Steps in the workflow:

1. **Notify Job Start**: The workflow clears any previous votes and notifies the start
   of the job on the Gerrit change.

2. **Actionlint Verification**: The workflow checks the validity of workflow files
   using `actionlint`, ensuring there are no issues in the GitHub Actions configuration.

3. **Pre-commit Hooks**: The workflow runs `pre-commit` hooks for static analysis and
   format checkers, helping identify any issues in the code or formatting.

4. **Jenkins Job Builder Verification**: The workflow verifies the correctness of the
   Jenkins job configurations using `jenkins-job-builder`, ensuring that any changes to
   the Jenkins configuration are valid.

5. **Workflow Conclusion**: The workflow concludes and reports the result
   back to Gerrit, updating the change status with the success or failure of the workflow.

:Example usage:

    - Trigger this workflow manually when a Gerrit change needs to undergo a series
      of validation steps, including GitHub Actions workflow linting, code analysis
      with `pre-commit`, and Jenkins Job Builder configuration checks.

    - The workflow validates the change, provides feedback, and reports the status
      to Gerrit for further action.

:Comment Trigger: ``recheck|reverify``

:Workflow Steps Summary:

    1. Notify the job start and clear any previous votes in Gerrit.
    2. Verify the GitHub Actions workflow using `actionlint`.
    3. Run `pre-commit` hooks for static analysis and formatting checks.
    4. Verify Jenkins job configurations using `jenkins-job-builder`.
    5. Report the workflow conclusion back to Gerrit.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
