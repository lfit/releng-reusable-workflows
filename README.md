<!--- SPDX-License-Identifier: CC-BY-4.0 -->
<!--- SPDX-FileCopyrightText: 2023 The Linux Foundation -->

# releng-reusable-workflows

GitHub actions/workflows developed by the Linux Foundation Release Engineering
team.

## Naming Conventions

### File and Directory Names

## Location, Naming and Labelling of Actions

You can find actions in folders under the directory:

    .github/actions/

These can be of three basic types, as described in the [GitHub documentation]
(<https://docs.github.com/en/actions/sharing-automations/creating-actions/about-custom-actions>)

The table below describes the labelling and locations:

| Description | Directory Suffix |
| ----------- | ---------------- |
| Composite   | -action          |
| Javascript  | -javascript      |
| Docker      | -docker          |

Documentation takes the form of a README.md or README.rst file hosted alongside
the YAML file in one of the directory locations described above.

## Naming of Workflow YAML files

The GitHub actions and workflows in this repository conform to the
following naming conventions:

| Filename Prefix | Functional Description                        |
| --------------- | --------------------------------------------- |
| call-           | Top-level parent workflows that call others   |
| reuse-          | Reusable workflows implementing workflow_call |

## Name/Labelling within Workflows

Labelling must avoid consuming excessive space in the web/portal sidebar.
With this in mind, some standard abbreviations help minimise text overflow.

### Workflow Naming Prefixes

The table below lists prefixes that apply, inside square brackets, to
top-level workflow names.

| Prefix Letter | Function | Description       |
| ------------- | -------- | ----------------- |
| R             | Reusable | on: workflow_call |

The above prefixes can combine into composites, but when doing so, preserve
alphabetic ordering for consistency/readability.

e.g.
name: "[R] Python Build"

### Gerrit Workflows

Gerrit 'calling' workflows may have different trigger event types. Those
embedded behaviours mean a naming convention helps distinguish the different
types.

They fall into four specific types:

| Description           | Filename in Repository            |
| --------------------- | --------------------------------- |
| Gerrit (Verify)       | gerrit-verify[-project-repo].yaml |
| Gerrit (Merge)        | gerrit-merge[-project-repo].yaml  |
| Gerrit (Generic)      | gerrit-[project-repo].yaml        |
| Gerrit (Word/Trigger) | gerrit-[word].yaml                |
