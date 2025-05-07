A CEDPDependency is a generic term to describe one of two different things:

- a [[dependency]] that must exist in order to _produce or compile_ a [[Compiled Executable Data Package|CEDP]] , further identified as a [[Compile Dependency]].
- a [[dependency]] that must be resolved in order to _execute_ a [[Compiled Executable Data Package|CEDP]] is known as an [[Execution Dependency]].

It is reasonable to denote that _test data_ is a [[Compile Dependency]] in that testing a compiled model should be done as part of compilation/artifact-generation

It is reasonable to denote that _model execution data_ (aka "the data and forcing data and all the other data") can be referred to as an [[Execution Dependency]] as it is required for the execution of the model.

Any given dependency can be thought of as one of two different actual data artifacts:

If an artifact is nominally "small" (defined by the execution), then the artifact is expected to be some file or archive that can be copied or extracted to the local scratch filesystem.

If an artifact is nominally "large" but not modified,  then that artifact is expected to be resolved to a read-only filesystem.

If an artifact is nominally "large" but expected to be modified, then the artifact is resolved as a _copy_ of a (previously resolved) read-only filesystem.  This means that the data is resolved once as a read-only system and then copied so that the resulting managed/modeled data is separate from the source data.



