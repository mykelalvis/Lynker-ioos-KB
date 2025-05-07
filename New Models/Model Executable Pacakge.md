---
date created: 2024-09-24 11:31
aliases:
  - MEP
---

A Model Executable Package (MEP) is the packaged result of running the equivalent of a [[continuous integration]] build against a [[Model Executable Catalog]] to package an [[artifact]].

To package an MEP
- A given model's source code must be fully comprehended
- All [[Compile Dependency|Compile Dependencies]] must be cataloged
- All cataloged [[Compile Dependency|Compile Dependencies]] must be resolved for the given catalog
- The source must be compiled for the architectures specified in the [[Model Executable Catalog]]
- The various compiled sources must be collected by the packaging process into an archive.
- Optionally, the packaging process may include within the archive:
    - All of the required dependencies
    - Logs of the compilation process


An MEP is required to define only the attributes that produced it.  Thus, a given MEP _should_ be replicable from it's catalog, assuming that all the existing sources remain available.  A given MEP _may_ package all of its dependencies within itself, up to and including the layers for the containers that were used to execute any processing steps.

An MEP manifests within the system as an archive that contains the packaged code (compiled for one or more given architectures, if the code is of a compiled language) as well as a list of the [[Compile Dependency|Compile Dependencies]].  Once an MEP is created, the 

