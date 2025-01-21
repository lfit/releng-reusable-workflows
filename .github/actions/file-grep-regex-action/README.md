<!--
[comment]: # SPDX-License-Identifier: Apache-2.0
[comment]: # SPDX-FileCopyrightText: 2024 The Linux Foundation
-->

# 📦 Action to run RegEx against a file using GREP

Returns values from a text file using the command-line tool, grep.

## file-grep-regex-action

## Usage Example

Call as a step in a larger composite action or workflow.

<!-- markdownlint-disable MD013 -->

```yaml
steps:
    - uses: lfit/releng-reusable-workflows/.github/actions/file-grep-regex-action@main # v1.0.0
      id: grep-file
      with:
          # https://regex101.com/r/axPzef/1
          flags: "-oP -m1"
          regex: '(?<=^\[testenv:docs\])*basepython = python\K(.*)'
          filename: "docs/tox.ini"
          no_fail: "true"
```

<!-- markdownlint-enable MD013 -->

## Inputs

<!-- markdownlint-disable MD013 -->

| Variable Name | Required | Default | Description                                           |
| ------------- | -------- | ------- | ----------------------------------------------------- |
| REGEX         | True     | N/A     | The regular expression to use                         |
| FILENAME      | True     | N/A     | The text file to search with GNU grep                 |
| FLAGS         | True     | N/A     | The flags passed to grep on the command line          |
| NO_FAIL       | False    | False   | Do not exit (if file not found or no string returned) |

<!-- markdownlint-enable MD013 -->

## Outputs

<!-- markdownlint-disable MD013 -->

| Variable Name    | Mandatory | Description                                    |
| ---------------- | --------- | ---------------------------------------------- |
| EXTRACTED_STRING | No        | The string extracted by the regular expression |

<!-- markdownlint-enable MD013 -->
