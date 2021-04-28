---
date updated: '2021-04-28T08:41:19-05:00'

---

# Configuration Management

Configuration Management (usually shortened to "CM") is the meta-discipline that encompasses the smooth and general [[availability|availability]] of software [[environment|environments]].

Configuration management is generally thought to be a sub-discipline of [[process engineering]], which is itself a type of [[industrial engineering]] and is a direct manifestation of proper application of the requirements of [[project management]].

# Procedures

Configuration management itself is not an automation process.  It concerns itself with defining the specific criteria under which a given [[environment]] is considered correct, and the application of those criteria.

## Criteria

For practical purposes, most things in the world have some form of configuration management applied to them:

- a car has a maintenance schedule that should be kept.
  - The tires are in good shape
  - The oil has been changed within the specified time period
  - The brakes have been inspected and found to be working acceptably
- a lawn might potentially have requirements
  - The grass has been cut
  - Weeds have been removed
  - Fertilizer has been applied
- a laptop
  - Has the tools necessary to perform desired or required tasks
  - Has up-to-date security patches applied
  - Has an account for the correct user in its internal configuration

These are obviously simplistic examples, but demonstrate _some_ of the criteria that might be applied to any given target [[environment]].  For purposes of this document, only computer hardware/software-based systems will be discussed further.

## Determinism

Configuration management concerns itself with producing an output (the [[environment]]) that can be determined to be valid or not.  A valid environment is one that possesses all the relevant attributes; an invalid environment violates one or more of those attributes.

### Temporal Determinism

Temporal determinism is available only by inspection of the environment[^oxymoron].  This is almost always the result of a reliance on "temporal requirements", which themselves rely on temporal external factors.  Temporal requirements are usually easily detected due to non-specific temporal language, like "most recent" or "latest".

Examples include "the latest version of X" or "most recent release of Y".

As a more complex example, if a criteria for a given environment includes "has the latest security patches applied", then that criteria specifies temporal determinism.  It is impossible to ensure that the environment is valid unless someone or something checks that status.  Given the nature of security patches, they are almost always applied from some outside source.  That source is rarely under local control.  Even if it was, the language implies something that requires a check every time.  However, these conditions _probably_ will not prevent the environment from operating.  The environment is [[compliance|non-compliant]] (i.e. does not fulfill the requirements) but likely still operational (continues to perform its task in a non-compliant manner).

As a general rule, it is best to avoid temporal determinism.  Temporally deterministic requirements are usually the result of attempting to lazily describe requirements.

### Objective Determinism

Objective determinism is achieved when it is possible to determine that a criteria has been achieved _regardless of when the check is made, or where the check is made from_.

Contrary to the above example, "has the latest security patches applied as of 2021-03-18" is not temporal; it is true or it is not at all times.  Once the patches have been applied once, that condition is valid forever.  It might not be a good idea, but it remains valid.

Another instance is "depends on the latest version of dependency XYZ".  This is another "temporal" situation.  The "latest" version of a dependency is not under local control.  Contrast this with "depends on XYZ-1.2.3".  The specific version implies an objectively deterministic outcome.

### Local Control

Often, determinism can or must be enforced using "local control".  This would imply that some degree of management (probably through additional configuration management) is in place that allows deterministic application of elements.

For instance, imagine some CM had a temporal requirement (" depends on the latest version of X _in our artifact repository_").  This is still only temporally deterministic, but that determinism has become less chaotic.  It does not change the nature of the requirement, but does move the responsibility of what that requirement ultimately resolves as to some internal/local control.

Local control _usually_ manages _some_ of the determinism issues of temporal requirements.

## Drift

Drift occurs when a given [[environment]] no longer exhibits the criteria that configuration management [[idempotence#Operational State|defined as acceptable]] to be named such an environment.  Since it is essential to clearly and crisply define the criteria for an environment, it should be relatively easy to determine if an environment is in a state of "drift".

Note that being in the state of "drift" (also "drifting", "drifted", or "having drift"), _entirely invalidates_ the [[environment]].  It does not mean that the environment cannot or does not fulfill some function, but rather that the binary [[idempotence#Operational State|operational state]] of validity for the environment has been set to `false`.

For instance, CM might define the environmental conditions that a valid developer's laptop has the latest version of `python` installed -- obviously a [[#Temporal Determinism|temporally deterministic]], not [[#Objective Determinism|objectively deterministic]], requirement.  At any given moment, the state of that [[environment]] could be queried to see if it was [[compliance|compliant]], but even if it was not the developer could _probably_ continue to work.  At some point, they might note themselves to have drifted out of compliance with the target [[idempotence#Operational State|state]], possibly because all their tests failed since everyone else had moved on to the latest version.

[^oxymoron]:  "temporal determinism" is an oxymoron.  As soon as it is determined that something is valid "right now", the moment at which it was valid has passed.  For practical purposes, it "works" once and then has to be checked immediately again because that's the way time works.
