.. _rrw-docs-compose-maven-merge:

###################
Compose Maven Merge
###################

This workflow will run when a merge completes for a Java project
built with Maven. It will build the package for release, run tests for final
integration testing, and can optionally sign and push the package for release.

This workflow is directly called, or can serve as a reference workflow for
modification (e.g. removing boolean inputs for pushes that will always occur
or hardcoding inputs/variables that apply to all repos in an organization).

:Required parameters:

    :GERRIT_BRANCH: This parameter comes from the Gerrit trigger.

:Optional parameters:

    :ARTIFACT_DIR: The location of build artifacts. Default:
        "${GITHUB_WORKSPACE}/m2repo"
    :JDK_VERSION: Version of OpenJDK to use. Default: 17
    :MVN_VERSION: Version of Maven to use. Default: 3.8.2
    :MVN_PARAMS: Parameters to pass to the mvn command.
    :MVN_PHASES: List of phases for mvn to execute. Default: "clean deploy"
    :MVN_OPTS: Options to pass to mvn. Default:
        -Dorg.slf4j.simpleLogger.log.org.apache.maven.cli.transfer.Slf4jMavenTransferListener=warn
        -Dmaven.repo.local=/tmp/r -Dorg.ops4j.pax.url.mvn.localRepository=/tmp/r
        -DaltDeploymentRepository=staging::default::file:"${GITHUB_WORKSPACE}"/m2repo
    :MVN_POM_FILE: Path to the pom.xml to use. Default: "pom.xml"
    :MVN_PROFILES: Comma-delimited list of profiles to activate.
    :SIGUL_SIGN: Boolean indicating whether to sign artifacts. Default: false
    :PUSH_NEXUS: Boolean indicating whether to push to a Nexus server. Default: false
    :PUSH_ARTIFACTORY: Boolean indicating whether to push to Artifactory. Default: false
    :PUSH_CENTRAL: Boolean indicating whether to push to Maven Central. Default: false
    :inputs.JFROG_OPTIONS: Options passed to the JFrog command when
        pushing to Artifactory (e.g. build number, module).
    :ARCHIVE_ARTIFACTS: Paths or glob patterns, relative to the workspace,
        to publish to the log server after every build, including failed
        ones. Separate entries with spaces or newlines. See
        `Log publishing`_. Default: "" (publishing skipped)
    :LOG_SUBDIR: Directory name appended to the per-run log path. Set it
        when a workflow calls this one more than once per run. Default: ""

:Outputs:

    :LOG_URL: URL of the published logs. Empty when nothing published to S3,
        for example on the workflow artifact fallback.

:Expected variables:

    :vars.GLOBAL_SETTINGS: Global settings.xml used by Maven.

:Expected variables - Log publishing:

    :vars.AWS_LOG_ROLE_ARN: IAM role assumed through GitHub OIDC.
    :vars.S3_LOG_BUCKET: Log bucket name.
    :vars.S3_LOG_REGION: Bucket region. Default: us-east-1
    :vars.LOG_BASE_URL: Public URL of the bucket, used to build LOG_URL.
    :vars.LOG_PUBLISH_DESTINATION: s3, artifact or both. Default: s3

Log publishing
==============

This replaces the Jenkins ``lf-infra-ship-logs`` builder, using
`log-publish-action <https://github.com/lfreleng-actions/log-publish-action>`_.
When ARCHIVE_ARTIFACTS has a value, the job publishes the ``archives``
directory and every file matching ARCHIVE_ARTIFACTS to
``logs/gha/<repo>/<workflow>/<run_id>/[<LOG_SUBDIR>/]``. ``<workflow>``
is the calling workflow's file name without its extension. A link
appears in the job summary and in the LOG_URL output. Re-running a
workflow reuses the run ID, so a re-run overwrites files of the same name.

The calling job must grant ``id-token: write`` so the job can assume the
role through OIDC:

.. code-block:: yaml

    maven-merge:
      permissions:
        contents: read
        id-token: write
      uses: lfit/releng-reusable-workflows/.github/workflows/compose-maven-merge.yaml@<sha>
      with:
        ARCHIVE_ARTIFACTS: "target/surefire-reports/** **/*.log"

Without OIDC, for example in a fork or without AWS_LOG_ROLE_ARN, the job
uploads the logs as a ``build-logs`` workflow artifact instead. A
publishing failure never changes the build result.

:Expected variables - Sigul signing:

    :secrets.SIGUL_KEY: If SIGUL_SIGN is true, the name of the signing key to
        use (the project's identifier on the Sigul server).
    :secrets.GHA_TOKEN: See `sigul-sign-action docs
        <https://github.com/lfit/sigul-sign-action>`_.
    :secrets.SIGUL_IP: See `sigul-sign-action docs
        <https://github.com/lfit/sigul-sign-action>`_.
    :secrets.SIGUL_URI: See `sigul-sign-action docs
        <https://github.com/lfit/sigul-sign-action>`_.
    :secrets.SIGUL_CONF: See `sigul-sign-action docs
        <https://github.com/lfit/sigul-sign-action>`_.
    :secrets.SIGUL_PASS: See `sigul-sign-action docs
        <https://github.com/lfit/sigul-sign-action>`_.
    :secrets.SIGUL_PKI: See `sigul-sign-action docs
        <https://github.com/lfit/sigul-sign-action>`_.

:Expected variables - Nexus upload:

    :env.NEXUS_REPO: Environment variable that defines what repo to push to
        (e.g. snapshosts, releases, staging, etc.).
    :vars.NEXUS_SERVER: Nexus server to upload to.
    :secrets.NEXUS_USERNAME: Username for push to Nexus.
    :secrets.NEXUS_PASSWORD: Password or token for push to Nexus.

:Expected variables - Artifactory upload:

    :vars.ARTIFACTORY_URL: Repo URL to push to.
    :secrets.ARTIFACTORY_ACCESS_TOKEN: The token to use to push to Artifactory.
    :vars.ARTIFACTORY_PATH: Path to the repo on Artifactory.

:Expected variables - Central upload:

    :env.NEXUS_REPO: Environment variable that defines what repo to push to
        (e.g. snapshosts, releases, staging, etc.).
    :vars.CENTRAL_URL: Repo URL to push to.
    :secrets.CENTRAL_USERNAME: Username for push to Maven Central.
    :secrets.CENTRAL_PASSWORD: Password or token for push to Maven Central.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
