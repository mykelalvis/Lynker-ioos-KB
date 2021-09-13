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

## Deployment Documentation

Workable deployment documentation makes unabiguous statements regarding the actions and verifications associated with a deployment.  The lack of ambiguity is essential, as that allows for relatively digestable precondition/validation work, as noted above.

A good de
