.. # SPDX-License-Identifier: Apache-2.0
   # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation

.. _compose-maven-verify-docs:

#############################
Compose Maven Verify Workflow
#############################

It verifies the integrity and correctness of Maven-based projects by running Maven build tasks such as `clean` and `deploy`. The workflow performs the following tasks:

1. Check out the code from the specified Gerrit branch.
2. Set up the environment for Maven and Python.
3. Run Maven to verify the project.

:Required parameters:

    :GERRIT_BRANCH: The branch for the change.
        Default: None (Required)
    :GERRIT_CHANGE_ID: The unique identifier for the change.
        Default: None (Required)
    :GERRIT_CHANGE_NUMBER: The Gerrit number for the change.
        Default: None (Optional)
    :GERRIT_CHANGE_URL: The URL to the change.
        Default: None (Optional)
    :GERRIT_EVENT_TYPE: The Gerrit event triggering the workflow.
        Default: None (Optional)
    :GERRIT_PATCHSET_NUMBER: The patch number for the change.
        Default: None (Optional)
    :GERRIT_PATCHSET_REVISION: The revision SHA for the patchset.
        Default: None (Optional)
    :GERRIT_PROJECT: The project in Gerrit that the change belongs to.
        Default: None (Optional)
    :GERRIT_REFSPEC: The refspec of the change in Gerrit.
        Default: None (Required)
    :JDK_VERSION: The version of OpenJDK to use for the build.
        Default: "17" (Optional)
    :MVN_VERSION: The version of Maven to use for the build.
        Default: "3.8.2" (Optional)
    :MVN_PARAMS: Parameters to pass to the Maven command.
        Default: "" (Optional)
    :MVN_PHASES: The phases to execute in Maven (e.g., `clean deploy`).
        Default: "clean deploy" (Optional)
    :MVN_OPTS: Maven options to pass during the build.
        Default:
        "-Dorg.slf4j.simpleLogger.log.org.apache.maven.cli.transfer.Slf4jMavenTransferListener=warn
        -Dmaven.repo.local=/tmp/r -Dorg.ops4j.pax.url.mvn.localRepository=/tmp/r
        -DaltDeploymentRepository=staging::default::file:"${GITHUB_WORKSPACE}"/m2repo" (Optional)
    :MVN_POM_FILE: The path to the `pom.xml` file.
        Default: "pom.xml" (Optional)
    :MVN_PROFILES: A comma-delimited list of Maven profiles to activate.
        Default: "" (Optional)
    :MVN_GLOBAL_SETTINGS: Global settings configuration for Maven.
        Default: ${{ vars.GLOBAL_SETTINGS }} (Optional)
    :ENV_VARS: GitHub variables to use as environment variables during
      the workflow execution.
        Default: "{}" (Optional)
    :ENV_SECRETS: GitHub secrets to use as environment variables during
      the workflow execution.
        Default: "{}" (Optional)

:Required secrets:

    :secrets.SIGUL_KEY: The key for artifact signing (if you enable signing).
    :secrets.GHA_TOKEN: GitHub token for signing artifacts (if applicable).
    :secrets.NEXUS_USERNAME: The username for Nexus authentication (if pushing
      to Nexus).
    :secrets.NEXUS_PASSWORD: The password or token for Nexus authentication
      (if pushing to Nexus).
    :secrets.ARTIFACTORY_ACCESS_TOKEN: The token for Artifactory access (if
      pushing to Artifactory).
    :secrets.CENTRAL_USERNAME: The username for Maven Central authentication
      (if pushing to Maven Central).
    :secrets.CENTRAL_PASSWORD: The password or token for Maven Central
      authentication (if pushing to Maven Central).

:Steps in the workflow:

1. **Checkout the Code**: Begin by checking out the code from the specified Gerrit branch. Include any submodules to ensure all dependencies are up to date.

2. **Setup Python Environment**: Set up Python 3.8, which you need for any Python-based tasks during the build process.

3. **Run Maven Build**: Run Maven to verify the project by executing the specified Maven phases (`clean deploy` by default). Use the defined version of Maven, JDK, and any custom Maven options or parameters provided. The `pom.xml` file serves as the default, and you can specify other profiles and settings.

:Expected environment variables:

    :vars.GERRIT_URL: The base URL for Gerrit (needed for the checkout step).
    :vars.GLOBAL_SETTINGS: Global settings for Maven.

:Example usage:

    - Trigger this workflow manually when you change the Maven-based
      project in Gerrit that needs verification.
    - The workflow builds the project using Maven, verifies the
      results, and you can extend it to include more Maven phases or options.

:Comment Trigger: recheck|reverify

:Workflow Steps Summary:

    1. Checkout the Gerrit change to get the latest code.
    2. Set up Python and Maven environments.
    3. Run Maven to verify the project.
