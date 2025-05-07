---
date created: 2024-09-24 11:34
aliases:
  - Compile Dependencies
---

A Compile Dependency is a [[CEDP Dependency]] that is _not_ required to be resolved in order to _execute_ a given [[CEDP]].

A Compile Dependency frequently looks like an [[Execution Dependency]], but it is only relevant for the compilation process of the [[CEDP]], not the execution process.

Like an [[Execution Dependency]], a Compile Dependency might be the installation of a package, the production of some file (like configuration), or the execution of some external process.  As the [[CEDP Dependency Resolver]] is extensible to any relatively-idempotent action that a language can take, Compile Dependency items are highly flexible.  However, it is generally safest to stick with the ones provided by the system.

The following types of Compile Dependency items exist

- [[Package Dependency|Package Dependencies]] install one or more available packages onto a given filesystem.
- [[File Dependency|File Dependencies]] are files, with some given content, that need to exist on the compilation filesystem.
- [[Data Dependency|Data Dependencies]] are usually files of working data or databases.  For a Compile Dependency, these are almost always test data.
- [[Process Dependency|Process Dependencies]] are executions of some process that must be performed prior to the compilation.  These processes _must_ be idempotent, as they could very likely be attempted many times across the lifecycle of the production of a [[Compiled Executable Data Package|CEDP]].
-
