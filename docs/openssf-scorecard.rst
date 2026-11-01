.. _openssf-scorecard:

##########################
OpenSSF Scorecard Workflow
##########################

The OpenSSF Scorecard Workflow runs the OpenSSF Scorecard to assess the security and best practices of a project. The Scorecard includes checks such as branch protection and maintenance evaluations, and it runs periodically to keep the project's security posture current.

The workflow calls the ``openssf-scorecard.yaml`` reusable workflow from
``lfreleng-actions/security-workflows``, pinned to a release commit SHA. That
workflow validates its inputs in a separate job, performs the following tasks
in a scan job behind a block-mode harden-runner, then adds a link to the
published Scorecard report to the run summary:

1. **Checkout Code**: The workflow checks out the code for analysis.
2. **Run OpenSSF Scorecard**: The workflow runs the OpenSSF Scorecard to assess the repository's security posture.
3. **Upload Results**: The workflow uploads the results of the scorecard scan.
4. **Upload to Code Scanning Dashboard**: The workflow uploads the SARIF file to GitHub's code-scanning dashboard.

:Required parameters:

    :None

:Required secrets:

    :None

:Required permissions:

    The calling job grants ``contents: read``, ``security-events: write``,
    ``id-token: write`` and ``actions: read``. The scan job requests all four,
    and a called job cannot hold more than its caller grants.

:Steps in the workflow:

1. **Checkout Code**: The workflow checks out the repository code for analysis.

2. **Run OpenSSF Scorecard**: The workflow uses the OpenSSF Scorecard to analyze
   the repository, assessing it against security metrics.

3. **Upload Results**: The workflow uploads the results of the scorecard scan as
   a SARIF file.

4. **Upload to Code Scanning Dashboard**: The results upload to GitHub's
   code-scanning dashboard for review.

:Example usage:

    - Trigger this workflow manually or on a schedule to periodically check the
      security posture of a repository using OpenSSF Scorecard.

    - The results upload for review, helping improve the project's security.

:Workflow Steps Summary:

    1. Checkout the code for analysis.
    2. Run the OpenSSF Scorecard to assess the repository's security.
    3. Upload the results of the scan.
    4. Upload the results to the code-scanning dashboard for visibility.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
