<!-- SPDX-FileCopyrightText: 2024 Simon Gene Gottlieb
     SPDX-License-Identifier: CC-BY-4.0
-->

# CPM check version

This is a helper packages to check for CPM updates.

The following script will check for updates of all CPM packages, excluding `cereal`:

```
CPMAddPackage("gh:SGSSGene/cpm_check_version@0.1.1")
enable_testing()
cpm_check_version(cereal)
```
