.. _compose-docker-verify-docs:

##############################
Compose Docker Verify Workflow
##############################


The workflow executes a docker build task to verify an test image build and discards the
test image upon completion. The workflow triggers when you submit a Gerrit change request.


It integrates with Gerrit and performs the following tasks:

1. Check out the code from the Gerrit repository.
2. Set up the Python environment for Docker-related tasks.
3. Log in to Nexus3 and DockerHub registries using provided credentials.
4. Retrieve the Docker image tag using specific methods.
5. Build and push the Docker image using the retrieved tag.

:Required parameters:

    :GERRIT_BRANCH: Git branch to fetch for the build.
    :GERRIT_CHANGE_ID: The unique identifier for the change.
    :GERRIT_CHANGE_NUMBER: The Gerrit number for the change.
    :GERRIT_CHANGE_URL: The URL to the change.
    :GERRIT_EVENT_TYPE: The Gerrit event triggering the workflow.
    :GERRIT_PATCHSET_NUMBER: The patch number for the change.
    :GERRIT_PATCHSET_REVISION: The revision SHA for the patchset.
    :GERRIT_PROJECT: The project in Gerrit for the change.
    :GERRIT_REFSPEC: The refspec of the change in Gerrit.
    :CONTAINER_TAG_METHOD: The method to locate the Docker container's tag (e.g., `latest`, `stream`, `git-describe`, or `yaml-file`).
    :CONTAINER_TAG_YAML_DIR: The location of the YAML file with Docker tag information (required if `CONTAINER_TAG_METHOD` is `yaml-file`).
    :DOCKER_BUILD_ARGS: Arguments for the Docker build command.

:Required secrets:

    :secrets.NEXUS3_PASSWORD: The Nexus3 organization's user password.
    :secrets.DOCKERHUB_PASSWORD: The DockerHub organization's user password.

:Steps in the workflow:

1. **Gerrit Checkout**: Check out the change from the Gerrit repository using the provided refspec, project, and other inputs.

2. **Setup Python Environment**: Set up Python 3.8 for Docker operations.

3. **Nexus3 and DockerHub Login**: Log in to Nexus3 and DockerHub using the provided credentials to ensure the image can push to both registries.

4. **Get Docker Image Tag**: Retrieve the Docker image tag based on the selected method:

   - **latest**: Use the `latest` tag.
   - **stream**: Use the branch name as the tag.
   - **git-describe**: Use the Git description command to generate a tag.
   - **yaml-file**: Retrieve the tag from a `container-tag.yaml` file.

    :container-tag.yaml example:

    .. code-block:: yaml

       ---
       tag: 1.0.0

5. **Docker Build and Push**: Build and push the Docker image using the tag obtained from the previous step. Push the image to the configured registries with the specified build arguments.

:Expected environment variables:

    :GERRIT_URL: The base URL for Gerrit.
    :NEXUS3_REGISTRY: The Nexus3 registry URL.
    :DOCKERHUB_USER: The DockerHub username.
    :NEXUS3_USER: The Nexus3 username.

:Expected secrets:

    :secrets.NEXUS3_PASSWORD: The Nexus3 password for authentication.
    :secrets.DOCKERHUB_PASSWORD: The DockerHub password for authentication.

:Example usage:

    - Trigger this workflow manually when a change occurs in the Docker project in Gerrit that requires building and pushing a Docker image.
    - Use different methods for determining the Docker tag and automatically handle the build and push to the registries.

:Workflow Steps Summary:

    1. Check out the Gerrit change to get the latest code.
    2. Set up the Python environment for Docker tasks.
    3. Log in to Nexus3 and DockerHub registries.
    4. Retrieve the Docker image tag based on the specified method.
    5. Build the Docker image and push it to the registries.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
