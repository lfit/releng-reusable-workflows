.. # SPDX-License-Identifier: Apache-2.0
   # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation

.. _composed-maven-sonar-cloud-docs:

###################################
Composed Maven Sonar Cloud Workflow
###################################

The Maven Sonar Cloud workflow installs the Maven, build a maven project and pushes to Sonar Cloud analyze code for bugs, code smells and security vulnerabilities, and to upload the result (possibly including code-coverage statistics)
to SonarCloud.io. Optionally runs a shell script before the build to install prerequisites.

It builds a Maven project, runs a SonarCloud scan, and analyzes the project's code quality. The workflow performs the following tasks:

1. Checks out the code from the provided Gerrit branch.
2. Runs a Maven build and initiates a SonarCloud scan to analyze the project's code quality.
3. Provides the necessary Maven parameters, profiles, and SonarCloud configuration.

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
    :MVN_VERSION: The Maven version for the build.
        Default: "3.8.2" (Optional)
    :MVN_PARAMS: Maven parameters to pass to the `mvn` command.
        Default: "" (Optional)
    :MVN_PHASES: The list of Maven phases to execute, such as `clean install`.
        Default: "clean install org.sonarsource.scanner.maven:sonar-maven-plugin:3.9.1.2184:sonar" (Optional)
    :MVN_OPTS: Maven options.
        Default: >-
          -Dsonar
          -Dsonar.host.url=https://sonarcloud.io/
          -Dsonar.projectKey=${{ inputs.SONAR_PROJECT_KEY }}
          -Dsonar.organization=${{ inputs.SONAR_ORG }}
          -Dsonar.login=${{ secrets.SONAR_TOKEN }}
          -Dorg.slf4j.simpleLogger.log.org.apache.maven.cli.transfer.Slf4jMavenTransferListener=warn
        (Optional)
    :MVN_POM_FILE: The directory containing the `pom.xml` file.
        Default: "pom.xml" (Optional)
    :MVN_PROFILES: A comma-delimited list of Maven profiles to activate.
        Default: "" (Optional)
    :SONAR_PROJECT_KEY: The dashed repo name prefixed by `<org>_`. Example: `onap_ccsdk-cds`.
        Default: None (Required)
    :SONAR_ORG: The organization name registered in SonarCloud.
        Default: None (Required)
    :SONAR_ARGS: Arguments to pass to the SonarCloud scanner.
        Default: "" (Optional)
    :ENV_VARS: GitHub variables passed as environment variables.
        Default: "{}" (Optional)
    :ENV_SECRETS: GitHub secrets passed as environment variables.
        Default: "{}" (Optional)

:Required secrets:

    :secrets.SONAR_TOKEN: The SonarCloud access token for authentication.

:Steps in the workflow:

1. **Gerrit Checkout**: The workflow checks out the code from the Gerrit repository
   based on the provided branch and refspec. This ensures that the latest change
   is available for the build and SonarCloud scan.

2. **Run Maven Build**: The workflow runs a Maven build using the specified version
   of OpenJDK and Maven, passing any parameters, phases, or options.
   The `pom.xml` file serves as the default for running the Maven build.

3. **Run SonarCloud Scan**: After the build, the workflow runs a SonarCloud scan
   using the SonarScanner for Maven. The scan configures with the provided
   SonarCloud project key, organization, and authentication token. You can pass arguments to the scanner for further configuration.

:Example usage:

    - Trigger this workflow manually when a change occurs in a Maven-based project
      that requires building and SonarCloud analysis for code quality.
    - The workflow builds the project using Maven, then performs a SonarCloud scan
      to analyze the project's code quality.

:Comment Trigger: ``run-sonar``

:Workflow Steps Summary:

    1. Checkout the Gerrit change to get the latest code.
    2. Run the Maven build using the specified parameters and options.
    3. Run the SonarCloud scan to analyze the project.
