---
date updated: '2021-03-16T03:55:52-05:00'

---

# Environment

<dl>
<dt>environment</dt>
<dd>some very precisely configured platform</dd>
</dl>

An environment is what happens when you make a deployment that generates a specifically configured platform instance.

## Configuration

Environments are instances of platforms that have well-defined criteria associated with them that were specified by the configuration that was exercised during the deployment that produced them.

Working machine(s) geared toward specific tasks are environments.  They require certain tools to perform whatever tasks a developer requires.  Configuration should be applied to that machine, preferably using [[IAC]], to force a given machine into the appropriate state.

## Recursion

> ["Ya can't get theah from heah"](https://www.urbandictionary.com/define.php?term=you%20can%27t%20get%20there%20from%20here) -- Resident of Maine c. forever

Sometimes reaching an appropriate environment is a set of steps that produce environments that can be used to make the target environment.  For instance, a [[build environment]] is an environment that is required to produce a [[build]], but it is also just like any other environment and needs to be created.  This might require some sort of recursive [[bootstrapping]] process, where one creates an underlying needed environment and uses that to launch the next layer of environments.  

## Environment Examples
For example, a developer's working laptop is an environment that would require specific tools in order to work properly.  A `python` developer's box might, for instance, require a specific version of `python`; a `golang` developer's box would need the `golang` environmental configuration applied. Both of these developers might be working against a database, though, and would perhaps need a local environment that is capable of connecting to that database (itself another environment).

A [[deployment|deployable]] target, `dev`,  could be considered an environment of the platform that a given application targets.  It perhaps has a different set of requirements that a developer.  For `golang`, perhaps the compiler is no longer needed, since `golang` outputs are statically linked binaries.  Instead, it is possible that `dev` points to an identically configured but entirely different database, requiring different configuration to produce an environment that is properly [[configuration management|configured]].

Likewise, `prod` could also be considered an environment, but perhaps `prod` is different than `dev` for various configuration reasons.  It has not developer tools, to ensure that no rogue processes try to compile errant code.  It has access to different databases or storage endpoints, properly restricted through [[configuration management]].

As an example of recursion, a given deployment might require several different Docker containers to be configured identically.  It makes little sense to do that configuration each time you build each container.  Instead, one might produce a configured _base_ container and layer the individual targets atop that.  Each of those builds would not need to reproduce the build that make the container they are based on.  That has already been accomplished as a separate task.
