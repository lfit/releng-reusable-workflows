<!--
# SPDX-License-Identifier: Apache-2.0
# SPDX-FileCopyrightText: 2025 The Linux Foundation
-->

# 🌍 Download Content from URL

Uses WGET to download the content from a given URL.

## url-download-action

## Usage Example

<!-- markdownlint-disable MD013 -->

```yaml
steps:
  - name: "Download build script"
    id: download-file
    uses: lfit/releng-reusable-workflows/.github/actions/url-download-action@main
    with:
      url: https://raw.githubusercontent.com/o-ran-sc/o-du-l2/refs/heads/main/sonarqube-cloud-build.sh
```

<!-- markdownlint-enable MD013 -->

## Implementation

Uses variations on the shell command below, depending on inputs:

`wget [URL] -O [path] --no-clobber -q`

## Inputs

<!-- markdownlint-disable MD013 -->

| Input        | Required | Description                            | Default | Example                       |
| ------------ | -------- | -------------------------------------- | ------- | ----------------------------- |
| URL          | True     | URL to download                        | None    | <https://linuxfoundation.org> |
| EXIT_ON_FAIL | False    | Exit with error if download fails      | True    | True/False                    |
| PATH         | False    | Saves file to given path               | None    | /tmp/requirements.txt         |
| NO_CLOBBER   | False    | Will NOT overwrite existing local file | False   | True/False                    |

<!-- markdownlint-enable MD013 -->
