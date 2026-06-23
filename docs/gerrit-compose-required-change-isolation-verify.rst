.. _gerrit-compose-required-change-isolation-verify-docs:

########################################################
Gerrit Compose Required Change Isolation Verify Workflow
########################################################

The Gerrit Compose Required Change Isolation Verify Workflow verifies that a
Gerrit change keeps its modifications isolated, and validates the top-level
``INFO.yaml`` against the project schema when that file changes. It runs the
validation alone; the calling workflow clears and sets Gerrit votes.

This workflow supersedes the legacy
``gerrit-compose-required-info-yaml-verify`` workflow. It isolates
``INFO.yaml`` changes and ring-fences CI/CD changes under ``.github`` so
that they are not combined with code or other content in the same change.

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

    None. This workflow performs validation alone and does not vote in
    Gerrit, so it requires no secrets. The calling workflow clears and
    sets Gerrit votes.

:Steps in the workflow:

1. **Gerrit Checkout**:
   The workflow checks out the specific Gerrit change based on the provided branch and refspec.

2. **Isolate INFO.yaml changes**:
   Uses the ``change-isolation-action`` with the ``/INFO.yaml`` pattern. The
   leading slash anchors the pattern to the repository root, so the pattern
   matches the top-level ``INFO.yaml`` and nothing nested.

3. **Isolate .github (CI/CD) changes**:
   Uses the ``change-isolation-action`` with the ``.github/**`` pattern. This
   step runs even when the ``INFO.yaml`` check fails, so both isolation
   results appear for a single patchset.

4. **Download Valid Schema**:
   If the change modifies ``INFO.yaml``, the workflow downloads the required
   validation schema files (``info-schema.yaml`` and ``yaml-verify-schema.py``).

5. **INFO.yaml File Check**:
   The workflow uses ``yamllint`` and a Python script to check the ``INFO.yaml``
   file, ensuring it follows the correct schema and contains the expected repository.

The two isolation checks make the ``INFO.yaml`` and ``.github`` scopes
mutually exclusive: a change that combines either scope with anything outside
it fails.

:Example usage:

    - Trigger this workflow manually, from a calling workflow that handles
      Gerrit voting, when a change requires verifying that ``INFO.yaml`` or
      ``.github`` changes remain isolated.

:Comment Trigger: recheck|reverify

:Workflow Steps Summary:

    1. Checkout the Gerrit change.
    2. Verify that ``INFO.yaml`` changes remain isolated.
    3. Verify that ``.github`` (CI/CD) changes remain isolated.
    4. When ``INFO.yaml`` changed, download the schema files for validation.
    5. Check the ``INFO.yaml`` file and confirm one matching repository.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2026 The Linux Foundation
