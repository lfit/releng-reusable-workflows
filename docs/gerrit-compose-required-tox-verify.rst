.. _compose-tox-verify-docs:

###########################
Compose Tox Verify Workflow
###########################

The Compose Tox Verify Workflow verifies changes in a Gerrit repository using `tox` environments.
The workflow ensures proper Python version setup, verifies tox environments, and executes pre-build scripts before running the tests.

The workflow performs the following tasks:

1. **Gerrit Checkout**: Checks out the specified Gerrit change and prepares the repository.
2. **Pre-build Script Handling**: Optionally, runs any pre-build scripts specified by the user.
3. **Tox Environment Setup**: Uses `tox` environments to run tests, linters, and build processes.
4. **Parallel Execution**: Runs `tox` in parallel across the specified environments if needed.

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
    :GERRIT_PROJECT: The project in Gerrit that the change belongs to.
        Default: None (Required)
    :GERRIT_REFSPEC: The refspec of the change in Gerrit.
        Default: None (Required)
    :TARGET_REPO: The target GitHub repository needing the required workflow.
        Default: `${{ github.repository }}` (Optional)
    :TOX_DIR: The directory containing the `tox.ini` file.
        Default: `"."` (Optional)
    :TOX_ENVS: A list of environments to run. Pass this as a string representing
    a list of strings, e.g. `'["lint", "build"]'`.
        Default: None (Required)
    :PYTHON_VERSION: The version of Python to use.
        Default: `"3.12"` (Optional)
    :PARALLEL: Whether to run jobs in parallel.
        Default: None (Optional)
    :PRE_BUILD_SCRIPT: A script to run before the build process begins.
        Default: `""` (Optional)
    :PRE_BUILD_SCRIPT_PATH: Path to the pre-build script.
        Default: `""` (Optional)
    :PRE_BUILD_SCRIPT_URL: URL of the pre-build script.
        Default: `""` (Optional)

:Required secrets:

    :secrets.RTD_TOKEN: The RTD API user token used to interact with the ReadTheDocs API.

:Steps in the workflow:

1. **Gerrit Checkout**:
   The workflow checks out the specified Gerrit change and prepares the repository.

2. **Pre-build Script Handling**:
   If specified, the workflow runs any pre-build scripts. Provide this in
   the form of a script, a path to a script, or a URL to fetch a script from.

3. **Tox Environment Setup**:
   The workflow sets up the appropriate Python environment and runs tests using `tox`.
   It reads the Python version from the `tox.ini` file or uses the specified version.

4. **Run Tox**:
   The workflow runs `tox` environments as specified in the `TOX_ENVS` parameter. It
   also handles parallel execution based on the `PARALLEL` parameter.

5. **Parallel Execution**:
   If the `PARALLEL` parameter equals `true`, the workflow runs the tests in parallel
   across the specified environments.

:Example usage:

    - Trigger this workflow manually when a Gerrit change needs validation against
      specific `tox` environments.
    - The workflow validates the change by running `tox` checks, ensuring the change
      passes the specified tests before merging.

:Comment Trigger: recheck|reverify

:Workflow Steps Summary:

    1. Checkout the Gerrit change and prepare the repository.
    2. Run any pre-build scripts if specified.
    3. Run tests and checks using `tox` environments.
    4. Run tests in parallel if specified.
    5. Provide feedback to Gerrit.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
