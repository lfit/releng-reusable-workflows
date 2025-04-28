.. # SPDX-License-Identifier: Apache-2.0
   # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation

.. _compose-rtdv3-verify-docs:

#############################
Compose RTDv3 Verify Workflow
#############################

The Compose RTDv3 Verify Workflow validates a Gerrit change against ReadTheDocs (RTD) configurations, ensuring the documentation undergoes verification before merging.
The workflow checks for the RTD configuration, installs necessary dependencies, and performs build and version checks on the RTD project.

The workflow performs the following tasks:

1. **Gerrit Checkout**: Checks out the specified Gerrit change and prepares the repository.
2. **Python Version Extraction**: Extracts the Python version from the `tox.ini` file.
3. **Python Setup**: Sets up the appropriate Python version based on the configuration.
4. **Install Dependencies**: Installs necessary dependencies such as `lftools`, `niet`, `cryptography`, `yq`, and `tox`.
5. **Verify ReadTheDocs Config**: Checks for the `readthedocs.yaml` configuration file.
6. **Run Tox**: Executes `tox` to run tests and perform static checks.
7. **Run RTDv3**: Triggers the RTD build process, managing RTD projects and subprojects, and updating version information.

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
        Default: ${github.repository} (Optional)

:Required secrets:

    :secrets.RTD_TOKEN: The RTD API user token used to interact with the ReadTheDocs API.

:Environment variables:

    :DEFAULT_PYTHON: The default Python version if not specified in the `tox.ini`.
        Default: `3.9` (Optional)
    :READTHEDOCS_FOUND: A flag indicating the presence of the RTD config file.
        Default: `true` (Optional)
    :TOX_ENVS: The tox environments for the build.
        Default: `docs, docs-linkcheck` (Optional)
    :TOX_DIR: The directory containing the documentation for building.
        Default: `docs/` (Optional)
    :DOC_DIR: The directory for the generated documentation.
        Default: `_build/html` (Optional)
    :PARALLEL: Whether to run jobs in parallel.
        Default: `true` (Optional)
    :DEFAULT_VERSION: The default version for RTD.
        Default: `latest` (Optional)
    :MASTER_RTD_PROJECT: The main RTD project.
        Default: `doc` (Optional)

:Steps in the workflow:

1. **Gerrit Checkout**:
   The workflow checks out the specified Gerrit change and prepares the repository.

2. **Extract Python Version**:
   The workflow extracts the Python version specified in the `tox.ini` file, ensuring the correct version applies to the documentation build.

3. **Setup Python**:
   The workflow sets up Python using the version extracted from `tox.ini` or defaults to `3.9` if unspecified.

4. **Install Dependencies**:
   Installs dependencies such as `lftools`, `niet`, `cryptography`, `yq`, and `tox` to ensure the necessary tools are available for the build.

5. **Verify ReadTheDocs Config**:
   Checks for the `readthedocs.yaml` configuration file. If absent, the process skips this step.

6. **Run Tox**:
   Executes `tox` to perform tests and checks based on the configuration. It uses `tox` environments defined in the `tox.ini` file to run documentation checks and link validation.

7. **Run RTDv3**:
   Triggers the RTD build process, creating or updating the RTD project and ensuring the correct version of the documentation builds.

:Example usage:

    - Trigger this workflow manually when a Gerrit change requires validation against ReadTheDocs.
    - The workflow validates the change by running `tox` checks, verifying RTD configuration, and triggering the RTD build.

:Comment Trigger: recheck|reverify

:Workflow Steps Summary:

    1. Checkout the Gerrit change and prepare the repository.
    2. Extract Python version from `tox.ini` or use the default version.
    3. Install necessary dependencies and verify RTD config.
    4. Run `tox` to perform static analysis and format checks.
    5. Trigger the RTD build process and ensure the documentation is up-to-date.
