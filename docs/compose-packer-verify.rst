.. _packer-verify-docs:

######################
Packer Verify Workflow
######################

The Packer Verify Workflow verifies the change using Packer. It checks the Packer files and ensures the environment is ready to execute Packer commands. The workflow identifies changes, sets up necessary environment variables, and runs Packer checks on the templates and associated variable files.

This workflow runs on GitHub when a trigger event, such as a Gerrit change or patch, occurs. It performs the following key tasks:

- Check the Packer templates and variables.
- Initialize the environment for Packer.
- Set up dependencies for OpenStack integration.
- Ensure the templates work as expected.

:Required parameters:

    :GERRIT_BRANCH: The branch for the change.
    :GERRIT_CHANGE_ID: The unique identifier for the change.
    :GERRIT_CHANGE_NUMBER: The Gerrit number for the change.
    :GERRIT_CHANGE_URL: The URL to the change.
    :GERRIT_EVENT_TYPE: The Gerrit event triggering the workflow.
    :GERRIT_PATCHSET_NUMBER: The number of the patchset.
    :GERRIT_PATCHSET_REVISION: The revision SHA of the patchset.
    :GERRIT_PROJECT: The project in Gerrit for the change.
    :GERRIT_REFSPEC: The refspec of the change in Gerrit.
    :ENV_VARS: GitHub variables to export as environment variables, passed in JSON format.
    :ENV_SECRETS: GitHub secrets to export as environment variables, passed in JSON format.

:Expected environment variables:

    :OS_CLOUD: The cloud provider environment to use. Default: "vex".
    :PACKER_VERSION: The version of Packer to use. Default: "1.9.1".
    :GERRIT_URL: The base URL for Gerrit.

:Steps in the workflow:

1. **Gerrit Checkout**: First, check out the change from Gerrit using the specified refspec, project, and URL.

2. **Check for Changes**: Determine if any files in the `packer/**` directory have changed. If no changes occur, stop the workflow.

3. **Setup Packer**: If changes occur, set up Packer with the specified version.

4. **Export Environment Variables**: Export environment variables defined in the `ENV_VARS` input for use throughout the process.

5. **Decode and Export Secrets**: Double base64 decode any secrets passed through `ENV_SECRETS` and store them as environment variables for later use.

6. **Create Cloud Environment File for Packer**: Decode a base64-encoded cloud environment file and save it in the necessary format for Packer.

7. **Create Cloud Configuration File for OpenStack**: Similarly, decode a base64-encoded cloud configuration file and save it to the appropriate location for OpenStack.

8. **Setup Python**: Set up Python 3.11, which some validation steps require.

9. **Install OpenStack Dependencies**: Install the required OpenStack client dependencies to ensure the environment is ready for testing.

10. **Check Packer Files**: Verify each Packer template and associated variable files, and log the results.

:Expected variables - Cloud Configuration:

    :CLOUDS_ENV_2XB64: Base64-encoded cloud environment file required for Packer.
    :CLOUDS_YAML_2XB64: Base64-encoded OpenStack YAML configuration file.

:Expected variables - Secrets:

    :secrets.ENV_SECRETS: Secrets passed from GitHub Actions for use during the workflow.

:Comment Trigger: recheck|reverify

:Example usage:

    - Trigger this workflow when you change something in the Gerrit system, ensuring the environment is properly configured and validated using Packer.
    - The variables and secrets provided ensure that the necessary configurations and credentials are securely applied throughout the process.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
