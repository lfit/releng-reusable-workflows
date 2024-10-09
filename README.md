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

The table below lists prefixes that applied, inside square brackets, to
top-level workflow names.

| Prefix Letter | Function     | Description               |
| ------------- | ------------ | ------------------------- |
| D             | Dispatch     | on: workflow_dispatch     |
| M             | Matrix       | Runs matrix jobs          |
| R             | Reusable     | on: workflow_call         |
| V             | Verification | Verification/Checks/Tests |

The above prefixes can combine into composites, but when doing so, preserve
alphabetic ordering for consistency/readability.

e.g.
name: "[MV] Check Action SHA Pinning"

### Gerrit Workflows

Gerrit 'calling' workflows may have different trigger event types. Those
embedded behaviours mean a naming prefix helps distinguish the different
types.

They fall into three specific types:

| Workflow Prefix | Description           | Filename in Repository            |
| --------------- | --------------------- | --------------------------------- |
| [GV]            | Gerrit (Verify)       | gerrit-verify[-project-repo].yaml |
| [GM]            | Gerrit (Merge)        | gerrit-merge[-project-repo].yaml  |
| None            | Gerrit (Generic)      | gerrit-[project-repo].yaml        |
| None            | Gerrit (Word/Trigger) | gerrit-[word].yaml                |

Discussion Points (on the table above)

We could locate deployed workflows in a separate folder structure from
the reusables, which describes their deployment locations more
accurately, such as:

    /deployed/project/repo (standard instances of repository workflows)
    /deployed/project/.github (workflows that run across an organisation)
    /deployed/project/gerrit (e.g. keyword triggers)

These workflows will have specific baked-in values and/or behaviours.

Do we want these stored alongside reusables and other templates?
In most cases, under the other naming proposals above, these would have
a call- prefix in the filename, which seems redundant when deployed based
on known patterns?
