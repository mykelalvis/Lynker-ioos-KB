---
date updated: '2021-03-08T07:35:10-06:00'

---

# Release Engineering Definitions

When discussing operational practices, the following definitions apply to the following terms.

No other operating definition will apply in that context.

_**Some definitions will migrate to independent pages over time.\
Please update your references as-needed.**_

# Artifact

<dl>
<dt>artifact</dt>
<dd>something produced somehow</dd>
<dd>a metaphorical bucket of bytes in software terms</dd>
</dl>

An artifact is something we care about.  For purposes of this document, it's a collection of bytes that composes some sort of semantic meaning within our company.  This could be a document, a collection of data, or a compiled and tested piece of code.

Some few examples of artifacts are:

- A `.jar` file from a Java Maven or Gradle build.
- A statically linked OS-specific binary from a Golang build.
- A PDF from building a set of markdown files using `pandoc`
- A `.zip` file created from collecting a set of documents and using InfoZip to create a ZIP file from them.
- A `.docx` generated with Microsoft Word or LibreOffice.

The goal of a delivery process is to reach the state of a released artifact.

Artifacts also have meta-conditions.  A [[#Release]]d artifact has attributes beyond those of an unreleased artifact.  Artifacts are also generally [[#Version]]ed.

# Version

<dl>
<dt>version</dt>
<dd>the entire identifying signature of an artifact, possibly but not necessarily one that has gone through a release process</dd>
</dl>

A version of an artifact is the entire signature that formally identifies it.  Depending on the type of artifact, this might be as durable as the signature of a  compiled archive stored in an [[#Artifact Respository]], or it might be as transient as the URL of a tag applied to a repository in [GitHub](https://github.com).

Versions can are divided into mutable and immutable types.  Most software does not contain sufficient complexity to distinguish between the two explicitly, but this is rarely cause for concern (until it is).

_By convention_, mutable versions are considered transient and disposable.  That is, they have a short and indeterminate lifespan and, since they technically should be easy to replicate, can be thrown away at any time.

In contrast, immutable versions are retained in perpetuity.  They are never deleted or changed.  The expectation is that they will be available effectively forever.

In theory, the artifact that an immutable version references under those _conventional_ circumstances would itself be considered _immutable_.

In reality, this is one of the Hard Problems™ and can be difficult to verify.

# Release

<dl>
<dt>release</dt>
<dd>(n.) an immutable version of an artifact produced by a build</dd>
<dd>(v.) to produce an immutable version of an artifact by performing a full build successfully</dd>
</dl>

A release is the production of an immutable [[#Version]] of an [[#Artifact]].  Since this implies the existence of an artifact, it also implies that that artifact has passed whatever [[#Gatekeeping]] operations are present in the [[#Build]].

# Platform

<dl>
<dt>platform</dt>
<dd>somewhere that you want some artifact to be available to its consumer</dd>
</dl>

A platform is a named target.  It is expected to be a place where you might induce availability of
services by way of deploying artifacts.

Specifically, the term "platform" is an abstraction.  For specific implementations, the
term is "[[#Environment]]" (i.e. an "environment" is a "platform")

The runtime for an application could be considered the platform.  In addition, the platform that this platform
runs on is _probably also_ an environment (and this a platform).

# Environment

<dl>
<dt>environment</dt>
<dd>some very precisely configured platform</dd>
</dl>

An environment is what happens when you make a deployment that generates a specifically configured platform instance.

Environments are instances of platforms that have well-defined criteria associated with them that were specified by the
configuration that was exercised during the deployment that produced them.

For example, "dev" could be considered an environment of the platform that a given application targets.  "Prod" could also be
considered an environment, but perhaps "prod" is different than "dev" for various configuration reasons.

# Availability

<dl>
<dt>availability</dt>
<dd>the general state of being accessible to a consumer</dd>
</dl>

Availability is a transient attribute indicating accessibility by valid consumers.
A service, platform, or artifact may or may not be available.
If it is available, then it is in a place where it could be accessed by a valid consumer.
If it is unavailable, then it has become (or possibly was never) accessible by its consumer base.

Note that as a transient attribute, availability will come and go depending on internal and external factors.

For instance, if AWS goes down and your application is hosted on AWS, then it has become inaccessible.  You might need to do
something to rectify this, or you might not.

# Artifact Repository

An artifact repository is an [[#Environment]] that has been specifically configured to store, manage, and distribute [[#Artifact]]s.  The [[#Platform]] that the environment is based on is determined by the type and scale of the desired repository.

## Examples

Some examples of artifact repositories are:

- Sonatype's Nexus (General Purpose)
- JFrog's Artifactory (General Purpose)
- PyPI (Python packages)
- Docker Hub (Docker containers)
- Elastic Container Registry on AWS (Docker containers)
- npm.org (NPM packages)

## Bad Examples

Some **very** rudimentary storage locations, which might be used in a pinch, are:

- Azure Artifacts
- GitHub Releases
- Jenkins' internal artifact store
- GitHub source repositories, using Tags

These tend to be terrible, and should be avoided if at all possible.

## Uses of an Artifact Repository

An artifact repository is designed to be the [ostensibly] durable storage location of [[#Artifact]] instances for use by some target audience. The presence of an artifact in such a repository indicates that some degree of [[#Gatekeeping]] was performed on the [[#Build]].  Depending on the platforms involved, sometimes this gatekeeping is "none", but the quality of the retained artifact is highly dependent on this.

Typically, a build would store built artifacts into the artifact repository as a final step of the build.  This implies that the artifact then has some [[#Version]] (i.e a named identifier) that uniquely indicates the stored artifact.  This identifier can then be used later to retrieve the artifact, generally for use in some other process.

### Examples of Artifact Repository Usage

#### Container Registry

Alice uses [Packer](https://packer.io) to build a [Docker]() container.  She namespaces it `alicecontainer/emulsified` and gives it a version of `1.0.0`. Since Alice has access to an artifact repository at <https://myartifactrepo.org/dockerregistry>, as part of her Packer `.json` file Alice performs a `push` action to that repository.  There is now an artifact in that repository called `alicecontainer/emulsified:1.0.0`

Bob wants to use Alice's container as a basis for his `Dockerfile` build. He points his registry endpoint to  <https://myartifactrepo.org/dockerregistry> and bases his build on `alicecontainer/emulsified:1.0.0`.  Docker then tries to fetch the container during his build, allowing Bob to layer his filesystem over the `emulsified` container's filesystem.

#### Python Package Repo

Carole is building a Python library for distribution to the other members of SampleCo, where she works.  There is an artifact repository for Python packages at <https://artifacts.sampleco.com/python>.  She builds and deploys her package as `carolesmith/mylib` version `2.3.2` to that repository.  Now it is available to other developers who have access to that repository inside SampleCo simply by indicating `carolesmith/mylib 2.3.2` in a Python `requirements.txt` file.

#### General Artifact Availability

Dave has a file full of private information that he needs to distribute to a specific group of people.  Dave, who also works at SampleCo, knows that there is an ACL-restricted generic repository at  <https://artifacts.sampleco.com/secretsauce> .  So Dave produces a file called `thesauce.zip` which he uploads to that repository with a version of `dave/sauce 1.0.0`.  He then builds a group `Sauceaholics` that has access to that repository (which is very different from general internal availability at SampleCo) and adds all the consumers that need access to the sauce `.zip` file to that group.  Those users can then go fetch the appropriate file, and if Dave uploads a newer version `dave/sauce 1.0.1` they can also obtain that one.

Ellen _also_ needs to send a file to the members of `Sauceaholics`.  She uploads her file as `ellen/notsauce 1.0.0` to that repository and now all members of `Sauceaholics` can also fetch Ellen's artifact.

# Build

<dl>
<dt>build</dt>
<dd>a process that might produce a potential artifact</dd>
</dl>

The build is one of the most common software activities.  It is the process by which we determine that some arbitrary collection of bytes has become less arbitrary and more important.

Frequently, a build has some ceremony around it.  One such task would be invoking a "build tool", such as `make` or `mvn` or `rake`.
Given proper management, a build will have a [[#Lifecycle]] that it defines.  The outcome of traversing these lifecycle states should be identifiable as either an [[#Artifact]] (which we might care about) or a "failed build" (which we would likely discard) through a failure in some form of [[#Gatekeeping]].

## Lifecycle

Everything that exists has a lifecycle, and builds are no different.\
Specific to this discussion, the lifecycle of most software looks something like:

1. Non-existent
2. Existent but not valid or valuable (an empty repository or document)
3. Existent and possibly valuable (being worked on)
4. Valuable (some series of criteria have been met)
5. Versioned with an identifier that signifies the value from step 4
6. Changed (after being versioned) and thus needing to do one of two things:
   1. Return to step 3, becoming _possibly_ valuable again<br/>OR
   2. Be determined to no longer have value, be removed from validity, and destroyed.

Any sort of [[#Artifact]] can and will transition through these (and usually many far-more-specific) states.

- A document will be created,  updated, and eventually some indicator made of its state.
- A software repository will be able to be examined and found to be valuable and this have that state saved.

It should be noted that what is generally considered a build does not _only_ have to be part of this lifecycle.
There can be additional intermittent tasks that are not directly relevant to the production of versions.   However, as they are not prescriptive in the [[#Lifecycle]], they are essentially considered one-offs.  Usually, these are ignored during the process to produce an [[#Artifact]].

## Gatekeeping

The task of ensuring that a given artifact is viable is gatekeeping.  The gatekeeping action falls inside steps 3-5 above.
Gatekeeping operations are the specific sub-states in the lifecycle of an artifact that must meet some criteria before proceeding to the next state.

Consider the following lifecycle, subsequent additions to list of general states (1-8) above:

3.1 Not lintered<br/>
3.2 Not compiled<br/>
3.3 Untested<br/>
3.4 Insufficiently Tested<br/>
3.5 Not packaged<br/>

In this case, we could say that for step 3.1 to be passed, the linter for the code could be passed and thus the contents of the code could be eligible for compilation.  Once 3.2 is passed, the code has been "compiled" (whatever that means).  Then tests (3.3)  might be run. If, and only if, they pass would we determine if they were sufficient to proceed (3.4).  If they were sufficient, then the process would move on to the packaging step to produce some potentially useful artifact that could move through some possible additional gatekeeping to move to step 4 above.

If 3.1, 3.2, and 3.3 passed, but 3.4 did not, then the code never made it past the state of being insufficiently tested.  It _remains_
insufficiently tested, and therefore is not eligible for packaging and thus cannot enter the "not packaged" state because it didn't pass the 3.4 gatekeeping operation.  As a "not packaged" thing, it never reached its [[#End State]] and cannot be identified as an [[#Artifact]].

Now, consider a document written in Google Drive by someone.  It contains some content, so it has reached step 3.

3.1 Not edited<br/>
3.2 Not reviewed<br/>
3.3 Unaccepted<br/>
3.4 Not packaged<br/>

The text might have been speed-typed into the document.
It would then need to pass "editing" and "Review" and the "Acceptance" to be assigned a version in the Google Document version history.
This would constitute "packaging" it for outside use.

Note that this lifecycle is not how non-engineers typically do their work.  It _IS_, however, the prescribed method for actually producing useful documents.  Further, it does not require one to export copies and rename them with new dates to pass around in emails like savages.

## End State

Note that the _actual drive for executing a build process is to _fail__.  This may seem counter-intuitive, but the quality of an artifact is actually the sum total of the criteria by which it had to pass in order to actually be created.

In slightly simpler terms, the testing and validation that an artifact _successfully passed_ is the only real indicator of its quality. The greater number of, and more difficult to pass, these test steps are, the greater the sum of quality that can be ascribed to the artifact.

Everything else is an _opinion_ about quality, for practical purposes, and should be discarded.

With the combination of increasingly more complex lifecycle steps and gatekeeping operations, the build and release process intrinsically produces artifacts that have gone through an increasingly more difficult set of known steps to determine their viability and quality by way of the various gatekeeping operations.

# Deployment

<dl>
<dt>deployment</dt>
<dd>(n.) induced availability of an artifact in an environment (see what I did there?) </dd>
</dl>

[Deploying](./deployments) (aka "doing a deploy" or "doing a deployment" [sic] ) is the process by which some version of an artifact becomes available to its intended audience.  This might be a service running in AWS, or it might be a tag in GitHub for a library, or it might be a RubyGem deployed to a GemStore ( a type of [[#Artifact Respository]]).
Deployment is typically the most human-intensive of the automatable processes that a build and release process defines.  Frequently there are human interventions that need to be performed to enable progress.

The goal of a good automated release engineering process is to reduce the number and frequency of those interventions.
The hopeful target of this is reduction to zero, where the entire process can be automated.

# Continuous Integration

<dl>
<dt>continuous integration</dt>
<dd>the act of determining if a given change to an artifacts repository/source passes a known set of gatekeeping operations and notifying consumers about that state.</dd>
</dl>

A continuous integration server, such as Jenkins or Travis-CI, is a tool that watches for changes in sources (i.e. repositories) and performs some specific task against that changeset.  The outcome of that task is almost always meant to be a binary decision (pass or fail) based on whatever gatekeeping operations were present in the task definition itself.

The specific task that is run is _nearly always_ the build lifecycle up to some point that can be easily replicated.  This point is _nearly always_ getting past the "not tested" state. When a CI build passes for a repository, it is generally because the "compile and test" parts of its build lifecycle were successful.  This allows the worker iterating on that repository to pass some of the work of "watching the build" off to the CI server, with the confidence that they will receive a notification of some sort when the end-state has been achieved and determination of success or failure is known.

Often, CI servers will be hooked into messaging services such that the notifications of success or failure do not require the worker to leave their work and go look at a status page on the CI server.  They can simply receive a "Build X passed" or "Build X failed" style notification in Slack, Teams, by text message, or email.  This is especially important when the task takes a long time to complete.

Continuous integration has been the _de facto_ operating standard of software development for several years now.  The lack of CI work on a project is an indicator of a lack of certain types of transparency.  This sort of implicit obfuscation is not necessarily an indication of poor software development practices, but it is generally perceived to be such.

# Continuous Delivery

<dl>
<dt>continuous delivery</dt>
<dd>the act of performing a deployment based on the results of a build and release lifecycle execution.</dd>
</dl>

Continuous delivery is an extremely contentious topic.\
On the one hand, the idea that software could simply be deployed when its quality is of a certain level is intriguing.\
On the other hand, it is generally regarded that software is hard, all software has bugs, and some of those bugs might be extremely dangerous.

CD comes with an inherent risk.  The mitigation of this risk has a lot to do with how much risk an enterprise is willing to bear, and how much work the enterprise is willing to put into the mitigation process itself.

As a general rule, most CD is not done into production. Rather, CD is usually done into development, testing, or staging environments. It is then observed for some period of time to determine that it has not become sentient and is plotting to destroy the world. Once the observation period has passed, a determination is made that the deployment should or should not be moved to production.

This determination is _usually_ informed by the aforementioned risk-reduction testing.  However, in many real-world scenarios one or more ~3 lb. neural net wetware processors are utilized to make that determination using this information.

# CI/CD Pipelines

The "pipeline" is the set of steps that compose the attributes above.  Depending on what tooling is used, this will mean configuration files in repositories or external configuration with additional tooling.

For now, the assumption is that the CI pipeline has pretty high value; the CD pipeline has somewhat lower value; and these should grow as time and effort permits.
