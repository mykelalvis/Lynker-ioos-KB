---
date created: 2024-09-24 11:34
aliases:
  - Execution Dependencies
---

An Execution Dependency is a [[CEDP Dependency]] that is required to be resolved before a given [[CEDP]] can be executed.

It is some artifact that describes filesystem data or processes that must have occurred prior to attempting to execute the [[CEDP]].

These will be things like packages that must be installed, files that must exist and possibly be validated, or the execution of some process.
