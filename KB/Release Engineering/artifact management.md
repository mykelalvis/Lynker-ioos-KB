---
date updated: '2021-04-28T09:07:21-05:00'
tags:
  - dependencies

---

# Artifact Management

Artifact management is an aspect of [[release engineering]] that concerns itself with the naming and disposition of artifacts, generally a software [[artifact]].

Artifact management is the rule set that determines how an artifact is named, retained, dispensed, and disposed of.

## Naming

[Names are hard](https://martinfowler.com/bliki/TwoHardThings.html).  Good names are even harder.  Determining the name of an [[artifact]] is a task that should not be undertaken lightly, but should conform to existing practices that prevent future failure.

### Specificity

A name should fully identify an [[artifact]], where "name" indicates the fully-qualified name of the thing.  Abbreviated names, sometimes called a "short" name, is possible and valid to use in casual conversation, but should be avoided when specificity is indicated (i.e. all non-casual situations).

For purposes of portability, the "name" of an artifact will be called it's `artifactId`

Names should include some namespace that groups similar items together.  Most packaging systems, [[dependency#Management|dependency management]] systems, and [slug](https://support.atlassian.com/bitbucket-cloud/docs/what-is-a-slug/)-dependent [resolvers](https://en.wikipedia.org/wiki/Boolean_satisfiability_problem) use some form of namespace to allow the actual [[version|identity]] of an artifact to be different even though the short name might be the same.  So `X:Y` or `X/Y` uses `X` as the namespace and `Y` as the abbreviated name.

> "You're in the Army?  I have a friend named 'Bill'.  He's in the Army.  Do you know him?"

### Avoid Collision

Naming collisions do not happen all that often, but they are not unheard of.  A practice that avoids this is the [Reverse Domain Notation](https://en.wikipedia.org/wiki/Reverse_domain_name_notation) generally popularized by the Java `package` system.  Since DNS names are owned by someone, using a namespace implies ownership of the space.

For purposes of portability, the "namespace" of an artifact will be called it's `groupId`

> "You wrote a package called 'test?  What a coincidence!  I _also_ wrote a package called 'test'!"

### Include Versioning

In the example above, `X:Y` was shown as a name.  However, this is a general name, not a specific one.  In order to increase specificity, the actual [[version]] number is expected.  `X:Y` is all `Y` artifacts in the `X` namespace, but `X:Y:1.2.5` is version `1.2.5` of artifact `Y` in the `X` namespace.

In most cases, the `NAMESPACE` + `SHORT NAME` + `VERSION` is sufficient to indicate a specific artifact, but not always.

> "Was it version 1.2.5 or 1.2.6 of `X:Y` that prevented the complete deletion of the database?"

### Include Typing

Sometimes, even the namespace and versioned identifier is insufficient to indicate the specific artifact.  Sometimes a type has to be applied, as well.  This is usually because there are different pacakges of different types for a single version. Sometimes it indicatea the purpose or packaging of the artifact in question.

This simple presupposes additional information in the types above:  `X:Y:1.2.5:zip` is the `zip` typed artifact of version `1.2.5` of `Y` in the `X` namespace, whereas `X:Y:1.2.5:tgz` would be the same artifact but as a tarball and `X:Y:1.2.5:docs` might be a zipfile full of documentation.

Types are open to convention and interpretation, and are really just present to allow further specificity to the artifact identification process.

> "Fetch the documentation for `X:Y:1.27`.  It's called `X:Y:1.2.7:docs`"

### Classification

An artifact occasionally has _even more differentiation_.  For instance, `X:Y:1.2.7:docs` might not be sufficient.  It's possible that there are `dev` docs and `prod` docs.  This additional differentiator is referred to as the `classifier` and comes at the end of the string that identifies the classified artifact.

> "The `dev` docs for `X:Y:1.2.7` are `X:Y:1.2.7:docs:dev`"

## Lifecycle

Artifacts are often, but not always, kept in an [[artifact repository]].  If they are, then lifecycle information about how to manage them across time _may_ become valuable or necessary.

### Retention

This is the amount of time that the artifact must be retained, and the conditions under which it must be kept.  Secrets should remain protected, and audit logs may need to be kept for years.

Some types of artifacts must be kept for extended periods of time.  This complicates the situation greatly.

### Disposal

Ridding oneself of artifacts, especially [[release|releases]], might seem counter to the point of retaining immutable releases in perpetuity.  However, it is typically assumed that the law overrides practices in release engineering and some laws require the disposal of various artifacts.  This is often after a mandated retention period[^retent].

## Dispensation

This is the mechanisms by which an [[artifact]] is made available to its [[audience]]. For an [[artifact repository]], the mechanisms are usually pretty clearly defined by the configuration of the tool.  Other systems might have different mechanisms, such as using an `S3` bucket as an artifact store means that read access needs to be available to fetch things to it, and write access is required to produce a new artifact.

[^retent]: So, yes, being forced to keep some things for 10 years after which they must be immediately destroyed is a thing.
