Thoughts/Notes for [[SandboxDevContainer]] :

- A [[buildbox]] can be of any distro
	- The work it takes to produce a given dependency for some [[devcontainer]] is entirely orthogonal and irrelevant to the actual distro.
	- [[Dependency]] issues must be managed.
	- A build box can be part of staged container build where we copy a finalized executable from the build container into a runtime container.
- Any [[devcontainer]] is based on an RPM-based distro _until we can do some more minimal distro equivalent_.
	- A stretch goal is to produce a minimal distro that will allow for development on a given model
	- Another goal is to produce a minimal distro that will allow for running a given model
- An arbitrary number of [[devcontainer]] [[variants]] are possible:
	- [[Gnu dev container]]s are probably gcc and gfortran (fortran90)
	- [[Intel OneAPI containers]] are Intel OneAPI based.
- It is likely that a given model will have some unique container as an element of its runtime, forcing us to either rebuild containers continually or manage containers in a [[container registry]] externally (i.e. likely [[DockerHub]])

## Standard Code
Some things are generally small or self-contained enough that installing them is common:
- [[python]] (3.x)
- [[direnv]]
- [[tfenv]]
- [[fpm]] (if a sufficient version of [[ruby]] is available )
- [[makedepf90]]
- [[pipx]] and some associated installs
	- [[poetry]]
	- [[ansible]]
	- [[AWS CLI]]

 
## Integrated Development Environment
 
 The [[SandboxDevContainer]] is primarily designed to work with [[devcontainer]] instances. This implies that the IDE is _probably_ [[VSCode]] and that the underlying container runtime is probably [[Docker Desktop]].
 
