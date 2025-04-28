.. _lint-github-actions-workflows:

#############################
Lint GitHub Actions Workflows
#############################

This workflow automatically lints GitHub Actions workflows and ensures that commit messages follow semantic conventions.

### Trigger Events:

- **push**: This workflow activates on any push event to the repository.
- **pull_request**: This workflow also activates for pull requests, checking commit messages.

### Permissions:

    :contents: read

### Workflow Jobs:

#### 1. **actionlint**:
   - **Runs-on**: `ubuntu-latest`
   - **Purpose**: This job checks the formatting of GitHub Actions workflow files using `actionlint`.
   - **Steps**:
     - **Checkout** the repository.
     - **Download actionlint**: Downloads the `actionlint` tool for linting workflow files.
     - **Check workflow files**: Runs `actionlint` on all the workflow files to ensure they meet standards.

#### 2. **commit-message**:
   - **Runs-on**: `ubuntu-latest`
   - **Condition**: This job runs when a pull request event occurs (`github.event_name == 'pull_request'`).
   - **Purpose**: This job checks the commit message for conformity to semantic versioning conventions and ensures the commit message matches the PR title.
   - **Steps**:
     - **Check PR for semantic commit message**: Ensures that the pull request has a semantic commit message using the `action-semantic-pull-request` action.
     - **Checkout the PR code**: Checks out the pull request at the last commit to review commit messages.
     - **Install gitlint**: Installs the `gitlint` tool to review commit messages.
     - **Run gitlint**: Runs `gitlint` on the commits to ensure they conform to the project’s commit message rules.

### Example usage:

- Use this workflow to automate the linting of GitHub Actions workflows, ensuring that workflow files meet formatting standards and follow best practices.
- It also ensures that all pull request commits adhere to semantic versioning standards, providing clear and consistent commit messages.

### Workflow Steps Summary:

    1. Lint the GitHub Actions workflow files to ensure correctness.
    2. Review pull request commit messages for semantic correctness.
    3. Run `gitlint` to enforce commit message guidelines.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
