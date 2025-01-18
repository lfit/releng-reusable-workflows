<!--
[comment]: # SPDX-License-Identifier: Apache-2.0
[comment]: # SPDX-FileCopyrightText: 2024 The Linux Foundation
-->

# 🎟️ Performs a Sonatype Lifecycle (Nexus IQ) Scan

Performs a Sonatype Lifecycle scan and uploads the results to the server.

## sonatype-lifecycle-action

## Usage Example

Pass the required server and authentication details/credentials.
Other inputs are discretionary and set to useful defaults.

```yaml
steps:
    - name: "Run Sonatype Lifecycle scan"
      # yamllint disable-line rule:line-length
      uses: lfit/releng-reusable-workflows/.github/actions/sonatype-lifecycle-action@main
      with:
          NEXUS_IQ_SERVER: "${{ vars.NEXUS_IQ_SERVER }}"
          NEXUS_IQ_USERNAME: "${{ vars.NEXUS_IQ_USERNAME }}"
          NEXUS_IQ_PASSWORD: "${{ secrets.NEXUS_IQ_PASSWORD }}"
```

## Inputs

<!-- markdownlint-disable MD013 -->

| Variable Name     | Required | Default      | Description                                 |
| ----------------- | -------- | ------------ | ------------------------------------------- |
| NEXUS_IQ_SERVER   | True     | N/A          | JSON array of key/value pairs               |
| NEXUS_IQ_USERNAME | True     | N/A          | Fixed preamble/string to embed/inject       |
| NEXUS_IQ_PASSWORD | True     | N/A          | When set false, checks for presence         |
| JAVA_DISTRIBUTION | False    | "temurin"    | JAVA SE distribution for the Nexus CLI tool |
| JAVA_VERSION      | False    | 17           | Java runtime for the Nexus CLI tool         |
| IQ_CLI_VERSION    | False    | "1.179.0-01" | Specific version of Nexus CLI to setup/run  |
| APPLICATION_ID    | False    | $org-$repo   | Organisation and project name in Nexus IQ   |
| SCAN_TARGETS      | False    | "."          | Location of file(s) or folder(s) to scan    |

<!-- markdownlint-enable MD013 -->

The APPLICATION_ID default is:

`${{ github.repository_owner }}-${{ github.event.repository.name }}`

Note: when testing in a fork this must be manually overridden for report
uploads to succeed.
