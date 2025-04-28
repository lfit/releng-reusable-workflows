.. _gerrit-compose-required-info-yaml-verify:

#################################################
Gerrit Compose Required INFO YAML Verify Workflow
#################################################

The Compose Required INFO YAML Verify Workflow verifies the correctness of the `INFO.yaml` file in a Gerrit change, ensuring the file remains separate from other changes, adheres to a valid schema, and matches the correct repository.

### Required Parameters:

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

### Required Secrets:

    :secrets.GERRIT_SSH_REQUIRED_PRIVKEY:
        The SSH key for the authorized user account to leave comments in Gerrit.

### Workflow Steps:

1. **Gerrit Checkout**:
   The workflow checks out the specific Gerrit change based on the provided branch and refspec.

2. **Verify Change Isolation**:
   This step ensures `INFO.yaml` changes remain separate from other file changes. If other files change, the workflow fails.

3. **Download Valid Schema**:
   If the change passes the isolation test, the workflow downloads the required validation schema files (`info-schema.yaml` and `yaml-verify-schema.py`).

4. **INFO.yaml File Check**:
   The workflow uses tools (`yamllint` and a Python script) to check the `INFO.yaml` file, ensuring it follows the correct schema and contains the expected repository.

### Example usage:

- Trigger this workflow manually when a project change requires validating and isolating `INFO.yaml` modifications in Gerrit.

:Comment Trigger: recheck|reverify

### Workflow Steps Summary:

    1. Checkout the Gerrit change.
    2. Verify that `INFO.yaml` is the sole file changed.
    3. Download the schema files for validation.
    4. Check the `INFO.yaml` file and ensure proper formatting.
    5. Confirm one repository appears, matching the project in Gerrit.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
