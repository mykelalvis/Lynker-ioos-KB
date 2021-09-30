---
date updated: '2021-03-16T03:55:27-05:00'

---

# Build

<dl>
<dt>build</dt>
<dd>a process that might produce a potential artifact</dd>
</dl>

The build is one of the most common software activities.  It is the process by which we determine that some arbitrary collection of bytes has become less arbitrary and more important.

Frequently, a build has some ceremony around it.  One such task would be invoking a "build tool", such as `make` or `mvn` or `rake`.

Given proper management, a build will execute on some well-defined [[environment]] and will defile a [[lifecycle]] for success.  The outcome of traversing the lifecycle states should be identifiable as either an [[artifact]] (which we might care about) or a "failed build" (which we would likely discard) through a failure in one or more aspects of [[gatekeeping]].

## Attributes

### Repeatable

Builds do not _necessarily_ follow the same path every time with the same results.  For example, a build might exist that only produces a viable [[artifact]] on alternate Thursdays, but does not inform the [[audience]] of this fact.

Such builds could easily be labeled as "bad."

A executed build that responds identically for a given set of [[source code]] is referred to as "repeatable" or "deterministic".  If the criteria for setting up the [[build environment]] are met, then the build should produce the same result (success or failure) every time it is executed.

Note that "repeatable" does not imply that the [[artifact]] produced by a build is binary-identical to any other artifact produced by that same build.  Internal metadata, often in the form of timestamps, often prevent this from being the case.  However, barring a change in [[source code]], a repeatable build should deterministically produce an artifact that is functionally identical to another from that same source.

### Independent

A build that only works on a singular specific environment might also be referred to as a "bad".  

This phenomenon, often known as the "magic build machine" or "works on my machine" build, depends on specific and often difficult-to-replicate criteria for the [[environment]] in which it builds.  Quite often, the reason for success on one platform and failure on another is unknown even to the originator of the code.

It is a goal of good build practices to make a build as transportable as possible.  This can be accomplished by making it as independent of a specific local environment as is reasonable.  This does not mean that a build should automagically work on **any** environment.  Rather this means that the build defines a set of environmental criteria that are required in order for the build to be performed reliably.

For practical purposes, [[continuous integration]] is impossible for builds that are not independent.

### Quality

This may seem counter-intuitive, but the quality of an [[artifact]] is actually the sum total of the criteria by which it had to pass in order to actually be created.

Possibly more clearly, the testing and validation that an [[artifact]] _successfully passed_ is the only real indicator of its quality. The greater number of, and more difficult to pass, these test steps are, the greater the sum of quality that can be ascribed to the artifact.

Everything else is an _opinion_ about quality, for practical purposes, and should be discarded.

Note that the _actual drive for executing a build process is to _ _**fail**_.  This is in order to have a means for rejecting anything that can be determined to be of less-than-desireable quality.


### Completion

With the combination of increasingly more complex [[lifecycle]] steps and [[gatekeeping]] operations, the [[build]]/[[release]] process intrinsically produces [[artifact]]s that have gone through an increasingly more difficult set of known steps to determine their viability and quality by way of those [[gatekeeping]] operations.
