.. # SPDX-License-Identifier: Apache-2.0
   # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation

.. _composed-maven-nexus-iq-docs:

################################
Composed Maven Nexus IQ Workflow
################################

Maven Nexus IQ Workflow builds a Maven project, runs a Nexus IQ scan, and assesses the project's dependencies for security and compliance. The workflow performs the following tasks:

1. Checks out the code from the provided Gerrit branch.
2. Runs a Maven build and initiates a Nexus IQ scan to analyze dependencies.
3. Generates the application ID for the project and sets it as an environment variable.
4. Assesses the project's compliance with the policies defined in Nexus IQ.

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
    :MVN_PHASES: The Maven phases to execute (e.g., `clean install`).
        Default: "" (Optional)
    :MVN_OPTS: Maven options.
        Default: >-
          -Dorg.slf4j.simpleLogger.log.org.apache.maven.cli.transfer.Slf4jMavenTransferListener=warn
          -Dmaven.repo.local=/tmp/r -Dorg.ops4j.pax.url.mvn.localRepository=/tmp/r
          -DaltDeploymentRepository=staging::default::file:"${GITHUB_WORKSPACE}"/m2repo (Optional)
    :MVN_POM_FILE: The location of the `pom.xml` file.
        Default: "pom.xml" (Optional)
    :MVN_PROFILES: Comma-delimited list of Maven profiles to activate.
        Default: "" (Optional)
    :ENV_VARS: GitHub variables passed as environment variables during
      the workflow execution.
        Default: "{}" (Optional)
    :ENV_SECRETS: GitHub secrets passed as environment variables during
      the workflow execution.
        Default: "{}" (Optional)

:Required secrets:

    :secrets.NEXUS_IQ_PASSWORD: The Nexus IQ password or token for the user.

:Steps in the workflow:

1. **Gerrit Checkout**: The workflow starts by checking out the code from the
   Gerrit repository based on the provided branch and refspec. This ensures that
   the latest change is available for the build and Nexus IQ scan.

2. **Run Maven Build**: The workflow runs a Maven build using the specified version
   of OpenJDK and Maven, passing any parameters, phases, or options.
   The `pom.xml` file serves as the default for running the Maven build.

3. **Generate Application ID**: The workflow creates an application ID for the project
   based on the Gerrit project and organization, storing it in the environment for
   further use in Nexus IQ scans.

4. **Nexus IQ Policy Assessment**: The workflow then triggers a Nexus IQ policy
   assessment to check the project's dependencies. This scan identifies any security
   vulnerabilities, licensing issues, or policy violations in the dependencies.

:Example usage:

    - Trigger this workflow manually when a change occurs in a Maven-based project
      that requires building and Nexus IQ policy assessment.
    - The workflow runs the Maven build, checks the project's dependencies with
      Nexus IQ, and ensures compliance with defined policies.

:Workflow Steps Summary:

    1. Checkout the Gerrit change to get the latest code.
    2. Run the Maven build using the specified parameters and options.
    3. Generate the application ID for the project.
    4. Assess the project with Nexus IQ for security, licensing, and policy compliance.
