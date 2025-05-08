.. _gerrit-nodejs-sonatype-lifecycle-docs:

##########################################
Gerrit Node.js Sonatype Lifecycle Workflow
##########################################

This workflow triggers when you merge changes to a Node.js project in Gerrit.
The workflow notifies the Gerrit system, builds the Node.js project, runs a Sonatype Lifecycle scan, and reports the scan's conclusion.

The main components of this workflow include:

1. Notify Gerrit about the workflow start and clear any existing votes.
2. Build the Node.js project to integrate the latest changes.
3. Run the Sonatype Lifecycle scan to analyze the project for security and license issues.
4. Report the workflow conclusion back to Gerrit, indicating the result.

:Required parameters:

    :GERRIT_BRANCH: The branch for the change.
        Default: None (Required)
    :GERRIT_CHANGE_ID: The ID for the change.
        Default: None (Required)
    :GERRIT_CHANGE_NUMBER: The Gerrit change number.
        Default: None (Required)
    :GERRIT_CHANGE_URL: The URL to the change.
        Default: None (Required)
    :GERRIT_EVENT_TYPE: The Gerrit event triggering the workflow.
        Default: None (Required)
    :GERRIT_PATCHSET_NUMBER: The patch number for the change.
        Default: None (Required)
    :GERRIT_PATCHSET_REVISION: The revision SHA for the patchset.
        Default: None (Required)
    :GERRIT_PROJECT: The project in Gerrit.
        Default: None (Required)
    :GERRIT_REFSPEC: The refspec of the change in Gerrit.
        Default: None (Required)

:Steps in the workflow:

1. **Notify Job Start**: Notify Gerrit about the job start by clearing any existing votes. Use the `gerrit-review-action` to send a notification to the Gerrit system.

2. **Allow Replication**: Wait for 10 seconds to allow the replication process to complete before moving on to the next steps.

3. **Build Node.js Project**: Use a reusable action (`node-build-action`) to build the Node.js project. This step ensures integration of the latest changes into the build process.

4. **Sonatype Lifecycle Scan**: After building the project, trigger a Sonatype Lifecycle scan using the `reuse-sonatype-lifecycle.yaml` workflow. This step analyzes the project for security vulnerabilities, licensing issues, and other quality concerns. Use the `NEXUS_IQ_PASSWORD` secret for authentication.

5. **Report Workflow Conclusion**: Report the workflow's conclusion back to Gerrit using the `gerrit-review-action`. Report the conclusion (success, failure, etc.) based on the workflow's execution results.

:Expected environment variables:

    :GERRIT_SERVER: The Gerrit server URL.
    :GERRIT_SSH_USER: The SSH user for Gerrit.
    :GERRIT_KNOWN_HOSTS: The known hosts for SSH connections to Gerrit.

:Expected secrets:

    :secrets.GERRIT_SSH_PRIVKEY: The private SSH key for authentication with Gerrit.
    :secrets.NEXUS_IQ_PASSWORD: The password for Nexus IQ used in the Sonatype Lifecycle scan.

:Example usage:

    - Trigger this workflow manually when you make a change in the Gerrit system that requires building the Node.js project and running the Sonatype Lifecycle scan.
    - The workflow steps ensure integration of the latest changes, conduct security and license scans, and report results back to Gerrit.

:Workflow Steps Summary:

    1. Notify job start to Gerrit and clear any existing votes.
    2. Allow replication for 10 seconds.
    3. Build the Node.js project to ensure it includes the latest changes.
    4. Run the Sonatype Lifecycle scan for security and license compliance.
    5. Report the conclusion of the workflow back to Gerrit.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
