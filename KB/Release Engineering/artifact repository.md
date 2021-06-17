---
date updated: '2021-03-16T03:55:14-05:00'

---

# Artifact Repository

## Definition
An artifact repository is an [[environment]] that has been specifically configured to store, manage, and distribute [[artifact|artifacts]].  The [[platform]] that the environment is based on is determined by the type and scale of the desired repository.

## Examples

Some examples of artifact repositories are:

- Sonatype's Nexus (General Purpose)
- JFrog's Artifactory (General Purpose)
- PyPI (Python packages)
- Docker Hub (Docker containers)
- Elastic Container Registry on AWS (Docker containers)
- npm.org (NPM packages)

## Bad Examples

Some rudimentary storage locations, which might be used in a pinch, are:

- Azure Artifacts - this artifact repository is, as of the time of this writing, severely limited in it's capabilities
- GitHub Releases - a per-repository file storage mechanism, but convenient if a given team only produces a very small number of artifacts (i.e. one or two)
- Jenkins' internal artifact store - entirely transient but useful during a Jenkins build
- GitHub source repositories, using Tags - this is effectively what constitutes a [[tag release]].  It is also the default mechanism for some parts of some languages (notably `golang` libraries).

### Why Tag Releases Are Bad
Tag releases tend to have negative effects and/or side effects that can be difficult to detect or deal with.  Notably, a tag release of a repository that one does not completely control ___cannot even be considered truly immutable___.  Being under the control of someone outside the organization, that person can change the contents of a tag and the underlying systems will notice, but will often not complain.  It is _possible_ to determine that a tag release has not been mutated, but it certainly is neither automatic or convenient to do so.

One (painful) way to deal with this is to fork such a repository and only pull in changes on a "per-tagging event" basis. This is very tedious and usually not worth the effort.  

## Uses of an Artifact Repository

An artifact repository is designed to be the [ostensibly] durable storage location of [[artifact]] instances for use by some target [[audience]]. The presence of an artifact in such a repository often indicates that some degree of [[gatekeeping]] was performed on the [[build]].  Depending on the [[platform|platforms]] involved, sometimes this gatekeeping is "none", but the quality of the retained artifact is highly dependent on this.

Typically, a build would store built artifacts into the artifact repository as a final step of the build.  This implies that the artifact then has some [[version]] (i.e a named identifier) that uniquely indicates the stored artifact.  This identifier can then be used later to retrieve the artifact, generally for use in some other process.

### Examples of Artifact Repository Usage

#### Container Registry

Alice uses [Packer](https://packer.io) to [[build]] a [Docker](https://www.docker.com/) container.  She namespaces it `alicecontainer/emulsified` and gives it a version of `1.0.0`. Since Alice has access to an [[artifact repository]] at <https://myartifactrepo.org/dockerregistry>, as part of her Packer `.json` file Alice performs a `push` action to that repository.  There is now an artifact in that repository called `alicecontainer/emulsified:1.0.0`

Bob wants to use Alice's container as a basis for his `Dockerfile` build. He points his registry endpoint to  <https://myartifactrepo.org/dockerregistry> and bases his build on `alicecontainer/emulsified:1.0.0`.  Docker then tries to fetch the container during his build, allowing Bob to layer his filesystem over the `emulsified` container's filesystem.

#### Python Package Repo

Carole is building a Python library for distribution to the other members of SampleCo, where she works.  There is an [[artifact repository]] for Python packages at <https://artifacts.sampleco.com/python>.  She builds and deploys her package as `carolesmith/mylib` version `2.3.2` to that repository.  Now it is available to other developers who have access to that repository inside SampleCo simply by indicating `carolesmith/mylib 2.3.2` in a Python `requirements.txt` file.

#### General Artifact Availability

Dave has a file full of private information that he needs to distribute to a specific group of people.  Dave, who also works at SampleCo, knows that there is an ACL-restricted generic repository at  <https://artifacts.sampleco.com/secretsauce> .  So Dave produces a file called `thesauce.zip` which he uploads to that repository with a version of `dave/sauce 1.0.0`.  He then builds a group `Sauceaholics` that has access to that repository (which is very different from general internal availability at SampleCo) and adds all the consumers that need access to the sauce `.zip` file to that group.  Those users can then go fetch the appropriate file, and if Dave uploads a newer version `dave/sauce 1.0.1` they can also obtain that one.

Ellen _also_ needs to send a file to the members of `Sauceaholics`.  She uploads her file as `ellen/notsauce 1.0.0` to that repository and now all members of `Sauceaholics` can also fetch Ellen's artifact.
