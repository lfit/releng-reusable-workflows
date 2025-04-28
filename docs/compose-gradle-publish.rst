.. _compose-gradle-publish-docs:

###############################
Compose Gradle Publish Workflow
###############################

This workflow publishes artifacts to a Nexus repository using Gradle.
It triggers manually via GitHub's `workflow_call` event that is  and integrates with Gerrit to perform the following tasks:

1. Check out the code from the Gerrit repository.
2. Set up the Java Development Kit (JDK) and Python environment.
3. Build the project using Gradle.
4. Publish the artifacts to the specified Nexus repository.

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
    :JDK_VERSION: The version of OpenJDK to use (default: 17).
    :NEXUS_URL: The Nexus repository URL where you publish the artifacts.
    :DIRECTORY: The build location of the artifacts.
    :FILE_EXTENSION: The artifacts file extension (default: jar).

:Required secrets:

    :secrets.NEXUS_PASSWORD: The password for the Nexus repository.

:Steps in the workflow:

1. **Gerrit Checkout**: Check out the change from the Gerrit repository using the provided refspec, project, and other inputs.

2. **Setup JDK and Python Environment**: Set up the specified JDK version and Python 3.8 for build operations.

3. **Build with Gradle**: Use Gradle to build the project, creating the necessary artifacts.

4. **Publish Artifacts**: Publish the built artifacts to the Nexus repository using the provided credentials and URL.

:Expected environment variables:

    :GERRIT_URL: The base URL for Gerrit.
    :NEXUS_URL: The Nexus repository URL.

:Expected secrets:

    :secrets.NEXUS_PASSWORD: The Nexus password for authentication.

:Example usage:

    - Trigger this workflow manually when a change occurs in the Gradle project in Gerrit that requires building and publishing artifacts.
    - Use the specified JDK version and directory to handle the build and publish process.

:Comment Trigger: recheck|reverify

:Workflow Steps Summary:

    1. Check out the Gerrit change to get the latest code.
    2. Set up the JDK and Python environment.
    3. Build the project using Gradle.
    4. Publish the artifacts to the Nexus repository.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
