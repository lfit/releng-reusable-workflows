.. _reuse-verify-github-actions:

####################################
Reuse Verify GitHub Actions Workflow
####################################

This workflow ensures that action and workflow calls use specific SHA commit values.

### Trigger Events:

- **workflow_dispatch**: Manually trigger the workflow.
- **pull_request**: Trigger the workflow for pull requests targeting the `main` or `master` branches, specifically for changes in the `.github/**` path.

### Permissions:

- This workflow does not need extra permissions.

### Concurrency:

- The workflow groups by `${{ github.workflow }}-${{ github.ref }}` and cancels any in-progress runs to prevent overlap.

### Jobs:

- **Verify**: This job uses the `lfit/releng-reusable-workflows` repository to verify GitHub Actions. It reads the contents permission and uses the workflow at the specified SHA `ac846b1cfeaf3a7cac6f28413a5206afc9951464` (version 0.2.11).

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
