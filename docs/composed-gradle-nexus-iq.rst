.. _composed-gradle-nexus-iq-docs:

##########################
Composed Gradle Nexus IQ Workflow
##########################

Gradle Nexus IQ Workflow builds a project using Gradle, runs a Nexus IQ scan, and analyzes the project's code quality. The workflow performs the following tasks:

1. Sets up the environment for running Gradle and Nexus IQ scans.
2. Runs a Gradle build and performs a Nexus IQ scan to analyze dependencies.
3. Retrieves the application ID for the project and triggers the Sonatype lifecycle scan.

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
    :GERRIT_PROJECT: The project in Gerrit that the change belongs to.
        Default: None (Required)
    :GERRIT_REFSPEC: The refspec of the change in Gerrit.
        Default: None (Required)
    :JDK_VERSION: The version of OpenJDK to use for the build.
        Default: "17" (Optional)
    :DISTRIBUTION: The OpenJDK distribution to use (e.g., `temurin`, `zulu`, etc.).
        Default: "temurin" (Optional)

:Required secrets:

    :secrets.NEXUS_IQ_PASSWORD: The Nexus IQ password or token required to access
      the Nexus IQ server and perform scans.

:Steps in the workflow:

1. **Gerrit Checkout**: The workflow starts by checking out the code from the
   Gerrit repository based on the provided branch and refspec. This ensures that
   the latest change is available for the build and scan.

2. **Run Gradle Build and CLM Scan**: The workflow runs a Gradle build to compile
   the project. After the build, the Nexus IQ scan checks the project's
   dependencies for any vulnerabilities or issues.

3. **Generate Application ID**: The workflow creates an application ID based on
   the Gerrit project and organization. This ID supports further scanning and
   reporting within Nexus IQ.

4. **Run Sonatype Lifecycle Scan**: The workflow triggers the Sonatype Lifecycle
   scan using the generated application ID. This scan checks the project's dependencies
   and licenses to ensure compliance.

:Example usage:

    - Trigger this workflow manually when a project change requires building with Gradle, scanning dependencies with Nexus IQ, and running a Sonatype Lifecycle scan.
    - The workflow builds the project, scans for vulnerabilities, and ensures compliance.

:Comment Trigger: recheck|reverify

:Workflow Steps Summary:

    1. Checkout the Gerrit change to get the latest code.
    2. Run Gradle build and Nexus IQ scan to analyze dependencies.
    3. Generate the application ID for the project.
    4. Trigger the Sonatype Lifecycle scan to check compliance.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
