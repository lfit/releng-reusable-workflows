.. _rrw-docs-compose-maven-merge:

###################
Compose Maven Merge
###################

This workflow is intended to be run when a merge is completed on a Java project
built with Maven. It will build the package for release, run tests for final
integration testing, and can optionally sign and push the package for release.

:Required parameters:

    :GERRIT_BRANCH: This should be received from the Gerrit trigger.

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

:Expected variables:

    :vars.GLOBAL_SETTINGS: Global settings.xml to be used by mvn.

:Expected variables - Sigul signing:

    :secrets.SIGUL_KEY: If SIGUL_SIGN is true, the name of the signing key to
        use (the project's identifier on the Sigul server) should be defined.
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
    :secrets.NEXUS_PASSWORD: Password or API key for push to Nexus.
