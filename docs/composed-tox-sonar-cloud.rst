.. _composed-tox-sonar-cloud-docs:

#################################
Composed Tox Sonar Cloud Workflow
#################################

Tox Sonar Cloud Workflow runs Sonar scans for Python based repos. The Tox Sonar Cloud Workflow runs a set of `tox` environments (typically for testing and linting) and then executes a SonarCloud scan to analyze the project's code quality.


The workflow performs the following tasks:

1. Sets up the environment for running `tox`.
2. Optionally runs a pre-build script to prepare the project.
3. Runs the specified `tox` environments.
4. Executes a SonarCloud scan to analyze the project's code quality.

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
    :SONAR_ARGS: Arguments to pass to the SonarCloud scanner.
        Default: "" (Optional)
    :SONAR_PROJECTBASEDIR: The directory for the `sonar.projectBaseDir` analysis property.
        Default: "." (Optional)
    :TOX_DIR: The directory containing the `tox.ini` file.
        Default: "." (Optional)
    :TOX_ENVS: A list of environments to run with `tox`. MUST be a
      string representing a list of strings (e.g., '["lint", "build"]').
        Default: None (Required)
    :PYTHON_VERSION: The version of Python for `tox`.
        Default: "3.12" (Optional)
    :PARALLEL: Whether to run `tox` jobs in parallel.
        Default: None (Optional)
    :PRE_BUILD_SCRIPT: A pre-build script to execute before running the verification.
        Default: "" (Optional)
    :PRE_BUILD_SCRIPT_PATH: Path to the pre-build script to trigger before verify run.
        Default: "" (Optional)
    :PRE_BUILD_SCRIPT_URL: URL of the pre-build script to trigger before verify run.
        Default: "" (Optional)

:Required secrets:

    :secrets.SONAR_TOKEN: The SonarCloud access token for authentication.

:Steps in the workflow:

1. **Gerrit Checkout**: The workflow starts by checking out the code from the
   Gerrit repository based on the provided branch and refspec.

2. **Run Tox**: The workflow runs the specified `tox` environments for linting, testing,
   or building, using the provided `tox.ini` file. You can run the jobs
   either sequentially or in parallel.

3. **Run Pre-build Script**: If a pre-build script exists, execute it
   before running the `tox` environments. This script can handle tasks such as
   installing dependencies or configuring the environment.

4. **SonarCloud Scan**: After the `tox` jobs, the workflow runs a SonarCloud scan
   to analyze the project’s code quality. The SonarCloud scanner configures
   with the provided project key, organization, and authentication token.

:Example usage:

    - Trigger this workflow manually when a change occurs in a project that
      requires running `tox` environments and performing SonarCloud analysis.
    - The workflow runs the specified `tox` environments, runs a SonarCloud scan,
      and analyzes the project.

:Comment Trigger: ``run-sonar``

:Workflow Steps Summary:

    1. Checkout the Gerrit change to get the latest code.
    2. Set up Python and `tox` environments.
    3. Run the specified `tox` environments for testing and linting.
    4. Run the SonarCloud scan to analyze the project.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
