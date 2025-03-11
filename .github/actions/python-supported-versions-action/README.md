<!--
# SPDX-License-Identifier: Apache-2.0
# SPDX-FileCopyrightText: 2024 The Linux Foundation
-->

# 🐍 Extract Python Versions Supported by Project

Parses pyproject.toml and extracts the Python versions supported by the
project. Determines the most recent version supported and provides JSON
representing all supported versions, for use in GitHub matrix jobs.

## python-supported-versions-action

## Usage Example

<!-- markdownlint-disable MD013 -->

```yaml
  - name: "Get project supported Python versions"
    uses: lfit/releng-reusable-workflows/.github/actions/python-supported-versions-action@main
```

<!-- markdownlint-enable MD013 -->

## Outputs

<!-- markdownlint-disable MD013 -->

| Variable Name | Description                                             |
| ------------- | ------------------------------------------------------- |
| BUILD_PYTHON  | Most recent Python version supported by project         |
| MATRIX_JSON   | All Python versions supported by project as JSON string |

<!-- markdownlint-enable MD013 -->

## Workflow Output Example

For a Python project with the content below in its pyproject.toml file:

```console
requires-python = "<3.13,>=3.10"
readme = "README.md"
license = { text = "Apache-2.0" }
keywords = ["Python", "Tool"]
classifiers = [
  "License :: OSI Approved :: Apache Software License",
  "Operating System :: Unix",
  "Programming Language :: Python",
  "Programming Language :: Python :: 3",
  "Programming Language :: Python :: 3.12",
  "Programming Language :: Python :: 3.11",
  "Programming Language :: Python :: 3.10",
]
```

A workflow calling this action will produce the output below:

```console
Build Python: 3.12 💬
Matrix JSON: {"python-version": ["3.10","3.11","3.12"]}
```

## Implementation Details

This action produces output by parsing the lines containing:

```console
  "Programming Language :: Python :: 3.12",
  "Programming Language :: Python :: 3.11",
  "Programming Language :: Python :: 3.10",
```

This may be different from other tooling that parses the pyproject.toml file
for information, such as GitHub's actions/setup-python. That appears to use
the "requires-python" string. Parsing that would require logic beyond the
current scope of this action, but may be desirable to improve behavioural
consistency.
