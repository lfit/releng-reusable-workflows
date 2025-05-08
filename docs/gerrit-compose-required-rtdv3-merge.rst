.. # SPDX-License-Identifier: Apache-2.0
   # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation

.. _compose-rtdv3-merge-docs:

############################
Compose RTDv3 Merge Workflow
############################

The Compose RTDv3 Merge Workflow manages the merge of changes into a ReadTheDocs (RTD) project for documentation builds.
The workflow verifies the existence of the RTD configuration, installs dependencies, and triggers the RTD build process.
It also handles the creation and management of RTD projects and subprojects, ensuring the changes integrate properly into the documentation site.

The workflow performs the following tasks:

1. **Checkout Gerrit Change**: Checks out the specified Gerrit change and prepares the repository.
2. **Install Dependencies**: Installs necessary dependencies like `lftools`, `niet`, `cryptography`, `yq`, and `tox`.
3. **Verify ReadTheDocs Config**: Checks for the `readthedocs.yaml` configuration file in the repository.
4. **Run RTDv3**: Triggers the RTD build process, including managing RTD projects and subprojects, and updating version information.
5. **Merge Action**: Performs the merge operation if everything is valid and proceeds to trigger the appropriate RTD build for the specified branch.

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

    :READTHEDOCS_FOUND: A flag indicating whether the RTD config file exists.
        Default: `true` (Optional)
    :TOX_ENVS: The tox environments to use for the build.
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

1. **Checkout Gerrit Change**:
   The workflow checks out the change from the specified Gerrit repository and branch, ensuring the latest changes are available.

2. **Install Dependencies**:
   Installs dependencies like `lftools`, `niet`, `cryptography`, `yq`, and `tox`. This ensures the necessary tools are available for building and managing the RTD project.

3. **Verify ReadTheDocs Config**:
   Checks for the `readthedocs.yaml` configuration file. If absent, the process skips this step.

4. **Run RTDv3**:
   Triggers the RTD build using the `lftools` tool. This includes creating or updating the RTD project and ensuring the correct version of the documentation builds.

5. **Merge Action**:
   If the RTD project and subproject exist, the workflow performs the merge action. It updates the RTD version information and triggers the appropriate build.

:Example usage:

    - Trigger this workflow manually when a change in Gerrit requires integration with RTD.
    - The workflow verifies the RTD configuration, installs dependencies, triggers the RTD build, and handles any necessary project or subproject creation.

:Comment Trigger: remerge

:Workflow Steps Summary:

    1. Checkout the Gerrit change and prepare the repository.
    2. Install the necessary dependencies for RTD and project management.
    3. Verify if the `readthedocs.yaml` configuration file exists.
    4. Trigger the RTD build process and handle project/subproject creation.
    5. Perform the merge action and trigger the appropriate build for the branch.
