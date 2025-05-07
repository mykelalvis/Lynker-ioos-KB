# How Does It Run?

*The Runtime Environment*

Equally important to the result of the work we do is the environment upon which a given model will run.  

In the "supercomputer" environments, this isn't actually a thing anyone speaks of.  
> The build environment is the runtime environment is the only environment.

However, in cloud native computing, and really in any virtualized platform, the code that is executed may or may not have been produced on the system upon which it is executed.  Usually, this is performed as part of an automated process when code is committed to source control: nominally, this is an attribute do "[[continuous integration]]".  

However, within the hydrological modelling world, it appears to be the case that very few clean methods of reliably producing an executable that will run on a targeted platform are present.  Certainly the initial method of "build it on the supercomputer and run it on the same supercomputer" is reasonably useful.  However, that is no longer the underlying fabric and replicating that situation merely delays the amount of time for getting models to run in a cloud-native fashion.

To that end, we have defined [[Model Build Environment|what it takes to build]] a model and now we need to define what it takes to run that model.

First, we should denote that both the build environment and the runtime environment are themselves instances of an [[environment]], per that definition.  Environments are themselves instances of a [[platform]].    If that is confusing, then please see the linked note.

