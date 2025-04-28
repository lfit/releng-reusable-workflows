.. # SPDX-License-Identifier: Apache-2.0
   # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation

.. _composed-generic-sonar-cloud-docs:

#####################################
Composed Generic Sonar Cloud Workflow
#####################################

The Generic Sonar Cloud workflow runs the Sonar Cloud Scanner to analyze code for bugs, code smells and security vulnerabilities, and to upload the result (possibly including code-coverage statistics)
to SonarCloud.io. Optionally runs a shell script before the build to install prerequisites.

It sets up an environment, runs a SonarCloud scan, and analyzes the project's quality. This process is essential for continuous integration (CI) and code quality checks.

The workflow performs the following tasks:

1. Sets up the environment for running SonarCloud scans.
2. Optionally runs a pre-build script before the verification process.
3. Executes a SonarCloud scan to analyze the project's code quality.

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
    :ENV_VARS: GitHub variables passed as environment variables during the workflow execution.
        Default: "{}" (Optional)
    :ENV_SECRETS: GitHub secrets passed as environment variables during the workflow execution.
        Default: "{}" (Optional)
    :PRE_BUILD_SCRIPT: The pre-build script to execute before the verification run.
        Default: "" (Optional)
    :SONAR_ARGS: Arguments to pass to the SonarCloud scanner.
        Default: "" (Optional)
    :SONAR_PROJECTBASEDIR: The directory for the `sonar.projectBaseDir` analysis property.
        Default: "." (Optional)

:Required secrets:

    :secrets.SONAR_TOKEN: The SonarCloud access token for authentication.

:Steps in the workflow:

1. **Gerrit Checkout**: The workflow checks out the code from the Gerrit repository
   based on the provided branch and refspec, ensuring analysis of the latest change.

2. **Setup Python Environment**: The workflow sets up Python 3.8, necessary for running scripts during the build process.

3. **Setup JDK**: The workflow installs the specified version of OpenJDK using the
   `setup-java` action, preparing the Java environment for the build.

4. **Export Environment Variables and Secrets**: The workflow exports the provided
   environment variables and secrets into the GitHub environment, making them available during the SonarCloud scan.

5. **Run Pre-build Script**: If you provide a pre-build script via the `PRE_BUILD_SCRIPT`
   input, the workflow executes it. Use this script to set up the environment or install any dependencies before running the SonarCloud scan.

6. **Run SonarCloud Scan**: The workflow runs a SonarCloud scan using the provided
   arguments and `sonar.projectBaseDir` property, analyzing the project's code quality.

:Example usage:

    - Trigger this workflow manually when you change a project requiring a SonarCloud scan for quality analysis.
    - The workflow sets up the environment, runs any pre-build scripts if provided, and then performs a SonarCloud scan to check the project's code quality.

:Comment Trigger: ``run-sonar``

:Workflow Steps Summary:

    1. Checkout the Gerrit change to get the latest code.
    2. Set up Python and OpenJDK environments.
    3. Export environment variables and secrets.
    4. Run the pre-build script (if provided).
    5. Run the SonarCloud scan to analyze the project.
