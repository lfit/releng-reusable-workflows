<!--- SPDX-License-Identifier: CC-BY-4.0 -->
<!--- SPDX-FileCopyrightText: 2023 The Linux Foundation -->

# releng-reusable-workflows

GitHub actions and workflows developed by the Linux Foundation Release Engineering team.

## Naming Conventions

### File and Directory Names

The GitHub actions and workflows in this repository conform to the following naming conventions:

| Filename Prefix  | Functional Description                        |
| ---------------- | --------------------------------------------- |
| call-            | Top-level parent workflows that call others   |
| composite-       | Composite actions                             |
| reuse-           | Reusable workflows implementing workflow_call |
| call-collection- | Workflows that combine reusable workflows     |

All directories under `.github/actions/` have the following naming suffix:

`<function>-action`

## Internal Labelling of Workflows

Reusable workflows are internally named with the prefix "[R]", followed by the workflow filename stripped
of the "reuse-" prefix and ".yaml" suffix. An example of this in the workflow YAML file would be:

`name: "[R] node-sonatype-lifecycle"`

This is to mark their functions in the web portal without consuming excessive space in the sidebar.
