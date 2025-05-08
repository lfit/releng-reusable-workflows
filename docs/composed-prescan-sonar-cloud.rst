.. _composed-prescan-sonar-cloud-docs:

#####################################
Composed Prescan Sonar Cloud Workflow
#####################################

This workflow triggers manually via GitHub's `workflow_call` event. It checks out the code, runs any necessary pre-build scripts, and performs a SonarCloud scan to analyze the project's code quality. This workflow is typically used for projects that require a pre-build check or analysis before the main build.

The workflow performs the following tasks:

1. Sets up the environment for running SonarCloud scans.
2. Optionally runs a pre-build script to prepare the project.
3. Executes a SonarCloud scan to analyze the project's code quality.

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
    :JDK_VERSION: The version of OpenJDK for the build.
        Default: "17" (Optional)
    :PRE_BUILD_SCRIPT: The pre-build script to execute before the verification run.
        Default: "" (Optional)
    :PRE_BUILD_SCRIPT_PATH: The path to the pre-build script to execute before the verification run.
        Default: "" (Optional)
    :PRE_BUILD_SCRIPT_URL: The URL of the pre-build script to execute before the verification run.
        Default: "" (Optional)
    :SONAR_ARGS: Arguments to pass to the SonarCloud scanner.
        Default: "" (Optional)
    :SONAR_PROJECTBASEDIR: The directory for the `sonar.projectBaseDir` analysis property.
        Default: "." (Optional)

:Required secrets:

    :secrets.SONAR_TOKEN: The SonarCloud access token for authentication.

:Steps in the workflow:

1. **Gerrit Checkout**: The workflow checks out the code from the Gerrit repository
   based on the provided branch and refspec. This ensures that the latest change
   is available for the analysis.

2. **Setup Python and JDK**: The workflow sets up Python 3.8 and the specified version
   of OpenJDK using the `setup-java` action, ensuring the environment is ready for
   the build process.

3. **Pre-build Script**: If you provide a pre-build script via `PRE_BUILD_SCRIPT`,
   `PRE_BUILD_SCRIPT_PATH`, or `PRE_BUILD_SCRIPT_URL`, the workflow downloads
   and runs the script to prepare the environment. This can be useful for tasks like
   setting up dependencies or configuring the environment before running the SonarCloud scan.

4. **Run SonarCloud Scan**: The workflow runs the SonarCloud scanner to analyze the
   project's code quality. The scan configures using the provided SonarCloud project
   key, organization, and authentication token. You can pass arguments to
   further configure the scan.

:Example usage:

    - Trigger this workflow manually when a change occurs in a project that
      requires SonarCloud analysis for code quality.
    - The workflow prepares the environment, runs any pre-build scripts if provided,
      and then performs a SonarCloud scan to analyze the codebase.

:Comment Trigger: ``run-sonar``

:Workflow Steps Summary:

    1. Checkout the Gerrit change to get the latest code.
    2. Set up Python and OpenJDK environments.
    3. Run the pre-build script (if provided).
    4. Run the SonarCloud scan to analyze the project.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
