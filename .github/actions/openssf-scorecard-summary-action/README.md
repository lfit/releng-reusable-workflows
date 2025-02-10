<!--
# SPDX-License-Identifier: Apache-2.0
# SPDX-FileCopyrightText: 2025 The Linux Foundation
-->

# 📄 OpenSSF Scorecard Summary Output

Provides summary output with a URL/link to the scorecard report.

Note: The upstream action has not implemented this feature.

## openssf-scorecard-summary-action

## Implementation Notes

Summary output originates from a separate job in the calling workflow. This is
because the OpenSSF upstream action performs security checks before
it will upload results. The calling workflow audit can block the reporting
of results. Be mindful during the implementation of surrounding
action/workflow code.

## Usage Example: Reusable Workflow

<!-- markdownlint-disable MD013 -->

```yaml
jobs:
    openssf-scorecard: [...]

    # Summary output MUST be in a separate job
    # (refer to the ossf/scorecard-action documentation)
    summary-output:
        name: "Link"
        needs: openssf-scorecard
        runs-on: ubuntu-24.04
        steps:
            - name: "Provide link to scorecard"
              # The upstream action does not provide any useful summary output
              # the action below adds a hyperlink to the OpenSSF Scorecard/report
              uses: lfit/releng-reusable-workflows/.github/actions/openssf-scorecard-summary-action@main
```

<!-- markdownlint-enable MD013 -->

## Example Implementation: Reusable Workflow

See: <https://github.com/lfit/releng-reusable-workflows/blob/main/.github/workflows/reuse-openssf-scorecard.yaml>
