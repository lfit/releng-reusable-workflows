.. _gerrit-required-change-isolation-verify-docs:

################################################
Gerrit Required Change Isolation Verify Workflow
################################################


The Gerrit Required Change Isolation Verify Workflow verifies that a Gerrit
change keeps its modifications isolated. It enforces two mutually exclusive
scopes and validates the top-level ``INFO.yaml`` against the project schema
whenever that file changes, before merging.

This workflow supersedes the legacy ``gerrit-required-info-yaml-verify``
workflow. It isolates ``INFO.yaml`` changes and ring-fences CI/CD changes
under ``.github`` so that they are not combined with code or other content
in the same change.

The workflow performs the following tasks:

1. **Clear Votes**: Clears any existing votes on the Gerrit change to reset the status.
2. **Gerrit Checkout**: Checks out the Gerrit change for the repository.
3. **Isolate INFO.yaml changes**: Ensures that a change touching the top-level ``INFO.yaml`` changes no other file.
4. **Isolate .github (CI/CD) changes**: Ensures that a change touching any file under ``.github`` changes nothing outside ``.github``.
5. **Download Valid Schema**: When ``INFO.yaml`` changed, downloads a valid schema for the file and prepares for validation.
6. **Info File Validation**: Validates the ``INFO.yaml`` file against the downloaded schema and checks its correctness.
7. **Set Vote**: The workflow reports the result of the checks to Gerrit, updating the change status with the success or failure of the workflow.

The two isolation checks make the ``INFO.yaml`` and ``.github`` scopes
mutually exclusive: a change that combines either scope with anything outside
it fails. Both checks always run, so the change author receives complete
feedback for a single patchset.

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

    :secrets.GERRIT_SSH_REQUIRED_PRIVKEY:
        The SSH private key for the authorized user account used to
        interact with Gerrit.

:Steps in the workflow:

1. **Clear Votes**:
   Clears any existing votes and notifies the start of the job on the Gerrit change.

2. **Gerrit Checkout**:
   The workflow checks out the specified Gerrit change and prepares the repository.

3. **Isolate INFO.yaml changes**:
   Uses the ``change-isolation-action`` with the ``/INFO.yaml`` pattern. The
   leading slash anchors the pattern to the repository root, so the pattern
   matches the top-level ``INFO.yaml`` and nothing nested.

4. **Isolate .github (CI/CD) changes**:
   Uses the ``change-isolation-action`` with the ``.github/**`` pattern. This
   step runs even when the ``INFO.yaml`` check fails, so both isolation
   results appear for a single patchset.

5. **Download Valid Schema**:
   If the change modifies ``INFO.yaml``, the workflow downloads a valid schema
   (``info-schema.yaml``) and a Python script (``yaml-verify-schema.py``) for validation.

6. **Info File Validation**:
   The ``INFO.yaml`` file undergoes validation against the downloaded schema using ``yamllint``
   and ``python yaml-verify-schema.py`` to ensure that it adheres to the correct structure.

7. **Set Vote**:
   The workflow sets the Gerrit vote based on the success or failure of the validation.

:Example usage:

    - Trigger this workflow manually when a change in Gerrit requires
      verifying that ``INFO.yaml`` or ``.github`` changes remain isolated.
    - The workflow will check the change and provide feedback to Gerrit.

:Comment Trigger: recheck|reverify

:Workflow Steps Summary:

    1. Clear any existing votes and notify the start of the job in Gerrit.
    2. Ensure ``INFO.yaml`` changes remain isolated in a separate change.
    3. Ensure ``.github`` (CI/CD) changes remain isolated in a separate change.
    4. When ``INFO.yaml`` changed, download the valid schema and check the file.
    5. Report the workflow conclusion back to Gerrit.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2026 The Linux Foundation
