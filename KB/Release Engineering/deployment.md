---
date updated: '2021-04-28T08:41:31-05:00'

---

# Deployment

<dl>
<dt>deployment</dt>
<dd>(n.) induced availability of an artifact in an environment</dd>
</dl>

Deploying (aka "doing a deploy" or "doing a deployment" [sic] ) is the process by which some version of an [[artifact]] becomes [[availability|available]] to its intended [[audience]].  This might be a service running in AWS, or it might be a tag in GitHub for a library, or it might be a RubyGem deployed to a GemStore ( a type of [[artifact repository]]).

Deployment is typically the most human-intensive of the automatable processes that a build and release process defines.  Frequently there are human interventions that need to be performed to enable progress.

The goal of a good automated release engineering process is to reduce the number and frequency of those interventions.  The hopeful target of this is reduction to zero, where the entire process can be automated.

## Deployment Steps

A deployment is broken up into several parts

### Deployment Preconditions
These are the preconditions that are [necessary and sufficient](https://en.wikipedia.org/wiki/Necessity_and_sufficiency) to allow a deployment to commence.  These preconditions are, themselves, frequently deployments that have their _own_ preconditions, such as a #bootstrapping effort.

### Deployable Artifact
This is the simplest of the elements of a deployment.  Either there exists an artifact to deploy, or there exists no deployment.  There are a lot of ways to mince words about that, but those things inevitably lead to non-deterministic outcomes or error.

### Deployment Validation
In order to start a deployment, it is necessary to understand what it means to succeed at that act.  If it is not known what the validation conditions are, then it seems difficult to ascertain that the work is complete.


## Standard Deployment Conditions

Sometimes, certain conditions are so common that they can be extracted to some centralized location and referenced at that location rather than repeating the steps.

For instance, if one was meant to deploy an application to some cloud provider in the "production account", it is reasonable to expect that they need "production account" keys.  However, _it is **not reasonable** to fail to indicate those standard conditions at least once_.

### Example Standard Deployment Conditions

Such a file might exist in a subdirectory of a repo called `deploy/aws/prod.md`

```
# Standard Deployment Conditions for Production AWS Account Deployment

## `direnv` 

Direnv is used to set environment variables during a deployment.  The direnv package must be locally available to the deployer.

## Standard .envrc file
The [[standard envrc file]] must be made available as part of the execution.  It is the responsibility of the deployer to select the proper .envrc.  

The `.envrc` file must contain _at least_ the following entries:
- `export AWS_ACCESS_KEY_ID={prod accessid}`
- `export AWS_SECRET_ACCESS_KEY={prod secret access key}`
- `export DEPLOYER=$(user)`

## Read access

The deployer needs read access to the specfic artifact as part of an artifact resolution chain within the [[artifact repository]].

```

## Deployment Documentation

Workable deployment documentation makes unabiguous statements regarding the actions and verifications associated with a deployment.  The lack of ambiguity is essential, as that allows for relatively digestable precondition/validation work, as noted above.

A good deployment document is usually:
1. References standard deployment conditions.
2. In the repository where someone is likely to be tasked with "doing a deployment"
3. Is up-to-date
4. Contains all the preconditions, indications of artifact, and validation mechanisms
5. Lives in a file called `deploy.md` 

```
# Deploy ThingX to AccountY in CloudProviderZ

## Preconditions
1. [Standard prod deployment preconditions](https://github.com/myorg/mystandards/deploy/aws/prod.md)
2. Read access to `ThingXNamespace/ThingX` version according to deployment request
3. Acquire artifact `ThingXNamespace/Preconditions/3.1.1` prior to deployment, as version 3.1.1 of the ThingXNamespace/Precodnitions must be deployed before ThingX is deployed.
4. Deploy `ThingXNamespace/Preconditions/3.1.1` according to `Preconditions/deploy.md` 

## Artifact
`ThingXNamespace/ThingX`

## Deployment Steps
1. `mkdir workdir`
2. `cd workdir`
3. `cp {STANDARD ENVRC FILE} .envrc`
4. `direnv allow`
5. `make`
6. 
6. `make deploy`
7. Expect that 

## Validation Conditions
1. `curl `


```

It should be obvious that step 4 includes the verification conditions associated with `ThingXNamespace/Preconditions/3.1.1` deployments.
