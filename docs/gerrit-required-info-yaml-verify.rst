.. _gerrit-required-info-yaml-verify-docs:

#########################################
Gerrit Required INFO Yaml Verify Workflow
#########################################


The Gerrit Required INFO Yaml Verify Workflow verifies changes in a Gerrit repository, specifically ensuring that the `INFO.yaml` file is properly modified and follows the necessary structure before merging.

The workflow performs the following tasks:

1. **Clear Votes**: Clears any existing votes on the Gerrit change to reset the status.
2. **Gerrit Checkout**: Checks out the Gerrit change for the repository.
3. **Verify Change Isolation**: Ensures that changes to the `INFO.yaml` file remain isolated in the commit.
4. **Download Valid Schema**: Downloads a valid schema for the `INFO.yaml` file and prepares for validation.
5. **Info File Validation**: Validates the `INFO.yaml` file against the downloaded schema and checks its correctness.
6. **Set Vote**: The workflow reports the result of the checks to Gerrit, updating the change status with the success or failure of the workflow.

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
    :TARGET_REPO: The target GitHub repository needing the required workflow.
        Default: None (Required)

:Required secrets:

    :secrets.GERRIT_SSH_REQUIRED_PRIVKEY: The SSH private key for the authorized user account
    used to interact with Gerrit.

:Steps in the workflow:

1. **Clear Votes**:
   Clears any existing votes and notifies the start of the job on the Gerrit change.

2. **Gerrit Checkout**:
   The workflow checks out the specified Gerrit change and prepares the repository.

3. **Verify Change Isolation**:
   Ensures that changes to the `INFO.yaml` file remain isolated in the commit. This helps avoid
   combining `INFO.yaml` changes with other modifications in a single commit.

4. **Download Valid Schema**:
   If the change involves an `INFO.yaml` file, the workflow downloads a valid schema
   (`info-schema.yaml`) and a Python script (`yaml-verify-schema.py`) for validation.

5. **Info File Validation**:
   The `INFO.yaml` file undergoes validation against the downloaded schema using `yamllint`
   and `python yaml-verify-schema.py` to ensure that it adheres to the correct structure.

6. **Set Vote**:
   The workflow sets the Gerrit vote based on the success or failure of the validation.

:Example usage:

    - Trigger this workflow manually when a change in Gerrit requires validation
      for the correctness of the `INFO.yaml` file.
    - The workflow will check the change and provide feedback to Gerrit.

:Comment Trigger: recheck|reverify

:Workflow Steps Summary:

    1. Clear any existing votes and notify the start of the job in Gerrit.
    2. Ensure that `INFO.yaml` file changes remain isolated in a separate commit.
    3. Download the valid schema and check the `INFO.yaml` file.
    4. Report the workflow conclusion back to Gerrit.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
