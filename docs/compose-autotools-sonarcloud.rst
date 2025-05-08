.. _composed-autotools-sonar-cloud-docs:

#######################################
Composed Autotools Sonar Cloud Workflow
#######################################


The Sonar Cloud workflow installs and runs the Sonarcloud Scanner to analyze
code for bugs, code smells and security vulnerabilities, and to upload the result
(possibly including code-coverage statistics) to SonarCloud.io.

Optionally runs a shell script before the build to install prerequisites.
The default configuration supplies a pre-build script that runs GNU Autotools to generate the configure shell script.
Must be overridden if that script is in the version-control system.

The worflow verifies a project by configuring the build system with autotools and analyzing
code quality with SonarCloud. The workflow triggers when the merge job completes.

1. Set up the environment for autotools and SonarCloud.
2. Optionally run a pre-build script before verification.
3. Configure the build system using the autotools script.
4. Execute a SonarCloud scan to analyze the project's code quality.

:Required parameters:

    :GERRIT_BRANCH: The branch for the change.
    :GERRIT_CHANGE_ID: The unique identifier for the change.
    :GERRIT_CHANGE_NUMBER: The Gerrit number for the change.
    :GERRIT_CHANGE_URL: The URL to the change.
    :GERRIT_EVENT_TYPE: The Gerrit event triggering the workflow.
    :GERRIT_PATCHSET_NUMBER: The patch number for the change.
    :GERRIT_PATCHSET_REVISION: The revision SHA for the patchset.
    :GERRIT_PROJECT: The project in Gerrit for the change.
    :GERRIT_REFSPEC: The refspec of the change in Gerrit.
    :JDK_VERSION: The OpenJDK version for the build.
    :PRE_BUILD_SCRIPT: The pre-build script to execute before verification.
    :PRE_BUILD_SCRIPT_PATH: The path to the pre-build script.
    :PRE_BUILD_SCRIPT_URL: The URL of the pre-build script.
    :SONAR_ARGS: Arguments for the SonarCloud scanner.
    :SONAR_PROJECTBASEDIR: The directory for the `sonar.projectBaseDir` analysis property.

:Required secrets:

    :secrets.SONAR_TOKEN: The SonarCloud access token for authentication.

:Steps in the workflow:

1. **Gerrit Checkout**: Check out the code from the Gerrit repository using the provided branch and refspec.

2. **Setup Python Environment**: Set up Python 3.8 for running scripts during the build process.

3. **Setup JDK**: Install the specified OpenJDK version using the `setup-java` action.

4. **Pre-build Script Handling**: If provided, download and run the pre-build script to prepare the build environment. Use this for tasks like dependency installation or custom environment setup.

5. **Run Autotools Script**: Run an autotools script to configure the build system.

6. **Run SonarCloud Scan**: Execute a SonarCloud scan using the `sonar-scanner` tool. Configure the analysis with the provided arguments and set the `sonar.projectBaseDir` property.

:Example usage:

    - Trigger this workflow manually when a project change requires autotools configuration and SonarCloud analysis.
    - Set up the build environment, run the autotools script, and perform a SonarCloud scan on the codebase.

:Workflow Steps Summary:

    1. Check out the Gerrit change to get the latest code.
    2. Set up the Python environment and install OpenJDK.
    3. Run the pre-build script (if provided).
    4. Run the autotools script to configure the build system.
    5. Run the SonarCloud scan to analyze the project.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
