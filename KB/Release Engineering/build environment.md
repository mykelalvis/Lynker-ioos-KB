---
date updated: '2021-04-21T12:17:32-05:00'

---

# Build Environment

The build environment is just that, an [[environment]] that is capable of performing some [[build]].

## Types

All build environments are not created equally.  It is generally desirable to have some form of internal [[gatekeeping]] operation that determines if an arbitrary build environment is capable of actually performing a [[release]], for instance.

In instances where there are specific [[role|roles]] that deal with [[release]], [[testing]], and general development, some specific [[configuration management|configurations]] might exist that determine these roles.

For example, a developer needs to [[build]] their code.  The build environment for that developer needs to be able to fetch [[dependency management|dependencies]], compile code, and perform other processes, such as a [[deployment]] to some `dev` [[environment]].  The requirements for that could be as simple as a pair of credentials, like an `ssh` key for dependencies and some cloud access key for deployment.

A tester, on the other hand, might need to deploy to a different environment (`test`, perhaps).  This would necessitate the same types of credentials but with different values.

Someone performing a [[release]] would need all the same types of credentials, but possibly for a third type of environment.  This user might additionally need more credentials for an [[artifact repository]] to actually deliver some [[intermediate form]] [[artifact]] to that repository.

## Creating

A build environment is produced much like any other [[environment]].  The environment's base is defined, such as "an operating system with the latest release of `python` 3.7 installed".

The key to creating any environment is to define the initial [[environment|environmrnts]] that define acceptable base conditions.  Hopefully, careful planning was applied to the situation, allowing these these base environments to be portable across different types of installations.

Once the base conditions are settled on, the act of producing an environment involves [[configuration management|applying some series of changes]].  If proper planning was applied, it is likely possible that these changes could be applied with a single set of [[IAC]] code for any given target environment.

This would imply some sort of artifact that defines a target [[role]].  An [[IAC]] role called `projectx-developer-tools` might define packages and required configurations for any system [[audience|entity]] that would need to execute development practices (most notably a [[build]]).  Another role, `projectx-tester-tools`, might extend the `projectx-developer-tools` role with additional testing tools that a developer wouldn't need or want.

Either of these roles might then be applied to some underlying [[environment]].  These environments might even be quite disparate -- a developer's base environment could be an up-to-date Windows 10 Pro machine, while a tester's was something else, perhaps an up-to-date MacBook Pro, and the final target runtime might be a Linux ARM box.  The same sort of configurations could be applied, but using different techniques.  Again, proper planning allows the use of [[IAC]] to maintain consistency within the environments.  Lack of this consistency results in [[configuration management#Drift|drift]].  If the developer tools were `golang` version 1.15 but the tester's tools were 1.14, this could cause some unnecessary friction.
