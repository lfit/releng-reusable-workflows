# 🔑 JSON Variable Key/Value Lookup

Action to perform a lookup from a JSON string containing a simple array of
key/value pairs.

## json-key-value-lookup-action

## Usage Example

Pass a JSON string as an input to the action, along with the key to lookup.
The action will return the corresponding value, using JQ to pass the JSON.

```yaml
- name: "Get ticket reference from JSON lookup table"
  uses: lfit/releng-reusable-workflows/.github/actions/json-key-value-lookup-action@main
  with:
      json: ${{ inputs.issue_id_lookup_json }}
      key: ${{ env.key }}
```

## Example JSON Variable

Provide JSON as an array containing simple key/value pairs, as shown below.

```json
[
    { "key": "dependabot[bot]", "value": "CIMAN-33" },
    { "key": "John Doe", "value": "CIMAN-1000" }
]
```

In the example above, the repository has a variable configured called: ISSUE_ID_LOOKUP_JSON

Set/define this under:

[ Github Repository ] -> Settings -> Secrets and variables -> Actions -> Variables

## Inputs

The action does not have any default values and both input parameters are mandatory.

| Variable Name | Required | Description                                |
| ------------- | -------- | ------------------------------------------ |
| JSON          | True     | JSON dictionary containing key/value pairs |
| KEY           | True     | Key to use to extract corresponding value  |
| DEFAULT       | False    | Default value in the event lookup fails    |

### Note 🗒️

In the event of a lookup failure, the action will exit with an error. Specifying
a default value will prevent this failure from taking place, allowing the calling
action/workflow to take alternative action, preventing the failure from
stopping the entire pipeline.

## Outputs

| Variable Name | Description                                            |
| ------------- | ------------------------------------------------------ |
| VALUE         | The value corresponding to the key (provided as input) |

## Requirements/Dependencies

Ensure validated JSON (in the format specified above) is in the variable.
The action performs validation before the lookup and will otherwise throw
an error.

If the key is not represented in the lookup table, the action will exit with an
error. This behaviour will result in the calling action/workflow failing.
Prevent this by setting a default return value, allowing upstream code to take
alternative paths.

<!--
[comment]: # SPDX-License-Identifier: Apache-2.0
[comment]: # SPDX-FileCopyrightText: 2024 The Linux Foundation
-->
