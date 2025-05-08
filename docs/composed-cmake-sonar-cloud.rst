.. # SPDX-License-Identifier: Apache-2.0
   # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation

.. _composed-cmake-sonar-cloud-docs:

###################################
Composed Cmake Sonar Cloud Workflow
###################################

The Sonar Cloud workflow installs and runs CMake, uses the build wrapper to invoke make, then runs the Sonar Cloud Scanner
to analyze code for bugs, code smells, and security vulnerabilities, and to upload the result (possibly including code-coverage statistics) to
SonarCloud.io. Optionally runs a shell script before the build to install prerequisites.

This workflow performs the following tasks:

1. Sets up the environment for running CMake and SonarCloud.
2. Optionally runs a pre-build script before the verification process.
3. Executes the CMake script to configure the build system.
4. Conducts a SonarCloud scan to analyze the project's code quality.

:Required parameters:

    :GERRIT_BRANCH: The branch for the change.
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
    :PRE_BUILD_SCRIPT: The pre-build script to execute before the verify run.
        Default: "" (Optional)
    :PRE_BUILD_SCRIPT_PATH: The path to the pre-build script to execute before the verify run.
        Default: "" (Optional)
    :PRE_BUILD_SCRIPT_URL: The URL of the pre-build script to execute before the verify run.
        Default: "" (Optional)
    :SONAR_ARGS: Arguments to pass to the SonarCloud scanner.
        Default: "" (Optional)
    :SONAR_PROJECTBASEDIR: The directory for the `sonar.projectBaseDir` analysis property.
        Default: "." (Optional)

:Required secrets:

    :secrets.SONAR_TOKEN: The SonarCloud access token for authentication.

:Steps in the workflow:

1. **Gerrit Checkout**: The workflow starts by checking out the code from the
   Gerrit repository based on the provided branch and refspec, including the project for analysis.

2. **Setup Python Environment**: The workflow sets up Python 3.8 for running scripts required during the build process.

3. **Setup JDK**: The workflow installs the specified version of OpenJDK using
   the `setup-java` action, preparing the Java environment for the build.

4. **Pre-build Script Handling**: If you provide a pre-build script via the
   `PRE_BUILD_SCRIPT`, `PRE_BUILD_SCRIPT_PATH`, or `PRE_BUILD_SCRIPT_URL` inputs,
   the workflow downloads and executes the script for tasks like installing dependencies or preparing the environment before verification.

5. **Run CMake Script**: The workflow runs a CMake script to configure the build
   system. It retrieves this script from a public GitHub repository and executes it to set up the project build environment.

6. **Run SonarCloud Scan**: The workflow runs a SonarCloud scan to analyze
   the codebase, using the provided arguments and setting the `sonar.projectBaseDir`
   property based on the specified project base directory.

:Example usage:

    - Trigger this workflow manually when you change a project requiring CMake configuration and SonarCloud analysis.
    - This workflow sets up the build environment, runs the CMake script, and performs a SonarCloud scan on the codebase.

:Comment Trigger: ``run-sonar``

:Workflow Steps Summary:

    1. Checkout the Gerrit change to get the latest code.
    2. Set up Python and OpenJDK environments.
    3. Run the pre-build script (if provided).
    4. Run the CMake script to configure the build system.
    5. Run the SonarCloud scan to analyze the project.
..
..  # SPDX-License-Identifier: Apache-2.0
