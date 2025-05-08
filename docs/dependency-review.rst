.. _dependency-review-docs:

##########################
Dependency Review Workflow
##########################

This Dependency Review Workflow scans dependency manifest files that change as part of a pull request. It surfaces known-vulnerable versions of the packages declared or updated in the pull request. When the workflows runs, it prevents pull requests that introduce known-vulnerable packages from merging.

### Trigger Events:

- **pull_request**: Activates when someone creates or updates a pull request.

### Required Permissions:

    :contents: read - Required to allow the workflow to read the contents of the repository.

### Workflow Steps:

1. **Checkout Repository**:
   The workflow checks out the repository using the `actions/checkout` action. This step ensures that the code from the pull request is available for review and dependency checks.

2. **Dependency Review**:
   The workflow uses the `actions/dependency-review-action` to perform a dependency review. It scans the dependency manifest files to identify any known-vulnerable packages and surfaces them. If it finds any vulnerabilities, it reports them, and the review settings may prevent the pull request from merging.

### Example Usage:

- This workflow automatically activates whenever someone creates or updates a pull request. It reviews the dependencies of the change and surfaces any vulnerabilities found in the updated or added packages.

### Workflow Steps Summary:

    1. Checkout the repository to ensure the latest changes undergo review.
    2. Scan the dependencies in the manifest files for vulnerabilities using the `dependency-review-action`.
    3. Surface any known vulnerabilities and, if necessary, prevent the merging of the pull request.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
