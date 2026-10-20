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
        Default: None (Optional)
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
    :ARCHIVE_ARTIFACTS: Paths or glob patterns, relative to the workspace,
      to publish to the log server after every build, including failed
      ones. Separate entries with spaces or newlines. See `Log publishing`_.
        Default: "" (Optional, publishing skipped)
    :LOG_SUBDIR: Directory name appended to the per-run log path. Set it
      when a workflow calls this one more than once per run.
        Default: "" (Optional)

:Outputs:

    :LOG_URL: URL of the published logs. Empty when nothing published to S3,
      for example on the workflow artifact fallback.

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
    :vars.AWS_LOG_ROLE_ARN: IAM role assumed through GitHub OIDC for log
      publishing.
    :vars.S3_LOG_BUCKET: Log bucket name.
    :vars.S3_LOG_REGION: Bucket region. Default: us-east-1
    :vars.LOG_BASE_URL: Public URL of the bucket, used to build LOG_URL.
    :vars.LOG_PUBLISH_DESTINATION: s3, artifact or both. Default: s3

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
    4. Publish logs when ARCHIVE_ARTIFACTS has a value, even if the build failed.

Log publishing
==============

This replaces the Jenkins ``lf-infra-ship-logs`` builder, using
`log-publish-action <https://github.com/lfreleng-actions/log-publish-action>`_.
When ARCHIVE_ARTIFACTS has a value, the job publishes the ``archives``
directory and every file matching ARCHIVE_ARTIFACTS to
``logs/gha/<repo>/<workflow>/<run_id>/[<LOG_SUBDIR>/]``. ``<workflow>``
is the calling workflow's file name without its extension, for example
``logs/gha/yangtools/gerrit-verify/38581567970/``. A link appears in the
job summary and in the LOG_URL output, so later jobs and runs can find the
files from the run ID alone. Re-running a workflow reuses the run ID, so a
re-run overwrites files of the same name.

The calling job must grant ``id-token: write`` so the job can assume the
role through OIDC. Callers that leave ARCHIVE_ARTIFACTS unset need no
change:

.. code-block:: yaml

    maven-verify:
      permissions:
        contents: read
        id-token: write
      uses: lfit/releng-reusable-workflows/.github/workflows/compose-maven-verify.yaml@<sha>
      with:
        ARCHIVE_ARTIFACTS: "allure-report/*.html target/surefire-reports/**"

    report:
      needs: maven-verify
      runs-on: ubuntu-latest
      steps:
        - run: echo "Logs at ${{ needs.maven-verify.outputs.LOG_URL }}"

Without OIDC, for example in a fork or without AWS_LOG_ROLE_ARN, the job
uploads the logs as a ``build-logs`` workflow artifact instead. A
publishing failure never changes the build result.
