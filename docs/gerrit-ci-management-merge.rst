.. _gerrit-ci-management-merge-docs:

###################################
Gerrit ci-management Merge Workflow
###################################

The ci-management Merge Workflow manages the merge process of a change in Gerrit and runs Jenkins Job Builder (JJB) to apply the configuration changes.
The workflow reports the conclusion back to Gerrit.

The workflow performs the following tasks:

1. Notifies the job start and clears any previous votes in Gerrit.
2. Merges the JJB configuration using the Gerrit change details.
3. Reports the workflow conclusion back to Gerrit.

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

:Required secrets:

    :secrets.JOBBUILDER_PROD_PSW: Jenkins admin user account token.
    :secrets.GERRIT_SSH_PRIVKEY: Private SSH key used for leaving comments in Gerrit.

:Steps in the workflow:

1. **Notify Job Start**: The workflow notifies the start of the job and clears
   any existing votes in Gerrit using the `gerrit-review-action`.

2. **JJB Merge**: The workflow uses `jenkins-job-builder` (JJB) to merge
   the Jenkins jobs configuration by applying the changes to the Jenkins server. It
   installs the necessary Python dependencies and configures JJB using the provided
   credentials and configuration files.

3. **Clean Up**: After merging, the workflow cleans up any temporary files created
   during the process, including configuration files and property files.

4. **Report Workflow Conclusion**: The workflow reports the conclusion
   to Gerrit using the `gerrit-review-action`. This includes updating
   the Gerrit change with the result of the workflow.

:Example usage:

    - Trigger this workflow manually when a change in Gerrit requires applying
      configuration changes via Jenkins Job Builder.
    - The workflow updates the jobs and applies the changes while reporting
      the status back to Gerrit.

:Comment Trigger: ``remerge``

:Workflow Steps Summary:

    1. Notify the start of the job and clear any previous votes in Gerrit.
    2. Use Jenkins Job Builder to apply the changes.
    3. Clean up temporary files.
    4. Report the workflow conclusion back to Gerrit.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
