.. _gerrit-compose-required-info-yaml-verify-docs:

#################################################
Gerrit Compose Required INFO Yaml Verify Workflow
#################################################

This workflow triggers when you create or update an INFO-yaml file in Gerrit.
This job verifies that ``INFO.yaml`` file changes using the schema defined in releng/global-jjb.

## Schema

See: <"https://github.com/lfit/releng-global-jjb/blob/master/schema/info-schema.yaml">


The workflow ensures the `INFO.yaml` file against a predefined schema, and confirms that the repository listed in the `INFO.yaml` matches the project from the Gerrit change.

The main tasks performed by this workflow are:

1. Check if you modified or created the `INFO.yaml` file.
2. Ensure that you isolate the `INFO.yaml` changes and do not combine them with other changes.
3. Download and check the `INFO.yaml` file against a schema.
4. Verify that the `INFO.yaml` file lists one repository and matches the current project.

:Required parameters:

    :GERRIT_BRANCH: The branch that the change targets.
    :GERRIT_CHANGE_ID: The unique identifier for the change.
    :GERRIT_CHANGE_NUMBER: The Gerrit number for the change.
    :GERRIT_CHANGE_URL: The URL to the change.
    :GERRIT_EVENT_TYPE: The Gerrit event triggering the workflow.
    :GERRIT_PATCHSET_NUMBER: The patch number for the change.
    :GERRIT_PATCHSET_REVISION: The revision SHA for the patchset.
    :GERRIT_PROJECT: The project in Gerrit for the change.
    :GERRIT_REFSPEC: The refspec of the change in Gerrit.
    :TARGET_REPO: The target GitHub repository needing the required workflow.

:Required secrets:

    :secrets.GERRIT_SSH_REQUIRED_PRIVKEY: The SSH Key for the authorized user account.

:Steps in the workflow:

1. **Gerrit Checkout**: Check out the code from the Gerrit repository using the provided refspec, branch, and other inputs. Access the target repository specified in the `TARGET_REPO` input.

2. **Setup Python Environment**: Set up Python 3.11 for validation tasks.

3. **Verify Change Isolation**: Ensure that you isolate the `INFO.yaml` file changes and do not combine them with other file changes. If you do not find the `INFO.yaml` file or combine it with other changes, exit the workflow.

4. **Download Valid Schema**: If the `INFO.yaml` file is valid and isolated, download the `info-schema.yaml` and `yaml-verify-schema.py` files from the specified URL.

5. **Info File Check**: Check the `INFO.yaml` file:

   - Check the syntax of the `INFO.yaml` file using `yamllint`.
   - Check the file against the `info-schema.yaml` using `yaml-verify-schema.py`.
   - Ensure that the `INFO.yaml` file lists one repository and matches the project specified in the Gerrit inputs.

:Expected environment variables:

    :GERRIT_URL: The base URL for Gerrit.

:Expected secrets:

    :secrets.GERRIT_SSH_REQUIRED_PRIVKEY: The SSH private key required for authentication with Gerrit.

:Example usage:

    - Trigger this workflow manually when a Gerrit change requires checking the `INFO.yaml` file.
    - The workflow checks if you isolate the `INFO.yaml` file and checks it according to the predefined schema, ensuring it references the correct repository.

:Comment Trigger: recheck|reverify

:Workflow Steps Summary:

    1. Check out the Gerrit change to get the latest code.
    2. Set up the Python environment for the validation tasks.
    3. Verify that you isolate `INFO.yaml` changes.
    4. Download the schema and validation script.
    5. Check the `INFO.yaml` file and ensure it matches the expected repository.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
