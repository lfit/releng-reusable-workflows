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

- **Verify**: This job calls the `action-pin-audit.yaml` workflow from `lfreleng-actions/security-workflows`, pinned to a release commit SHA, with the contents read permission. After validating its inputs, that workflow audits each `uses:` reference behind a block-mode harden-runner and fails on any that does not name a commit SHA. On pull requests it audits the workflow files the change touches.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
