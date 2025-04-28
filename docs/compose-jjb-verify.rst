.. _gerrit-compose-jjb-verify-docs:

##################################
Gerrit Compose JJB Verify Workflow
##################################

The JJB Verify Workflow runs when you submit a change to the Gerrit ci-management repository.
It verifies Jenkins Job Builder (JJB) configurations in a Gerrit change. The workflow checks out the code, sets up the environment for JJB, and verifies the job configurations in the project repository.

The main tasks performed by this workflow are:

1. Check out the Gerrit change.
2. Set up the Python environment for Jenkins Job Builder.
3. Run Jenkins Job Builder (JJB) to verify the job configurations.

:Required parameters:

    :GERRIT_BRANCH: The branch for the change.
        Default: None (Required)
    :GERRIT_CHANGE_ID: The unique identifier for the change.
        Default: None (Required)
    :GERRIT_CHANGE_NUMBER: The Gerrit number for the change.
        Default: None (Required)
    :GERRIT_CHANGE_URL: The URL to the change.
        Default: None (Required)
    :GERRIT_EVENT_TYPE: Gerrit event type that triggered the workflow.
        Default: None (Required)
    :GERRIT_PATCHSET_NUMBER: The patch number for the change.
        Default: None (Required)
    :GERRIT_PATCHSET_REVISION: The revision SHA for the patchset.
        Default: None (Required)
    :GERRIT_PROJECT: The project in Gerrit that the change belongs to.
        Default: None (Required)
    :GERRIT_REFSPEC: The refspec of the change in Gerrit.
        Default: None (Required)

:Required environment variables:

    :vars.GERRIT_URL: The base URL for Gerrit.
    :vars.JJB_VERSION: The version of Jenkins Job Builder to use.

:Steps in the workflow:

1. **Gerrit Checkout**: The workflow checks out the change from the Gerrit
   repository using the provided refspec, project, and other inputs. It ensures
   that the target repository and submodules are properly included.

2. **Setup Python Environment**: The workflow sets up Python 3.11 required
    to install and run Jenkins Job Builder.

3. **Run JJB Verify**: The workflow installs the specified version of Jenkins
   Job Builder (JJB) and runs it to verify the job configurations in the `jjb/`
   directory. It performs the following tasks:

   - Installs the required version of `jenkins-job-builder` via pip.
   - Sets up a configuration file for Jenkins Job Builder (`jenkins_jobs.ini`).
   - Runs the `jenkins-jobs test` command to verify the job configurations.

:Example usage:

    - Trigger this workflow manually when a Gerrit change involves Jenkins Job
      Builder configurations (in the `jjb/` directory).
    - The workflow ensures that the JJB configurations are valid and follow the
      correct structure.

:Comment Trigger: recheck|reverify

:Workflow Steps Summary:

    1. Checkout the Gerrit change to get the latest code and job configurations.
    2. Set up Python and install the required version of Jenkins Job Builder.
    3. Verify JJB configurations using the `jenkins-jobs test` command.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
