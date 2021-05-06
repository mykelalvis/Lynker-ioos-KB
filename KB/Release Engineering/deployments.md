---
date updated: '2021-03-16T03:55:49-05:00'

---

# Deployments

> In order to bake an apple pie, you must first create the universe.  -- Dr. Carl Sagan

A [[deployment | deployment]] is the primary action associated with providing access to any given system.  The primary heavy-lift of a deployment is performed by Terraform.  In order to make it possible for Terraform to deal with deployments in a rational and manageable way, there are numerous requirements that must be fulfilled.

# Requirements

Deploy should involve a process that has the following attributes:

1. Have a well-defined [[Lifecycle|lifecycle]].
2. Fed from a predefined and known[^known] set of [[artifact|artifacts]].
3. Performed on a fully automated [[environment|environment]].[^env1]
4. Be downstream-destroyable from any point in the lifecycle, so that for every step in the lifecycle, destruction of this deployment implies destruction of all downstream lifecycle states.<br/>When you destroy the root, you destroy the tree.
5. Have a unique identifier that denotes the specific deployment and allows for easy destruction
6. Be safe enough to use in any given environment
7. Be flexible enough to allow a given environment to change from the defined norm

# Environmental Deployments

In order to execute a deploy, it is necessary to produce the [[environment|environment]] to which that deployment is intended to run.  This involves a cascading series of steps for each environmental deployment until the executor reaches the limits of its capacity.

We refer to this form of [[bootstrapping]] as the "apple pie" method, per Dr. Sagan's statement.

For instance, to deploy to AWS, there are a number of requirements that must first be met:

1. What's this deployment's artifact?  The deployment itself must be based on a single artifact, although that artifact will almost certainly point to additional artifacts.  That artifact must be accessible by the executor and valid.
2. What will this deployment be called if it succeeds?  Everything needs a name.
3. Which account?  The executor needs to know the targeted account in which an AWS deployment is performed.  IF that account does not exist, then the deployment will fail.
4. Which user? The executor must perform a deployment as a user.  If that user does not exist, or the executor cannot work as that user, then the deployment will fail.
5. Which role?  The executor's user needs to be granted a role that will allow the deployment to complete.  If that role does not exist or is not granted to the user, the deployment will fail.
6. Which network?  It is possible, and almost certain, that a given deployment will use some form of networking.  The executor must be able to target a network (VPC for AWS) and determine if that network is already present or if it needs to be created to proceed.

It is possible that for any of the aforementioned steps, if a value is not available it should be created and added to the context of the nascent environment.
These are decisions that must be made with forethought.

# Precursor Environments

It is also entirely possible for each of the above decisions to be _the entirety of an environment_, and thus a [[dependency]] to a future environment.

If all environments require an account to target, then any environment that requires that specific account will [tautologically] point to the same account.  Therefore it makes sense to produce precursor environments that perform basic setups that enable downstream environments to succeed more easily.

In a situation where the same account is used for both production environments and development environments, multiple roles would likely be necessary as the production resources would be considered inaccessible by the certain resources.  These are probably known situations, and thus could be part of a precursor environment that gets built before any other environments are created.  The outputs of this precursor are then consumed downstream by the subsequent environmental builds.

The ability to define and execute precursor environments is frequently thought of as "setup", and is often performed manually.  The purpose of automation is to reduce the non-determinism that comes from such manual work.  It should be the case that all (or at least as much as possible) of this work can be performed using a precursor deployment.

Since multiple precursor environments could exist (5 instances of "develop" and 3 instances of "production"), it is necessary to perform some type of name-mapping to allow a subsequent environmental setup to select the proper precursor.

Obviously, any given environment that is required is a dependency and is a precursor environment to the given deployment.  For naming purposes, only the most durable of precursors will be referred to as such.

# Target Environment Determination

For a given deployment, a known set of conditions must exist.  From a clean slate in AWS, for instance, some form of root account email must be available in order to execute the deployment that will ultimately result in a "base precursor".
The following labels will determine this for a given deployment:

- If an environment is truly a "base precursor", then the only inputs would be some configuration that denotes whatever root credentials and setup information are necessary to produce that dependency.
- If an environment is some subsequent configuration atop a base precursor (examples might be "development environments in each AWS region"), then it will refer to the output identifier of its base precursor as a dependency.  These would be called "factory bases".
- If an environment is the _setup_ for an environment that will host an application, it will be called an "application base".  This might include seeded data and specific forms of access and values.  Its dependency would be a non-empty list of factory bases.  It is expected that a given application base contains the entirety of necessary resources to run some version of the application.
- Applications are referred to as "application deployments".  They refer to some identifier of a single artifact that understands how to determine which application base the candidate application is meant to reside in, as well as any additional resources that are application-specific.<br/>Application deployments are specified by a unique identifier, and they contain an attribute that is their [version](definitions.md#version).

# Types of Deployment

Applications may be meant to be deployed alongside other applications, in which case shared resources would need to be extracted and placed in some precursor environment.  Conversely, they may be meant to live in an isolated world where they share no resources with anything outside of their environment.  This is an aspect of the artifacts used to deploy the application.

The code used to deploy the application determines the type.  There are several attributes that might define how an application is meant to be deployed:

## Isolated

This application must be protected from outside interference.  It likely has some form of input and some form of output, but none of its specific resources are meant to be accessible from any other application.

## Open

The state of not being Isolated.  Open deployments may have published roles meant to allow other resources to use them.

## Durable

This application is stateful. When destroyed, its state must be preserved as part of the destruction and that state might be used as a dependency in future deployments of versions of this application.

## Stateless

The opposite of Durable.  This application can be created and destroyed with abandon.  This is the classic stateless lambda (not a lambda-based application, which is likely to be Durable).

## Protected

This application, most likely an Isolated one, contains dangerous data and extra work should be done to ensure that this data is safely guarded.

## Public

This application, most likely an Open one, is the opposite of Protected.  No guarantees about the privacy of the data in it are made.

## Examples

For precursor environments, most resources are likely to be accessible downstream.  These are thus Open deployments.  Conversely, most applications have durable data that needs to be privately processed.  These are most likely Isolated Durable Protected deployments.

TODO Add examples

[^known]: but not necessarily [[release|released]] yet
[^env1]:   Thus, a dependency of a given deployment is that the deployment for its [[environment]] has been performed successfully and the identity of that environment is known.