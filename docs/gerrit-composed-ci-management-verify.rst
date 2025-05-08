.. _gerrit-composed-ci-management-verify-docs:

#############################################
Gerrit Composed ci-management Verify Workflow
#############################################


The Compose ci-management Verify Workflow handles specific verification tasks for Gerrit changes,
including repository linting, Jenkins Job Builder (JJB) verification, Packer verification, and voting on the change.

1. Clears any existing votes on the Gerrit change.
2. Runs repository linting to ensure the code follows the required standards.
3. Verifies Jenkins Job Builder configurations.
4. Verifies the Packer configurations.
5. Votes on the Gerrit change based on the results of the verification steps.

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
    :ENV_VARS: GitHub variables passed as environment variables during
      the workflow execution.
        Default: "{}" (Optional)

:Required secrets:

    :secrets.ENV_SECRETS: GitHub secrets passed as environment variables
      during the workflow execution.
    :secrets.GERRIT_SSH_PRIVKEY: SSH key for the authorized user account to
      access Gerrit.
    :secrets.CLOUDS_ENV_2XB64: Base64-encoded Packer cloud environment credentials.
    :secrets.CLOUDS_YAML_2XB64: Base64-encoded Openstack cloud environment credentials.

:Steps in the workflow:

1. **Clear Votes**: The workflow starts by clearing any existing votes on the Gerrit
   change using the `gerrit-review-action`.

2. **Repository Linting**: The workflow runs repository linting using the
   `compose-repo-linting` reusable workflow. This step ensures that the repository
   adheres to the required code standards and guidelines.

3. **JJB Verification**: The workflow runs Jenkins Job Builder (JJB) verification
   using the `compose-jjb-verify` reusable workflow. This step verifies the correctness
   of the Jenkins Job Builder configuration.

4. **Packer Verification**: The workflow runs Packer verification using the
   `compose-packer-verify` reusable workflow. This ensures that the Packer configurations
   are valid and ready for deployment.

5. **Voting**: Once all the verification steps are complete, the workflow votes
   on the Gerrit change based on the outcome of the verification steps. The vote is
   either positive or negative, depending on the results.

:Example usage:

    - Trigger this workflow manually when a Gerrit change requires repository
      linting, JJB verification, and Packer verification.
    - The workflow clears existing votes, runs the necessary verification steps,
      and votes on the change based on the results.

:Comment Trigger: recheck|reverify

:Workflow Steps Summary:

    1. Clear any existing votes on the Gerrit change.
    2. Run repository linting to ensure the code meets the required standards.
    3. Verify Jenkins Job Builder configurations.
    4. Verify Packer configurations.
    5. Vote on the Gerrit change based on the results.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
